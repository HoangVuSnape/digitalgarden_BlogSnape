---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/nlp/lora-qlora-paramester/","pinned":"false"}
---

﻿# Phân tích sâu các hyperparameter trong LoRA: rank r, alpha và các tham số liên quan

## Overview

LoRA (Low-Rank Adaptation) là một kỹ thuật fine-tuning tham số-hiệu quả, trong đó ma trận trọng số gốc $W$ được giữ cố định và ta chỉ học một cập nhật hạng thấp $\Delta W$. Cập nhật này được mô hình hóa dưới dạng tích của hai ma trận hạng thấp $A$ và $B$ sao cho $\Delta W = B A$, và đầu ra lớp trở thành $W' = W + \Delta W$. Các hyperparameter chính của LoRA – rank $r$, alpha $\alpha$, cùng với dropout, target modules, learning rate, epochs… – quyết định trực tiếp dung lượng thích nghi, độ ổn định, khả năng tổng quát và mức độ "quên" của mô hình sau khi fine-tuning.[^1][^2]

Báo cáo này đi từ "what, how, when" cho từng nhóm tham số: định nghĩa toán học, vai trò, trực giác, khuyến nghị thực nghiệm và cách lựa chọn khi bạn làm legal Vietnamese LLM với LoRA/QLoRA.

## LoRA core: công thức và trực giác

### Công thức chuẩn của LoRA

Trong một lớp tuyến tính với trọng số gốc $W_0$, LoRA thêm một cập nhật hạng thấp $\Delta W$ và giữ $W_0$ cố định.[^2][^1]

$$
W' = W_0 + \Delta W = W_0 + B A \quad (1)
$$

Ở nhiều implementation (PEFT, Unsloth, rsLoRA…), cập nhật này được scale bởi $\alpha$ và rank $r$:[^3][^2]

$$
\hat{W} = W_0 + \frac{\alpha}{r} B A \quad (2)
$$

Với Rank-Stabilized LoRA (rsLoRA), người ta đề xuất:[^2]

$$
\hat{W}_{\text{rslora}} = W_0 + \frac{\alpha}{\sqrt{r}} B A \quad (3)
$$

Trong các công thức trên:

- $r$ là rank của ma trận hạng thấp, tức số chiều latent mà adapter dùng để mã hóa cập nhật.[^1][^2]
- $\alpha$ là hệ số scale, điều khiển cường độ tác động của cập nhật LoRA lên $W_0$.[^3]
- Tỉ lệ $\alpha / r$ (hoặc $\alpha / \sqrt{r}$) là "effective scale" thực sự nhân lên cập nhật $B A$.[^2][^3]

### Trực giác về dung lượng và regularization

- Rank thấp $r$ làm cho $\Delta W$ bị ràng buộc ở một không gian hạng thấp, đóng vai trò như một regularizer cấu trúc: mô hình chỉ có thể thay đổi hành vi theo một số chiều hạn chế.[^1][^2]
- $\alpha$ điều chỉnh "âm lượng" của các thay đổi đó: $\alpha$ lớn khiến LoRA có thể đẩy đầu ra ra xa khỏi hành vi gốc; $\alpha$ nhỏ giữ mô hình gần base hơn.[^3]
- Việc chia cho $r$ hoặc $\sqrt{r}$ giúp tách cường độ cập nhật khỏi bản thân size của rank, tránh việc tăng rank vô tình thổi phồng độ lớn của $\Delta W$.[^2]

Trong bối cảnh legal LLM, bạn muốn rank đủ lớn để encode các pattern pháp lý phức tạp (QA, NLI, syllogism), nhưng vẫn giữ $\alpha / r$ ở mức vừa phải để tránh phá vỡ tính tổng quát và gây catastrophic forgetting trên domain gốc.[^1][^2]

## Rank r: "What" – định nghĩa, ý nghĩa và trade-off

### Định nghĩa toán học của rank

