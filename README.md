# Tích hợp Grafana OnCall OSS với Slack

## Triển khai Grafana OnCall OSS tại Local

1. Triển khai Docker

```bash
docker-compose up --build
```

2. Đăng nhập sử dụng admin/admin

![](images/0.png)

3. Trang chủ với các hướng dẫn thêm Nguồn Dữ Liệu/Bảng Điều Khiển

![](images/1.png)

## Mở Grafana OnCall

![](images/13.png)

## Mở port ra ngoài public mạng

1. Cài đặt zrok

https://docs.zrok.io/docs/guides/install/

![](images/3.png)

2. Đăng nhập zrok lấy token để xác thực

https://api-v1.zrok.io

![](images/4.png)

![](images/5.png)

```bash
zrok.exe enable <token>
```

![https://3o7ygs7sgg6f.share.zrok.io](images/6.png)

3. Vì port của OnCall Engine là 8080 nên ta sẽ chạy lệnh sau

![](images/3.png)

```bash
zrok.exe share public 8080
```

## Cài đặt Slack WebHook

1. Tạo Workspace

https://slack.com/create

![](images/7.png)

2. Tạo Slack App

https://api.slack.com/apps

![](images/8.png)

Chọn From a manifest và chọn workspace vừa tạo để điền nội dung từ manifest.yml vô.

Nhớ đổi **<ONCALL_ENGINE_PUBLIC_URL>** thành link zrok vừa tạo ở trên

![](images/9.png)

![](images/10.png)

![](images/11.png)

## Từ thông tin Slack App, nhập vào Grafana OnCall Setup ENV VARIABLES

1. Nhập thông tin

![](images/12.png)

```js
SLACK_CLIENT_OAUTH_ID = Basic Information -> App Credentials -> Client ID
SLACK_CLIENT_OAUTH_SECRET = Basic Information -> App Credentials -> Client Secret
SLACK_SIGNING_SECRET = Basic Information -> App Credentials -> Signing Secret
SLACK_INSTALL_RETURN_REDIRECT_HOST = << OnCall external URL >>
```

![](images/14.png)

![](images/15.png)

![](images/15a.png)

![](images/15b.png)

![](images/15c.png)

2. Cài đặt thông báo qua Slack cho tài khoản

![](images/18.png)

3. Test nhận thông báo qua Slack

![](images/21.png)

![](images/22.png)


## Alert Manager

1. Cài đặt Link URL Integration

![](images/16.png)

![](images/17.png)

Nhập URL vào file **./alertmanager/alertmanager.yml**

2. Cài đặt Prometheus Alert Manager

```bash
docker-compose -f docker-prometheus-compose.yml up --build
```

![](images/19.png)

3. Gắn link Integration qua bên Prometheus Alert Manager

![](images/23.png)

### Kết quả cuối cùng

![](images/26.png)

![](images/25.png)

