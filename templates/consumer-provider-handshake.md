# Consumer–Provider Handshake — A6 Core Business (team-core)

## Thông tin chung

- Lab: FIT4110 Lab 03
- Ngày: 2026-05-21
- Provider team: team-vision (A4 AI Vision)
- Consumer team: team-core (A6 Core Business)
- Provider service: AI Vision Service
- Consumer service: Core Business Service

## Contract

- Contract file: `contracts/ai-vision.openapi.yaml`
- Mock base URL: `http://localhost:4011`
- Auth method: Bearer Token (`Authorization: Bearer {{authToken}}`)
- Endpoint được test: `POST /detect`

## Smoke test

### Request

```http
POST /detect
Authorization: Bearer lab-token
Content-Type: application/json
```

```json
{
  "camera_id": "CAM-01",
  "image_url": "https://storage.campus.local/frames/CAM-01-001.jpg"
}
```

### Expected response

```json
{
  "detection_id": "DET001",
  "camera_id": "CAM-01",
  "label": "person",
  "confidence": 0.91,
  "risk_level": "medium"
}
```

### Assertions A6 cần kiểm tra

| Field | Kiểu | Ràng buộc | A6 dùng để |
|---|---|---|---|
| `detection_id` | string | required | Gắn vào VisionDetectionEvent |
| `label` | string | enum: person/vehicle/unknown | Xác định loại đối tượng |
| `confidence` | number | 0.0 – 1.0 | Quyết định mức rủi ro |
| `risk_level` | string | enum: low/medium/high | Map sang Severity của A6 |

## Kết quả

- [x] Consumer gọi mock thành công.
- [x] Consumer parse được field cần dùng (`detection_id`, `label`, `confidence`, `risk_level`).
- [x] Consumer hiểu lỗi 4xx provider trả về (401 Unauthorized, 400 Bad Request).
- [ ] Có Newman report hoặc screenshot.

## Ghi chú thay đổi hợp đồng

| Nội dung | Trước | Sau | Người đồng ý |
|---|---|---|---|
| risk_level mapping | low/medium/high (A4) | LOW/MEDIUM/HIGH (A6) | team-core + team-vision |

> **Lưu ý:** A4 AI Vision trả `risk_level` dạng lowercase (`low`, `medium`, `high`).
> A6 Core Business dùng Severity dạng UPPERCASE (`LOW`, `MEDIUM`, `HIGH`, `CRITICAL`).
> A6 phải tự mapping khi nhận kết quả từ A4.

## Xác nhận

- Provider representative: team-vision
- Consumer representative: team-core