Rank $r$ trong LoRA là số chiều latent của cập nhật hạng thấp $\Delta W$. Trong thực tế, nếu lớp có kích thước $d_{\text{in}} \times d_{\text{out}}$, người ta chọn:[^1][^2]

- $A \in \mathbb{R}^{r \times d_{\text{in}}}$
- $B \in \mathbb{R}^{d_{\text{out}} \times r}$

Tổng số tham số trainable trong adapter là $r (d_{\text{in}} + d_{\text{out}})$, thay vì $d_{\text{in}} d_{\text{out}}$ nếu full fine-tuning.[^1]

Ví dụ, với lớp 768×768, full FT có 589,824 tham số; nếu $r = 32$, adapter chỉ có khoảng 49,152 tham số, tiết kiệm hơn 90%.[^1]

### Ảnh hưởng của r lên dung lượng và VRAM

- **Dung lượng biểu diễn**: r càng lớn, adapter có thể biểu diễn hàm cập nhật phức tạp hơn, gần với full-FT khi $r$ tiến dần tới chiều ẩn của tầng.[^4][^1]
- **Overfitting**: r quá lớn trên dataset nhỏ (ví dụ vài chục nghìn câu hỏi pháp lý) dễ dẫn đến overfitting vì adapter có quá nhiều freedom.[^2]
- **VRAM và tốc độ**: r lớn làm tăng số tham số trainable và FLOPs, nên tốn VRAM và chậm hơn; r nhỏ cho phép train nhanh trên GPU hạn chế, đặc biệt khi kết hợp QLoRA 4-bit.[^2][^1]

Nghiên cứu tổng hợp cho thấy với LoRA trên LLM, các range r phổ biến là 8–128; r=8 hoặc r=16 thường đủ cho task đơn giản, còn r=32–64 cho domain phức tạp; một số case dùng r≥128 để tiệm cận full-FT.[^2][^1]

### Khuyến nghị thực nghiệm về r

Các hướng dẫn thực tế (Hugging Face, Unsloth…) khuyến cáo:[^1][^2]

- Bắt đầu với r = 16 hoặc 32 cho fine-tuning chỉ trên attention+MLP.
- Với SLM (0.6B–4B) và task legal VN trung bình, r=16 là cấu hình "an toàn"; nếu thấy underfitting, tăng lên r=32.
- Với mô hình lớn hơn (7B–15B) và benchmark đa dạng, có thể thử r=64, nhưng cần kiểm soát overfitting bằng dropout, weight decay và early stopping.[^2]

Các phân tích gần đây còn chỉ ra rằng optimum learning rate cho LoRA adapter có thể scale theo $r^{-0.84}$, gợi ý rằng khi tăng rank, nên giảm learning rate thấy bởi adapter, hoặc chỉnh $\alpha / r$ tương ứng.[^4]

## Alpha α: "What" – vai trò, công thức và trực giác

### Alpha trong forward pass

Trong implementation phổ biến (PEFT, Unsloth), alpha xuất hiện như một hệ số scale cho $B A$:[^3][^2]

$$
h = W_0 x + \frac{\alpha}{r} (B A x) \quad (4)
$$

Ở đây:

- $W_0 x$ là đầu ra từ trọng số gốc, mang tri thức pretrain/instruct.[^3]
- $B A x$ là phần điều chỉnh hạng thấp do LoRA học.[^1]
- $\alpha / r$ điều chỉnh mức đóng góp tương đối của $B A x$ so với $W_0 x$.[^3]

### Trực giác về $\alpha$ như "cần gạt cường độ"

Theo phân tích chi tiết:[^3]

- **$\alpha$ lớn**: tăng cường độ của cập nhật LoRA, giúp mô hình thích nghi nhanh, nhưng dễ đẩy hành vi ra xa khỏi pretrain, tăng rủi ro overfitting, mất tính tổng quát hoặc gây "quên" mạnh trên domain gốc.
- **$\alpha$ nhỏ**: giảm tác động, giữ mô hình gần base, có lợi cho generalization và giảm forgetting, nhưng có thể khiến fine-tune quá "nhẹ" và underfit trên task mới.

