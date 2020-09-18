---
title: vps 初始化脚本
date: 2020-09-10 10:25:54
tags: 
 - shell
categories: 
 - linux
 - shell
---
# vps 初始化脚本

## 问题

> vps频繁更换，有些东西都需要从新安装一遍，费事，还容易忽略



## 脚本内容列表

- **安装环境**
  - **nodejs**
  - **jdk8**
  - **python3**
- **安装基础命令**
  - **git**
  - **wget**
  - **glances**
  - **tmux**
  - **lsof**
  - **bzip2**
  - **pstree**
- **安装进阶命令**
  - **zsh**
  - **oh-my-zsh**
  - **nvim**
  - **trash-cli**
  - **autojump**
  - **busybox**
  - **nginx**
  - **docker**
  - **docker-compose**
  - **v2ray**
  - **proxychains**
  - **tig**
- **自定义nvim**
- **自定义alias**
- **自定义zsh**
- **设置时区**

## 准备工作

> 需要 **`shell`** 编程的知识

### 执行命令，结果保存到变量

 ```shell
v_node=$('命令')
v_node=$(命令)
 ```

### 判断命令是否安装

```shell
if [[ -x $(command -v 命令) ]]; 
```

> -x 判断文件是否有可执行权限
>
> 判断不准确 ，有时候没安装但是会返回一个#，不知道原因

```shell
command -v 命令 > /dev/null 2>&1
if [[ $? -eq 0 ]]; 
```

> 推荐使用这种

### 判断文件，文件夹是否存在

```shell
if [[ -d '/path' ]]; 
if [[ -f '/path' ]]; 
```

> -d 判断文件夹是否存在
>
> -f 判断文件是否存在
>
> ! 取反，注意要和 -d 、-f 、-x 隔一个空格，要不然会报 `unknown operan` 的错误

### if 、else 判断

```shell
if [[ "$v_trash" != "" ]]; then
        echo "0"
else
        echo '1'
fi
```

> [[ x ]] ; ：x 两边都需要有**空格**，后边如果跟 `then` ，需要有 ; 

#### 常用判断

##### 文件

```shell
-d 是否为目录
-f 是否为文件
-e 是否存在
-s 存在且大小不等于0
```

##### 文件权限

```shell
-r 是否可读
-x 是否可执行
-w 是否可写
```

##### 字符串

```shell
-n 长度不等于0
-z 长度等于0
```

##### 比较

```shell
if [[ $? -ne 0 ]]; then

-ne 不等于
-eq 等于
-gt 大于
-lt 小于
```



### 定义方法

```shell
function xxx() {
    echo '1'
}
v_node=$(xxx)
###
function xxx() {
    v='$1'
}
v_node=$(xxx h)
```

> `shell` 的返回值是将标准输出返回给变量，注意方法中的输出 ，如果使用 `return` ，只能返回 `0 - 255` 的整数，使用 `echo $?` 获取
>
> 调用方法不需要括号
>
> 方法必须在使用前定义
>
> `$n` 是方法中取参数，n < 10，如果大于10，需要使用 ${10}，一般配合 `for` 使用
>
> `function` 可以省略

### for 

#### 循环变量

```shell
for i
do
 echo $i
done
```

> 循环所有变量值

#### 循环目录

```shell
for file in `ls /usr/local`;

for file in $(ls /usr/local);

for file in "$(ls /usr/local)"; #不会换行
```

#### 循环文件内容

```shell
for file in `cat /usr/local/xx.xx`;
```




## 脚本

