FROM golang:1.24-bookworm

LABEL maintainer="Ving <ving@nivin.cn>"

ENV GOPROXY https://goproxy.cn,direct

# 安装必要的软件包和依赖包
USER root
#RUN sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
#    sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
#    sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
RUN echo 'deb https://mirrors.aliyun.com/debian bookworm main contrib non-free' > /etc/apt/sources.list && \
    echo 'deb https://mirrors.aliyun.com/debian bookworm-updates main contrib non-free' >> /etc/apt/sources.list && \
    echo 'deb https://mirrors.aliyun.com/debian-security bookworm-security main contrib non-free' >> /etc/apt/sources.list && \
    > /etc/apt/sources.list.d/debian-security.sources && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    curl \
    zip \
    unzip \
    git \
    vim \
    screen

# 安装 goctl
USER root
ENV GOPROXY="https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,direct"
ENV GOSUMDB="sum.golang.google.cn"

#GOPROXY=https://goproxy.cn/,https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,direct
#RUN GOPROXY=https://goproxy.cn/,direct go install github.com/zeromicro/go-zero/tools/goctl@latest
RUN GOPROXY=https://goproxy.cn/,https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,direct go install github.com/zeromicro/go-zero/tools/goctl@latest
# 安装 protoc
USER root
COPY protoc-3.19.1-linux-x86_64.zip /tmp/protoc.zip
RUN unzip -d /tmp/protoc /tmp/protoc.zip && \
    mv /tmp/protoc/bin/protoc $GOPATH/bin

# 安装 protoc-gen-go
USER root
RUN GOPROXY=https://goproxy.cn/,https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,direct go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

# 安装 protoc-gen-go-grpc
USER root
RUN GOPROXY=https://goproxy.cn/,https://goproxy.cn,https://mirrors.aliyun.com/goproxy/,direct go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest

# $GOPATH/bin添加到环境变量中
ENV PATH $GOPATH/bin:$PATH

# 清理垃圾
USER root
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog

# 设置工作目录
WORKDIR /usr/src/code

EXPOSE 8000
EXPOSE 8001
EXPOSE 8002
EXPOSE 8003
EXPOSE 9000
EXPOSE 9001
EXPOSE 9002
EXPOSE 9003
