# Nguồn sơ đồ — trang "10 kênh, một cái tên"

Hai hình trong `../aipub-account-setup-2026-11-23.html` được vẽ bằng Graphviz, không vẽ tay. File `.dot` ở đây là nguồn duy nhất.

| Nguồn | Xuất ra |
| --- | --- |
| `1-bundle.dot` | `../aipub-account-setup-2026-11-23-1.svg` |
| `2-channels.dot` | `../aipub-account-setup-2026-11-23-2.svg` |

Tạo lại (cần `graphviz`):

```bash
dot -Tsvg 1-bundle.dot -o ../aipub-account-setup-2026-11-23-1.svg
dot -Tsvg 2-channels.dot -o ../aipub-account-setup-2026-11-23-2.svg
```

Ghi chú khi sửa: giữ `fontname="DejaVu Sans"` để chữ khớp với phần còn lại của trang; đổi danh sách kênh thì sửa `2-channels.dot` và cập nhật cả bảng trong trang HTML cho khớp. Kết quả render đã được đối chiếu byte-identical với file SVG đang dùng.
