ARG ZEROTIER_VERSION
FROM zyclonite/zerotier:${ZEROTIER_VERSION}

ARG ZEROTIER_VERSION
ARG CHINA_SOURCE

COPY ./config/identity* /var/lib/zerotier-one/

## Change source for china
RUN if [ "${CHINA_SOURCE}" = true ]; then \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories; fi

## Auto detect public ipv4 and generate planet file
RUN apk add --no-cache curl g++ && \
    ip=`curl https://ip.3322.net` && addr=${ip}/9993 && identity=`cat /var/lib/zerotier-one/identity.public` && \
    echo "**************************************************************************" && \
    echo "addr: ${addr}" && echo "identity: ${identity}" && \
    curl https://codeload.github.com/zerotier/ZeroTierOne/tar.gz/refs/tags/${ZEROTIER_VERSION} --output /tmp/ZeroTierOne.tar.gz && \
    cd /tmp && tar fxz /tmp/ZeroTierOne.tar.gz && \
    mv /tmp/ZeroTierOne-* /tmp/ZeroTierOne && \
    cd /tmp/ZeroTierOne/attic/world/ && \
    sed -i '/roots.push_back/d' ./mkworld.cpp && sed -i '/roots.back()/d' ./mkworld.cpp && \
    sed -i '85i roots.push_back(World::Root());' ./mkworld.cpp && \
    sed -i '86i roots.back().identity = Identity(\"'"$identity"'\");' ./mkworld.cpp && \
    sed -i '87i roots.back().stableEndpoints.push_back(InetAddress(\"'"$addr"'\"));' ./mkworld.cpp && \
    sed -i 's/c++ /c++ -DOMIT_JSON_SUPPORT /g' build.sh && \
    sh ./build.sh && ./mkworld && mv ./world.bin /var/lib/zerotier-one/planet && \
    apk del curl g++ && rm -rf /var/cache/apk/* && rm -rf /root/.cache && rm -rf /tmp/*

RUN echo "{\"settings\": {\"portMappingEnabled\": true,\"softwareUpdate\": \"disable\",\"allowManagementFrom\": [\"0.0.0.0/0\"]}}" > /var/lib/zerotier-one/local.conf

EXPOSE 9993/tcp
EXPOSE 9993/udp

ENTRYPOINT ["zerotier-one"]
