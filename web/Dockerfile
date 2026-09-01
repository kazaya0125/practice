FROM node:18 AS base
CMD [ "bash" ]

# Build
FROM base AS test-and-build
COPY . /workspace
WORKDIR /workspace
ARG REACT_APP_API_SERVER
RUN npm ci && npm run test && npm run build

# Hosting in nginx
FROM nginx:latest AS production
COPY --from=test-and-build /workspace/build /var/www
COPY nginx.conf /etc/nginx/conf.d/default.conf


# node:18
# Node.js 18を使用
# baseというステージ名を付ける
# → ReactのWebアプリを開発・ビルドするための環境

# CMD ["bash"]
# コンテナ起動時のデフォルトコマンドとしてbashを指定


# test-and-build
# baseステージをベースにして、test-and-buildというステージを作成

# COPY . /workspace
# Dockerのビルドコンテキストにある資材を
# コンテナ内の/workspaceへコピー

# WORKDIR /workspace
# 以降の処理を/workspaceで実行

# ARG REACT_APP_API_SERVER
# Dockerイメージのビルド時に使用する変数を定義

# RUN npm ci && npm run test && npm run build
# npmを使用して、
# ① npm ci → 必要なNode.jsパッケージを準備
# ② npm run test → テストを実行
# ③ npm run build → ReactのWebアプリをビルド
# → ビルド成果物として/workspace/buildが作成される


# nginx:latest
# nginxのイメージを使用
# productionというステージ名を付ける
# → 完成したWebアプリを配信するための環境


# COPY --from=test-and-build /workspace/build /var/www
# test-and-buildステージで作成された
# /workspace/buildをproductionステージの/var/wwwへコピー
# → ビルド環境のNode.jsなどは持っていかず、
#   完成したWebアプリの成果物だけをnginxへ渡す


# COPY nginx.conf /etc/nginx/conf.d/default.conf
# ビルドコンテキストにあるnginx.confを
# nginxの設定ディレクトリへdefault.confという名前でコピー
# → nginxの動作設定を変更する