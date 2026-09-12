# 03-ai-log.md

## Nhật ký tương tác AI

### 1. AI đã giúp gì?
- Giúp tôi rà soát các bài toán thực tế tại Vin Smart Future.
- Hỗ trợ lựa chọn 3 ý tưởng đáng chú ý dựa trên độ rõ ràng, độ khả thi và mức giá trị vận hành.
- Hỗ trợ viết bộ Quick Problem Cards theo đúng format của lab.
- Hỗ trợ định hình Deep Dive cho bài toán Xanh SM, bao gồm bottleneck, problem statement và future-state flow.

### 2. AI đã trả lời sai / hallucination ở đâu?
- Ban đầu, AI có xu hướng đưa quá nhiều ý tưởng chung mà không gắn sát với quy trình vận hành thực tế.
- Khi tôi yêu cầu rõ hơn về bottleneck, metric và human-in-the-loop, AI bắt đầu cho ra các mô tả sát thực tế hơn.
- Một điểm cần cảnh giác là AI có thể gợi ý quá nhiều giải pháp phức tạp hoặc dùng Agentic Loop khi bài toán thực tế không cần đến mức đó.

### 3. Tôi đã sửa prompt / ranh giới như thế nào?
- Tôi yêu cầu AI chỉ chọn các bài toán có workflow rõ ràng, có bottleneck cụ thể và dễ đo metric.
- Tôi nhấn mạnh rằng ranh giới an toàn phải có Human-in-the-loop, đặc biệt ở bài toán liên quan đến pin xe và tin nhắn gửi cho tài xế.
- Tôi yêu cầu AI mô tả rõ AI fit là Rule / LLM / Agentic Loop và chỉ chọn LLM Feature cho bài toán này.

### 4. Kết quả học được
- Trong các bài toán AI, ưu tiên những vấn đề có quy trình rõ, bottleneck dễ đo, và không quá nhạy cảm về an toàn.
- Không phải bài toán nào cũng phù hợp dùng AI; cần xét kỹ ranh giới, dữ liệu và khả năng kiểm soát sai sót.
- AI là người hỗ trợ tư duy, nhưng quyết định cuối cùng vẫn phải dựa trên hiểu biết nghiệp vụ thực tế.

### 5. Phản ánh cá nhân
- Tôi thấy AI rất hữu ích trong việc brainstorm và định dạng nội dung, nhưng không nên tin tuyệt đối vào mọi câu trả lời.
- Khi yêu cầu AI đánh giá lại theo góc độ vận hành và rủi ro, kết quả trở nên đáng tin cậy hơn nhiều.
- Bài lab này giúp tôi hiểu rõ rằng AI Product Scoping không chỉ là tìm giải pháp AI, mà là phải biết chọn đúng bài toán, vẽ đúng ranh giới và đo được giá trị kinh doanh.
