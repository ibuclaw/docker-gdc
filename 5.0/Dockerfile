FROM buildpack-deps:wheezy

ENV GCC_VERSION 5-20141207
ENV GDC_VERSION 5

RUN apt-get update \
	&& apt-get install -y flex \
	&& rm -r /var/lib/apt/lists/* \
	&& curl -SL "ftp://gcc.gnu.org/pub/gcc/snapshots/$GCC_VERSION/gcc-$GCC_VERSION.tar.bz2" -o gcc.tar.bz2 \
	&& mkdir -p /usr/src/gcc \
	&& git clone --branch master --single-branch --depth=1 https://github.com/D-Programming-GDC/GDC.git /usr/src/gcc \
	&& tar -xvf gcc.tar.bz2 -C /usr/src/gcc --strip-components=1 \
	&& rm gcc.tar.bz2* \
	&& cd /usr/src/gcc \
	&& patch -p1 -i ./gcc/d/patches/patch-toplev-$GDC_VERSION.x \
	&& patch -p1 -d ./gcc -i ./d/patches/patch-gcc-$GDC_VERSION.x \
	&& patch -p1 -d ./gcc -i ./d/patches/patch-versym-cpu-$GDC_VERSION.x \
	&& patch -p1 -d ./gcc -i ./d/patches/patch-versym-os-$GDC_VERSION.x \
	&& ./contrib/download_prerequisites \
	&& { rm *.tar.* || true; } \
	&& dir="$(mktemp -d)" \
	&& cd "$dir" \
	&& /usr/src/gcc/configure \
		--enable-languages=c,c++,d \
		--enable-checking=release \
		--disable-multilib \
		--with-bugurl="http://bugzilla.gdcproject.org" \
	&& make -j"$(nproc)" \
	&& make install-strip \
	&& cd .. \
	&& rm -rf "$dir" \
	&& rm -rf /usr/src/gcc \
	&& apt-get purge -y --auto-remove flex gcc g++

