---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/nlp/cac-thuat-toan-o-lo-ra-n-lo-ra-mo-la-slim-nen-tang-regularization-constraint-va-trien-khai-thuc-te-1-1/","pinned":"false"}
---

﻿---
updated:
  - 2026-06-27T15:01:28+07:00
---
  Các thuật toán O-LoRA, N-LoRA, MoLA, SLIM: nền tảng regularization/constraint và triển khai thực tế

## 1. Bối cảnh chung: LoRA, continual learning và regularization

Fine-tuning LLM bằng LoRA cho phép cập nhật mô hình trong một không gian hạng thấp, tiết kiệm tham số và VRAM, nhưng khi học nhiều task/domain **tuần tự** thì vẫn dễ gặp catastrophic forgetting. Điều này thúc đẩy một thế hệ phương pháp mới như O-LoRA, N-LoRA, MoLA, SLIM, sử dụng **regularizer** và **constraint** để kiểm soát cách adapter học và tương tác giữa các nhiệm vụ.[^1][^2][^3][^4][^5]

Trong máy học, regularization là kỹ thuật thêm **hàm phạt** vào loss để kiểm soát độ phức tạp mô hình, giảm overfitting và cải thiện generalization.[^6][^7][^8] Về toán, thay vì tối ưu $L(\theta)$ ta tối ưu $L(\theta) + \lambda R(\theta)$, trong đó $R(\theta)$ đo mức "xấu" của tham số (ví dụ L2 $R(\theta) = \sum_i w_i^2$ hay L1 $R(\theta) = \sum_i |w_i|$).[^8][^9] Các phương pháp LoRA cải tiến ở đây đều dùng các biến thể của $R(\theta)$ hoặc constraint cứng/mềm để ép mô hình tuân thủ thêm điều kiện phụ (orthogonality, non-collision, cấu trúc mixture...), không chỉ giảm loss task.

## 2. Thiết lập toán học chung cho LoRA trong CL

### 2.1 LoRA cơ bản

Với một lớp tuyến tính/MLP có trọng số gốc $W_0 \in \mathbb{R}^{d_\text{out} \times d_\text{in}}$, LoRA thêm một cập nhật hạng thấp $\Delta W = B A$ và giữ $W_0$ cố định:[^1]


$$
y = W_0 x + b + B A x \quad (1)
$$


ở đây $A \in \mathbb{R}^{r \times d_\text{in}}$, $B \in \mathbb{R}^{d_\text{out} \times r}$, $r$ là rank hạng thấp. Nhiều implementation dùng dạng scale:


$$
y = W_0 x + b + \frac{\alpha}{r} B A x \quad (2)
$$


với $\alpha$ là hệ số scale LoRA.[^10][^1]

Trong continual learning, ta có một chuỗi task $\{T_1, T_2, \dots, T_K\}$. Một cách tự nhiên là gán cho mỗi task một adapter LoRA riêng $\Delta W^{(k)}$, và khi train theo thứ tự $T_1 \to T_2 \to \dots$, các adapter hoặc bị share, hoặc bị thêm/ghi đè. Bài toán chính: làm sao **học $T_k$** mà **không phá hỏng** performance trên $T_1, \dots, T_{k-1}$.[^2][^5]

### 2.2 Regularizer và constraint trong ngữ cảnh LoRA

- **Regularizer**: thêm vào loss một term như $R_\text{ortho}$, $R_\text{collision}$, $R_\text{TwD}$, L1 trên adapter..., để ép tham số adapter thoả các tính chất mong muốn (orthogonal, non-collision, decorrelated...).[^3][^4][^5][^2]
- **Constraint được enforced**: thay vì chỉ "mong" subspace mới tự orthogonal hay ít collision, ta thiết kế loss/optimizer sao cho nghiệm tối ưu **phải** gần thoả các điều kiện đó, ví dụ bằng việc thêm penalty lớn nếu inner-product giữa subspace cũ/mới vượt ngưỡng, hoặc nếu collision cao.[^5][^11][^2]

Các thuật toán O-LoRA, N-LoRA, MoLA, SLIM chính là các thiết kế cụ thể của $R(\theta)$ và cấu trúc mixture để kiểm soát continual learning với LoRA.

