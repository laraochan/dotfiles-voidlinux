# larao's dotfiles for Void Linux

```sh
# NOTE: xbps-dump is custom script
xbps-dump > package.txt
```

```sh
xargs sudo xbps-install -S < package.txt
```

## 新規 PC でのセットアップ

```sh
# 1. パッケージ導入
xargs sudo xbps-install -S < package.txt

# 2. dotfiles を chezmoi で適用
git clone git@github.com:laraochan/dotfiles-voidlinux.git ~/.local/share/chezmoi
chezmoi apply

# 3. システム設定を適用 (root)
sudo cp system/etc/greetd/config.toml /etc/greetd/config.toml
sudo cp system/etc/acpi/handler.sh /etc/acpi/handler.sh
sudo install -Dm755 system/usr/local/bin/start-river /usr/local/bin/start-river

# 4. システムサービスを有効化 (root)
for s in NetworkManager acpid dbus greetd seatd tlp turnstiled; do
    sudo ln -s /etc/sv/$s /var/service/
done

# 5. git hooks (lefthook) を有効化
mise trust && mise install && lefthook install
```

`system/` は root 権限を要するため chezmoi の管理対象外 (`.chezmoiignore`)。
git で追跡しつつ手動で `/etc/`, `/usr/local/bin/` へコピーする運用です。

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
設定は `lefthook.yml`、ツールは `mise.toml` で管理しています。