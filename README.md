# Kế hoạch công việc · Phạm Minh Kiên

Trang kế hoạch công việc cá nhân, chạy trên Vercel. Trang chỉ để xem: mọi thay đổi đi qua file `tasks.json`, do Claude cập nhật rồi đẩy lên nhánh `main`. Vercel tự triển khai lại sau mỗi lần đẩy.

## Cấu trúc `tasks.json`

Ví dụ dưới đây chỉ minh họa cấu trúc; file thật bắt đầu trống.

```json
{
  "updatedAt": "2026-11-22T02:10:00+07:00",
  "schedule": [
    { "days": [1], "label": "Lịch mẫu có giờ", "start": "07:00", "end": "12:00" },
    { "days": [2, 4], "label": "Lịch mẫu theo buổi", "slot": "Buổi tối" }
  ],
  "goals": [
    { "id": "g-202612-a1b2c3", "title": "Mục tiêu mẫu", "scope": "month", "period": "2026-12", "status": "todo", "note": "" }
  ],
  "tasks": [
    {
      "id": "t-20261122-a1b2c3",
      "title": "Việc mẫu",
      "date": "2026-11-22",
      "time": "19:30",
      "category": "Khác",
      "status": "todo",
      "important": false,
      "goal": "",
      "originalDate": "",
      "postponed": 0,
      "link": "",
      "note": "",
      "doneAt": null,
      "createdAt": "2026-11-22T02:10:00+07:00"
    }
  ]
}
```

### `roadmap`: lộ trình lớn (dải mốc ở đầu trang)

| Trường | Ý nghĩa |
|---|---|
| `id` | `m-...`, không đổi. |
| `title` | Tên mốc. |
| `when` | Thời gian hoặc ghi chú ngắn, đúng lời thầy; rỗng nếu chưa đặt. |
| `status` | `done` (đã qua), `current` (đang ở đây), `next` (phía trước). Chỉ một mốc `current`. |

### `schedule`: lịch cố định hằng tuần (hiện mờ trên lịch, không tính là việc)

| Trường | Ý nghĩa |
|---|---|
| `days` | Thứ trong tuần: 1 = Thứ Hai … 6 = Thứ Bảy, 7 = Chủ nhật. |
| `label` | Tên lịch. |
| `start`, `end` | Giờ `HH:MM` nếu biết chính xác. |
| `slot` | Mô tả khi không có giờ chính xác, ví dụ "Buổi tối", "Cả ngày". Chỉ ghi đúng điều thầy nói. |
| `from`, `until` | Ngày bắt đầu và ngày kết thúc hiệu lực, `YYYY-MM-DD`, tính cả hai đầu. Bỏ trống nghĩa là không giới hạn phía đó. |
| `cancelled` | Danh sách ngày `YYYY-MM-DD` mà buổi đó bị huỷ. Trang gạch ngang buổi đó và ghi "Đã huỷ"; các tuần khác giữ nguyên. |
| `done` | Danh sách ngày `YYYY-MM-DD` mà buổi đó đã xong. Trang gạch ngang và ghi "Đã xong". |

### `goals`: mục tiêu tuần, tháng

| Trường | Ý nghĩa |
|---|---|
| `id` | `g-YYYYMM-xxxxxx` hoặc `g-YYYYMMDD-xxxxxx`. |
| `scope` | `month` hoặc `week`. |
| `period` | Với `month`: `YYYY-MM`. Với `week`: ngày Thứ Hai của tuần, `YYYY-MM-DD`. |
| `status` | `todo` (đang theo đuổi) hoặc `done` (đã đạt). |
| `milestone` | `id` của mốc lộ trình mà mục tiêu phục vụ, hoặc bỏ trống. |

### `tasks`: đầu việc

| Trường | Ý nghĩa |
|---|---|
| `id` | `t-YYYYMMDD-xxxxxx`, YYYYMMDD là ngày hạn lúc tạo, `xxxxxx` là 6 ký tự ngẫu nhiên. Không đổi id khi dời hạn. |
| `title` | Tên việc ngắn, đúng chữ của thầy. |
| `date` | Hạn hiện tại, `YYYY-MM-DD`, giờ Việt Nam. |
| `time` | `HH:MM` (24 giờ) hoặc chuỗi rỗng. |
| `category` | `Giảng dạy`, `Học tập`, `Dự án`, `Tài liệu`, `Cá nhân` hoặc `Khác`. |
| `status` | `todo` (chưa xong) hoặc `done` (đã xong). |
| `important` | `true` khi thầy nói việc này quan trọng hoặc không được trễ. |
| `goal` | `id` của mục tiêu mà việc phục vụ, hoặc chuỗi rỗng. |
| `originalDate` | Hạn ban đầu, điền ở lần dời hạn đầu tiên; giữ nguyên ở các lần sau. |
| `postponed` | Số lần đã dời hạn; tăng 1 mỗi lần dời. |
| `link` | Link sản phẩm (bắt đầu bằng `https://`) hoặc chuỗi rỗng. |
| `note` | Ghi chú ngắn hoặc chuỗi rỗng. |
| `doneAt` | Thời điểm xong, ISO 8601 có `+07:00`, hoặc `null`. |
| `createdAt` | Thời điểm tạo, ISO 8601 có `+07:00`. |

Mỗi lần sửa, cập nhật `updatedAt` ở đầu file.