## 3. O-LoRA: Orthogonal LoRA cho continual learning

### 3.1 Ý tưởng thuật toán

O-LoRA (Orthogonal Low-Rank Adaptation) được đề xuất cho continual learning trong LLM: mỗi task được học trong một **subspace hạng thấp khác nhau** và các subspace này được ép **orthogonal** để giảm interference. Ý tưởng:[^2]

- Cho mỗi task $T_k$, ta gán một adapter LoRA với subspace $\mathcal{U}_k$ (span bởi cột của $B^{(k)}$ hoặc hàng của $A^{(k)}$).
- Khi học task mới $T_{k}$, ta thêm adapter mới và dùng regularizer để ép $\mathcal{U}_k$ gần orthogonal với $\mathcal{U}_1, \dots, \mathcal{U}_{k-1}$. Điều này giảm gradient interference và catastrophic forgetting.[^2]

### 3.2 Hàm mất mát và regularizer orthogonality

Gọi $\theta_k$ là tham số adapter LoRA cho task $T_k$. Loss cơ bản trên batch $B_k$ của task $T_k$ là $L_k(\theta_k; B_k)$. O-LoRA thêm regularizer orthogonality $R_\text{ortho}(\theta_k, \theta_{<k})$:[^2]


$$
\mathcal{L}_k = L_k(\theta_k; B_k) + \lambda_\text{ortho} R_\text{ortho}(\theta_k, \theta_{<k}) \quad (3)
$$


Một dạng điển hình của $R_\text{ortho}$ là phạt inner-product giữa subspace mới và cũ, ví dụ:


$$
R_\text{ortho} = \sum_{j < k} \| U_k^\top U_j \|_F^2 \quad (4)
$$


với $U_k$ là ma trận basis subspace của adapter $k$ (lấy từ $B^{(k)}$ hoặc $A^{(k)}$), $\|\cdot\|_F$ là Frobenius norm.[^2] Nếu $U_k^\top U_j \approx 0$, subspace gần orthogonal, interference giảm.

### 3.3 Quy trình train O-LoRA theo từng task

Một quy trình triển khai điển hình:

1. **Khởi tạo**: freeze base LLM (trọng số $W_0$), chưa có adapter nào.  
2. **Task $T_1$**:  
   - Tạo adapter LoRA $\theta_1$ ($A^{(1)}, B^{(1)}$) với init chuẩn (Gaussian nhỏ, hoặc orthonormal nếu kết hợp OLoRA).[^12][^13]
   - Train $\theta_1$ bằng loss $\mathcal{L}_1 = L_1$ (chưa có regularizer vì chưa có subspace cũ).  
3. **Task $T_k$** ($k > 1$):  
   - Tạo adapter mới $\theta_k$. Giữ frozen toàn bộ $\theta_1, \dots, \theta_{k-1}$.[^2]
   - Với mỗi batch $B_k$ của task $T_k$, compute loss $L_k(\theta_k; B_k)$.  
   - Tính $U_k$ từ $\theta_k$ và $U_j$ từ $\theta_j$ ($j < k$), tính $R_\text{ortho}$ theo (4).  
   - Tối ưu $\theta_k$ w.r.t. $\mathcal{L}_k$ bằng SGD/AdamW.  
4. **Inference**: tuỳ thiết kế, có thể:  
   - Chọn adapter $\theta_k$ theo task ID,  
   - Hoặc dùng một rule cho mixture các adapter (ít phổ biến hơn trong O-LoRA gốc).

Các tác giả cho thấy cách này cho phép LLM học thêm task mới mà vẫn giữ generalization trên task cũ tốt hơn so với các CL baseline, với overhead tham số rất nhỏ.[^2]

### 3.4 Khác biệt so với LoRA thường

- LoRA thường: mỗi downstream task share cùng adapter hoặc mỗi task có adapter riêng nhưng **không có ràng buộc** giữa subspace; gradient của task mới có thể đè knowledge cũ.[^1][^2]
- O-LoRA: thêm ràng buộc orthogonality, enforced bằng regularizer (3)(4), giảm overlap giữa subspace của nhiệm vụ, từ đó giảm forgetting.[^2]

