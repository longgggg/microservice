# Kong Admin API

Kong đi kèm các RESTful Admin API bên trong để quản lý. Yêu cầu Admin API có thể được gửi tới bất kỳ node nào trong cluster, và Kong sẽ giữ cấu hình phù hợp trên tất cả các node.

`8001:` là cổng mặc định mà Admin API lắng nghe.

`8444:` là cổng mặc định cho truy cập HTTPS đến Admin API.

API này được thiết kế để sử dụng nội bộ bên trong và cung cấp toàn quyền kiểm soát Kong, vì thế cẩn thận khi thiết lập các môi trường Kong để tránh tiết lộ các API này. Xem [tài liệu](https://getkong.org/docs/0.12.x/secure-admin-api/) này để biêt các cách bảo mật API.

## Kiểu nội dung được hỗ trợ

The Admin API chấp nhận 2 loại nội dung API trên mỗi endpoint:
- application/x-www-form-urlencoded

Đủ cho các yêu cầu cơ bản, có thể hầu như bạn sẽ sử dụng nó. Lưu ý rằng khi sử dụng các giá trị lồng nhau, Kong mong muốn các đối tượng được tham chiếu với dấu chấm.

    config.limit=10&config.period=seconds

- aplication / json

Dễ dàng với những cấu trúc phức tạp, chỉ cần gửi một đại diện JSON của dữ liệu bạn muốn gửi.

    {
        "config": {
            "limit": 10,
            "period": "seconds"
        }
    }

-----

# Các luồng thông tin

## Truy vấn thông tin node

Truy vấn các nội dung chi tiết về một node

### Endpoint:    

    /

### Response

    HTTP 200 OK
*

    {
    "hostname": "",
    "node_id": "6a72192c-a3a1-4c8d-95c6-efabae9fb969",
    "lua_version": "LuaJIT 2.1.0-alpha",
    "plugins": {
        "available_on_server": [
            ...
        ],
        "enabled_in_cluster": [
            ...
        ]
    },
    "configuration" : {
        ...
    },
    "tagline": "Welcome to Kong",
    "version": "0.11.0" 
    }

* `node_id`: Một UUID đại diện cho node Kong đang chạy. UUID này được generated ngẫu nhiên khi Kong khởi động, nên các node sẽ có `node_id` khác nhau khi khởi động lại. 
* `available_on_server`: tên của các plugin đã được cài đặt trên node,
* `enabled_in_cluster`: tên của plugin được cấu hình. Tức là, các cấu hình plugin hiện tại trong kho dữ liệu được chia sẻ bởi tất cả các node Kong.

*****

# Retrieve node status

