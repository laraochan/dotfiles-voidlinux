# larao's dotfiles for Void Linux

```sh
# NOTE: xbps-dump is custom script
xbps-dump > package.txt
```

```sh
xargs sudo xbps-install -S < package.txt
```

## Git hooks (lefthook)

pre-commit で package.txt の同期・whitespace check・shellcheck を実行します。

```sh
mise trust      # mise.toml を信頼済みにする
mise install    # lefthook / shellcheck を導入 (mise.toml)
lefthook install  # git hooks を有効化 (.git/hooks)
```