Trong bài toán legal VN, bạn có thể coi mỗi domain pháp luật (Luật chung, Lao động, Thuế, Hình sự...) là một task, gán mỗi domain một LoRA subspace "riêng" và dùng regularizer orthogonality để tránh domain mới phá domain cũ.

## 4. N-LoRA: Non-collision LoRA

### 4.1 Ý tưởng: collision quan trọng hơn orthogonality

Paper COLING 2025 “Is Parameter Collision Hindering Continual Learning in LLMs?” chỉ ra rằng việc chỉ tập trung vào orthogonality như O-LoRA không đủ; **parameter collision** – tham số cho các task khác nhau bị trùng hoặc đè nhau – mới là nguyên nhân cốt lõi của forgetting. N-LoRA (Non-collision LoRA) đề xuất xây dựng các **non-collision parameter subspaces** cho LoRA, giúp preserve knowledge nhiều domain tốt hơn.[^14][^5]

### 4.2 Định nghĩa collision và non-collision

Trực giác:

- **Collision**: cùng một tham số (hoặc chiều latent) trong adapter bị dùng mạnh cho nhiều task khác nhau, khiến update cho task mới phá hỏng tác động đã tốt cho task cũ.[^5][^14]
- **Non-collision**: các task sử dụng tham số adapter khác nhau nhiều hơn (sparse, ít overlap), nên update cho task mới ít ảnh hưởng đến task cũ.[^5]

Paper phân tích rằng non-collision parameter subspaces tạo ra một dạng orthogonality task-level "đủ tốt", và quan trọng hơn orthogonality vector-level thuần tuý.[^5]

### 4.3 Hàm phạt collision và loss N-LoRA

N-LoRA xây dựng một **collision regularizer** $R_\text{collision}$ đo mức chồng lấn tham số giữa LoRA cho các task:[^15][^5]


$$
\mathcal{L}_k = L_k(\theta_k; B_k) + \lambda_\text{col} R_\text{collision}(\theta_1, \dots, \theta_k) \quad (5)
$$


Dạng cụ thể của $R_\text{collision}$ trong paper khá chi tiết, nhưng ý tưởng chung là:

- Xây dựng một metric trên các tham số adapter (ví dụ nuclear norm, overlap giữa support của các vector, hoặc các chỉ số collision được tác giả định nghĩa).  
- Penalize các cấu hình tham số nơi adapter của nhiều task share cùng "hướng mạnh" hoặc cùng vị trí quan trọng, khuyến khích **sparsity và non-overlap**.[^14][^5]

Nhờ đó, gradient step cho task mới ưu tiên sử dụng các tham số "chưa bị chiếm", giữ knowledge cũ trong các vùng non-collision.

### 4.4 Quy trình train N-LoRA

Quy trình tương tự O-LoRA, nhưng regularizer thay từ orthogonality sang collision:

1. Freeze base LLM.  
2. Với mỗi task $T_k$:  
   - Tạo adapter LoRA $\theta_k$.  
   - Trong training, với mỗi batch: tính loss $L_k$ trên task $T_k$.  
   - Tính $R_\text{collision}$ dựa trên các adapter $\theta_1, \dots, \theta_k$.  
   - Tối ưu $\theta_k$ với loss (5).  
3. Inference: chọn adapter theo task/domain hoặc dùng mixture.

Thực nghiệm cho thấy N-LoRA đạt +2.9% accuracy, 4.1× task orthogonality và 58.1× giảm collision rate so với O-LoRA trên nhiều benchmark CL. Điều này củng cố luận điểm: **giảm collision** là yếu tố then chốt cho CL với LoRA.[^16][^5]

### 4.5 So sánh O-LoRA vs N-LoRA

- O-LoRA: tập trung vào orthogonality vector-level giữa subspace các task; giảm interference nhưng chưa tối ưu collision parameter.[^5][^2]
- N-LoRA: coi **non-collision parameters** là quan trọng hơn, xây dựng regularizer trực tiếp trên collision metric; đạt kết quả tốt hơn về forgetting và tổng performance.[^14][^5]

Ở legal VN, nếu bạn muốn đo "quên" đúng như định nghĩa performance trên QA/NLI/syllogism legal giảm khi thêm domain mới, N-LoRA là một cơ chế tự nhiên để thiết kế loss bảo toàn parameter subspace cho domain cũ.

