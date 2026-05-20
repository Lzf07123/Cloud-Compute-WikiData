FROM bffwjdfjzq3wikw5yl.xuanyuan.run/library/node:latest

RUN npm install -g hexo-cli && cd /opt/ && git clone https://github.com/Lzf07123/Cloud-Compute-WikiData.git
CMD ["cd", "/opt/Cloud-Compute-WikiData"]
CMD ["hexo", "server"]
CMD ["sleep", "3000"]
