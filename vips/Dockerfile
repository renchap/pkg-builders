ARG DEBIAN_VERSION=stable
ARG MAINTAINER="Renaud Chaput <renchap@gmail.com>"

FROM debian:${DEBIAN_VERSION}-slim

ARG VIPS_VERSION=8.7.2
ARG VIPS_URL=https://github.com/libvips/libvips/releases/download
ARG DEV_DEPS="libjpeg62-turbo-dev libpng-dev libwebp-dev libgif-dev libtiff-dev libpango1.0-dev liborc-0.4-dev liblcms2-dev libexif-dev zlib1g-dev"
ARG MAKE_OPTIONS

RUN apt-get update && apt-get install -q -y --no-install-recommends build-essential pkg-config libexpat1-dev glib2.0-dev ruby rubygems ruby-dev curl gzip

RUN gem install --no-rdoc --no-ri fpm

RUN apt-get install -q -y --no-install-recommends ${DEV_DEPS}

RUN cd /usr/local/src \
  && curl -OL ${VIPS_URL}/v${VIPS_VERSION}/vips-${VIPS_VERSION}.tar.gz \
  && tar xzf vips-${VIPS_VERSION}.tar.gz

RUN cd /usr/local/src/vips-${VIPS_VERSION} \
  && CFLAGS=-O3 CXXFLAGS=-O3 ./configure --prefix=/usr \
  && make ${MAKE_OPTIONS} \
  && mkdir /tmp/installdir \
  && make install DESTDIR=/tmp/installdir

COPY debian /root/debian
RUN cd /root && dpkg-shlibdeps /tmp/installdir/usr/lib/libvips.so
RUN mkdir -p /dist

RUN cd /root && ruby -e "%x[fpm -s dir -t deb -n vips -v ${VIPS_VERSION} -C /tmp/installdir -p /dist/libvips42_VERSION_ARCH.deb --provides libvips --license LGPL-2.1 --maintainer '${MAINTAINER}' --deb-shlibs debian/shlibs --description 'A fast image processing library with low memory needs.' --url 'https://libvips.github.io/libvips/' #{File.open('./debian/substvars').read.strip.gsub('shlibs:Depends=', '').tr('()', '').split(', ').map{ |a| '-d \''+a+'\'' }.join(' ')} usr]"