## 5. MoLA: Mixture of Low-rank Adapters

### 5.1 Kiến trúc tổng quát

MoLA (Mixture of Low-rank Adapters) dùng nhiều adapter LoRA song song gắn vào một backbone share, nhằm giải quyết **training heterogeneous data** (multi-task, multi-domain) với một mô hình unified. Khác với LoRA thường (fine-tune adapter trên mô hình pretrain fixed), MoLA **train backbone và adapter end-to-end**, nên vừa là PEFT vừa là kiến trúc MTL cho dữ liệu dị heterogeneous.[^3]

Trên mỗi lớp (ví dụ conv layer), MoLA gắn $m$ adapter hạng thấp song song với weight backbone, với contribution weight $\gamma_i$ cho adapter thứ $i$:[^3]


$$
y = W x + \sum_{i=1}^m \gamma_i B_i A_i x \quad (6)
$$


$W$ là weight backbone, $B_i, A_i$ là yếu tố hạng thấp của adapter thứ $i$, $\gamma_i$ do router hoặc task identifier quyết định.[^3]

### 5.2 Hai biến thể: MoLA-Grad và MoLA-Router

Paper đề xuất hai biến thể:[^3]

- **MoLA-Grad (target-aware)**: có task identifier rõ ràng. Mỗi task được gán một adapter cụ thể, gradients cho task đó chỉ flow qua adapter tương ứng → explicit gradient separation.[^3]
- **MoLA-Router (target-agnostic)**: không dùng task ID ở inference. Một **router network** học mixing weights $\gamma_i$ dựa trên input, với một **Task-wise Decorrelation (TwD) loss** để ép router phân bổ adapter theo task một cách có tổ chức.[^3]

### 5.3 Loss TwD và gradient separation implicit

TwD loss được thiết kế để:[^3]

- Làm **contribution weights** $\gamma$ của các sample cùng task **giống nhau** hơn.  
- Làm $\gamma$ của các sample khác task **khác nhau** hơn.  

Trực giác: TwD phạt khi router phân bổ mixing weights của các task khác nhau quá giống, khiến gradient cho các task bị lẫn lộn. Bằng cách này, MoLA-Router đạt **implicit gradient separation**: mỗi task có combination adapter riêng, dù không explicit gán adapter theo ID.[^3]

Total loss trong MoLA gồm:


$$
\mathcal{L}_\text{MoLA} = \sum_k L_k + \lambda_\text{TwD} R_\text{TwD} \quad (7)
$$


với $L_k$ là loss của từng task, $R_\text{TwD}$ là TwD loss (chỉ bật cho MoLA-Router).[^3]

### 5.4 Quy trình triển khai MoLA

Bước triển khai điển hình:[^3]

1. **Chọn backbone**: ResNet, Transformer, hoặc LLM.  
2. **Chèn MoLA module** vào một số layer (không cần mọi layer).  
3. **Chọn mode**:  
   - MoLA-Grad: dùng task ID, gán mỗi task một adapter, explicit gradient separation.  
   - MoLA-Router: train một router chung cho tất cả MoLA layer, kết hợp với TwD loss.  
4. **Train end-to-end**: backbone + adapter + router.  
   - Optimizer: paper dùng AdamW, SGD, Adam tuỳ dataset.  
   - Batch size 128 trong thực nghiệm trên vision.[^3]
5. **Inference**:  
   - MoLA-Grad: dùng task ID để chọn adapter.  
   - MoLA-Router: router tự sinh mixing weights $\gamma$ cho mỗi input.

### 5.5 Liên hệ với LoRA

- LoRA: PEFT trên mô hình pretrain fixed; adapter chủ yếu cho một downstream task.[^1][^3]
- MoLA: multiple LoRA experts + router, train cùng backbone; thiết kế cho multi-task/multi-domain heterogeneous training, với gradient separation (explicit hoặc implicit) để giảm conflict.[^3]

Trong legal VN, MoLA cho phép bạn gắn nhiều LoRA adapter legal khác nhau (NLI luật, QA án lệ, syllogism, counterfactual QA...) trên cùng backbone, và dùng router để chọn/mix adapter theo input hoặc meta-data, thay vì merge mọi knowledge vào một adapter duy nhất.

