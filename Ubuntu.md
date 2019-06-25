# Ubuntu#

sudo su  进入root模式

cd /进入根目录  cd composetest 执行docker-compose up -d 后台运行docker-compose

docker-compose stop停止当前运行中的compose

jdk1.8.0_191

JAVA_HOME=/java/jdk1.8.0_191

-javaagent:/java/idea-IU-183.4284.148/bin/JetbrainsIdesCrack-3.4-release-enc.jar

sudo gedit 文本框编辑

find -name "检索文件"

cp -r 可以解决略过文件夹问题

sudo nautilus 进入root文件夹，ctrl+h显示隐藏文件夹





maven手动导入jar包

mvn install:install-file -Dfile=D:/home/twisted/下载/xuggle-xuggler-5.4.jar -DgroupId=xuggle -DartifactId=xuggle-xuggler -Dversion=5.4 -Dpackaging=jar

mvn install:install-file -Dfile=/home/twisted/下载/kefu-lite-daas-core-3.0.1.FINAL.jar -DgroupId=com.easemob.cec -DartifactId=kefu-lite-daas-core -Dversion=3.0.1.FINAL -Dpackaging=jar





### 软链接

文件夹建立软链接（用绝对地址）

　　ln -s 源地址  目的地址

　　比如我把linux文件系统rootfs_dir软链接到/home/jyg/目录下

　　ln -s /opt/linux/rootfs_dir  /home/jyg/rootfs_dir就可以了