Việc chia cho $r$ thường nhằm bình thường hóa variance của $B A$ theo rank: nếu $A$ khởi tạo Gaussian và $B$ khởi tạo 0, variance của $B A$ có thể scale theo $r$; chia cho $r$ giúp $\alpha$ giữ vai trò thuần túy kiểm soát cường độ, thay vì bị lẫn với kích thước rank.[^3]

### Các cách đặt $\alpha$ phổ biến

Thực hành có một số convention:[^5][^2][^3][^1]

1. **$\alpha = r$**: heuristic đơn giản; nếu dùng $\alpha / r$, khi $\alpha = r$ thì $\alpha / r = 1$, tức cập nhật không bị scale thêm, $\hat{W} = W_0 + B A$.
2. **$\alpha = 2 r$**: Microsoft trong LoRA gốc dùng $\alpha = 2 r$, làm $\alpha / r = 2$, tức nhân đôi ảnh hưởng của adapter.[^5]
3. **$\alpha$ cố định**: chọn $\alpha$ = 16, 32, 64… không phụ thuộc r, dùng $\alpha / r$ như thước đo cường độ, để tune $\alpha$ giống một hyperparameter độc lập với rank.[^3]
4. **Tuning $\alpha$ như hyperparameter độc lập**: grid search, random search, Bayesian optimization để tìm $\alpha$ tối ưu cho từng task, kết hợp với tuning learning rate và r.[^3]

Unsloth và nhiều tài liệu hyperparameter guide đề xuất thực dụng: $\alpha = r$ hoặc $\alpha = 2r$, đảm bảo $\alpha / r \geq 1$. Điều này giúp adapter có đủ sức ảnh hưởng mà không quá mạnh nếu r chọn vừa phải.[^2]

## Quan hệ giữa α và r: "How" – scale, rsLoRA và learning rate

### Tỉ lệ $\alpha / r$ và rsLoRA

Công thức chuẩn của LoRA trong nhiều tài liệu được viết dưới dạng:[^2]

$$
\hat{W} = W_0 + \frac{\alpha}{r} A B \quad (5)
$$

rsLoRA đề xuất thay $r$ bằng $\sqrt{r}$:[^2]

$$
\hat{W}_{\text{rslora}} = W_0 + \frac{\alpha}{\sqrt{r}} A B \quad (6)
$$

Phân tích thực nghiệm cho thấy nếu giữ $\alpha / r \geq 1$, LoRA có thể đạt chất lượng gần full-FT khi r đủ lớn và target modules đủ rộng (attention + MLP).[^1][^2]

Unsloth khuyến nghị:[^2]

- Đặt $\alpha = r$ hoặc $\alpha = 2r$ để $\alpha / r = 1$ hoặc $2$.
- Khi bật rsLoRA (`use_rslora=True`), $\alpha / \sqrt{r}$ là scale mới; việc này ổn định hơn cho rank lớn vì giảm tốc độ tăng của effective scale theo r.[^2]

### Liên hệ với learning rate của adapter

Phân tích trên blog Trelis (dựa trên sweep của Thinking Machines) cho thấy:[^4]

- Learning rate tối ưu mà adapter "thấy" (tức LR_lora = LR * $\alpha / r$) có quan hệ gần $r^{-0.84}$.
- Điều này nằm giữa hai quy ước: LoRA truyền thống $r^{-1}$ và rescaled-LoRA $r^{-0.5}$.[^4]

Nếu thư viện dùng $\alpha / r$, việc lựa chọn $\alpha$ ảnh hưởng trực tiếp đến LR_lora:[^4]

$$
\text{LR}_{\text{lora}} = \text{LR} \cdot \frac{\alpha}{r} \quad (7)
$$

