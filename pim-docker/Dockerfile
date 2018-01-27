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
  qmake -recursive CONFIG+=usb-moded DEFINES+=USE_KEEPALIVE . && \
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
  qmake . && \
  make install
RUN ln -s /usr/share/qt5/mkspecs/features/iodata-qt5.prf /usr/lib/x86_64-linux-gnu/qt5/mkspecs/features/

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
RUN cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0.1.18 -DCMAKE_INSTALL_PREFIX=/usr . && \
  make -j `nproc` install
WORKDIR /root/

RUN apt-get install -y \
   graphviz
RUN git clone https://git.merproject.org/mer-core/statefs.git
WORKDIR /root/statefs
COPY statefs.patch .
RUN cat statefs.patch | patch -p1
RUN cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0 -DCMAKE_INSTALL_PREFIX=/usr . && \
  make -j `nproc` all doc install
WORKDIR /root/

RUN git clone https://git.merproject.org/mer-core/qtaround.git
WORKDIR /root/qtaround
COPY qtaround.patch .
RUN cat qtaround.patch | patch -p1
RUN cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0 -DCMAKE_INSTALL_PREFIX=/usr . && \
  make -j `nproc` install
WORKDIR /root/

RUN git clone https://git.merproject.org/mer-core/statefs-qt.git
RUN cd statefs-qt && \
  cmake -DCMAKE_BUILD_TYPE=DEBUG -DVERSION=0 -DCMAKE_INSTALL_PREFIX=/usr . && \
  make -j `nproc` doc all install 
RUN sed 's/prefix=/prefix=\/usr/' -i  /usr/lib/pkgconfig/statefs-qt5.pc
RUN ln -s /usr/include/qt5/statefs /usr/include/x86_64-linux-gnu/qt5/

RUN apt-get install -y \
   libsystemd-dev
RUN git clone https://git.merproject.org/mer-core/timed.git
WORKDIR /root/timed
COPY timed.patch .
RUN cat timed.patch | patch -p1
RUN mkdir -p src/h/timed-qt5
RUN ln -sf ../../lib/qmacro.h src/h/timed-qt5
RUN qmake . && \
  make install
RUN ln -s /usr/share/qt5/mkspecs/features/timed-qt5.prf /usr/lib/x86_64-linux-gnu/qt5/mkspecs/features/
RUN ln -s /usr/share/qt5/mkspecs/features/timed-voland-qt5.prf /usr/lib/x86_64-linux-gnu/qt5/mkspecs/features/

WORKDIR /root/

RUN apt-get install -y \
  uuid-dev libical-dev libsqlite3-dev

RUN git clone https://git.merproject.org/mer-core/kcalcore.git
RUN cd kcalcore && \
  qmake . && \
  make install

RUN git clone https://git.merproject.org/mer-core/mkcal.git
WORKDIR /root/mkcal
COPY mkcal.patch .
RUN cat mkcal.patch | patch -p1
RUN qmake . && \
  make all doc install
WORKDIR /root/

RUN apt-get install -y qtbase5-private-dev #qtpim5-dev

RUN git clone https://git.merproject.org/mer-core/qtpim.git
RUN cd qtpim && rm -rf examples
RUN cd qtpim && \
  qmake . && \
  make install

RUN git clone https://git.merproject.org/mer-core/mlite.git
RUN cd mlite && \
  qmake . && \
  make install
RUN cp /root/mlite/src/mgconfitem.h /usr/include/
RUN cp /root/mlite/src/MGConfItem /usr/include/

RUN git clone https://git.merproject.org/mer-core/libmlocale.git
# zh.txt os broken somehow...
RUN cp  /root/libmlocale/src/icu-extradata/data/coll/uk.txt /root/libmlocale/src/icu-extradata/data/coll/zh.txt
RUN cd libmlocale && ./configure --prefix=/usr  && \
  make -j `nproc` install

RUN git clone https://git.merproject.org/mer-core/qtcontacts-sqlite.git
RUN cd qtcontacts-sqlite && \
  qmake . && \
  make install
# wtf?
RUN ln -s /lib/pkgconfig/qtcontacts-sqlite-qt5-extensions.pc /usr/lib/pkgconfig/ 
RUN mv /include/qtcontacts-sqlite-qt5-extensions /usr/include

#RUN git clone https://git.merproject.org/mer-core/libcontacts.git
# RUN cd libcontacts && \
#   qmake . && \
#   make install
#

# RUN git clone https://git.merproject.org/mer-core/ssu.git
# RUN cd ssu && \
#   qmake . && \
#   make install

RUN git clone https://git.merproject.org/mer-core/buteo-sync-plugins-social.git
WORKDIR /root/buteo-sync-plugins-social
RUN sed 's/onedrive//' -i src/src.pro 
RUN sed 's/twitter//' -i src/src.pro
RUN sed 's/dropbox//' -i src/src.pro 
RUN sed 's/vk//' -i src/src.pro 
RUN sed 's/tst_facebook//' -i /root/buteo-sync-plugins-social/tests/tests.pro 
RUN sed 's/tst_twitter//' -i /root/buteo-sync-plugins-social/tests/tests.pro 
RUN qmake . && \
  make install
WORKDIR /root/

ENV DISPLAY :0
# XSOCK=/tmp/.X11-unix/X0
# docker run -v $XSOCK:$XSOCK 
CMD ["/bin/bash"]
