FROM ubuntu:22.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && \
    apt-get install -y \
      software-properties-common  build-essential gcc-multilib make ccache libncurses5 repo m4

ENV VENDOR=sony
ENV DEVICE=xqau52
ENV HABUILD_DEVICE=pdx201
ENV FAMILY=seine
ENV ANDROID_VERSION_MAJOR=10
ENV HAVERSION="sony-aosp-"$ANDROID_VERSION_MAJOR

WORKDIR /workdir
RUN repo init -u https://github.com/mer-hybris/android.git -b $HAVERSION -m tagged-localbuild.xml
RUN repo sync --fetch-submodules
RUN git clone --recurse-submodules https://github.com/mer-hybris/droid-src-sony droid-src -b "hybris-"$HAVERSION
RUN ln -s droid-src/patches

# :-)
RUN git config --global user.email "you@example.com" && \
    git config --global user.name "Your Name"

RUN droid-src/apply-patches.sh --mb

RUN git clone https://github.com/sailfishos/droidmedia external/droidmedia
RUN bash -c "source build/envsetup.sh && lunch aosp_$DEVICE-user && make libdroidmedia"