Mối quan hệ $r^{-0.84}$ gợi ý rằng khi tăng rank, nên giảm $\alpha / r$ hoặc learning rate để giữ $\text{LR}_{\text{lora}}$ trong vùng tối ưu. Tuy vậy, đa số implementation thực dụng vẫn dùng một LR chung (ví dụ 2e-4) và chọn $\alpha = r$ hoặc $2r$ cho r trong khoảng vừa phải (8–64).[^4][^2]

## LoRA Dropout: "What" – cơ chế và khi nào dùng

### Định nghĩa

LoRA dropout là một cơ chế regularization: trong quá trình training, một phần các kích hoạt liên quan đến adapter được đặt thành 0, tương tự như dropout trong mạng neuron thường.[^1][^2]

Trong nhiều toolkit, tham số này là `lora_dropout`, với domain khoảng 0–0.1.[^2]

### Vai trò và thực nghiệm gần đây

- Dropout giúp giảm overfitting bằng cách tránh việc adapter phụ thuộc quá mạnh vào một vài chiều latent.[^3]
- Tuy nhiên, nghiên cứu gần đây với các training run ngắn (1–3 epoch) cho thấy `lora_dropout` có thể là regularizer không ổn định, đôi khi không mang lại lợi ích rõ ràng.[^2]
- Unsloth tối ưu hóa code cho `lora_dropout=0`, coi đây là mặc định nhanh và ổn; chỉ khuyên dùng giá trị ~0.1 nếu bạn thấy dấu hiệu overfitting rõ ràng.[^2]

Trong bối cảnh legal LLM với dataset vừa phải (vài chục nghìn–trăm nghìn mẫu) và SLM nhỏ, một cấu hình hợp lý là bắt đầu với `lora_dropout=0` để giữ training đơn giản, sau đó nếu training loss xuống rất thấp (<0.2) nhưng validation/bên ngoài không cải thiện, có thể thử `lora_dropout=0.05` hoặc 0.1.[^2]

## Target modules: "What" – áp LoRA lên đâu và tác động

### Các module thường gặp

Các hướng dẫn hyperparameter hiện đại khuyến nghị áp LoRA lên cả attention lẫn MLP:[^1][^2]

- Attention: `q_proj, k_proj, v_proj, o_proj`
- MLP/FFN: `gate_proj, up_proj, down_proj`

LoRA gốc và một số blog cũ chỉ áp lên `q_proj` và `v_proj`, nhưng kết quả tổng hợp (QLoRA paper, Unsloth guide) cho thấy hiệu quả tốt nhất khi target toàn bộ các linear chính.[^1][^2]

### Tác động lên chất lượng và VRAM

- Áp LoRA lên nhiều module tăng số tham số adapter và FLOPs, nhưng lợi ích chất lượng thường lớn hơn chi phí VRAM bổ sung, đặc biệt với QLoRA.[^1][^2]
- Kịch bản chỉ áp LoRA lên attention hoặc chỉ lên MLP dẫn đến chất lượng thấp hơn đáng kể (ví dụ RougeL giảm) so với áp lên cả hai.[^2]

Trong bài toán legal VN, việc cho LoRA "đi" cả attention và MLP giúp mô hình vừa học pattern ngữ nghĩa (attention) vừa học mapping phi tuyến (MLP) liên quan đến cấu trúc lập luận, NLI và syllogism.[^1][^2]

## Learning rate, epochs, batch size: "How" – các tham số chung nhưng tương tác với LoRA

### Learning rate

Unsloth tổng hợp hàng trăm kết quả và khuyến nghị:[^2]

- LoRA/QLoRA supervised (SFT, NLI, QA): LR khởi điểm 2e-4.
- RL-style (DPO, GRPO): LR thấp hơn, khoảng 5e-6.

