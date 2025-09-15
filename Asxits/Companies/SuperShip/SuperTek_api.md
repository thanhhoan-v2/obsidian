# SuperTek API

## Giới Thiệu

SuperShip APIs là một tập hợp các giao diện lập trình ứng dụng (API) được cung cấp bởi SuperShip. Các API này cho phép các nhà phát triển và doanh nghiệp tích hợp dịch vụ vận chuyển của SuperShip vào các ứng dụng của mình.

### Tên Thương Hiệu

Quý đối tác, khách hàng vui lòng sử dụng đúng tên thương hiệu của SuperShip, cụ thể như sau:

-   Cách viết đúng: **SuperShip** (lưu ý 2 chữ S phải viết hoa), **SUPERSHIP**
-   Cách viết sai: **Supership** (chữ S thứ 2 chưa được viết hoa), **suppership** (viết dư chữ p), **shupership** (viết dư chữ h ở đầu), **supersip** (viết thiếu chứ h phía sau)...

### Logo

Các đối tác khách hàng muốn hiển thị Logo SuperShip thì vui lòng sử dụng các tài nguyên sau:

-   Logo Vuông Đỏ PNG: [Xem](https://mdl.supership.net/images/SuperShip-Logo.png)
-   Logo Vuông Đỏ SVG: [Xem](https://mdl.supership.net/images/SuperShip-Logo.svg)
-   Logo Ngang Đỏ PNG: [Xem](https://mdl.supership.net/images/SuperShip-Logo-Ngang-Do-Moi.png)
-   Logo Ngang Trắng PNG: [Xem](https://mdl.supership.net/images/SuperShip-Logo-Ngang-Trang-Moi.png)
-   Logo Vuông Trắng Nền Đỏ PNG: [Xem](https://mdl.supership.net/images/SuperShip-Logo-Vuong-Trang-Nen-Do.png)
-   Logo Vuông Đỏ Nền Trắng PNG: [Xem](https://mdl.supership.net/images/SuperShip-Logo-Vuong-Do-Nen-Trang.png)
-   Logo Đứng Chuẩn PNG: [Xem](https://mdl.supership.net/images/SuperShip-Logo-Chuan.png)

### Liên Hệ

Các khách hàng, đối tác cần hỗ trợ, hợp tác thì có thể liên hệ:

-   Telegram: [SuperShip Support APIs](https://t.me/+Ul3BoTm5jmUzZjQ1)
-   Email: [supertek@supership.vn](mailto:supertek@supership.vn)

### Request Format

-   Giá trị `Accept` trong header request phải là `application/json`.
-   Giá trị `Content-Type` trong header request phải là `application/json`.

### Response Format

Giá trị sẽ là JSON khi request gửi kèm thông tin `Accept` có giá trị là `application/json`.

#### Thành công

```json
{
    "status": "Success",
    "message": "<Nội-Dung-Tin-Nhắn>",
    "results": "<Dữ-Liệu-Thành-Công>"
}
```

#### Thất bại

```json
{
    "status": "Error",
    "message": "<Mô-Tả-Lỗi>",
    "errors": "<Thông-Tin-Chi-Tiết-Lỗi>"
}
```

### Môi Trường

-   URL Production: [https://api.mysupership.vn](https://api.mysupership.vn/)
-   URL Sandbox: Tạm thời đang bảo trì, sử dụng TK Test được SuperShip cấp để test trực tiếp trên Production. Vui lòng tham dự Nhóm Hỗ Trợ API trên Telegram [SuperShip Support APIs](https://t.me/+Ul3BoTm5jmUzZjQ1) để được hỗ trợ.

### Các Tính Năng

-   **Areas API**: API này cho phép các nhà phát triển lấy danh sách các tỉnh, huyện và xã được hỗ trợ bởi SuperShip cho việc lấy hàng, giao hàng và trả hàng.
-   **Auth API**: API này cho phép các nhà phát triển đăng ký người dùng mới và lấy Mã Truy Cập thông qua tên đăng nhập và mật khẩu.
-   **Orders API**: API này cho phép các nhà phát triển lấy thông tin về mức phí vận chuyển, tạo đơn hàng mới, lấy thông tin về đơn hàng, lấy danh sách trạng thái đơn hàng và tạo nhãn vận chuyển.
-   **Warehouses API**: API này cho phép các nhà phát triển tạo kho hàng mới, chỉnh sửa kho hiện tại và lấy thông tin về tất cả các kho hàng.
-   **Webhooks API**: API này cho phép các nhà phát triển đăng ký webhook mới, chỉnh sửa webhook hiện tại và lấy danh sách webhook đã đăng ký.

## Xác Thực

Các Request có yêu cầu xác thực người dùng thì phải gửi kèm các thông tin như sau trong Headers.

| Key           | Value              |
| :------------ | :----------------- |
| Accept        | application/json   |
| Authorization | Bearer `Access-Token` |

Trong đó `Access-Token` là Mã Xác Thực của người dùng.

> **BẠN CÓ BIẾT**
>
> Hiện tại, có 2 cách để lấy `Access-Token`, dùng **Personal Access Tokens** hoặc **Password Grant Tokens**.

### Personal Access Tokens

Xem `Access-Token` tại phần [API](https://mysupership.vn/settings?tab=api) của SuperShip.

> **LƯU Ý**
>
> -   ***Mục này dành cho các đơn vị muốn kết nối thông qua Token Được Cung Cấp Sẵn, các khách hàng cá nhân hoặc các đơn vị chỉ có 1 Shop cần kết nối.***
> -   Phần lớn Khách Hàng/Đối Tác sử dụng Phương Thức Xác Thực này.

### Password Grant Tokens

API này cho phép bạn lấy Mã Xác Thực của người dùng thông qua email và mật khẩu.

> **LƯU Ý**
>
> **Mục này chỉ dành cho đối tác Sàn Thương Mại Điện Tử, Các Đơn Vị Phần Mềm có nhiều khách hàng bên trong cần kết nối SuperShip.**

#### Endpoint

`POST /v1/partner/auth/login`

#### Request

##### Các Tham Số

| Thuộc Tính    | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                           |
| :------------ | :------- | :------ | :------------------------------------------------------- |
| client_id     | Có       | String  | Client ID cấp cho Đối Tác. Ví dụ: `AZN6QUo40w`.          |
| client_secret | Có       | String  | Client Secret cấp cho Đối Tác. Ví dụ: `C4fFVeFPkISEDQ8acNo9oSHUd8yIGuvoLWJdX9zY`. |
| username      | Có       | String  | Email của Shop/Công ty. Ví dụ: `hmn.store@gmail.com`.      |
| password      | Có       | String  | Mật Khẩu.                                                |
| partner       | Có       | String  | Mã Bí Mật. Dành cho các Đối Tác Thương Mại Điện Tử lớn với SuperShip. |

##### Ví Dụ

```bash
curl --request POST \
     --url https://api.mysupership.vn/v1/partner/auth/login \
     --header 'Accept: application/json' \
     --header 'Content-Type: application/json' \
     --data '{
    	"client_id": "AZN6QUo40w",
    	"client_secret": "C4fFVeFPkISEDQ8acNo9oSHUd8yIGuvoLWJdX9zY",
    	"username": "hmn.store@gmail.com",
    	"password": "323423",
    	"partner": "lPxLuxfiTotCyZ1ZnQjMepUL24HLd05ybNBhVGFN"
    }'
```

#### Response

##### Kết Quả Trả Về

| Thuộc Tính   | Kiểu DL | Mô Tả Chi Tiết                                     |
| :----------- | :------ | :------------------------------------------------- |
| token_type   | String  | Loại Token. Ví dụ: `Bearer`.                       |
| expires_in   | Integer | Khoảng thời gian (giây) hết hạn của Access Token. Ví dụ: `31536000`. |
| access_token | String  | Access Token. Ví dụ: `ZT2PS0pmHPHDKjRu6EMIcoM8rFM8XYHZ1Ye3zRiQ`. |

##### Ví dụ

```json
{
    "status": "Success",
    "results": {
        "token_type": "Bearer",
        "expires_in": 31536000,
        "access_token": "<Access-Token>"
    }
}
```

## Khu Vực

Các API về khu vực cho phép bạn xem danh sách các Tỉnh/Thành phố, Quận/Huyện, Phường/Xã mà SuperShip đang hỗ trợ lấy, giao và trả hàng.

### Lấy Thông Tin Tỉnh/Thành Phố

API này cho phép bạn lấy thông tin Tỉnh/Thành Phố mà SuperShip đang hỗ trợ.

#### Endpoint

`GET /v1/partner/areas/province`

#### Request

##### Ví Dụ

```bash
curl --request GET \
     --url https://api.mysupership.vn/v1/partner/areas/province
```

#### Response

##### Tham Số Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| code       | String  | Mã Tỉnh/Thành Phố. Ví dụ: `79`.                      |
| name       | String  | Tên Tỉnh/Thành Phố mà SuperShip đang hỗ trợ lấy, giao và trả hàng. Ví dụ: `Thành phố Hồ Chí Minh`. |

##### Ví Dụ

```json
{
  "status": "Success",
  "results": [
    {
      "code": "01",
      "name": "Thành phố Hà Nội"
    },
    {
      "code": "79",
      "name": "Thành phố Hồ Chí Minh"
    }
  ]
}
```

### Lấy Thông Tin Quận/Huyện

API này cho phép bạn lấy thông tin Quận/Huyện mà SuperShip đang hỗ trợ.

#### Endpoint

`GET /v1/partner/areas/district`

#### Request

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| province   | Có       | String  | Mã Tỉnh/Thành Phố mà SuperShip đang hỗ trợ lấy, giao và trả hàng. Ví dụ: `79`. |

##### Ví Dụ

```bash
curl --request GET \
     --url 'https://api.mysupership.vn/v1/partner/areas/district?province=79'
```

#### Response

##### Tham Số Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| code       | String  | Mã Quận/Huyện. Ví dụ: `785`.                         |
| name       | String  | Tên Quận/Huyện. Ví dụ: `Huyện Bình Chánh`.           |
| province   | String  | Tên Tỉnh/Thành Phố mà SuperShip đang hỗ trợ lấy, giao và trả hàng. Ví dụ: `Thành phố Hồ Chí Minh`. |

##### Ví Dụ

```json
{
  "status": "Success",
  "results": [
    { "code": "785", "name": "Huyện Bình Chánh", "province": "Thành phố Hồ Chí Minh" },
    { "code": "783", "name": "Huyện Củ Chi", "province": "Thành phố Hồ Chí Minh" },
    { "code": "784", "name": "Huyện Hóc Môn", "province": "Thành phố Hồ Chí Minh" },
    { "code": "786", "name": "Huyện Nhà Bè", "province": "Thành phố Hồ Chí Minh" },
    { "code": "760", "name": "Quận 1", "province": "Thành phố Hồ Chí Minh" },
    { "code": "771", "name": "Quận 10", "province": "Thành phố Hồ Chí Minh" },
    { "code": "772", "name": "Quận 11", "province": "Thành phố Hồ Chí Minh" },
    { "code": "761", "name": "Quận 12", "province": "Thành phố Hồ Chí Minh" },
    { "code": "769", "name": "Quận 2", "province": "Thành phố Hồ Chí Minh" },
    { "code": "770", "name": "Quận 3", "province": "Thành phố Hồ Chí Minh" },
    { "code": "773", "name": "Quận 4", "province": "Thành phố Hồ Chí Minh" },
    { "code": "774", "name": "Quận 5", "province": "Thành phố Hồ Chí Minh" },
    { "code": "775", "name": "Quận 6", "province": "Thành phố Hồ Chí Minh" },
    { "code": "778", "name": "Quận 7", "province": "Thành phố Hồ Chí Minh" },
    { "code": "776", "name": "Quận 8", "province": "Thành phố Hồ Chí Minh" },
    { "code": "763", "name": "Quận 9", "province": "Thành phố Hồ Chí Minh" },
    { "code": "777", "name": "Quận Bình Tân", "province": "Thành phố Hồ Chí Minh" },
    { "code": "765", "name": "Quận Bình Thạnh", "province": "Thành phố Hồ Chí Minh" },
    { "code": "764", "name": "Quận Gò Vấp", "province": "Thành phố Hồ Chí Minh" },
    { "code": "768", "name": "Quận Phú Nhuận", "province": "Thành phố Hồ Chí Minh" },
    { "code": "766", "name": "Quận Tân Bình", "province": "Thành phố Hồ Chí Minh" },
    { "code": "767", "name": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "762", "name": "Quận Thủ Đức", "province": "Thành phố Hồ Chí Minh" }
  ]
}
```

### Lấy Thông Tin Phường/Xã

API này cho phép bạn lấy thông tin Phường/Xã mà SuperShip đang hỗ trợ.

#### Endpoint

`GET /v1/partner/areas/commune`

#### Request

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| district   | Có       | String  | Mã Quận/Huyện Phố mà SuperShip đang hỗ trợ lấy, giao và trả hàng. Ví dụ: `767`. |

##### Ví Dụ

```bash
curl --request GET \
     --url 'https://api.mysupership.vn/v1/partner/areas/commune?district=767'
```

#### Response

##### Tham Số Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| code       | String  | Mã Phường/Xã. Ví dụ: `27037`.                        |
| name       | String  | Tên Phường/Xã. Ví dụ: `Phường Hiệp Tân`.             |
| district   | String  | Tên Quận/Huyện. Ví dụ: `Quận Tân Phú`.               |
| province   | String  | Tên Tỉnh/Thành Phố mà SuperShip đang hỗ trợ lấy, giao và trả hàng. Ví dụ: `Thành phố Hồ Chí Minh`. |

##### Ví Dụ

```json
{
  "status": "Success",
  "results": [
    { "code": "27037", "name": "Phường Hiệp Tân", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27034", "name": "Phường Hòa Thạnh", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27028", "name": "Phường Phú Thạnh", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27025", "name": "Phường Phú Thọ Hòa", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27031", "name": "Phường Phú Trung", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27016", "name": "Phường Sơn Kỳ", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27019", "name": "Phường Tân Quý", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27010", "name": "Phường Tân Sơn Nhì", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27022", "name": "Phường Tân Thành", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27040", "name": "Phường Tân Thới Hòa", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" },
    { "code": "27013", "name": "Phường Tây Thạnh", "district": "Quận Tân Phú", "province": "Thành phố Hồ Chí Minh" }
  ]
}
```

## Đơn Hàng

Các API về đơn hàng cho phép bạn tạo, xem, cập nhật và xóa một hay nhiều đơn hàng.

### Tính Cước Phí

API này cho phép bạn tính trước cước phí của một đơn hàng.

#### Endpoint

`GET /v1/partner/orders/price`

#### Request

##### Headers

| Key           | Value              |
| :------------ | :----------------- |
| Accept        | application/json   |
| Authorization | Bearer `Access-Token` |

##### Các Tham Số

| Thuộc Tính        | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :---------------- | :------- | :------ | :------------------------------------------------- |
| sender_province   | Có       | String  | Tên Tỉnh/Thành Phố của Người Gửi. Ví dụ: `Thành phố Hồ Chí Minh`. |
| sender_district   | Có       | String  | Tên Quận/Huyện của Người Gửi. Ví dụ: `Huyện Bình Chánh`. |
| receiver_province | Có       | String  | Tên Tỉnh/Thành Phố của Người Nhận. Ví dụ: `Thành phố Hà Nội`. |
| receiver_district | Có       | String  | Tên Quận/Huyện của Người Nhận. Ví dụ: `Quận Tây Hồ`. |
| weight            | Có       | Integer | - Khối lượng của Đơn Hàng. Ví dụ: `200`. <br>- Đơn vị tính `gram`. |
| value             | Không    | Integer | - Trị giá của Đơn Hàng. Ví dụ: `4200000`. <br>- Dùng để tính Phí Bảo Hiểm. <br>- Đơn vị tính `đồng`. |

##### Ví Dụ

```bash
curl --request GET \
  --url 'https://api.mysupership.vn/v1/partner/orders/price?sender_province=Hồ Chí Minh&sender_district=Bình Chánh&receiver_province=Hà Nội&receiver_district=Tây Hồ&weight=200&value=12000000' \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <Access-Token>'
```

#### Response

##### Tham Số Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| service    | String  | - Tên Gói Dịch Vụ. Ví dụ: `Tốc Hành`. <br>Các giá trị có thể: <br>- `Tốc Hành`. |
| fee        | Integer | Cước Phí Giao Hàng. Ví dụ: `20000`.                  |
| insurance  | Integer | Cước Phí Bảo Hiểm. Ví dụ: `42000`.                   |
| pickup     | Object  | Thời gian dự kiến Lấy Hàng.                         |
| delivery   | Object  | Thời gian dự kiến Giao Hàng.                        |

##### Ví Dụ

```json
{
    "status": "Success",
    "results": [
        {
            "service": "Tốc Hành",
            "fee": 35000,
            "insurance": 120000,
            "pickup": {
                "name": "Chiều nay - 03/07/2018"
            },
            "delivery": {
                "name": "Sáng mốt - 05/07/2018"
            }
        }
    ]
}
```

### Tạo Đơn Hàng

API này cho phép bạn tạo một đơn hàng mới.

#### Endpoint

`POST /v1/partner/orders/add`

#### Request

##### Headers

| Key           | Value              |
| :------------ | :----------------- |
| Accept        | application/json   |
| Authorization | Bearer `Access-Token` |
| Content-Type  | application/json   |

##### Các Tham Số

| Thuộc Tính      | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :-------------- | :------- | :------ | :------------------------------------------------- |
| pickup_code     | Không    | String  | Mã Kho hàng/Điểm Lấy Hàng của Người Gửi. Nếu mục này có giá trị khác rỗng thì sẽ ưu tiên lấy theo trường này. |
| pickup_phone    | Có       | String  | Số Điện Thoại của Điểm Lấy Hàng, sMan của SuperShip sẽ liên hệ số này khi lấy hàng, giao hàng, trả hàng. Ví dụ: `0989999999`. <br>Lưu ý: *_Chỉ bắt buộc nếu trường `pickup_code` rỗng/không điền/không có giá trị_*. **Các trường bắt đầu bằng chữ pickup như bên dưới thì tương tự**. |
| pickup_address  | Có       | String  | Địa chỉ của Điểm Lấy Hàng. Ví dụ: `45 Nguyễn Chí Thanh`. |
| pickup_province | Có       | String  | Tên Tỉnh/Thành Phố của Người Gửi. Ví dụ: `Thành phố Hà Nội`. |
| pickup_district | Có       | String  | Tên Quận/Huyện của Người Gửi. Ví dụ: `Quận Ba Đình`. |
| pickup_commune  | Có       | String  | Tên Phường/Xã của Người Gửi. Ví dụ: `Phường Ngọc Khánh`. |
| pickup_name     | Không    | String  | Tên Kho Hàng/Điểm Lấy Hàng. Ví dụ: `Kho Tân Bình`. |
| pickup_contact  | Không    | String  | Tên người liên hệ Lấy Hàng. Ví dụ: `Hoàng Mạnh Nam`. |
| name            | Có       | String  | Tên của Người Nhận. Ví dụ: `Trần Ngọc Nam`.         |
| phone           | Có       | String  | Số Điện Thoại của Người Nhận.                      |
| address         | Có       | String  | Địa Chỉ của Người Nhận. Ví dụ: `56 Trương Công Định`. |
| province        | Có       | String  | Tên Tỉnh/Thành Phố của Người Nhận. Ví dụ: `Thành phố Hồ Chí Minh`. |
| district        | Có       | String  | Tên Quận/Huyện của Người Nhận. Ví dụ: `Quận Tân Bình`. |
| commune         | Có       | String  | Tên Phường/Xã của Người Nhận. Ví dụ: `Phường 14`.    |
| amount          | Có       | Integer | Số tiền thu hộ. Ví dụ: `200000`. <br>Đơn vị tính `đồng`. |
| value           | Không    | Integer | Trị Giá của đơn hàng. Ví dụ: `4200000`. <br>**Dùng để tính Phí Bảo Hiểm.** <br>Đơn vị tính `đồng`. |
| weight          | Có       | Integer | Khối lượng của đơn hàng. Ví dụ: `200`. <br>Đơn vị tính `gram`. |
| soc             | Không    | String  | **Mã Đơn Riêng** của Người Gửi. Ví dụ: `KR-180703-034`. |
| note            | Không    | String  | Ghi chú thêm về Đơn Hàng của Người Gửi. Ví dụ: `Hàng dễ vỡ, lưu ý dùm shop`. |
| service         | Có       | String  | Mã Gói Dịch Vụ. Ví dụ: `1`. <br>Các giá trị có thể: <br>- Gói Tốc Hành: `1` |
| config          | Có       | String  | Người Nhận có được quyền xem/thử sản phẩm. Ví dụ: `1`. <br>Các giá trị có thể: <br>- Cho Xem Hàng Nhưng Không Cho Thử Hàng: `1` <br>- Cho Thử Hàng: `2` <br>- Không Cho Xem Hàng: `3` |
| payer           | Có       | String  | Người Trả Phí. Ví dụ: `1`. <br>Các giá trị có thể: <br>- Người Gửi: `1` <br>- Người Nhận: `2` |
| product_type    | Có       | String  | Cách truyển sản phẩm. Ví dụ: `1`. <br>Các giá trị có thể: <br>- Dạng Chuỗi: `1` <br>- Dạng Mảng: `2` |
| product         | Không    | String  | Bắt buộc khi giá trị của `product_type` là `1`. Ví dụ: `Quần áo`. |
| products        | Không    | Array   | Bắt buộc khi giá trị của `product_type` là `2`. <br>Các Tham Số: <br>- `sku`: Mã Sản Phẩm. Ví dụ: `P899234`. <br>- `name`: Tên Sản Phẩm. Ví dụ: `Áo khoác P4`. <br>- `price`: Giá Sản Phẩm. Ví dụ: `200000`. <br>- `weight`: Khối Lượng. Ví dụ: `200`. <br>- `quantity`: Số Lượng. Ví dụ: `1`. |
| barter          | Không    | String  | Tùy chọn Đổi/Lấy hàng về. Nếu có yêu cầu này, đơn hàng sẽ được hỗ trợ đổi hàng và trả hàng về. Ví dụ: `1`. |
| partner         | Không    | String  | Mã Bí Mật. Dành cho các Đối Tác Thương Mại Điện Tử lớn với SuperShip. |

##### Ví Dụ

```bash
curl --request POST \
  --url https://api.mysupership.vn/v1/partner/orders/add \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <Access-Token>' \
  --header 'Content-Type: application/json' \
  --data '{
	"pickup_phone": "0989999999",
	"pickup_address": "45 Nguyễn Chí Thanh",
	"pickup_commune": "Phường Ngọc Khánh",
	"pickup_district": "Quận Ba Đình",
	"pickup_province": "Thành phố Hà Nội",
	"name": "Trương Thế Ngọc",
	"phone": "0945900350",
	"email": null,
	"address": "35 Trương Định",
	"province": "Thành phố Hồ Chí Minh",
	"district": "Quận 3",
	"commune": "Phường 6",
	"amount": "220000",
	"value": null,
	"weight": "200",
	"payer": "1",
	"service": "1",
	"config": "1",
	"soc": "KAN7453535",
	"note": "Giao giờ hành chính",
	"product_type": "2",
    "products": [
    	{
    		"sku": "P899234",
    		"name": "Tên Sản Phẩm 1",
    		"price": 200000,
    		"weight": 200,
    		"quantity": 1
    	},
    	{
    		"sku": "P899789",
    		"name": "Tên Sản Phẩm 2",
    		"price": 250000,
    		"weight": 300,
    		"quantity": 2
    	}
    ]
}'
```

#### Response

##### Các Tham Số

| Thuộc Tính  | Kiểu DL | Mô Tả Chi Tiết                                     |
| :---------- | :------ | :------------------------------------------------- |
| code        | String  | Mã Đơn Hàng của SuperShip.                         |
| sorting     | String  | Mã Phân Loại của SuperShip.                        |
| shortcode   | String  | Mã Đơn Ngắn của SuperShip. <br>**Dùng Mã Này để hiển thị Mã Vạch, không dùng Mã Đơn Đầy Đủ ở trường `code`.** |
| soc         | String  | Mã Đơn Hàng của Người Gửi.                         |
| phone       | String  | Số Điện Thoại của Người Nhận.                      |
| address     | String  | Địa Chỉ của Người Nhận. Ví dụ: `47 Huỳnh Văn Bánh, Phường 5, Quận Phú Nhuận, Thành phố Hồ Chí Minh`. |
| amount      | Integer | Số Tiền cần Thu Hộ.                                |
| value       | Integer | Trị Giá của Đơn Hàng.                              |
| weight      | Integer | Khối Lượng của Đơn Hàng.                           |
| fee         | Integer | Cước Phí Giao Hàng.                                 |
| status      | String  | Mã Trạng Thái của Đơn Hàng.                        |
| status_name | String  | Tên Trạng Thái của Đơn Hàng.                       |

##### Ví Dụ

```json
{
    "status": "Success",
    "message": "",
    "results": {
        "code": "BPCS983262NM.810000026",
        "sorting": "LUC3-J5",
        "shortcode": "810000026",
        "soc": "PO8542245763",
        "phone": "0987654321",
        "amount": 160000,
        "collection": 160000,
        "value": 1600000,
        "weight": 200,
        "fee": 22000,
        "insurance": 8000,
        "status": "2",
        "status_name": "Chờ Lấy Hàng"
    }
}
```

### Trạng Thái Đơn Hàng

API này cho phép bạn lấy thông tin danh sách các trạng thái đơn hàng hiện có tại SuperShip.

#### Endpoint

`GET /v1/partner/orders/status`

#### Request

##### Ví Dụ

```bash
curl --request GET \
     --url 'https://api.mysupership.vn/v1/partner/orders/status'
```

#### Response

##### Các Tham Số

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết           |
| :--------- | :------ | :----------------------- |
| key        | String  | Mã Trạng Thái Đơn Hàng.  |
| value      | String  | Tên Trạng Thái Đơn Hàng. |

##### Ví Dụ

```json
{
    "status": "Success",
    "message": "Lấy Danh Sách Trạng Thái thành công.",
    "results": [
        { "key": "1", "value": "Chờ Duyệt" },
        { "key": "2", "value": "Chờ Lấy Hàng" },
        { "key": "3", "value": "Đang Lấy Hàng" },
        { "key": "4", "value": "Đã Lấy Hàng" },
        { "key": "5", "value": "Hoãn Lấy Hàng" },
        { "key": "6", "value": "Không Lấy Được" },
        { "key": "7", "value": "Đang Nhập Kho" },
        { "key": "8", "value": "Đã Nhập Kho" },
        { "key": "9", "value": "Đang Chuyển Kho Giao" },
        { "key": "10", "value": "Đã Chuyển Kho Giao" },
        { "key": "11", "value": "Đang Giao Hàng" },
        { "key": "12", "value": "Đã Giao Hàng Toàn Bộ" },
        { "key": "13", "value": "Đã Giao Hàng Một Phần" },
        { "key": "14", "value": "Hoãn Giao Hàng" },
        { "key": "15", "value": "Không Giao Được" },
        { "key": "16", "value": "Đã Đối Soát Giao Hàng" },
        { "key": "17", "value": "Đã Đối Soát Trả Hàng" },
        { "key": "18", "value": "Đang Chuyển Kho Trả" },
        { "key": "19", "value": "Đã Chuyển Kho Trả" },
        { "key": "20", "value": "Đang Trả Hàng" },
        { "key": "21", "value": "Đã Trả Hàng" },
        { "key": "22", "value": "Hoãn Trả Hàng" },
        { "key": "0", "value": "Huỷ" },
        { "key": "23", "value": "Đang Vận Chuyển" },
        { "key": "24", "value": "Xác Nhận Hoàn" },
        { "key": "25", "value": "Hàng Thất Lạc" },
        { "key": "26", "value": "Không Trả Được" },
        { "key": "27", "value": "Đã Bồi Hoàn" }
    ]
}
```

### Lấy Thông Tin Đơn Hàng

API này cho phép bạn lấy thông tin chi tiết của một Đơn Hàng.

#### Endpoint

`GET /v1/partner/orders/info`

#### Request

##### Headers

| Key           | Value              |
| :------------ | :----------------- |
| Accept        | application/json   |
| Authorization | Bearer `Access-Token` |

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| code       | Có       | String  | Mã Đơn Hàng. Ví dụ: `SGNS983262NT.595050186`.        |
| type       | Không    | String  | Loại của Mã Đơn Hàng. Giá trị mặc định: `1`. <br>Các giá trị có thể: <br>- Mã Đơn Hàng của SuperShip: `1` <br>- Mã Đơn Hàng của Người Gửi: `2`. |

##### Ví Dụ

```bash
curl --request GET \
     --url 'https://api.mysupership.vn/v1/partner/orders/info?code=SGNS983262NT.595050186&type=1' \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer <Access-Token>'
```

#### Response

##### Các Tham Số

| Thuộc Tính   | Kiểu DL | Mô Tả Chi Tiết                                     |
| :----------- | :------ | :------------------------------------------------- |
| code         | String  | Mã Đơn Hàng của SuperShip.                         |
| soc          | String  | Mã Đơn Hàng của Người Gửi.                         |
| status       | String  | Mã Trạng Thái của Đơn Hàng. Ví dụ: `12`.            |
| status_name  | String  | Tên Trạng Thái của Đơn Hàng. Ví dụ: `Đã Giao Hàng Toàn Bộ`. |
| receiver     | Object  | Thông Tin Người Nhận.                              |
| amount       | Integer | Số Tiền Thu Người Nhận.                             |
| value        | Integer | Trị Giá Thực Tế của Đơn Hàng.                      |
| weight       | Integer | Khối Lượng của Đơn Hàng.                           |
| fee          | Object  | Các loại Cước Phí của Đơn Hàng.                    |
| payer        | String  | Người Trả Phí.                                     |
| config       | String  | Cấu Hình xem/thử hàng.                             |
| journeys     | Object  | Thông tin hành trình của Đơn Hàng.                 |
| notes        | Object  | Các ghi chú của SuperShip.                         |
| created_at   | String  | Thời Gian Tạo của Đơn Hàng.                        |
| updated_at   | String  | Thời Gian Cập Nhật của Đơn Hàng.                   |

##### Ví Dụ

```json
{
    "status": "Success",
    "results": {
        "code": "SGNS983262NT.595050186",
        "soc": "2102040725580332",
        "status": "0",
        "status_name": "Huỷ",
        "receiver": {
            "name": "Chị Định (Đk)",
            "phone": "098****294",
            "address": "187/9 Mai Xuân Thưởng",
            "formatted_address": "187/9 Mai Xuân Thưởng, Phường 14, Quận 6, Thành phố Hồ Chí Minh"
        },
        "fee": {
            "shipment": 16000,
            "insurance": 0,
            "return": 0,
            "barter": 0,
            "address": 0
        },
        "payer": "Người Gửi",
        "amount": 418000,
        "value": 418000,
        "weight": 300,
        "config": "Không Cho Xem Hàng",
        "journeys": [
            {
                "time": "2021-02-04T07:25:19+07:00",
                "status": "Chờ Duyệt",
                "province": "Thành phố Hồ Chí Minh",
                "district": "Quận Tân Bình",
                "note": "Tạo Đơn hàng"
            },
            {
                "time": "2021-02-04T07:25:19+07:00",
                "status": "Chờ Lấy Hàng",
                "province": "Thành phố Hồ Chí Minh",
                "district": "Quận Tân Bình",
                "note": "Duyệt Đơn hàng"
            },
            {
                "time": "2021-02-04T08:41:17+07:00",
                "status": "Huỷ",
                "province": "Thành phố Hồ Chí Minh",
                "district": "Quận Tân Bình",
                "note": "Hủy Đơn hàng"
            }
        ],
        "notes": [
            {
                "created_at": "2021-02-04T07:25:19+07:00",
                "type": "5",
                "note": "Trường Phường/Xã có thể chưa được chính xác."
            }
        ],
        "calllogs": [],
        "last_sman": null,
        "created_at": "2021-02-04T07:25:19+07:00",
        "updated_at": "2021-02-04T08:41:17+07:00"
    }
}
```

### Tạo Token In Nhãn Giao Hàng

API này cho phép bạn lấy Token dành cho việc in Nhãn Giao Hàng.

#### Endpoint

`POST /v1/partner/orders/token`

#### Request

##### Headers

| Key           | Value              |
| :------------ | :----------------- |
| Accept        | application/json   |
| Authorization | Bearer `Access-Token` |
| Content-Type  | application/json   |

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                               |
| :--------- | :------- | :------ | :------------------------------------------- |
| code       | Có       | Array   | Mảng chứa danh sách Mã Đơn Hàng của SuperShip. |

##### Ví Dụ

```bash
curl --location --request POST 'https://api.mysupership.vn/v1/partner/orders/token' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <Access-Token>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "code": [
        "SGNS983262NT.593593647",
        "TGGS983262NM.613593645"
    ]
}'
```

#### Response

##### Các Tham Số

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| token      | String  | Token dành cho mục đích In Nhãn Đơn Hàng. <br>Ví dụ: `6cf2ce20-80e9-11eb-8974-cd483a610abd`. |

##### Ví Dụ

```json
{
    "status": "Success",
    "results": {
        "token": "6cf2ce20-80e9-11eb-8974-cd483a610abd"
    }
}
```

### In Đơn Hàng

API này cho phép bạn in Nhãn Giao Hàng của một hoặc nhiều Đơn Hàng trực tiếp trên trình duyệt.

#### Print URL

`GET https://khachhang.supership.vn/orders/awb`

#### Request

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| token      | Có       | String  | Token dành cho mục đích In Đơn Hàng. <br>Ví dụ: `6cf2ce20-80e9-11eb-8974-cd483a610abd`. |
| size       | Có       | String  | Kích thước Nhãn In Đơn Hàng. <br>Các giá trị có thể: <br>- Khổ giấy A5: `A5` <br>- Khổ giấy K46: `K46` <br>- Khổ giấy T2: `T2` <br>- Khổ giấy K50: `K50` <br>- Khổ giấy K75: `K75` <br>- Khổ giấy K80: `K80` <br>- Khổ giấy S8: `S8` <br>- Khổ giấy S9: `S9` <br>- Khổ giấy S10: `S10` <br>- Khổ giấy S11: `S11` <br>- Khổ giấy S12: `S12` <br>- Khổ giấy S13: `S13` <br>- Khổ giấy S14: `S14` |

##### Ví Dụ

```bash
curl --request GET \
     --url 'https://khachhang.supership.vn/orders/awb?token=6cf2ce20-80e9-11eb-8974-cd483a610abd&size=A5'
```

#### Response

##### Ví Dụ

![](https://docs.developers.supership.vn/supership-tem-giao-hang.png)

### Hủy Đơn Hàng

API này cho phép bạn hủy một Đơn Hàng.

#### Endpoint

`POST /v1/partner/orders/cancel`

#### Request

##### Headers

| Key           | Value              |
| :------------ | :----------------- |
| Accept        | application/json   |
| Authorization | Bearer `Access-Token` |
| Content-Type  | application/json   |

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                               |
| :--------- | :------- | :------ | :------------------------------------------- |
| code       | Có       | String  | Mã Đơn Hàng của SuperShip. Ví dụ: `SGNS983262NT.595050186`. |

##### Ví Dụ

```bash
curl --request POST \
     --url https://api.mysupership.vn/v1/partner/orders/cancel \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer <Access-Token>' \
     --header 'Content-Type: application/json' \
     --data '{
        "code": "SGNS983262NT.595050186"
     }'
```

#### Response

##### Các Tham Số

| Thuộc Tính  | Kiểu DL | Mô Tả Chi Tiết             |
| :---------- | :------ | :------------------------- |
| code        | String  | Mã Đơn Hàng của SuperShip. |
| soc         | String  | Mã Đơn Hàng của Người Gửi. |
| address     | String  | Địa Chỉ của Người Nhận.    |
| status      | String  | Mã Trạng Thái của Đơn Hàng. |
| status_name | String  | Tên Trạng Thái của Đơn Hàng.|

##### Ví Dụ

```json
{
    "status": "Success",
    "results": {
        "code": "SGNS983262NT.595050186",
        "soc": "2102040725580332",
        "address": "187/9 Mai Xuân Thưởng, Phường 14, Quận 6, Thành phố Hồ Chí Minh",
        "status": "0",
        "status_name": "Hủy"
    }
}
```

## Kho Hàng

Các API về kho hàng cho phép bạn tạo, xem, cập nhật và xóa một hay nhiều kho hàng.

### Lấy Thông Tin Kho Hàng

API này cho phép bạn lấy thông tin tất cả các kho hàng hiện có.

#### Endpoint

`GET /v1/partner/warehouses`

#### Request

##### Ví dụ

```bash
curl --request GET \
     --url https://api.mysupership.vn/v1/partner/warehouses \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer <Access-Token>'
```

#### Response

##### Kết Quả Trả Về

| Thuộc Tính        | Kiểu DL | Mô Tả Chi Tiết                                     |
| :---------------- | :------ | :------------------------------------------------- |
| code              | String  | Mã Kho Hàng. Ví dụ: `W1BZE00005`.                    |
| name              | String  | Tên Kho Hàng. Ví dụ: `Kho Bàu Cát`.                  |
| address           | String  | Địa Chỉ Kho Hàng. Ví dụ: `37 Bàu Cát 1`.             |
| formatted_address | String  | Địa Chỉ Đầy Đủ của Kho Hàng. Ví dụ: `37 Bàu Cát 1, Phường 14, Quận Tân Bình, Thành phố Hồ Chí Minh`. |
| status            | String  | Trạng Thái của Kho Hàng. Ví dụ: `1`. <br>Các giá trị có thể: <br>- Đang Hoạt Động: `1`. <br>- Dừng Hoạt Động: `2`. |
| status_name       | String  | Tên Trạng Thái của Kho Hàng.                       |
| primary           | String  | Cấu Hình Kho Mặc Định. Ví dụ: `2`. <br>Các giá trị có thể: <br>- Kho Mặc Định: `1`. <br>- Kho Thường: `2`. |
| primary_name      | String  | Tên Cấu Hình Kho Mặc Định.                         |
| created_at        | String  | - Thời Gian Tạo của Kho Hàng. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |
| updated_at        | String  | - Thời Gian Cập Nhật của Kho Hàng. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |

##### Ví dụ

```json
{
    "status": "Success",
    "results": [
        {
            "code": "WCQ1I07047",
            "name": "Kho Hoàng Nam",
            "address": "47 Huỳnh Văn Bánh",
            "formatted_address": "47 Huỳnh Văn Bánh, Phường 13, Quận Tân Bình, Thành phố Hồ Chí Minh",
            "status": "1",
            "status_name": "Hoạt Động",
            "primary": "2",
            "primary_name": "Kho Thường",
            "created_at": "2018-08-14T03:35:58+07:00",
            "updated_at": "2018-08-14T03:44:43+07:00"
        },
        {
            "code": "WLKGT07050",
            "name": "Kho Hbt",
            "address": "47 Lê Lợi",
            "formatted_address": "47 Lê Lợi, Phường Thanh Trì, Quận Hoàng Mai, Thành phố Hà Nội",
            "status": "1",
            "status_name": "Hoạt Động",
            "primary": "1",
            "primary_name": "Kho Mặc Định",
            "created_at": "2018-08-14T03:44:43+07:00",
            "updated_at": "2018-08-14T04:15:48+07:00"
        }
    ]
}
```

### Tạo Kho Hàng

API này cho phép bạn tạo một kho hàng mới.

#### Endpoint

`POST /v1/partner/warehouses/create`

#### Request

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| name       | Có       | String  | Tên Kho Hàng. Ví dụ: `Kho Bàu Cát`.                  |
| phone      | Có       | String  | Số Điện Thoại của Người Liên Lạc. Ví dụ: `0989999888`. |
| contact    | Có       | String  | Tên của Người Liên Lạc. Ví dụ: `Trần Ngọc Minh`.     |
| address    | Có       | String  | Địa Chỉ của Kho Hàng. Ví dụ: `56 Trương Công Định`.  |
| province   | Có       | String  | Tên Tỉnh/Thành Phố của Kho Hàng. Ví dụ: `Thành phố Hồ Chí Minh`. |
| district   | Có       | String  | Tên Quận/Huyện của Kho Hàng. Ví dụ: `Quận Tân Bình`. |
| commune    | Có       | String  | Tên Phường/Xã của Kho Hàng. Ví dụ: `Phường 14`.      |
| primary    | Có       | String  | Cấu Hình Kho Mặc Định. Ví dụ: `2`. <br>Các giá trị có thể: <br>- Kho Mặc Định: `1`. <br>- Kho Thường: `2`. |
| partner    | Không    | String  | Mã Bí Mật. Dành cho các Đối Tác Thương Mại Điện Tử lớn với SuperShip. |

##### Ví Dụ

```bash
curl --request POST \
     --url https://api.mysupership.vn/v1/partner/warehouses/create \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer <Access-Token>' \
     --header 'Content-Type: application/json' \
     --data '{
        "name": "Kho HM",
        "phone": "0989999888",
        "contact": "Trần Cao Cường",
        "address": "47 Lê Lợi",
        "province": "Thành phố Hồ Chí Minh",
        "district": "Quận Tân Bình",
        "commune": "Phường 14",
        "primary": "1"
    }'
```

#### Response

##### Kết Quả Trả Về

| Thuộc Tính        | Kiểu DL | Mô Tả Chi Tiết                                     |
| :---------------- | :------ | :------------------------------------------------- |
| code              | String  | Mã Kho Hàng. Ví dụ: `W1BZE00005`.                    |
| name              | String  | Tên Kho Hàng. Ví dụ: `Kho Bàu Cát`.                  |
| address           | String  | Địa Chỉ Kho Hàng. Ví dụ: `37 Bàu Cát 1`.             |
| formatted_address | String  | Địa Chỉ Đầy Đủ của Kho Hàng. Ví dụ: `37 Bàu Cát 1, Phường 14, Quận Tân Bình, Thành phố Hồ Chí Minh`. |
| status            | String  | Trạng Thái của Kho Hàng. Ví dụ: `1`. <br>Các giá trị có thể: <br>- Đang Hoạt Động: `1`. <br>- Dừng Hoạt Động: `2`. |
| status_name       | String  | Tên Trạng Thái của Kho Hàng.                       |
| primary           | String  | Cấu Hình Kho Mặc Định. Ví dụ: `2`. <br>Các giá trị có thể: <br>- Kho Mặc Định: `1`. <br>- Kho Thường: `2`. |
| primary_name      | String  | Tên Cấu Hình Kho Mặc Định.                         |
| created_at        | String  | - Thời Gian Tạo. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |
| updated_at        | String  | - Thời Gian Cập Nhật. Ví dụ: `2018-08-14T03:44:43+07:00`. <br>- Định dạng `ISO 8601` |

##### Ví dụ

```json
{
    "status": "Success",
    "results": {
        "code": "WLKGT07050",
        "name": "Kho HBT",
        "address": "47 Lê Lợi",
        "formatted_adress": "47 Lê Lợi, Phường Thanh Trì, Quận Hoàng Mai, Thành phố Hà Nội",
        "status": "1",
        "status_name": "Hoạt Động",
        "primary": "1",
        "primary_name": "Kho Mặc Định",
        "created_at": "2018-08-14T03:44:43+07:00",
        "updated_at": "2018-08-14T03:44:43+07:00"
    }
}
```

### Sửa Kho Hàng

API này cho phép bạn sửa một kho hàng hiện có.

> **LƯU Ý**
>
> Để thay đổi các giá trị về Địa Chỉ, Phường/Xã, Quận/Huyện, Tỉnh/Thành Phố thì hãy **Tạo Kho Hàng Mới**.

#### Endpoint

`POST /v1/partner/warehouses/update`

#### Request

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| code       | Có       | String  | Mã Kho Hàng. Ví dụ: `WLKGT07050`.                    |
| name       | Có       | String  | Tên Kho Hàng. Ví dụ: `Kho Bàu Cát`.                  |
| phone      | Có       | String  | Số Điện Thoại của Người Phụ Trách kho hàng. Ví dụ: `0989999888`. |
| contact    | Có       | String  | Tên của Người Liên Lạc. Ví dụ: `Trần Ngọc Minh`.     |

##### Ví Dụ

```bash
curl --request POST \
     --url https://api.mysupership.vn/v1/partner/warehouses/update \
     --header 'Accept: application/json' \
     --header 'Authorization: Bearer <Access-Token>' \
     --header 'Content-Type: application/json' \
     --data '{
        "code": "WLKGT07050",
        "name": "Kho Hbt",
        "phone": "0989999888",
        "contact": "Dương Mạnh Quân"
    }'
```

#### Response

##### Kết Quả Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| code       | String  | Mã Kho Hàng. Ví dụ: `WLKGT07050`.                    |
| diff       | Object  | Các trường xuất hiện trong Object này là giá trị được cập nhật thành công. Ví dụ: trường `name` đã được cập nhật từ `Trần Cao Cường` sang `Lê Nam Quân`. |
| updated_at | String  | - Thời Gian Cập Nhật. Ví dụ: `2018-08-14T15:03:59+07:00`. <br>- Định dạng `ISO 8601` |

##### Ví dụ

```json
{
    "status": "Success",
    "results": {
        "code": "WLKGT07050",
        "diff": {
            "name": "Kho Ba Đình",
            "contact": "Lê Nam Quân"
        },
        "updated_at": "2018-08-14T15:03:59+07:00"
    }
}
```

## Webhooks

Các API về webhook cho phép bạn tạo, xem, cập nhật và xóa một hay nhiều webhook.

### Cập Nhật Mẫu

Mỗi khi có sự thay đổi Trạng Thái Đơn Hàng ở Hệ Thống SuperShip, chúng tôi sẽ gửi một cập nhật sang hệ thống của Đối Tác thông qua Callback URL mà Đối Tác đã thiếp lập trước đó.

#### Request

##### Các Tham Số

| Thuộc Tính  | Kiểu DL | Mô Tả Chi Tiết                                     |
| :---------- | :------ | :------------------------------------------------- |
| type        | String  | Loại Cập Nhật. Ví dụ: `update_status`. <br>Các giá trị có thể: <br>- Trạng Thái: `update_status`. <br>- Khối Lượng: `update_weight`. |
| code        | String  | Mã Đơn Hàng của SuperShip. Ví dụ: `SGNS336484LM.883568271`. |
| shortcode   | String  | Mã Đơn Ngắn của SuperShip. Ví dụ: `883568271`.      |
| soc         | String  | Mã Đơn Hàng của Người Gửi. Ví dụ: `JLN-1805-1456`.   |
| phone       | String  | Số Điện Thoại của Người Nhận. Ví dụ: `01629091355`.  |
| address     | String  | Địa Chỉ của Người Nhận. Ví dụ: `47 Huỳnh Văn Bánh, Phường 5, Quận Phú Nhuận, Thành phố Hồ Chí Minh`. |
| amount      | Integer | Số Tiền cần Thu Hộ. Ví dụ: `450000`.                |
| weight      | Integer | Khối Lượng của Đơn Hàng. Ví dụ: `200`.              |
| fshipment   | Integer | Cước Phí Giao Hàng. Ví dụ: `25000`.                  |
| status      | String  | Mã Trạng Thái của Đơn Hàng. Ví dụ: `12`.            |
| status_name | String  | Tên Trạng Thái của Đơn Hàng. Ví dụ: `Đã Giao Hàng Toàn Bộ`. |
| partial     | String  | Có Phải Đơn Đã Giao Hàng Một Phần? Ví dụ: `1`.      |
| barter      | String  | Có Phải Đơn Có Hàng Đổi Trả? Ví dụ: `1`.             |
| reason_code | String  | Mã Lý Do. Ví dụ: `304`.                              |
| reason_text | String  | Chi Tiết Lý Do. Ví dụ: `Không liên lạc được với Người Nhận`. |
| created_at  | String  | - Thời Gian Tạo của Đơn Hàng. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |
| updated_at  | String  | - Thời Gian Cập Nhật của Đơn Hàng. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |
| pushed_at   | String  | - Thời Gian Đẩy Webhook. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |

##### Ví Dụ

```bash
curl --request POST \
  --url https://example.com/listen/supership \
  --header 'Content-Type: application/json' \
  --data '{
    "type": "update_status",
    "code": "SGNS336484LM.883568271",
    "shortcode": "883568271",
    "soc": "EG27533038-24",
    "name": "Anh Nam",
    "phone": "078882888",
    "address": "Đường Hữu Trí - Thị Trấn Tân Túc -Bình Chánh",
    "payer": "1",
    "amount": 230000,
    "weight": 800,
    "fshipment": 40000,
    "finsurance": 0,
    "status": "14",
    "status_name": "Hoãn Giao Hàng",
    "partial": "0",
    "barter": "0",
    "reason_code": "304",
    "reason_text": "Không liên lạc được với Người Nhận",
    "created_at": "2022-05-10T10:27:26+07:00",
    "updated_at": "2022-05-11T22:26:11+07:00",
    "pushed_at": "2022-05-11T22:26:11+07:00"
}'
```

#### Response

SuperShip dựa vào HTTP Response Status Code để xác định xem lần gửi cập nhật Đơn Hàng sang Hệ Thống Đối Tác. Nếu kết quả là 200 thì SuperShip xác định đó là cập nhật thành công.

### Lấy Thông Tin Webhook

API này cho phép bạn lấy thông tin tất cả các webhook hiện có.

#### Endpoint

`GET /v1/partner/webhooks`

#### Request

##### Ví Dụ

```bash
curl --request GET \
  --url https://api.mysupership.vn/v1/partner/webhooks \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <Access-Token>'
```

#### Response

##### Kết Quả Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| url        | String  | URL nhận cập nhật thông tin đơn hàng từ SuperShip. Ví dụ: `https://example.com/listen/supership`. |
| created_at | String  | - Thời Gian Tạo của Webhook. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |
| updated_at | String  | - Thời Gian Cập Nhật của Webhook. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |

##### Ví Dụ

```json
{
  "status": "Success",
  "results": {
    "url": "https://example.com/listen/supership",
    "created_at": "2017-09-29 01:52:09",
    "updated_at": "2017-09-29 02:08:28"
  }
}
```

### Tạo Webhook

API này cho phép bạn tạo một webhook mới.

> **BẠN CÓ BIẾT**
>
> Đối với những đơn hàng thuộc **Đối Tác Thương Mại Điện Tử, Đối Tác Phần Mềm Bán Hàng, Đối Tác Lớn** khác thì **Webhook Tự Động Áp Dụng** đối với đơn hàng đã điền **Mã Đối Tác** khi **Tạo Đơn Hàng** qua **Kênh API**. Vì thế, không cần sử dụng API này để thêm Webhook thủ công cho từng Người Dùng.

> **MẸO & LƯU Ý**
>
> -   Để thay đổi Webhook hiện tại, bạn chỉ cần dùng API này với giá trị mới.
> -   Các đơn hàng tạo sau thời điểm thay đổi Webhook thành công thì mới áp dụng Webhook mới.

#### Endpoint

`POST /v1/partner/webhooks/create`

#### Request

##### Các Tham Số

| Thuộc Tính | Bắt Buộc | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------- | :------ | :------------------------------------------------- |
| url        | Có       | String  | URL mà đối tác muốn nhận cập nhật thông tin đơn hàng từ SuperShip. Ví dụ: `https://example.com/listen/supership`. |

##### Ví Dụ

```bash
curl --request POST \
  --url https://api.mysupership.vn/v1/partner/webhooks/create \
  --header 'Accept: application/json' \
  --header 'Authorization: Bearer <Access-Token>' \
  --header 'Content-Type: application/json' \
  --data '{
	"url": "https://example.com/listen/supership"
}'
```

#### Response

##### Kết Quả Trả Về

| Thuộc Tính | Kiểu DL | Mô Tả Chi Tiết                                     |
| :--------- | :------ | :------------------------------------------------- |
| url        | String  | URL nhận cập nhật thông tin đơn hàng từ SuperShip. Ví dụ: `https://example.com/listen/supership`. |
| created_at | String  | - Thời Gian Tạo của Webhook. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |
| updated_at | String  | - Thời Gian Cập Nhật của Webhook. Ví dụ: `2018-07-03T17:18:29+07:00`. <br>- Định dạng `ISO 8601` |

##### Ví Dụ

```json
{
  "status": "Success",
  "results": {
    "url": "https://example.com/listen/supership",
    "created_at": "2017-09-29 01:52:09",
    "updated_at": "2017-10-13 00:09:18"
  }
}
```