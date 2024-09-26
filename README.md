# Mock http and grpc server

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