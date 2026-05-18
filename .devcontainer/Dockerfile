# セキュアで最小限のBun開発環境
# 非rootユーザー / 権限制限 / プロジェクトディレクトリのみ
FROM oven/bun:1.3

# 必須ツールのみ（git, jq, curl, nano）
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    jq \
    curl \
    ca-certificates \
    nano  \
    bubblewrap \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

# 非rootユーザーを作成 (uid=1000 でホストユーザーと一致させる)
RUN if getent passwd 1000 > /dev/null 2>&1; then \
      usermod -l vscode -d /home/vscode -m $(getent passwd 1000 | cut -d: -f1); \
    else \
      useradd --uid 1000 -ms /bin/bash vscode; \
    fi
USER vscode
WORKDIR /nano-code

# 基本環境変数（nanoはテキストエディタ、プロジェクト名とは無関係）
ENV EDITOR=nano
ENV DEVCONTAINER=true
ENV PATH="/home/vscode/.bun/bin:$PATH"

# Bunのキャッシュをユーザーごとに分離
ENV BUN_INSTALL_CACHE=/home/vscode/.bun-cache
RUN mkdir -p $BUN_INSTALL_CACHE

CMD ["bash"]