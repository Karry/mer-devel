FROM ubuntu:artful

RUN apt-get update && apt-get install -y \
  aptitude git cmake g++ vim strace gdb pkg-config wget \
  qt5-qmake qttools5-dev-tools qtbase5-dev qt5-default \
  libsignon-qt5-dev libaccounts-qt5-dev libdbus-1-dev \
  libglib2.0-dev qtsystems5-dev libiphb-dev mce-dev \
  qtdeclarative5-dev doxygen libudev-dev

WORKDIR /root

RUN git clone https://git.merproject.org/mer-core/libsailfishkeyprovider.git
RUN cd libsailfishkeyprovider && \
  qmake . && \
  make install

RUN git clone https://git.merproject.org/mer-core/nemo-keepalive.git
RUN cd nemo-keepalive && \
  qmake . && \
  make install

RUN git clone https://git.merproject.org/mer-core/buteo-syncfw.git
RUN cd buteo-syncfw && \
  qmake -recursive CONFIG+=usb-moded DEFINES+=USE_KEEPALIVE && \
  make
RUN cd buteo-syncfw && \
  make install || true

RUN git clone https://git.merproject.org/mer-core/libsocialcache.git
RUN cd libsocialcache && \
  qmake . && \
  make install

RUN apt-get update && apt-get install -y bison flex
RUN git clone https://git.merproject.org/mer-core/libiodata.git
RUN sed 's/LOG_LEVEL LOG_WARNING/LOG_LEVEL LOG_NONE/' -i /root/libiodata/src/log.h
RUN cd libiodata && \
  qmake && \
  make install

RUN apt-get install -y \
  libboost-dev libboost-system-dev libboost-filesystem-dev \
  libfuse-dev python
  
RUN git clone https://git.merproject.org/mer-core/tut.git
RUN cd tut && \
  ./waf configure --prefix=/usr/ --version=0.0.4 && \
  ./waf build && \
  ./waf install

RUN git clone https://git.merproject.org/mer-core/cor.git
WORKDIR /root/cor
COPY cor.patch .
RUN cat cor.patch | patch -p1
RUN cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0.1.18 . && \
  make -j `nproc` install
WORKDIR /root/

RUN apt-get install -y \
   graphviz
RUN git clone https://git.merproject.org/mer-core/statefs.git
WORKDIR /root/statefs
COPY statefs.patch .
RUN cat statefs.patch | patch -p1
RUN cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0 . && \
  make -j `nproc` all doc install
WORKDIR /root/

RUN git clone https://git.merproject.org/mer-core/qtaround.git
WORKDIR /root/qtaround
COPY qtaround.patch .
RUN cat qtaround.patch | patch -p1
RUN cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0 . && \
  make -j `nproc` install
WORKDIR /root/

RUN git clone https://git.merproject.org/mer-core/statefs-qt.git
RUN cd statefs-qt && \
  cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0 -DCMAKE_INSTALL_PREFIX=/usr . && \
  make -j `nproc` doc all install 
RUN sed 's/prefix=/prefix=\/usr/' -i  /usr/lib/pkgconfig/statefs-qt5.pc
RUN ln -s /usr/include/qt5/statefs /usr/include/x86_64-linux-gnu/qt5/

########################################################

# RUN git clone https://git.merproject.org/mer-core/statefs-providers.git

RUN apt-get install -y \
   libsystemd-dev
RUN git clone https://git.merproject.org/mer-core/timed.git
WORKDIR /root/timed
COPY timed.patch .
RUN cat timed.patch | patch -p1
RUN mkdir -p src/h/timed-qt5
RUN ln -sf ../../lib/qmacro.h src/h/timed-qt5

RUN find src/server/ -name *.type -exec iodata-qt5-type-to-c++ {} \;
RUN qmake
# RUN make install
 
WORKDIR /root/

# RUN git clone https://git.merproject.org/mer-core/mkcal.git
# 
# RUN git clone https://git.merproject.org/mer-core/buteo-sync-plugins-social.git
# RUN cd buteo-sync-plugins-social && \
#   qmake . && \
#   make install
#
CMD ["/bin/bash"]
