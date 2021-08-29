# 添加arch linux 源
```bash
1.
sudo vim /etc/pacman.conf

2.
[archlinuxcn]
#Server = http://mirrors.163.com/archlinux-cn/$arch
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

do this better

vim /etc/pacman.d/mirrorlist

Server=https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch

3.
sudo pacman -Syu

4.
sudo pacman -S archlinuxcn-keyring
```

# 安装字体
```bash
yay -S ttf-dejavu ttf-fira-code

Chinese
yay -S wqy-bitmapfont wqy-microhei wqy-microhei-lite wqy-zenhei adobe-source-han-mono-cn-fonts adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts

Emoji
yay -S ttf-linux-libertine ttf-inconsolata ttf-joypixels ttf-twemoji-color noto-fonts-emoji ttf-liberation ttf-droid
```

# 输入法

```bash
1.
yay -S fcitx5-im

2.
vim ~/.pam_environment

GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
SDL_IM_MODULE DEFAULT=fcitx

3.
yay -S fcitx5-rime rime-cloverpinyin base-devel fcitx5-pinyin-zhwiki-rime

4.
mkdir ~/.local/share/fcitx5/rime/

vim ~/.local/share/fcitx5/rime/default.custom.yaml

patch:
  "menu/page_size": 5
  schema_list:
    - schema: clover

5. 两个主题
[Gruvbox](https://link.zhihu.com/?target=https%3A//github.com/ayamir/fcitx5-gruvbox) 
[Nord](https://link.zhihu.com/?target=https%3A//github.com/ayamir/fcitx5-nord) 
```

# OMZ
```bash
1.
yay -S zsh

2.
chsh -s /usr/bin/zsh

3.
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh

4.
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone git://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions

5.
vim .zshrc

plugins=(git zsh-syntax-highlighting zsh-autosuggestions incr sudo extract)
```

# ZIM

```bash
1.
curl -fsSL https://raw.githubusercontent.com/zimfw/install/master/install.zsh | zsh

2.
zimfw install
```




