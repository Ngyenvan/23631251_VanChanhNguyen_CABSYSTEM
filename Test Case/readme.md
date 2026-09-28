# CAB System Test Case Index

Mỗi test bám `srs.md` v1.5 và API theo service ownership. Không có test cho công thức cước, phí hủy, thang điểm, retention, quyền chi tiết, ETA, channel/retry thông báo hoặc giới hạn payment retry vì các nội dung đó còn Open Issue.

Chuỗi tích hợp: Booking -> Dispatch PRIMARY/RETRY/RECOVERY -> Trip -> Fare -> Payment -> Notification/Reporting/Audit. Test Dispatch, Booking, Trip và Payment là nhóm bắt buộc kiểm tra liên service.
