FROM ubuntu:22.04
RUN apt-get update && apt-get install -y python3.10 python3-pip \
    && pip3 install numpy \
    && rm -rf /var/lib/apt/lists/*