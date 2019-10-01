FROM store/oracle/serverjre:8

LABEL Creator: "Bhanuchander"

WORKDIR /usr

RUN mkdir -p CygNet

WORKDIR /usr/CygNet

COPY . .

RUN ln -sf /usr/java/jdk1.8.0_211/ jre

WORKDIR /usr/CygNet/bin

CMD sh start.sh && exec /bin/sh -c "trap : TERM INT; (while true; do sleep 1000; done) & wait"

