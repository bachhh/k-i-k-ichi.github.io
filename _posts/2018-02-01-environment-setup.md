---
layout: post
title: "Setup Environment"
date: 2018-2-04 4:20:00
categories: programming
featured_image: /images/cover.jpg
---

Well if your have a full beards, thick frame glasses
and your outfit consist mostly of earth tone colors.
Chances are you are already using Macbook and have a full terminal setup
that you would be so proud of, and showing off to your coworker.


But if you are like me, student and not so privileged to own a Macbook,
looking for the perfect middle ground between form (aesthetic) and function
then this guide is for setting up your environment in a minimal way.


This guide also serve as a reference for when you accidentally sudo rm rf /
(well not really, but I did managed to overwritten both my ubuntu partition
and windows partition) just like what I did yesterday first thing in the morning OTL

## Notes

#Copying config and .dotfiles

The prefered way is to symlink your config files, that way you can keep your file
in your github folder withouth having to cp every time for backing up

{% highlight bash %}
ln -sf /ABSOLUTE/PATH/SOURCE /ABSOLUTE/PATH/DEST
{% endhighlight %}

# File owner and permission

Most of the commands will somehow change the owner of the directory to root, this can
cause problems especially with Vim's Vundle for managing plugins.
It is not clear the cause, we just have to change them manually now

{% highlight bash %}
sudo chmod 764 -R $DIRECTORY
sudo chown -R $USER: $DIRECTORY
{% endhighlight %}

## Ricing your distro (Ubuntu)

I never wanted to be a ricer, but the ugliness Unity of Ubuntu is an insult
to my aestheti, so I did what I have to do.


Depends on how much of a ricer you are, you could change almost anything. Linux
has no limit after all. But the only things I am concerns with are the menubar
panels and icons.

{% highlight bash %}
sudo apt install unity-tweak-tools
# add their repository first
sudo apt install flatabulous
sudo apt install ultra-flat-icon
{% endhighlight %}


## Vim

Vim 8 can be install as easily as

{% highlight bash %}
sudo add-apt-repository ppa:jonathonf/vim
sudo apt install vim
{% endhighlight %}



However, in case the above repo maintainer is no longer active or we want more
supports for interpreted languages

{% highlight bash %}
sudo apt-get remove --purge vim vim-runtime vim-gnome vim-tiny vim-gui-common

sudo apt install liblua5.1-dev luajit
sudo apt install libluajit-5.1 python-dev ruby-dev libperl-dev
sudo apt install libncurses5-dev libatk1.0-dev libx11-dev libxpm-dev libxt-dev

#Optional: so vim can be uninstalled again via `dpkg -r vim`
sudo apt-get install checkinstall

sudo rm -rf /usr/local/share/vim /usr/bin/vim

cd ~
git clone https://github.com/vim/vim
cd vim
git pull && git fetch

#In case Vim was already installed
cd src
make distclean
cd ..

./configure \
--enable-multibyte \
--enable-perlinterp=dynamic \
--enable-rubyinterp=dynamic \
--with-ruby-command=/usr/local/bin/ruby \
--enable-pythoninterp=dynamic \
--with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu \
--enable-python3interp \
--with-python3-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu \
--enable-luainterp \
--with-luajit \
--enable-cscope \
--enable-gui=auto \
--with-features=huge \
--with-x \
--enable-fontset \
--enable-largefile \
--disable-netbeans \
--with-compiledby="yourname" \
--enable-fail-if-missing

make && sudo make install
{% endhighlight %}

## Git

Nothing to say more about git

{% highlight bash %}
sudo apt install git
git config --global user.email "k-i-k-ichi@users.noreply.github.com"
git config --global user.name "k-i-k-ichi"

ssh-keygen -t rsa -b 4096 -C "k-i-k-ichi@users.noreply.github.com"
ssh-add ~/.ssh/id_rsa
// Add id_rsa.pub to your github list of authenticaed ssh key
{% endhighlight %}


Also you might want to copy your aliases stored in $HOME/.gitconfig
the only one that I currently have is

{% highlight bash %}
history = log --oneline --decorate=full --graph --color --all
{% endhighlight %}

## Tmux

And also the latest tmux, because you would absolutely want true color support
{% highlight bash %}

sudo apt install -y automake
sudo apt install -y build-essential
sudo apt install -y pkg-config
sudo apt install -y libevent-dev
sudo apt install -y libncurses5-dev


git clone https://github.com/tmux/tmux.git /tmp/tmux
cd /tmp/tmux

sh autogen.sh
./configure && make
sudo make install

rm -rf /tmp/tmux
{% endhighlight %}


## Shell

{% highlight bash %}
sudo apt install zsh
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/ \
                      \ oh-my-zsh/master/tools/install.sh -O -)"

{% endhighlight %}

Also to add to my .zshrc

{% highlight bash %}
setxkbmap -option ctrl:swapcaps
tmux
task calendar
task list
{% endhighlight %}

## Ruby

{% highlight bash %}
sudo apt install ruby-dev
sudo apt install gem
sudo gem install bundler
sudo gem install jekyll
sudo apt-get install build-essential patch ruby-dev zlib1g-dev liblzma-dev
sudo gem install nokogiri
bundler install
{% endhighlight %}

## Vim plugins

Mainly this section is for YCM, but I will update as things goes on

# YouCompleteMe

Install dependencies

{% highlight bash %}
sudo apt-get install build-essential cmake
sudo apt-get install python-dev python3-dev
cd ~/.vim/bundle/YouCompleteMe
./install.py --clang-completer
ln -sf $HOME/.dotfiles/.vim/.ycm_extra_conf.py $HOME/.vim/.ycm_extra_conf.py

let g:ycm_global_ycm_extra_conf = '~/.vim/.ycm_extra_conf.py'
{% endhighlight %}

## Not so essential

{% highlight bash %}
sudo apt-install screenfetch
sudo apt-install taskwarrior
sudo apt-install mpv
sudo apt-install pinta
sudo apt-install tranmission
sudo apt-install okular
sudo apt-install tree

# java web start for topcoder aplet
sudo apt-get install icedtea-netx
{% endhighlight %}