LR quá cao có thể gây training không ổn định, đặc biệt nếu $\alpha / r$ lớn; LR quá thấp trên run ngắn lại có thể dẫn đến overfitting hoặc không học đủ.[^4][^2]

### Epochs

- 1–3 epoch là recommended cho instruction-based dataset; nhiều hơn thường không mang lại cải thiện tương xứng và dễ overfit.[^2]
- Với legal VN, nếu dataset không quá lớn, lựa chọn 2 epoch là điểm cân bằng, kết hợp early stopping nếu evaluation loss tăng.[^2]

### Effective batch size và gradient accumulation

Unsloth nhấn mạnh effective batch size = batch_size × gradient_accumulation_steps, với giá trị 4–16 là vùng an toàn cho đa số task.[^2]

- Batch size lớn dùng nhiều VRAM, nên trên GPU hạn chế thường chọn batch_size nhỏ (1–2) và tăng gradient_accumulation_steps để giữ effective batch size đủ lớn.[^2]

Điều này rất quan trọng với QLoRA trên SLM 0.6B–4B khi bạn chạy trên GPU 24GB; LoRA giúp giảm tham số trainable, QLoRA giảm footprint model, còn gradient accumulation xử lý constraint VRAM.[^2]

## QLoRA: "When" – khi nào dùng LoRA vs QLoRA

### QLoRA cơ bản

QLoRA (Quantized LoRA) lưu model base ở 4-bit (NF4, double quantization) và chỉ train adapter ở precision cao hơn (thường bfloat16), tiết kiệm ~33% VRAM so với LoRA 16-bit trong nhiều setup.[^1]

Kết quả cho thấy:[^1][^2]

- QLoRA giảm VRAM rất mạnh, cho phép fine-tune LLaMA 70B trong <48GB VRAM.
- Training chậm hơn LoRA ~39% vì overhead quantize/dequantize.
- Chất lượng mô hình gần như không bị ảnh hưởng, đặc biệt khi target modules đầy đủ.

### Quyết định LoRA vs QLoRA

- Nếu VRAM là bottleneck (24GB, 16GB) và bạn muốn train mô hình 7B–15B hoặc nhiều adapter song song, QLoRA là lựa chọn tự nhiên.[^2]
- Nếu bạn train trên SLM nhỏ (0.6B–4B) và VRAM đủ thoải mái, LoRA 16-bit có thể nhanh hơn và đơn giản hơn về tooling.[^2]

Trong pipeline legal VN với VRAM hạn chế, một chiến lược hợp lý là dùng QLoRA cho mô hình ≥4B, và LoRA thường cho mô hình ≤1.7B, giữ cùng công thức hyperparameter (r, $\alpha$, target_modules) nhưng chú ý LR và batch size.[^2]

## Overfitting, underfitting và control bằng α, r, dropout

### Overfitting: dấu hiệu và cách xử lý

Unsloth đề xuất: nếu training loss xuống dưới 0.2, rất có thể mô hình đang overfit, nghĩa là performance trên dữ liệu chưa thấy sẽ kém.[^2]

Các biện pháp:[^3][^2]

- Giảm learning rate.
- Giảm số epoch (chỉ 1–2 vòng qua data).
- Tăng weight decay (0.01–0.1).
- Tăng `lora_dropout` (0.05–0.1).
- Tăng effective batch size.
- Mở rộng dataset, hoặc trộn thêm data open-source chất lượng.
- Early stopping dựa trên validation loss.
- **LoRA alpha scaling**: sau training, giảm $\alpha$ bằng cách nhân 0.5, tương đương việc merge base + adapter rồi chia trọng số kết quả cho 2.[^2]

Alpha scaling là một vũ khí mạnh: nó làm giảm tác động của adapter tại inference, giúp kết quả bớt "gắt" trên task fine-tune nhưng giữ được phần tri thức mới; trong pháp lý, đây là cách giảm rủi ro output quá "overfit" theo dataset huấn luyện.[^2]

