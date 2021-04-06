# 自分のための勉強ノート

友達の影響を受けたため、頑張って勉強したことをまとめる．  
ぶっちゃけ、github使いにくいのである．
普通に考えて、ブログのほうが数倍楽なのだが、どうやら世界標準なのはgithubのようであるので、頑張っていきたい．

# 一覧
## 1．gitbookを使ってみる．

すでに、githubがもう嫌になってしまったのだ．  
htmlに逃げだしたい．しかし、Markdownファイルを利用することに慣れる必要がある．どうすればいいかと悩んだ結果、gitbookを利用することに決めた．  
どうやら、Markdown言語で書かれたものからHTMLに変換してくれるらしい．いいものだ．とりあえず、以下のURLを参考にして作成してみる．  

[URL](https://r-ngtm.hatenablog.com/entry/2020/06/18/193235)

どうやら、npmをインストールするらしいので、インストールする．

[URL](https://www.virment.com/how-to-install-nodejs-ubuntu/)

そんで、nodejsのバージョンは、14.16.0にした．

# NvidiaとDockerを入れるスクリプト

```
#!/bin/bash -eu

#--- nouveauを無効化
sudo bash -c "echo -e 'blacklist nouveau\\noptions nouveau modset=0' > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
sudo update-initramfs -u

#--- nvidiaドライバーの最新版を入手
sudo add-apt-repository -y ppa:graphics-drivers/ppa
sudo apt update

#--- nvidia-driverのインストール
sudo ubuntu-drivers autoinstall

#--- dockerをインストール
sudo apt-get update
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER

#--- nvidia container toolkitsのインストール
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker

#--- 終了
echo "正常に完了しました！"

echo -n "再起動してもよろしいですか？ [Y/n]: "
read ANS
case $ANS in
  "" | [Yy]* )
    # ここに「Yes」の時の処理を書く
    sudo reboot
    ;;
  * )
    # ここに「No」の時の処理を書く
    echo ""
    ;;
esac

```

# ROSのインストール

```
#!bin/bash -eu
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

sudo apt update

sudo apt install -y ros-melodic-desktop-full

echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc

shopt -s expand_aliases
source ~/.bashrc

sudo apt install -y python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential

sudo rosdep init

rosdep update

mkdir -p ~/catkin_ws/src

cd ~/catkin_ws

catkin_make

echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc

source ~/.bashrc

```
