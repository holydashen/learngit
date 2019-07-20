centos安装vim7.4

 

系统版本centos6.4;

root权限

su - root  

 

卸载

$ rpm -qa | grep vim

$ yum remove vim vim-enhanced vim-common vim-minimal  

 

[下载](http://www.2cto.com/soft)、解压

$ wget ftp://ftp.vim.org/pub/vim/unix/vim-7.4.tar.bz2  

$ wget ftp://ftp.vim.org/pub/vim/extra/vim-7.2-extra.tar.gz

$ wget ftp://ftp.vim.org/pub/vim/extra/vim-7.2-lang.tar.gz

$  

$ tar jxvf vim-7.4.tar.bz2  

$ tar zxvf vim-7.2-extra.tar.gz  

$ tar zxvf vim-7.2-lang.tar.gz  

$  

$ mv vim72 vim74  

 

 

安装编译环境

$ yum install ncurses-devel  

$ yum install gcc

 

编译安装

$ cd vim74/src  

$ ./configure --enable-multibyte \--with-features=huge \--disable-selinux  

$ make  

$ make install  

 

测试

   

$ vim --version  

VIM - Vi IMproved 7.4