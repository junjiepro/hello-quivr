# hello-quivr

国内搭建 quivr 会遇到一些资源访问的问题，这里记录一下解决方案。

## backend

```python
# backend/main.py
...
@app.on_event("startup")
async def startup_event():
    # - pypandoc.download_pandoc()
    # + 使用 https://github.moeyy.xyz/ 加速
    pypandoc.download_pandoc("https://github.moeyy.xyz/https://github.com/jgm/pandoc/releases/download/3.1.5/pandoc-3.1.5-1-amd64.deb")
...
```

```dockerfile
# backend/Dockerfile
...
# + debian中apt-get源替换为阿里源
# https://developer.aliyun.com/mirror/debian
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak
RUN echo " " > /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/debian-archive/debian/ stretch main non-free contrib" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/debian-archive/debian/ stretch main non-free contrib" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/debian-archive/debian-security stretch/updates main" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/debian-archive/debian-security stretch/updates main" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.aliyun.com/debian-archive/debian/ stretch-backports main non-free contrib" >> /etc/apt/sources.list
RUN echo "deb-src http://mirrors.aliyun.com/debian-archive/debian/ stretch-backports main non-free contrib" >> /etc/apt/sources.list
# Install GEOS library
RUN apt-get update && apt-get install -y libgeos-dev
...
```

```bash
# backend/scripts/start.sh
...
    # + 在 docker 里 git clone，容易失败，因而另外clone gpt4all项目，并映射
    cd /tmp/gpt4all/gpt4all-backend/
    # cd /tmp && git clone --recurse-submodules https://github.com/nomic-ai/gpt4all
    mkdir build && cd build
...
```

## docker compose

```yml
# docker-compose.yml
...
    volumes:
      - ./backend/:/code/
      - ~/.config/gcloud:/root/.config/gcloud
      - ./backend/scripts/start.sh:/code/scripts/start.sh
      # + 映射gpt4all项目到容器
      - /root/tmp/:/tmp/
      # + 映射模型
      - /root/local_models/:/local_models/
    ports:
      - 5050:5050
...
```
