---
{"dg-publish":true,"permalink":"/knowledge/02-tech-second-brain/ai-core/nlp/lora-va-r/","pinned":"false"}
---


Ý chính: _r_ mà bài “LoRA Learns Less and Forgets Less” nói tới là **hạng nội tại của perturbation Δ trong full fine‑tuning/continued pretraining (CPT)**, còn _r_ bạn đặt trong Unsloth chỉ là **rank của adapter LoRA** – hai cái này khác nhau về bản chất và về scale. Trong CPT, SVD cho thấy Δ có intrinsic rank rất cao (cỡ 1.500–2.000 với ma trận 4096×4096), còn LoRA/Unsloth chủ động chọn r nhỏ (16–64) để tiết kiệm VRAM, chấp nhận việc adapter không thể “ôm hết” lượng tri thức thô đó.[[arxiv](https://arxiv.org/abs/2405.09673)]

Mình đi từ từ, tách rõ 3 loại “r”: r của SVD/Δ (CPT), r của LoRA trong Unsloth, và r trong setting continual.

---

## 1. r trong bài LoRA Learns Less and Forgets Less (CPT)

Trong paper Biderman 2024, họ xét **full fine‑tuning/continued pretraining** trên các domain như code, toán, với mô hình Llama‑2‑7B. Khi full FT, trọng số mới là:[[arxiv](https://arxiv.org/html/2405.09673v2)]

Wft=Wpre+ΔW_{\text{ft}} = W_{\text{pre}} + \DeltaWft​=Wpre​+Δ

Trong đó Δ là **ma trận cập nhật “thật”** chứa tri thức mới model học được từ domain target.[[arxiv](https://arxiv.org/abs/2405.09673)]

Họ dùng SVD trên Δ (và các ma trận liên quan như WqW_qWq​) để xem: “Để giải thích ~90% phương sai, cần bao nhiêu singular value?” Kết quả:

- Với lớp Wq∈R4096×4096W_q \in \mathbb{R}^{4096\times 4096}Wq​∈R4096×4096, **intrinsic rank** (số singular value lớn cần giữ) để capture 90% variance **> 1500, thường ~2000**.[[arxiv](https://arxiv.org/html/2405.09673v2)]
    
- Nghĩa là perturbation Δ sau CPT **thực sự nằm gần một subspace rank cao** – khoảng 1.5–2k chiều latent.[[arxiv](https://arxiv.org/abs/2405.09673)]
    

So sánh: LoRA bạn đang dùng thường đặt r = 16, 32, 64, cùng lắm 256 – nhỏ hơn intrinsic rank Δ **từ 10 tới 100 lần**. Do đó, về mặt hình học, một LoRA rank=16–256 chỉ có thể xấp xỉ Δ rất thô; những tri thức thô, giàu variance (facts, code, toán) không thể “nhét” trọn vào một subspace bé như vậy.[[arxiv](https://arxiv.org/html/2405.09673v2)]

**Kết luận của paper**: LoRA “learns less” (học được ít pattern raw hơn full FT) nhưng “forgets less” (giữ được performance trên task gốc tốt hơn) chính vì adapter bị ràng buộc vào một subspace hạng thấp. Với facts high‑rank, muốn LoRA học giống full FT thì phải tăng r lên gần intrinsic rank – điều này phá vỡ lợi thế VRAM/compute vốn là lý do PEFT tồn tại.[[arxiv](https://arxiv.org/abs/2405.09673)]

---

## 2. r trong Unsloth (SFT) – tại sao lại nhỏ?

Unsloth và đa số hướng dẫn LoRA/QLoRA thực tế đi theo một triết lý khác: **tối ưu trade‑off VRAM ↔ chất lượng trên dạng task cụ thể (instruction, chat, domain adaptation)**.[[unsloth](https://unsloth.ai/docs/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide)]

Từ guide và các heuristic mới nhất:

- Unsloth default:
    
    - SFT (instruction/domain adaptation): `r = 16`, `lora_alpha = 16`, target modules = toàn bộ attention + MLP (q/k/v/o + gate/up/down).[[leeroopedia](https://leeroopedia.com/index.php/Heuristic:Unslothai_Unsloth_LoRA_Rank_Selection)]
        
    - RL‑style (GRPO, DPO): đề xuất `r = 64` vì reward‑driven optimization cần capacity rộng hơn.[[leeroopedia](https://leeroopedia.com/index.php/Heuristic:Unslothai_Unsloth_LoRA_Rank_Selection)]
        
- Lý do: họ thấy **r = 16** đủ cho đa số SFT, khi bạn chủ yếu dạy _format, style, domain bias_, chứ không nhồi raw facts khổng lồ như CPT 20B token.[[unsloth](https://unsloth.ai/docs/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide)]
    

Nói cách khác:

- Paper Biderman đo **intrinsic rank của Δ trong CPT** – đây là kịch bản “đổ 20B token raw” vào model để thay đổi tri thức nặng.[[arxiv](https://arxiv.org/abs/2405.09673)]
    
- Unsloth default r=16 được tune cho kịch bản **SFT trên ~100k–1M mẫu**: chủ yếu thay đổi hành vi theo task (cách trả lời, cách reasoning trong domain), ít hơn về raw world knowledge.[[unsloth](https://unsloth.ai/docs/get-started/fine-tuning-llms-guide/lora-hyperparameters-guide)]
    

Trong bối cảnh đó, chọn r nhỏ:

- Giảm VRAM và tốc độ train (quan trọng với pipeline của bạn).
    
- Giữ base model gần như nguyên vẹn → LoRA “forgets less” một cách natural, không cần constraint phức tạp.[[openreview](https://openreview.net/pdf?id=W3caXQAjcu)]
    

Khi bạn chuyển sang CPT (legal pretraining vài chục tỉ token), nếu **thực sự muốn LoRA cạnh tranh với full FT về tri thức thô**, rank cần ở vùng gần intrinsic rank mà SVD đo được – tức vài trăm đến ~2000 – điều này gần như không thực tế trên GPU bạn đang dùng.[[arxiv](https://arxiv.org/html/2405.09673v2)]

---

## 3. r trong continual learning: r cho mỗi nhiệm vụ/domain

Trong continual learning với LoRA (O‑LoRA, N‑LoRA, MoLA, SLIM), “r của continual” có thể hiểu theo 2 lớp:

1. **Rank per adapter / per task**: mỗi domain/task được gán một LoRA adapter với rank riêng rkr_krk​. Đây là capacity mà bạn _cấp cho từng nhiệm vụ_ để nó encode knowledge mới.[[openreview](https://openreview.net/forum?id=L7ZBpZZ8Va)]
    
2. **Effective rank tổng cộng**: tổng dung lượng latent mà toàn bộ hệ gồm nhiều adapter đang chiếm trong space của weight – có thể rất lớn nếu bạn có nhiều task và mỗi task có r>0, kể cả khi từng r_k nhỏ.[[aclanthology](https://aclanthology.org/2025.coling-main.286/)]
    

Liên hệ đặt câu hỏi của bạn:

> “r của continual là r khi dùng unsloth; r của CPT thì rất là lớn à?”

Có thể hiểu như:

- **r trong Unsloth CL/SFT**: rank adapter mà bạn tự set (16, 32, 64…). Đây là **chọn capacity** trong low‑rank space để học behaviour mới cho mỗi task/domain. Với nhiều task (continual), còn có thêm chiều là _bao nhiêu adapter, tổ chức subspace thế nào_ (orthogonal, non‑collision…).[[openreview](https://openreview.net/forum?id=L7ZBpZZ8Va)]
    
- **r trong CPT SVD**: intrinsic rank của Δ nếu bạn FT full model trên một lượng dữ liệu rất lớn. Đây là **năng lực cần thiết** để encode fully tri thức mới; nó không đồng nhất với r của adapter, mà là “điều kiện cần” nếu bạn muốn LoRA xấp xỉ FT kịch bản đó.[[arxiv](https://arxiv.org/html/2405.09673v2)]
    

Cho continual:

- Nếu mục tiêu là **giữ lại tri thức gốc** (legal base + world knowledge) và thêm behaviour/phán đoán mới theo từng domain/quy tắc legal, bạn _không nhất thiết_ phải bắt LoRA match intrinsic rank Δ_CPT.
    
- Bạn có thể dùng nhiều adapter rank nhỏ (16–32), tổ chức chúng bằng orthogonality (O‑LoRA) hoặc non‑collision (N‑LoRA), và/hoặc MoE‑style (MoLA, SLIM) để phân bổ knowledge theo task/domain.[[arxiv](https://arxiv.org/html/2412.01004v7)]
    

Nói đơn giản: **continual r** = capacity mỗi adapter + cách dàn trải nhiều adapter; nó liên quan tới intrinsic rank CPT nhưng không buộc phải bằng. Bạn có thể dùng fact từ Biderman 2024 như một _upper bound_:

- Nếu hội đồng hỏi “r=256 có đủ để học thuộc toàn bộ luật mới không?”, bạn có thể trả lời:
    
    - “Trong CPT toán/code, phân tích SVD cho thấy perturbation của full FT có intrinsic rank ~1500–2000 để giữ 90% variance; LoRA r=256 chỉ là subspace bé hơn 10–100 lần, nên không thể encode hết tri thức thô kiểu đó. Vì vậy em chủ động thiết kế hệ thống để push phần knowledge law ra RAG, còn LoRA chỉ học policy/mapping/legal reasoning.”[[arxiv](https://arxiv.org/abs/2405.09673)]

---

## 4. Kết nối với pipeline của bạn (CPT vs SFT vs CL legal)

Trong bài toán legal VN của bạn:

- **Pretraining/CPT legal (CPT)**: nếu bạn làm một bước CPT lớn trên corpora luật, intrinsic rank Δ nhiều khả năng cũng rất cao (cùng bản chất với kết quả Biderman nói cho math/code).[[arxiv](https://arxiv.org/html/2405.09673v2)]
    
- **SFT legal (QA/NLI/syllogism)**: bạn đang chủ yếu dạy model cách trả lời, cách lập luận, chứ không đổ lại tri thức luật từ đầu. Ở đây Unsloth r=16–32 là hợp lý vì mục tiêu là _behaviour mapping_ hơn là raw fact learning.[[leeroopedia](https://leeroopedia.com/index.php/Heuristic:Unslothai_Unsloth_LoRA_Rank_Selection)]
    
- **Continual legal (nhiều domain, nhiều bench)**: r mỗi adapter là cách bạn chia capacity cho từng domain/task; orthogonality/non‑collision/mix MoLA/SLIM là cách bạn tránh adapter mới phá adapter cũ.[[aclanthology](https://aclanthology.org/2025.naacl-long.246.pdf)]
    

Vậy nên có thể nói:

- r_CPT (intrinsic rank Δ) rất lớn → fact mạnh là CPT FT học “nhiều hơn” nhưng cũng quên nhiều hơn.
    
- r_LoRA_Unsloth (16–64) nhỏ → học ít tri thức thô hơn nhưng quên ít hơn, phù hợp PEFT trên GPU của bạn.
    
- r_CL (cho O‑LoRA/N‑LoRA/MoLA/SLIM) là biến thiết kế: bạn chọn rank vừa phải, rồi dùng regularizer/constraint để tổ chức subspace/adapter sao cho hệ vừa learn more vừa forget less.
    

Nếu bạn muốn mình có thể viết sẵn một đoạn giải thích bằng tiếng Việt theo ngôn ngữ luận văn (phần “Theoretical Motivation”) để bạn quote thẳng: tách rõ “intrinsic rank Δ trong CPT” vs “adapter rank r trong LoRA/Unsloth”, rồi dẫn Biderman 2024 như chứng cứ cho lựa chọn RAG + LoRA rank nhỏ trong framework legal của bạn.