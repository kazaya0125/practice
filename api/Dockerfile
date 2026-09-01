FROM gradle:7 AS base
CMD ["bash"]

# test-and-build
FROM base AS test-and-build
COPY . /workspace
WORKDIR /workspace
RUN ./gradlew build

# production
FROM eclipse-temurin:17-jre AS production
COPY --from=test-and-build /workspace/build/libs/api-0.0.1-SNAPSHOT.jar ./api.jar
CMD ["java", "-jar", "api.jar"]


#gradleというビルドツールを使用する　バージョンは７　もしなかったらインストールされる　baseという処理名に命名
#基本コマンド　bash

#baseという処理をベースとして使用　test-and-buildという処理名に命名
#ローカル環境側：現在に格納されている資材をコンテナ側：/workspaceへコピー
#コンテナ側：workspaceディレクトリへ移動
#現在ディレクトリのgradlewをbulidする

#17-ireを使用する　もしなかったらインストールする　productionという処理名に命名
#test-and-buildという処理でbuildされた「/workspace/build/libs/api-0.0.1-SNAPSHOT.jar」を現在ディレクトリ/api.jarにリネームしてコピー
#上の説明　
#本処理ではビルドするためのgradleやコンパイルツールjdk本体はいらない　コンパイルされた後の成果物だけあればよい　その成果物だけをコピー
#別ステージでbuildされたものは　COPY --from 処理名 成果物で指定する
#基本コマンド　java,jar,api.jar