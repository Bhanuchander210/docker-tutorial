FROM ubuntu:16.04
LABEL Creator: "Bhanuchander"
RUN apt-get update && apt-get install -y curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /home/Examples
ENV USER bhanuchander 
ENV SHELL /bin/bash
ENV LOGNAME bhanuchander
CMD ["bash"]
