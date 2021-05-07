# schnell3526/ds_base

# ビルド環境
FROM python:3.6.13-buster as build-stage
COPY requirements.txt /root/
RUN pip install -r /root/requirements.txt

# 実行環境
FROM python:3.6.13-slim-buster
## ビルド用イメージ内でPythonモジュールをビルドした際のキャッシュを利用する
COPY --from=build-stage /root/.cache/pip /root/.cache/pip
COPY --from=build-stage /root/requirements.txt /root

### パッケージのアップデートとPythonモジュールのインストール
RUN pip3 install -r /root/requirements.txt \
    && pip uninstall -y jedi \
    && rm -rf /root/.cache/pip
COPY .bashrc /root/