### Underfitting: dấu hiệu và cách xử lý

Underfitting xảy ra khi mô hình không học đủ pattern từ data; loss vẫn cao, và chất lượng trên task mới thấp.[^2]

Các biện pháp:[^2]

- Tăng learning rate (trên các run ngắn).
- Tăng số epoch, nhưng vẫn giám sát validation.
- Tăng rank $r$ và $\alpha$: với SLM và dataset phức tạp, $r$ nên ≥$\alpha$ và trong khoảng 4–64.[^2]
- Giảm batch size xuống 1 để cập nhật mô hình "mạnh" hơn trên mỗi sample.
- Sử dụng dataset sát với domain và nhiệm vụ hơn.

Điểm cân bằng giữa overfitting và underfitting phụ thuộc chặt chẽ vào cặp $r, \alpha$; do đó, bạn nên coi chúng như knobs chính để chỉnh dung lượng và cường độ thích nghi, kết hợp với LR và số epoch.[^3][^2]

## Bảng tóm tắt hyperparameter chính của LoRA

| Nhóm tham số | Ý nghĩa chính | Giá trị khuyến nghị (thực dụng) |
|-------------|---------------|----------------------------------|
| Rank r | Dung lượng adapter, kiểm soát số tham số trainable trong cập nhật hạng thấp | 8–32 cho task đơn giản; 16–32 là khởi điểm tốt cho legal VN trên SLM; 64+ cho domain rất phức tạp và dataset lớn.[^1][^2] |
| Alpha $\alpha$ | Cường độ tác động của LoRA lên base, tham gia dưới dạng $\alpha / r$ hoặc $\alpha / \sqrt{r}$ | $\alpha = r$ hoặc $2r$; giữ $\alpha / r \geq 1$; tune thêm nếu thấy over/underfitting.[^2][^5][^3] |
| LoRA dropout | Regularization trên adapter, giảm overfitting | 0–0.1; mặc định 0 để tận dụng tối ưu hóa, tăng lên 0.05–0.1 nếu overfit rõ.[^2][^3] |
| Target modules | Vị trí áp LoRA (attention, MLP) | `q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj` để đạt chất lượng gần full-FT.[^2][^1] |
| Learning rate | Mức điều chỉnh trọng số adapter mỗi bước | 2e-4 cho LoRA/QLoRA supervised; 5e-6 cho DPO/GRPO.[^2][^4] |
| Epochs | Số vòng qua dataset | 1–3; đa số case legal VN nên dừng ở 2 với early stopping.[^2] |
| Effective batch size | Độ ổn định gradient, ảnh hưởng đến VRAM và tốc độ | 4–16, dùng batch_size nhỏ (1–2) + gradient_accumulation_steps để tránh OOM.[^2] |
| LoRA vs QLoRA | Precision và footprint VRAM | QLoRA cho mô hình ≥4B trên GPU hạn chế; LoRA 16-bit cho SLM nhỏ nếu VRAM đủ.[^1][^2] |

## "When" – một số kịch bản lựa chọn cụ thể cho legal Vietnamese LLM

### Kịch bản 1: SLM 0.6B–1.7B, VRAM ~24GB, NLI/QA/syllogism legal

- Chọn LoRA 16-bit (không cần QLoRA) để giữ training đơn giản.[^2]
- `r = 16`, `lora_alpha = 32` ($\alpha = 2r$), `lora_dropout = 0.05`, `target_modules` = attention + MLP.[^6][^2]
- LR = 2e-4, epochs = 2, effective batch size ~16 bằng cách batch_size = 2, grad_accum = 8.[^2]

Đây là cấu hình "an toàn" đã được khuyến nghị cho NLI trên Unsloth: đủ dung lượng để encode pattern legal mà vẫn kiểm soát overfitting, phù hợp với pipeline distillation legal VN bạn đang xây.[^6][^2]



