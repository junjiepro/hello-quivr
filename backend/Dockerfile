FROM python:3.11-bullseye

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

WORKDIR /code

COPY ./requirements.txt /code/requirements.txt

RUN pip install --no-cache-dir -r /code/requirements.txt --timeout 100

#You may need to run `chmod +x ./backend/scripts/start.sh` on your host machine if you get a permission error
COPY ./scripts/start.sh /code/scripts/start.sh
RUN chmod +x /code/scripts/start.sh

COPY . /code/

ENTRYPOINT ["/code/scripts/start.sh"]
