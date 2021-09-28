FROM ubuntu:14.04

RUN apt-get update -y && \
apt-get install software-properties-common -y && \
apt-get install wget

ENV BITCOIN_VER="0.17.0.1"
ENV BITCOIN_FOLDER_VER="0.17.0"
ENV BITCOIN_SHASUM="6ccc675ee91522eee5785457e922d8a155e4eb7d5524bd130eb0ef0f0c4a6008"
WORKDIR /tmp

RUN wget -O bitcoin-${BITCOIN_VER}-x86_64-linux-gnu.tar.gz https://bitcoin.org/bin/bitcoin-core-${BITCOIN_VER}/bitcoin-${BITCOIN_VER}-x86_64-linux-gnu.tar.gz

RUN CURRENTHASH=$(sha256sum "bitcoin-${BITCOIN_VER}-x86_64-linux-gnu.tar.gz" | cut -d' ' -f1); \
if [ "${CURRENTHASH}" = "${BITCOIN_SHASUM}" ] ; \
then echo "checksum ok"; \
else echo "checksum failed for bitcoin-${BITCOIN_VER}-x86_64-linux-gnu.tar.gz"; exit 255 ; \
fi
RUN tar -xvzf bitcoin-${BITCOIN_VER}-x86_64-linux-gnu.tar.gz
RUN install -C -m 755 -o root -g root --backup=off bitcoin-${BITCOIN_FOLDER_VER}/bin/* /usr/bin/
RUN rm bitcoin-${BITCOIN_VER}-x86_64-linux-gnu.tar.gz && rm -rf bitcoin-${BITCOIN_FOLDER_VER}


RUN mkdir -p /root/.bitcoin/

WORKDIR /root/.bitcoin/

EXPOSE 8332 8333 18332 18333

CMD ["/usr/bin/bitcoind", "-deprecatedrpc=accounts"]