- Teacher 4B: QLoRA với `r = 32`, `alpha = 64`, target full modules, LR ~2e-4, epochs 1–2; dùng chủ yếu để sinh supervision (logits, answers), không cần tối ưu tuyệt đối.[^1][^2]
- Student 0.6B: LoRA 16-bit với `r = 16`, `alpha = 32`, tập trung vào matching distribution của teacher trên các task NLI/QA/syllogism.[^2]

Việc chọn r nhỏ hơn cho student giúp quá trình distillation thực sự "nén" tri thức từ teacher vào một không gian hạng thấp, có lợi cho generalization và tránh overfitting trên corpus pháp lý cụ thể.[^1]

### Kịch bản 3: Mitigation catastrophic forgetting khi thêm domain mới

Khi bạn thêm domain mới (ví dụ Thuế, Lao động) vào một legal agent đã học domain cũ (VLQA, án lệ chung), các hyperparameter LoRA đóng vai trò điều khiển mức độ "quên":[^7][^1]

- Giữ $r$ vừa phải (16–32) và $\alpha / r$ không quá lớn để adapter không "đè" lên behavior cũ quá mạnh.
- Dùng LoRA/QLoRA thay vì full-FT để bảo tồn phần lớn weights gốc; LoRA bản thân chúng có tính regularization tốt hơn về forgetting.[^1]
- Theo dõi các benchmark cũ song song với benchmark mới; nếu score trên domain cũ giảm mạnh, thử alpha scaling (giảm $\alpha$), giảm số epoch, hoặc bật rsLoRA để ổn định hơn với rank hiện tại.[^3][^2]

Như vậy, các tham số r, $\alpha$, dropout, target modules không chỉ là "knob kỹ thuật" mà là phần của chính sách kiểm soát mức độ học và quên của agent pháp lý trong thiết kế nghiên cứu của bạn.

---

## References

1. [What are the LoRA hyperparameters (r, alpha) - Avichala](https://www.avichala.com/blog/what-are-the-lora-hyperparameters-r-alpha) - Explore What are the LoRA hyperparameters (r, alpha) with Avichala — a global Generative AI learning...

2. [LoRA fine-tuning Hyperparameters Guide | Unsloth Documentation](https://unsloth.ai/docs/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide) - Increase LoRA Rank ( r ) and alpha: Rank should at least equal to the alpha number, and rank should ...

3. [LoRA Scaling Parameter Alpha - ApX Machine Learning](https://apxml.com/courses/lora-peft-efficient-llm-training/chapter-2-lora-in-depth/lora-scaling-alpha) - The scaling parameter, denoted as α \alpha α, is an important hyperparameter in Low-Rank Adaptation ...

4. [How to choose LoRA Hyper-parameters? - Trelis Research](https://trelis.substack.com/p/how-to-choose-lora-hyper-parameters) - Low Rank Adapters (LoRA) are a way to continue training a neural net with less compute and a lot les...

5. [LoRA Fine-tuning & Hyperparameters Explained (in Plain English)](https://www.entrypointai.com/blog/lora-fine-tuning/) - In the LoRA codebase, Microsoft sets alpha to 2x the rank in all their examples, meaning that the we...

6. [Hyperparam recommend LoRA Unsloth task NLI](https://www.perplexity.ai/search/1f1bd1ba-8a52-43c8-9486-b00582af8f33) - Có. Cho NLI trên Unsloth, mình khuyên bạn bắt đầu bằng một cấu hình an toàn, ổn định, ít overfit trư...

7. [CHo mình hỏi nha nếu là rag mà trong bài toán này 

thì nó đang tiếp cận ở đoạn nào bạn vẽ cho mình flow md được không](https://www.perplexity.ai/search/386ce85b-f1c0-4782-b3e3-bbceba89b9b5) - Trong bài toán của bạn, RAG chính là lớp Semantic Memory của agent pháp lý, nằm giữa:
dữ liệu pháp l...

