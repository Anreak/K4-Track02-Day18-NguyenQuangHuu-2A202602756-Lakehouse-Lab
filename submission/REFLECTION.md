# Reflection — K4-Track02-Day18 Lakehouse Lab

Lựa chọn: **Vector Lifecycle Bug** (tách rời Vector DB khỏi Lakehouse).

Khi xử lý văn bản và phản hồi LLM, kiến trúc thường lưu embeddings ở Vector DB độc lập và metadata ở Lakehouse. Do thiếu transaction hai pha, khi người dùng yêu cầu xóa dữ liệu hoặc tài liệu được cập nhật ở Lakehouse, pipeline thường bỏ quên sự kiện xóa trên Vector DB. Hậu quả là external index vẫn trả về vector cũ, khiến mô hình sinh phản hồi chứa thông tin vi phạm hoặc sai lệch vd: như NB7 tái hiện: 0 hit trong bảng nhưng >0 hit trên index.

**Cách phòng tránh:** Dùng Lakehouse làm Single Source of Truth, lưu embeddings trực tiếp trong bảng Delta/Iceberg và tận dụng Change Data Feed (CDF) để đồng bộ hóa có kiểm soát phiên bản sang index phái sinh khi cần scale.