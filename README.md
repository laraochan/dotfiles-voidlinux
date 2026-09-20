# larao's dotfiles for Void Linux

```sh
# NOTE: xbps-dump is custom script
xbps-dump > package.txt
```

```sh
xargs sudo xbps-install -S < package.txt
```

## 独自ビルドのパッケージ

package.txt には公式 Void リポジトリに無いものも含まれています
（例: `opencode`, `swaylock-effects`）。これらは
[laraochan/void-packages](https://github.com/laraochan/void-packages) の
xbps-src でビルドしたものを導入しています。依存パッケージ一覧は都度変わります。

```sh
git clone https://github.com/laraochan/void-packages
cd void-packages
./xbps-src pkg <package-name>
sudo xbps-install --repository="$PWD/hostdir/binpkgs" <package-name>
```

## Git hooks (lefthook)

pre-commit で package.txt の同期・whitespace check・shellcheck を実行します。

```sh
mise trust      # mise.toml を信頼済みにする
mise install    # lefthook / shellcheck を導入 (mise.toml)
lefthook install  # git hooks を有効化 (.git/hooks)
```