```shell
#!/bin/bash
zshrc='/root/.zshrc'
vimrc='/root/.vimrc'
Green_font_prefix="\033[32m" && Red_font_prefix="\033[31m" && Green_background_prefix="\033[42;37m" && Red_background_prefix="\033[41;37m" && Font_color_suffix="\033[0m"
Info="${Green_font_prefix}[信息]${Font_color_suffix}"
Error="${Red_font_prefix}[错误]${Font_color_suffix}"
Tip="${Green_font_prefix}[注意]${Font_color_suffix}"
Separator_1="——————————————————————————————"

install_nodejs() {
    command -v node >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} nodejs 已存在"
    else
        wget https://nodejs.org/dist/v14.9.0/node-v14.9.0-linux-x64.tar.xz -P /usr/local
        tar -xvf node-v14.9.0-linux-x64.tar.xz && mv node-v14.9.0-linux-x64 nodejs

        echo 'export PATH=$PATH:/usr/local/nodejs/bin' >>/etc/profile && source /etc/profile && command -v node >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} nodejs 安装成功"
        else
            echo -e "${Error} nodejs 安装失败"
        fi
    fi
}
install_jdk() {
    command -v java >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} jdk 已存在"
    else
        yum install java-1.8.0-openjdk -y
        command -v java >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} jdk 安装成功"
        else
            echo -e "${Error} jdk 安装失败"
        fi
    fi
}

install_trashCli() {
    command -v trash-put >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} trash-cli 已存在"
    else
        git clone https://github.com/andreafrancia/trash-cli.git /usr/local/trash-cli
        python /usr/local/trash-cli/setup.py install
        command -v trash-put >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} trash-cli 安装成功"
        else
            echo -e "${Error} trash-cli 安装失败"
        fi
    fi
}

install_autojump() {
    command -v autojump >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} autojump 已存在"
    else
        git clone git://github.com/joelthelion/autojump.git /usr/local/autojump

        python /usr/local/autojump/install.py
        echo '[[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && . ~/.autojump/etc/profile.d/autojump.sh' >>$zshrc
        command -v autojump >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} autojump 安装成功"
        else
            echo -e "${Error} autojump 安装失败"
        fi
    fi
}

install_ohMyZshCommand() {
    if [[ $? -ne 0 ]]; then
        echo -e "${Info} zsh未安装"
        exit 1
    fi
    if [[ ! -f $zshrc ]]; then
        echo -e "${Info} zsh 配置文件 未找到"
        exit 1
    fi
    command -v zsh > /dev/null 2>&1

    if [[ -d '/root/.oh-my-zsh' ]]; then
        echo -e "${Info} oh-my-zsh 已存在"
    else
        ZSH_CUSTOM='/root/.oh-my-zsh/custom'
        wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O - | sh
        chsh -s /bin/zsh root
        git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
        git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-highlighting
        echo 'export ZSH="/root/.oh-my-zsh"' >>$zshrc
        echo 'source $ZSH/oh-my-zsh.sh' >>$zshrc
        echo 'source "$ZSH_CUSTOM/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh"' >>$zshrc
        if [[ -d '/root/.oh-my-zsh' ]]; then
            echo -e "${Info} oh-my-zsh 安装成功"
        else
            echo -e "${Error} oh-my-zsh 安装失败"
        fi
    fi
}
install_zsh() {
    command -v zsh
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} zsh 已存在"
    else
        yum -y install zsh
        command -v zsh
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} zsh 安装成功"
        else
            echo -e "${Error} zsh 安装失败"
        fi
    fi
}

install_command() {

    for i; do
        command -v $i >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} $i 已存在"
        else
            yum install -y $i
            command -v $i >/dev/null 2>&1
            if [[ $? -eq 0 ]]; then
                echo -e "${Info} $i 安装成功"
            else
                echo -e "${Info} $i 安装失败"
            fi
        fi
    done

}

install_nginx() {

    for i; do
        command -v nginx >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} $i 已存在"
        else
            yum install -y nginx
            command -v $i >/dev/null 2>&1
            if [[ $? -eq 0 ]]; then
                echo -e "${Info} nginx 安装成功"
            else
                echo -e "${Info} nginx 安装失败"
            fi
        fi
    done

}
install_busybox() {
    command -v busybox >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} busybox 已存在"
    else
        cd /usr/local
        wget https://busybox.net/downloads/busybox-1.31.0.tar.bz2 -P /usr/local
        tar -xjvf /usr/local/busybox-1.31.0.tar.bz2 && cd /usr/local/busybox-1.31.0 && make defconfig && make install
        echo 'export PATH=/usr/local/busybox-1.31.0/_install/bin:$PATH' >>/etc/profile
        source /etc/profile
        command -v busybox >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} busybox 安装成功"
        else
            echo -e "${Error} busybox 安装失败"
        fi
    fi
}

set_selinux() {
    cp /etc/selinux/config /etc/selinux/config.bak
    echo 'SELINUX=disabled' >/etc/selinux/config
    echo 'SELINUXTYPE=targeted' >>/etc/selinux/config
    setenforce 0
    echo -e "${Info} selinux 已关闭"

}
customize_vim() {
    if [ -f '/root/.vimrc' ]; then

        wget https://wei-picgo.oss-cn-beijing.aliyuncs.com/.vimrc.simple
        echo -e "${Tip} .vimrc文件存在，新文件为 .vimrc.simple"
    else
        wget https://wei-picgo.oss-cn-beijing.aliyuncs.com/.vimrc.simple -O /root/.vimrc
        echo -e "${Tip} .vimrc文件下载完成"
    fi
    if [ ! -d '/root/.vim' ]; then
        mkdir -p /root/.vim/autoload
        wget https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim -P /root/.vim/autoload
    fi
    echo -e "${Info} .vimrc修改完成"
}

customize_alias() {
    echo 'alias rm="Please use <trash-put> command to delete file.";false' >>$zshrc
    echo "alias tp='trash-put'" >>$zshrc
    echo "alias h='history'" >>$zshrc
    echo "alias tl='trash-list'" >>$zshrc
    echo "alias tr='trash-restore'" >>$zshrc
    echo "alias vi='vim'" >>$zshrc
    echo "alias src='source /etc/profile'" >>$zshrc
    echo "alias srcc='source $zshrc'" >>$zshrc
    echo "alias pp='pstree -p'" >>$zshrc
    echo "alias top='glances'" >>$zshrc
    source $zshrc
    echo -e "${Info} 设置 alias 完成"
}

customize_zshrc() {
    if [ ! -f $zshrc ]; then
        echo -e "${Error} 未找到 zsh 配置文件"
    else
        sed -i 's/^ZSH_THEME.*$/ZSH_THEME="ys"/g' $zshrc
        v='plugins=(git zsh-autosuggestions zsh-syntax-highlighting extract vi-mode)'
        sed -i "s/^plugins.*)$/$v/g" $zshrc
        source /root/.zshrc
        echo -e "${Info} 自定义zsh完成"
    fi

}

set_localtime() {
    timedatectl set-timezone Asia/Shanghai
    echo -e "${Info} 设置时区完成"

}

install_proxychains() {

    command -v proxychains4 >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} proxychains 已存在"
    else
        git clone https://github.com/rofl0r/proxychains-ng.git /usr/local/proxychains-ng
        cd /usr/local/proxychains-ng
        ./configure --prefix=/usr --sysconfdir=/etc
        make
        make install
        make install-config
        command -v proxychains4 >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} proxychains安装完成，请修改 /etc/proxychains.conf "

        else
            echo -e "${Info} proxychains 安装失败"
        fi
    fi

}

install_pstree() {
    command -v pstree >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} pstree 已存在"
    else
        yum install -y psmisc
        command -v pstree >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} pstree 安装成功"
        else
            echo -e "${Info} pstree 安装失败"
        fi
    fi
}
install_v2ray() {

    command -v v2ary >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} v2ary 已存在"
    else

        wget https://wei-picgo.oss-cn-beijing.aliyuncs.com/install_v2ray.sh -P /usr/local
        sh /usr/local/install_v2ray.sh
        command -v v2ray >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} v2ray安装完成，请修改 /usr/local/etc/v2ray/config.json 后使用 systemctl start v2ray"
        else
            echo -e "${Info} v2ray 安装失败"
        fi
    fi

}

install_docker() {
    command -v docker >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} docker 已存在"
    else
        yum install -y yum-utils device-mapper-persistent-data lvm2
        yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
        yum install docker-ce
        command -v docker >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} docker 安装完成"
        else
            echo -e "${Info} docker 安装失败"
        fi
    fi
}

install_nvim() {
    command -v nvim >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} nvim 已存在"
    else
        yum install -y neovim
        command -v pip >/dev/null 2>&1
        if [[ $? -ne 0 ]]; then
            echo -e "${Error} 未找到 pip 命令"
        else
            pip install neovim
        fi
        command -v npm >/dev/null 2>&1
        if [[ $? -ne 0 ]]; then
            echo -e "${Error} 未找到 npm 命令"
        else
            npm install -g neovim
        fi
        command -v nvim >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} nvim 安装完成"
        else
            echo -e "${Info} nvim 安装失败"
        fi
    fi
}

install_tig() {
    command -v tig >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} tig 已存在"
    else
        yum install -y ncurses-devel
        wget -P /usr/local https://github.com/jonas/tig/releases/download/tig-2.5.1/tig-2.5.1.tar.gz
        tar zxvf /usr/local/tig-2.5.1.tar.gz && cd /usr/local/tig-2.5.1 && make && make install
        command -v tig >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} tig 安装完成"
        else
            echo -e "${Info} tig 安装失败"
        fi
    fi
}

customize_nvim() {
    command -v nvim >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Error} 未找到 nvim "
    else

        if [ -d '/root/.config/nvim' ]; then
            mv /root/.config/nvim /root/.config/nvim_bak
            echo -e "${Tip} /root/.config/nvim 目录已存在 已重命名为 nvim_bak"
        fi
        mkdir -p /root/.config/nvim/autoload
        wget https://wei-picgo.oss-cn-beijing.aliyuncs.com/init.vim -O /root/.config/nvim
        wget -P /root/.config/nvim/autoload https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
    fi
}

install_python3() {

    if [[ -n $(python -V | awk '{print $2}' | grep '^3.*') ]]; then
        echo -e "${Info} python 已经是3版本"
    else
        yum -y install zlib-devel bzip2-devel readline-devel sqlite sqlite-devel openssl-devel xz xz-devel libffi-devel python3-dev
        wget http://npm.taobao.org/mirrors/python/3.8.0/Python-3.8.0.tgz -P /usr/local
        tar -zxvf /usr/local/Python-3.8.0.tgz && cd /usr/local/Python-3.8.0 && mkdir /usr/local/python3 && configure --prefix=/usr/local/python3 && make && make install
        if [ -x '/usr/bin/python' ]; then
            mv /usr/bin/python /usr/bin/python_bak
            echo -e "${Tip} /usr/bin/python 已存在 已重命名为 python_bak"
        fi
        if [ -x '/usr/bin/pip' ]; then
            mv /usr/bin/pip /usr/bin/pip_bak
            echo -e "${Tip} /usr/bin/pip 已存在 已重命名为 pip_bak"
        fi
        #sed  '1c#!\/usr\/bin\/python_bak'  /usr/bin/yum
        #sed  '1c#!\/usr\/bin\/python_bak'  /usr/libexec/urlgrabber-ext-down
        ln -s /usr/local/python3/bin/python3 /usr/bin/python
        ln -s /usr/local/python3/bin/pip3 /usr/bin/pip
        if [[ -n $(python -V | awk '{print $2}' | grep '^3.*') ]]; then
            echo -e "${Info} python3 安装完成"
            echo -e -n "${Info} 请修改 /usr/bin/yum   /usr/libexec/urlgrabber-ext-down "
        else
            echo -e "${Error} python3 安装失败"
        fi

    fi

}

install_docker_compose() {
    command -v docker-compose >/dev/null 2>&1
    if [[ $? -eq 0 ]]; then
        echo -e "${Info} dcoker-compose 已存在"
    else
        curl -L https://github.com/docker/compose/releases/download/1.27.2/docker-compose-Linux-x86_64 -o /usr/local/bin/docker-compose

        chmod u+x /usr/local/bin/docker-compose
        command -v docker-compose >/dev/null 2>&1
        if [[ $? -eq 0 ]]; then
            echo -e "${Info} dcoker-compose 安装完成"
        else
            echo -e "${Info} dcoker-compose 安装失败"
        fi
    fi
}

echo -e "  初始化脚本
——————————— 安装环境
  ${Green_font_prefix}1a.${Font_color_suffix} 安装 jdk
  ${Green_font_prefix}1b.${Font_color_suffix} 安装 python3
  ${Green_font_prefix}1c.${Font_color_suffix} 安装 nodejs
 ———————————— 安装基础命令 
  ${Green_font_prefix}2a.${Font_color_suffix} 安装 git/wget/glances/tmux/lsof/bzip2/gcc/pstree
 ———————————— 安装进阶命令
  ${Green_font_prefix}3a.${Font_color_suffix} 安装 zsh && oh-my-zsh
  ${Green_font_prefix}3b.${Font_color_suffix} 安装 nvim
  ${Green_font_prefix}3c.${Font_color_suffix} 安装 trash-cli
  ${Green_font_prefix}3d.${Font_color_suffix} 安装 autojump
  ${Green_font_prefix}3e.${Font_color_suffix} 安装 busybox
  ${Green_font_prefix}3f.${Font_color_suffix} 安装 nginx
  ${Green_font_prefix}3g.${Font_color_suffix} 安装 docker
  ${Green_font_prefix}3h.${Font_color_suffix} 安装 docker-compose
  ${Green_font_prefix}3i.${Font_color_suffix} 安装 proxychains
  ${Green_font_prefix}3j.${Font_color_suffix} 安装 v2ary
  ${Green_font_prefix}3k.${Font_color_suffix} 安装 tig
———————————— 自定义
 ${Green_font_prefix}4a.${Font_color_suffix} 自定义 nvim
 ${Green_font_prefix}4b.${Font_color_suffix} 自定义 alias
 ${Green_font_prefix}4c.${Font_color_suffix} 设置 自定义zsh
———————————— 其他
 ${Green_font_prefix}5a.${Font_color_suffix} 关闭 selinux
 ${Green_font_prefix}5b.${Font_color_suffix} 设置 上海时区
 "
echo && read -p "请输入对应的符号 " num
case "$num" in
1a)
    install_jdk
    ;;
1b)
    install_python3
    ;;
1c)
    install_nodejs
    ;;
2a)
    install_command git wget glances tmux lsof bzip2 gcc pstree
    ;;
3a)
    install_zsh
    install_ohMyZshCommand
    ;;
3b)
    install_nvim
    ;;
3c)
    install_trashCli
    ;;
3d)
    install_autojump
    ;;
3e)
    install_busybox
    ;;
3f)
    install_nginx
    ;;
3g)
    install_docker
    ;;
3h)
    install_docker_compose
    ;;
3i)
    install_proxychains
    ;;
3j)
    install_v2ray
    ;;
3k)
    install_tig
    ;;

4a)
    customize_nvim
    ;;
4b)
    customize_alias
    ;;
4c)
    customize_zshrc
    ;;

5a)
    set_selinux
    ;;
5b)
    set_localtime
    ;;
*)
    echo -e "${Error} 请输入正确的符号"
    ;;
esac


```

## 脚本需要注意问题

> 1、tar 命令解压完后面命令找不到目录 ，怀疑是 未等到 tar 命令结束就执行下边的命令，但是解压完的文件也找不到。不知道真正原因。解决办法：使用 && 保证顺序执行
>
> 2、脚本能判断条件的就要判断条件，要保证安装出错，可以恢复