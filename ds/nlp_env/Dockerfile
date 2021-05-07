#schnell3526/nlp_env


# ビルド環境
FROM python:3.6.13-buster as builder

ENV LANG=C.UTF-8
ARG JUMANPP_VERSION=2.0.0-rc3
ARG PYTHON_VERSION=3.6.6
ARG KNP_VERSION=4.20
ARG DEBIAN_FRONTEND=noninteractive

# ビルドツールのインストール
RUN apt update -qy && apt install -qy cmake

# jumanppのインストール
WORKDIR /tmp/src
RUN wget https://github.com/ku-nlp/jumanpp/releases/download/v${JUMANPP_VERSION}/jumanpp-${JUMANPP_VERSION}.tar.xz \
    && tar xvf jumanpp-${JUMANPP_VERSION}.tar.xz \
    && cd jumanpp-${JUMANPP_VERSION} \
    && mkdir build \
    && cd build
WORKDIR /tmp/src/jumanpp-${JUMANPP_VERSION}/build
RUN cmake .. -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local
RUN make
RUN make install

# knpのインストール
WORKDIR /tmp/src
RUN wget https://nlp.ist.i.kyoto-u.ac.jp/nl-resource/knp/knp-${KNP_VERSION}.tar.bz2 \
    && tar jxvf knp-${KNP_VERSION}.tar.bz2
WORKDIR /tmp/src/knp-${KNP_VERSION}
RUN ./configure
RUN make
RUN make install


# 実行環境
FROM schnell3526/ds_base

# jumanppのバイナリをコピー
COPY --from=builder /usr/local/bin/jumanpp /usr/local/bin/
COPY --from=builder /usr/local/libexec/jumanpp/* /usr/local/libexec/jumanpp/

# knpのバイナリをコピー
COPY --from=builder /usr/local/etc/jumanrc /usr/local/etc/
COPY --from=builder /usr/local/etc/knprc /usr/local/etc/
COPY --from=builder /usr/local/include/juman*.h /usr/local/include/
COPY --from=builder /usr/local/share/knp /usr/local/share/knp
COPY --from=builder /usr/local/share/juman /usr/local/share/juman/
COPY --from=builder /usr/local/lib/libjuman.* /usr/local/lib/
COPY --from=builder /usr/local/bin/knp /usr/local/bin/
COPY --from=builder /usr/local/libexec/knp /usr/local/libexec/knp
COPY --from=builder /usr/local/libexec/juman /usr/local/libexec/juman

# pyknpをインストール
RUN pip3 install pyknp \
    && rm -rf /root/.cache/pip