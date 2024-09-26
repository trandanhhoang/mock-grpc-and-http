# Mock http and grpc server

# Mục đích
- Khi làm việc ở local, thay vì gọi vô các service ở môi trường qc, bạn nên mock value để dễ dàng test local.
- Đây là một cách để giúp bạn mock các service http và grpc một cách dễ dàng.

# Ví dụ kết quả.
```bash
curl -L -X POST http://localhost:28080/pe-bank-wrapper/send-bank-request-v2
{
  "bc_trans_id": 240926000010431,
  "return_code": 6,
  "return_message": "Giao dịch đang xử lý."
}                  
```

# Ghi chú
- Lúc này http sẽ chạy ở port 28080, còn grpc chạy ở port 29090, bạn có thể tự tuỳ chỉnh lại nếu muốn

# How to use
## HTTP
- Bạn chỉ cần config mapping từ api tới file response.json là xong
- Bước 1:
  - **/mappings: define với path "/pe-bank-wrapper/send-bank-request-v2" sẽ trả về file "bank-wrapper-send-bank-request.json"
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
- Bước 2:
  - **/files: chứa file response
```json
{
  "bc_trans_id": 240926000010431,
  "return_code": 6,
  "return_message": "Giao dịch đang xử lý."
}
```

- Bước 3: reload http service
  - $ docker-compose down
  - $ docker-compose up -d

- Cái này nghĩa là map lời gọi Post tới /pe-bank-wrapper/send-bank-request-v2 sẽ trả về data trong file bank-wrapper-send-bank-request.json
- Lúc này gọi curl to test:
  - $ curl -X POST http://localhost:28080/pe-bank-wrapper/send-bank-request-v2
  - Kết quả sẽ trả về nội dung trong file bank-wrapper-send-bank-request.json

## GRPC
- Giống HTTP ở bước 1 và bước 2
- Bước 3: config file proto vào **/protos.
- Bước 4: reload grpc service
  - $ docker-compose up -d -- build

# Mẹo alias
```bash
cat ~/.zshrc
export PATH=/usr/local/bin:$PATH:/opt/homebrew/bin/

# alias
alias dcud="docker-compose up -d"
alias dcudf="docker-compose up -d --force-recreate"
alias dcd="docker-compose down"
alias dcdv="docker-compose down -v"
alias dcdall="docker-compose down -v --rmi all"

export GOPATH=$HOME/go
```

- Lúc này thì dùng dcd vs hay dcud -- build là xong ùi.