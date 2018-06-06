title: "MSYS2+GTK3+CMAKE"
date: 2016-03-20 12:20:05
tags:
- gtk3
- msys2
- cmake
categories: soft_use
description: win10下使用MSYS2+GTK3编程
feature: /images/mainmenu/042.jpg
toc: true
---
{% label 吐槽向: danger %}{% textcolor success %}中国的github(cafe)离开了我们,还是原版更靠谱!!!.{% endtextcolor %}
#### 0x00 我的软件环境 
Win10.0.10586
#### 0x01 USE MSYS2 
##### Decide Your Applicatio Spec
- 32位 or 64位
- Dependent Libraries 手动编译 or 自动构建.

##### Download Prepare
* tdm-gcc-5.1.0-3.exe
* msys2-i686-20160205.exe
* cmake-3.5.0-win32-x86.msi
* zlib-1.2.8.tar.gz
* openssl-1.0.2g.tar.gz
* libssh2-1.7.0.tar.gz
* curl-7.48.0.tar.bz2
* gtk+-bundle_3.10.4-20131202_win32.zip

<!-- more -->
##### install gcc and gtk3
有时其他的模拟环境也需要这个编译工具,tdm-gcc32-5.1.0,为方便加入windows全局变量.
{% img http://7vztq1.com1.z0.glb.clouddn.com/2016/MSYS2_GTK3_CMAKE/win10_env.png [env_path [#1] %}
##### MSYS2 Upgrade
决定手动编译的包不要安装,因为麻烦.
```bash
pacman -Syu
pacman -S --needed filesystem msys2-runtime bash libreadline libiconv libarchive libgpgme libcurl pacman ncurses libintl base-devel vim 
##pacman -S mingw-w64-i686-gtk3
##pacman -S  mingw-w64-x86_64-toolchain mingw-w64-i686-toolchain mingw-w64-i686-gtk3 mingw-w64-x86_64-gtk3 mingw-w64-i686-gcc
```

##### cmake
```bash
./bootstrap --prefix="/D/workspace_CL/Trends/solo_opensource_lib/cmake-3.5.0"
make && make install
MSYS Makefiles
export PATH=$PATH:/d/workspace_CL/Trends/solo_opensource_lib/cmake-3.5.0/bin;
##pacman -S make
##pacman -Ss cmake binutils
ld --verbose | grep SEARCH_DIR | tr -s ' ;' \\012
```
##### zlib
```bash
make -fwin32/Makefile.gcc --prefix=$PWD/dist -no-undefined
make install -fwin32/Makefile.gcc DESTDIR=$PWD/dists/ INCLUDE_PATH=include LIBRARY_PATH=lib BINARY_PATH=bin
make install -fwin32/Makefile.gcc DESTDIR=$PWD/distd/ INCLUDE_PATH=include LIBRARY_PATH=bin BINARY_PATH=bin SHARED_MODE=1
DESTDIR="$PWD/dist"  -no-undefined
```
##### openssl
```bash
./Configure --prefix=$PWD/dist no-idea no-mdc2 no-rc5 shared mingw(64)
make depend && make && make install
```
##### config .bashrc
```bash
export PATH="$PATH:/d/workspace_CL/Trends/solo_opensource_src/openssl-1.0.2g/dist/bin"
export INCLUDE="$INCLUDE:/d/workspace_CL/Trends/solo_opensource_src/openssl-1.0.2g/dist/include"
export LIB="$LIB:/d/workspace_CL/Trends/solo_opensource_src/openssl-1.0.2g/dist/lib"
export PATH="$PATH:/d/workspace_CL/Trends/solo_opensource_src/curl-7.48.0/dist/bin"
export INCLUDE="$INCLUDE:/d/workspace_CL/Trends/solo_opensource_src/curl-7.48.0/dist/include"
export LIB="$LIB:/d/workspace_CL/Trends/solo_opensource_src/curl-7.48.0/dist/lib"	
##LD_RUN_PATH	C_INCLUDE_PATH		CPLUS_INCLUDE_PATH	LD_LIBRARY_PATH	LIBRARY_PATH
```
##### libssh2
```bash
./configure --prefix=$PWD/dist --with-openssl --disable-examples-build		//--with-libz 
make && make install
```
##### curl
```bash
./configure --prefix=$PWD/dist
make && make install
```
##### Trends #####
[CMakeLists.txt](https://github.com/soloxyq/trends/blob/master/CMakeLists.txt)
GTK3依赖部分如下:
```cmake
pkg-config --cflags gtk+-3.0
	-mms-bitfields -pthread -mms-bitfields -I/mingw32/include/gtk-3.0 -I/mingw32/include/cairo -I/mingw32/include -I/mingw32/include/pango-1.0 -I/mingw32/include/atk-1.0 -I/mingw32/include/cairo -I/mingw32/include/pixman-1 -I/mingw32/include -I/mingw32/include/freetype2 -I/mingw32/include/libpng16 -I/mingw32/include/harfbuzz -I/mingw32/include/glib-2.0 -I/mingw32/lib/glib-2.0/include -I/mingw32/include -I/mingw32/include/freetype2 -I/mingw32/include -I/mingw32/include/harfbuzz -I/mingw32/include/libpng16 -I/mingw32/include/gdk-pixbuf-2.0 -I/mingw32/include/libpng16 -I/mingw32/include/glib-2.0 -I/mingw32/lib/glib-2.0/include
pkg-config --libs gtk+-3.0
	-L/mingw32/lib -LC:/building/msys64/mingw32/lib -L/mingw32/lib -LC:/building/msys64/mingw32/lib/../lib -L/mingw32/lib -lgtk-3 -lgdk-3 -lgdi32 -limm32 -lshell32 -lole32 -Wl,-luuid -lwinmm -ldwmapi -lz -lepoxy -lpangocairo-1.0 -lpangoft2-1.0 -lpangowin32-1.0 -lgdi32 -lusp10 -lpango-1.0 -lm -latk-1.0 -lcairo-gobject -lcairo -lz -lpixman-1 -lfontconfig -lexpat -lfreetype -liconv -lexpat -lfreetype -lz -lbz2 -lharfbuzz -lpng16 -lz -lgdk_pixbuf-2.0 -lpng16 -lz -lgio-2.0 -lz -lgmodule-2.0 -pthread -lgobject-2.0 -lffi -lglib-2.0 -lintl -pthread -lws2_32 -lole32 -lwinmm -lshlwapi -lintl
##cmake .. -G "MinGW Makefiles"  -DCMAKE_BUILD_TYPE=Debug -DCMAKE_EXPORT_COMPILE_COMMANDS=1
cmake .. -G "MSYS Makefiles"  -DCMAKE_BUILD_TYPE=Debug -DCMAKE_EXPORT_COMPILE_COMMANDS=1
##cmake .. -DCMAKE_BUILD_TYPE=Debug -DCMAKE_EXPORT_COMPILE_COMMANDS=1
```
#### Publish && Write Something
##### Publish
```bash
git init
git add .
git commit -m "first commit"
git remote add origin git@github.com:soloxyq/trends.git
git push -u origin master
```
##### blog
```bash
cd d:/workspace
hexo init blog
cd blog
npm install
git clone https://github.com/wzpan/hexo-theme-freemind.git themes/freemind
npm install hexo-tag-bootstrap --save
npm install hexo-generator-search --save
npm install hexo-math --save
npm install hexo-deployer-git --save
hexo d -g
hexo s -p 5000 -i 127.0.0.1
```
##### qiniu图片外链
```bash
qrsync res.json
{
    "src": "d:/workspace/blog/res_20160401",
    "dest": "qiniu:access_key=223333&secret_key=223333bucket=gitcafe",
    "debug_level": 1
}
```
#### 0x02 备注
##### 附录I: Code Highlight支持的语言
actionscript, apache, bash, clojure, cmake, coffeescript, cpp, cs, css, d, delphi, django, erlang, go, haskell, html, http, ini, java, javascript, json, lisp, lua, markdown, matlab, nginx, objectivec, perl, php, python, r, ruby, scala, smalltalk, sql, tex, vbscript, xml
##### 附录II: 包管理Pacman常用命令
```bash
pacman -Sy abc					#和源同步后安装名为abc的包 
pacman -S  abc					#从本地数据库中得到abc的信息，下载安装abc包 
pacman -Sf abc                  #强制安装包abc 
pacman -Ss abc                  #搜索有关abc信息的包 
pacman -Si abc                  #从数据库中搜索包abc的信息 
pacman -Q						# 列出已经安装的软件包
pacman -Q abc					# 检查 abc 软件包是否已经安装
pacman -Qi abc                  #列出已安装的包abc的详细信息 
pacman -Ql abc					# 列出abc软件包的所有文件
pacman -Qo /path/to/abc			# 列出abc文件所属的软件包
pacman -Syu						#同步源，并更新系统 
pacman -Sy						#仅同步源 
pacman -Su						#更新系统
pacman -R   abc					#删除abc包 
pacman -Rd abc					#强制删除被依赖的包
pacman -Rc abc                  #删除abc包和依赖abc的包 
pacman -Rsc abc					#删除abc包和abc依赖的包 
pacman -Sc						#清理/var/cache/pacman/pkg目录下的旧包 
pacman -Scc						#清除所有下载的包和数据库 
pacman -U   abc					#安装下载的abs包，或新编译的abc包
pacman -Sd abc					#忽略依赖性问题，安装包abc 
pacman -Su --ignore foo			#升级时不升级包foo 
pacman -Sg abc					#查询abc这个包组包含的软件包	
```
##### 附录III:重命名远程分支
```bash
git branch -m <old_name> <new_name>
git push <remote> --set-upstream new_name
```
##### 附录IV
```bash
curl version:     7.48.0
Host setup:       i686-pc-mingw32
Install prefix:   /d/workspace_CL/Trends/solo_opensource_src/curl-7.48.0/dist
Compiler:         gcc
SSL support:      enabled (OpenSSL)
SSH support:      enabled (libSSH2)
zlib support:     enabled
GSS-API support:  no      (--with-gssapi)
TLS-SRP support:  enabled
resolver:         default (--enable-ares / --enable-threaded-resolver)
IPv6 support:     enabled
Unix sockets support: no      (--enable-unix-sockets)
IDN support:      no      (--with-{libidn,winidn})
Build libcurl:    Shared=yes, Static=yes
Built-in manual:  enabled
--libcurl option: enabled (--disable-libcurl-option)
Verbose errors:   enabled (--disable-verbose)
SSPI support:     no      (--enable-sspi)
ca cert bundle:   no
ca cert path:     no
ca fallback:      no
LDAP support:     enabled (winldap)
LDAPS support:    enabled
RTSP support:     enabled
RTMP support:     no      (--with-librtmp)
metalink support: no      (--with-libmetalink)
PSL support:      no      (libpsl not found)
HTTP2 support:    disabled (--with-nghttp2)
Protocols:        DICT FILE FTP FTPS GOPHER HTTP HTTPS IMAP IMAPS LDAP LDAPS POP3 POP3S RTSP SCP SFTP SMB SMBS SMTP SMTPS TELNET TFTP
```