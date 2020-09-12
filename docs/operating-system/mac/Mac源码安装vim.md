
## 核心点

#### 切换为macvim
```shell
# Target /usr/local/bin/vim  already exists. You may want to remove it:
$ rm '/usr/local/bin/vim'

# To force the link and overwrite all conflicting files:
$ brew link --overwrite macvim

# To list all files that would be deleted:
$ brew link --overwrite --dry-run macvim
```

#### 我的mac默认安装

```shell
# 编译选项配置: python2/3 perl ruby lua
$ ./configure \
--enable-multibyte \
--enable-perlinterp=dynamic \
--enable-rubyinterp=dynamic \
--enable-pythoninterp=dynamic \
--with-python-config-dir=/usr/lib/python2.7/config \
--enable-python3interp \
--with-python3-config-dir=/usr/local/opt/python/Frameworks/Python.framework/Versions/3.7/lib/python3.7/config-3.7m-darwin \
--enable-luainterp \
--with-lua-prefix=/usr/local/Cellar/lua/5.3.5_1 \
--enable-cscope \
--enable-gui=auto \
--with-features=huge \
--enable-fontset \
--enable-largefile \
--disable-netbeans \
--enable-fail-if-missing \
--prefix=/usr/local/vim8


# 确认是否安装成功: 并可以检查是否支持了 python2/3 lua等解释器
# 由于设置了 --prefix=/usr/local/vim8 所以全路径是这样
$ /usr/local/vim8/bin/vim --version

# 查看 老的vim路径, 并删除掉
$ ll `which vim`
$ rm /usr/local/bin/vim

# 创建软链
$ ln -s /usr/local/vim8/bin/vim /usr/local/bin/vim

# 再次查看版本号, 确定是否已经支持
$ vim --version

```

> 引用文章
> [Mac源码安装vim 支持python2/3,lua,ruby,perl解释器](https://xu3352.github.io/mac/2018/07/22/vim-install-from-source-on-mac-support-python2-python3-lua-ruby-perl)


