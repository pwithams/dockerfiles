# CSharp Dev Environment
from mcr.microsoft.com/dotnet/core/sdk:2.2
RUN apt-get update
# python requirements
RUN apt-get install -y build-essential checkinstall
RUN apt-get install -y libssl-dev libreadline-gplv2-dev libncursesw5-dev \
    libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev
RUN mkdir /python_install
WORKDIR /python_install
RUN wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz
RUN tar -xzf Python-3.8.0.tgz
WORKDIR /python_install/Python-3.8.0
RUN ./configure --enable-shared && make && make install
WORKDIR /
# vim requirements
RUN apt-get install -y ruby-full git zip gcc libncurses5-dev libgnome2-dev libgnomeui-dev \
  libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
  libcairo2-dev libx11-dev libxpm-dev libxt-dev
# install vim
RUN mkdir /vim_install
WORKDIR /vim_install
RUN wget https://github.com/vim/vim/archive/v8.1.2267.zip
RUN unzip v8.1.2267.zip
WORKDIR /vim_install/vim-8.1.2267/src
RUN ./configure --enable-python3interp --enable-rubyinterp --with-python3-config-dir=/usr/local/lib/python3.8/config-3.8-x86_64-linux-gnu/ && make && make install
# install node js
RUN curl -sL https://deb.nodesource.com/setup_13.x | bash -
RUN apt-get install -y nodejs
RUN apt-get install -y cmake
# install go
RUN mkdir /go_install
WORKDIR /go_install
RUN wget https://dl.google.com/go/go1.13.4.linux-amd64.tar.gz
RUN tar -xzf *
RUN mv go /usr/local
WORKDIR /root/
# additional packages
RUN apt-get install exuberant-ctags
# remove install directories
RUN rm -rf /vim_install /python_install /go_install
# create dev user
RUN apt-get -y install sudo
RUN useradd -m docker && echo 'docker:docker' | chpasswd && adduser docker sudo
USER docker
WORKDIR /home/docker
# install rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > install.sh && sh install.sh -y
# install fd
RUN export PATH=/home/docker/.cargo/bin:$PATH && cargo install fd-find
# update bashrc with go paths
RUN echo 'export GOROOT=/usr/local/go' >> /home/docker/.bashrc
RUN echo 'export PATH=$GOPATH/bin:$GOROOT/bin:$PATH' >> /home/docker/.bashrc
# get personal vim configuration files
RUN git clone https://github.com/paddyw2/.vim
RUN cp /home/docker/.vim/.vimrc /home/docker/.vimrc
RUN mkdir /home/docker/code/
# install plugins
RUN vim +PlugInstall +qall
# add go to path for YCM install and compile YouCompleteMe
RUN export PATH=/usr/local/go/bin:$PATH && python3 /home/docker/.vim/plugged/YouCompleteMe/install.py --all
CMD /bin/bash