## 6. SLIM: Soft LoRA + Identity Mixture

### 6.1 Mục tiêu: learn more, forget less

SLIM (“Let LLM Learn More and Forget Less with Soft LoRA and Identity Mixture”) là một MoE PEFT framework mới, nhằm đồng thời **tăng khả năng học downstream** và **giảm catastrophic forgetting** của general capability. Nó kết hợp:[^17][^4]

- LoRA adapters như các expert học downstream.  
- Các **identity layers** làm expert "đường cao tốc" để bypass LoRA khi cần giữ general capability.  
- Một router + cơ chế **weight yielding với sliding clustering** để quyết định routing dựa trên phân bố input.  
- Một cơ chế **dynamic merging** để convert mixing LoRA thành model merging formulation, giúp forgetting mitigation không cần data replay.[^18][^4]

### 6.2 Kiến trúc MoE với LoRA + identity

Ở mỗi layer được chọn, SLIM có tập expert $\{f_i\}$ gồm:[^4]

- Một số LoRA adapters $f_i(x) = B_i A_i x$.  
- Một số identity layers $f_j(x) = 0$ (ở level weight; output $g(x) + F(x) = g(x)$).[^4]

Router $R(x)$ sinh ra routing logits, sau đó weight yielding biến chúng thành routing weight $\hat{w}_i$. Đầu ra layer là:


$$
y = W x + b + \sum_i \hat{w}_i f_i(x) \quad (8)
$$


Nếu $\hat{w}_i$ lớn cho LoRA, layer nghiêng về tri thức downstream; nếu $\hat{w}_j$ lớn cho identity, layer giữ hành vi base model, giảm forgetting.[^4]

### 6.3 Weight yielding với sliding clustering

SLIM giả thiết rằng phân bố input downstream là mixture of Gaussians; dùng **sliding clustering** để ước lượng cluster center và variance cho downstream distribution. Quy trình:[^4]

1. Khởi tạo tập cluster center $C = \{c_i\}$.  
2. Với mỗi input $x$, tính khoảng cách đến mỗi cluster và gán vào cluster gần nhất.  
3. Cập nhật center và variance theo batch (trong training).  
4. Tại inference, distance $d$ từ $x$ đến cluster quyết định việc **yielding** routing weight:  
   - Nếu $d$ nhỏ (input giống downstream), tăng trọng số cho LoRA experts.  
   - Nếu $d$ lớn (input out-of-domain), tăng trọng số cho identity layers.[^4]

Bằng cách này, router được "chỉnh" để **đưa downstream-like samples vào LoRA**, còn **out-of-domain/generic samples vào identity**, giữ general capability của LLM tốt hơn.[^4]

### 6.4 Dynamic merging LoRA thành model merging

Tác giả dựa trên DARE (một framework model merging) để chuyển mixing LoRA thành dynamic merging giữa base model và các LoRA adapters:[^4]

- LoRA được coi như "task vector" $\tau$.  
- Merging weights được mask một phần (theo random mask) để tránh override toàn bộ base.  
- Dynamic merging sử dụng mask trên ma trận hạng thấp (ở level hàng/cột của $B, A$) như một xấp xỉ của random sampling, giúp thực thi nhanh.[^4]

Fast implementation: thay vì tính $B A$ full rồi Hadamard mask, SLIM mask trực tiếp hàng/cột của $B, A$ để xấp xỉ, giảm chi phí tính toán mà không giảm nhiều performance.[^4]

### 6.5 Quy trình train SLIM

Theo paper:[^4]

1. **Base model**: dùng OpenChat-8B (mở rộng Llama3-8B-Instruct).  
2. **Chèn SLIM experts** (LoRA + identity) vào một số layer.  
3. **Khởi tạo cluster** cho sliding clustering; router là vài residual block + gate.  
4. **Train PEFT**:  
   - Setting SDS (single downstream dataset) hoặc MDS (multi-dataset).  
   - Rank LoRA cho LoRA/DoRA set high (128) để fair; LoRAMoE, MixLoRA, MoLA rank=16, Nexperts=8; SLIM Nexperts=10 với K=2 identity layers.[^4]
   - 2 epoch, batch size 16 trên A100 80GB.  
5. **Loss**: task loss trên downstream + các term cho clustering và merging (không có data replay).[^4]

