Tác giả đưa link dẫn tới 1 repo trên trang docker: `https://hub.docker.com/r/vinhph2/hcmus-ctf-2021`.

Làm theo hướng dẫn trên Stackoverflow (tại link này)[https://stackoverflow.com/questions/19104847/how-to-generate-a-dockerfile-from-an-image] để generate `Dockerfile` từ link repo và lấy được flag.

```bash
alias dfimage="sudo docker run -v /var/run/docker.sock:/var/run/docker.sock --rm alpine/dfimage"
dfimage -sV=1.36 vinhph2/hcmus-ctf-2021
```

Flag: `HCMUS-CTF{d0ck6r_1mag6_1nsp6ct}`