# pull source and build docker auto in docker hub
FROM centos/systemd

WORKDIR /root/

ENV TARS_INSTALL  /usr/local/tars/cpp/deploy

ARG FRAMEWORK_TAG=master
ARG WEB_TAG=master 

# RUN rpm -ivh https://repo.mysql.com/mysql57-community-release-el7.rpm
RUN yum makecache fast \
	&& yum install -y yum-utils mysql psmisc file which net-tools wget unzip telnet git gcc gcc-c++ make glibc-devel flex bison ncurses-devel protobuf-devel zlib-devel openssl-devel \
	# Install cmake
	&& mkdir -p /tmp/cmake/  \
	&& cd /tmp/cmake \
	&& wget https://tars-thirdpart-1300910346.cos.ap-guangzhou.myqcloud.com/src/cmake-3.16.4.tar.gz  \
	&& tar xzf cmake-3.16.4.tar.gz \
	&& cd cmake-3.16.4 \
	&& ./configure  \
	&& make && make install \
	# Install Tars framework
	&& mkdir -p /tmp/Tars \
	&& cd /tmp/Tars \
	&& git clone --branch $FRAMEWORK_TAG https://github.com/TarsCloud/TarsFramework framework --recursive \
	&& git clone --branch $WEB_TAG https://github.com/TarsCloud/TarsWeb web \
	&& mkdir -p /data \
	&& chmod u+x /tmp/Tars/framework/build/build.sh \
	&& cd /tmp/Tars/framework/build/ \
	&& ./build.sh all \
	&& ./build.sh install \
	&& cp -rf /tmp/Tars/web /usr/local/tars/cpp/deploy/ \
	&& cd /tmp \
	# Install node environment
	&& wget https://github.com/nvm-sh/nvm/archive/v0.35.1.zip \
	&& unzip v0.35.1.zip \
	&& cp -rf /tmp/nvm-0.35.1 /root/.nvm \
	&& echo ". /root/.nvm/nvm.sh" >> /root/.bashrc \
	&& echo ". /root/.nvm/bash_completion" >> /root/.bashrc \
	&& source /root/.bashrc \
	&& nvm install v12.13.0 \
	&& npm install -g npm pm2 \
	&& source $HOME/.bashrc	\
	# Clean build dependents
	&& rm -rf /tmp/* \
	&& yum remove -y yum-utils unzip git glibc-devel ncurses-devel protobuf-devel zlib-devel openssl-devel \
	&& yum autoremove -y \
	&& yum clean all \
	&& rm -rf /var/cache/yum \
	&& ${TARS_INSTALL}/tar-server.sh

ENTRYPOINT [ "/usr/local/tars/cpp/deploy/docker-init.sh" ]

EXPOSE 3000
EXPOSE 3001
