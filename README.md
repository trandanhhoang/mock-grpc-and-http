# Mock http and grpc server

# What is it ?
- Khi làm việc ở local, thay vì gọi vô các service ở môi trường qc, bạn nên mock value để dễ dàng test local.
- Đây là một cách để giúp bạn mock các service http và grpc một cách dễ dàng.

# Thành quả
```bash
curl -L -X POST http://localhost:28080/pe-bank-wrapper/send-bank-request-v2
{
  "bc_trans_id": 240926000010431,
  "return_code": 6,
  "return_message": "Giao dịch đang xử lý."
}                  
```

# How to use
- Config, run DockerCompose
  - $ docker-compose up -d (I will use dcud for short, you can try to use alias too)
  - eg:
```json
{
  "request": {
    "method": "POST",
    "urlPath": "/pe-bank-wrapper/send-bank-request-v2"
  },
  "response": {
    "status": 200,
    "bodyFileName": "bank-wrapper-send-bank-request.json",
    "transformers": [
      "response-template"
    ]
  }
}
```
- Cái này nghĩa là map lời gọi Post tới /pe-bank-wrapper/send-bank-request-v2 sẽ trả về data trong file bank-wrapper-send-bank-request.json
- Lúc này gọi curl to test:
  - $ curl -X POST http://localhost:28080/pe-bank-wrapper/send-bank-request-v2
  - Kết quả sẽ trả về nội dung trong file bank-wrapper-send-bank-request.json

- Lúc này http sẽ chạy ở port 28080, còn grpc chạy ở port 29090, bạn có thể tự tuỳ chỉnh lại nếu muốn

# How to config
- Config json at folder (for both **http** and **grpc**) 
  - **/mappings: for mapping from request to file that contains response
  - **/files: for hold response as json value
- For grpc: 
  - **/protos: need add file.proto to run, for sure.
- First time run:
  - $ dcud
- For GRPC, 
  - $ dcud --build
- For Http:
  - $ dcd
  - $ dcud