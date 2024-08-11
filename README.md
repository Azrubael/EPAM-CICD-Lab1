_2024-08-11  18:44_

# To check if the container works well separately
```bash
docker build -t azw_app . && docker run --rm -d --name azw_container -p 3000:3000 azw_app
```
