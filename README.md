Docker image 作成用。

Ubuntu 上に以下をセットアップする。

- PostgreSQL
- OpenCV
- Caffe
- Stack
- Node.js
- Elm


## build

イメージ名を `dev-base` としてビルドする場合。

```
$ docker build dev-base
```

## プライベート拡張

`dev-base` をベースにしてプライベートに拡張するとよい。

以下は、`zsh` を導入して `PostgreSQL` のセットアップをする例。

```
FROM dev-base:latest
MAINTAINER somebody "somebody@example.com"


RUN apt-get install -y zsh

USER postgres
RUN service postgresql start && \
    psql --command "DO SOMETHING ON POSTGRES" && \
    psql --command "DO YET ANOTHER THING" && \
    createdb -O postgres nicedatabase

USER root

CMD ["zsh"]
```

これをイメージ名 `dev-hoge` としてビルドする。

```
$ docker build dev-hoge
```


## 使う

カレントディレクトリを `/root` にマウントし、ポート `3000` をフォワーディングする例。

```
$ docker run -it -v $(pwd):/root -p 3000:3000 dev-hoge
```


## Tips

この例だと、起動時にはまっさらな `zsh` が立ち上がる。

適宜 `.zhsrc` などをコンテナ内のホームに置いておくと便利。

起動時に走らせたいスクリプトなども合わせて書いておくとよいかもしれない。

`Dockerfile`
```
ENV HOME=/root
WORKDIR $HOME

ADD .zshrc $HOME/.zshrc
```

`.zshrc`
```
bindkey -e

HISTFILE=~/.histfile
HISTSIZE=10000
SAVEHIST=10000

export PATH="$HOME/.local/bin:$PATH"
export SOMETHING_PATH=/path/to/something

service postgresql start
```

このようにして埋め込んだ `.zshrc` は、イメージビルド時に固定される。

変更をイメージに反映させるには、イメージを再ビルドする。