Thực nghiệm cho thấy SLIM đạt chất lượng downstream so sánh được với SOTA PEFT, đồng thời giữ general tasks (MMLU, GSM8K, PIQA) tốt hơn, giảm catastrophic forgetting đáng kể so với LoRA, DoRA, MixLoRA, LoRAMoE, MoLA.[^17][^4]

### 6.6 Liên hệ với regularizer/constraint

SLIM không viết loss dưới dạng một $R_\text{ortho}$ đơn giản, nhưng bản chất vẫn là **constraint/regularization**:

- Clustering + weight yielding chính là constraint mềm: xử lý các điểm out-of-domain bằng identity expert.  
- Dynamic merging với mask là một dạng regularization trên task vectors (LoRA), giới hạn mức độ chỉnh sửa base model.  

Các cơ chế này được **enforced** qua thiết kế loss và router, khiến mô hình phải học theo chính sách “learn more, forget less” chứ không đơn thuần minimize loss downstream.[^4]

## 7. Đặt các thuật toán cạnh nhau và gợi ý triển khai cho legal VN

### 7.1 Bảng so sánh ngắn

| Thuật toán | Ý tưởng chính | Loại regularizer/constraint | Mục tiêu chính |
|-----------|---------------|-----------------------------|----------------|
| O-LoRA | Mỗi task có LoRA subspace hạng thấp orthogonal | Phạt $U_k^\top U_j$ để subspace gần orthogonal | Giảm forgetting bằng orthogonal subspace.[^2] |
| N-LoRA | Giảm parameter collision giữa task, coi non-collision quan trọng hơn orthogonality | Regularizer collision (giảm overlap tham số) | Giữ knowledge nhiều domain tốt hơn, outperform O-LoRA.[^5][^14] |
| MoLA | Mixture of LoRA adapters + shared backbone, train end-to-end | TwD loss decorrelate mixing weights theo task | Giảm conflict trong heterogeneous data training, multi-task/multi-domain.[^3] |
| SLIM | MoE với LoRA + identity, weight yielding + dynamic merging | Clustering + routing constraint + mask merging | PEFT cho LLM vừa học downstream tốt vừa giảm forgetting general tasks.[^4] |

### 7.2 Gợi ý triển khai “Continual Legal LoRA”

Cho bài toán legal VN (QA/NLI/syllogism) với định nghĩa "quên" = performance trên domain A giảm sau khi học domain B, có thể dựng pipeline nghiên cứu như sau:

- **Baseline LoRA thường**: student legal SLM với một adapter duy nhất cho tất cả domain, đo forgetting rõ ràng.  
- **O-LoRA / N-LoRA-style**:  
  - Mỗi domain pháp luật (Luật chung, Lao động, Thuế...) là một task, mỗi task có LoRA adapter riêng.  
  - Thêm regularizer orthogonality (O-LoRA) hoặc collision (N-LoRA) vào loss khi train từng domain mới.  
  - Đo đường cong accuracy trên QA/NLI/syllogism của domain cũ khi thêm domain mới.  
- **MoLA-style**:  
  - Gắn nhiều LoRA adapter legal vào backbone; dùng MoLA-Grad nếu bạn biết domain của câu hỏi (metadata) hoặc MoLA-Router nếu muốn target-agnostic.  
  - Sử dụng TwD loss để router học phân bổ adapter theo domain/task legal một cách rõ ràng.  
- **SLIM-style**:  
  - Thêm identity expert bên cạnh các LoRA legal adapter trong vài layer trọng yếu.  
  - Dùng clustering trên embedding của câu hỏi để phân biệt câu hỏi "generic world" vs "chuyên legal", route tương ứng LoRA vs identity.  
  - Dùng dynamic merging để tránh việc LoRA override hoàn toàn base model.

Các thuật toán này đều dựa trên cùng nền tảng **regularizer/constraint để enforced behaviour mong muốn**: orthogonality, non-collision, gradient separation, soft routing và mixture với identity. Điều này cho phép bạn thiết kế một khung nghiên cứu chính thống cho "Legal Continual LoRA" với giải thích rõ ràng ở mức loss function và mechanics, phù hợp để viết phần phương pháp trong luận văn hoặc paper.

---

## References

1. [What are the LoRA hyperparameters (r, alpha) - Avichala](https://www.avichala.com/blog/what-are-the-lora-hyperparameters-r-alpha) - Explore What are the LoRA hyperparameters (r, alpha) with Avichala — a global Generative AI learning...

2. [Orthogonal Subspace Learning for Language Model Continual ...](https://openreview.net/forum?id=L7ZBpZZ8Va) - In this paper, we propose orthogonal low-rank adaptation (O-LoRA), a simple and efficient approach f...

3. [Exploring Training on Heterogeneous Data with Mixture of Low-rank Adapters](https://arxiv.org/html/2406.09679)

4. [[PDF] Let LLM Learn More and Forget Less with Soft LoRA and Identity ...](https://aclanthology.org/2025.naacl-long.246.pdf) - In this work, we propose a novel MoE architec- ture with Soft LoRA and Identity Mixture (SLIM), ... ...

5. [Is Parameter Collision Hindering Continual Learning in LLMs?](https://aclanthology.org/2025.coling-main.286/) - Shuo Yang, Kun-Peng Ning, Yu-Yang Liu, Jia-Yu Yao, Yong-Hong Tian, Yi-Bing Song, Li Yuan. Proceeding...

6. [What Is Regularization? | IBM](https://www.ibm.com/think/topics/regularization) - Regularization is a set of methods that correct for multicollinearity and overfitting in predictive ...

7. [What is Regularization in Machine Learning?](https://www.ultralytics.com/glossary/regularization) - Explore how regularization prevents overfitting in machine learning. Learn to implement dropout and ...

8. [Regularization](https://www.sciencedirect.com/topics/computer-science/regularization) - Regularization is a fundamental technique in computer science, particularly within machine learning ...

9. [Overfitting: L2 regularization | Machine Learning](https://developers.google.com/machine-learning/crash-course/overfitting/regularization) - Learn how the L2 regularization metric is calculated and how to set a regularization rate to minimiz...

10. [LoRA Scaling Parameter Alpha - ApX Machine Learning](https://apxml.com/courses/lora-peft-efficient-llm-training/chapter-2-lora-in-depth/lora-scaling-alpha) - The scaling parameter, denoted as α \alpha α, is an important hyperparameter in Low-Rank Adaptation ...

11. [Integration between constrained optimization and deep networks](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2024.1414707/full) - Integration between constrained optimization and deep networks has become of significant interest to...

12. [Orthonormal Low-Rank Adaptation of Large Language Models - arXiv](https://arxiv.org/abs/2406.01775) - In this paper, we present OLoRA, an enhancement to the LoRA method that leverages orthonormal matrix...

13. [[Literature Review] OLoRA: Orthonormal Low-Rank Adaptation of ...](https://www.themoonlight.io/en/review/olora-orthonormal-low-rank-adaptation-of-large-language-models) - This paper introduces OLoRA (Orthonormal Low-Rank Adaptation), a new method for efficiently fine-tun...

14. [Non-Collision Low-Rank Adaptation (N-LoRA) for Continual Learning in Large Language Models](https://linnk.ai/sv/insight/machine-learning/non-collision-low-rank-adaptation-n-lora-for-continual-learning-in-large-language-models-rKOxlggt/) - Preventing parameter collisions, rather than solely focusing on orthogonality, is crucial for effect...

15. [PKU-YuanGroup/N-LoRA: 【COLING 2025  】Code for the ... - GitHub](https://github.com/PKU-YuanGroup/N-LoRA) - 【COLING 2025】Is Parameter Collision Hindering Continual Learning in LLMs? Code for the N-LoRA method...

16. [#coling2025 | yuyang liu - LinkedIn](https://www.linkedin.com/posts/yuyang-liu-03a587279_coling2025-activity-7273243255410954240-BHSI) - 🎉 Excited to share that our paper, “Is Parameter Collision Hindering Continual Learning in LLMs?”, h...

17. [SLIM: Let LLM Learn More and Forget Less with Soft LoRA ... - arXiv](https://arxiv.org/html/2410.07739v1)

18. [SLIM: Let LLM Learn More and Forget Less with](https://openreview.net/pdf/7f738e57b6f42a650ecf56d77ef3f0a7386e6805.pdf)

