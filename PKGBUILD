# Maintainer: Frank Siegert < frank dot siegert at googlemail dot com >
# Contributor: Sebastien Binet binet-at-cern-ch
# Maintainer: Wainer Vandelli <wainer dot vandelli at gmail dot com>
# Contributor: Konstantin Gizdov < arch at kge dot pw >
pkgname=cvmfs
pkgver=2.4.4
pkgrel=1
pkgdesc="A client-server file system implemented in FUSE and developed to deliver software distributions onto virtual machines in a fast, scalable, and reliable way."
arch=('x86_64')
url="http://cernvm.cern.ch/portal/filesystem"
license=('BSD')
depends=('fuse2' 'sqlite' 'curl' 'python' 'c-ares' 'intel-tbb' 'leveldb' 'openssl-1.0' 'pacparser')
makedepends=('cmake' 'python' 'unzip' 'gtest' 'python-geoip' 'sparsehash')
backup=('etc/cvmfs/default.local')
install=cvmfs.install
options=('!emptydirs')
source=("https://ecsft.cern.ch/dist/$pkgname/$pkgname-$pkgver/$pkgname-$pkgver.tar.gz"
        'settings.cmake')
md5sums=('74c438d8d5067f90422fc1775d5e108c'
         '20dc60c61077f4a3711463e8686d260d')

prepare() {
    cd "$srcdir/$pkgname-$pkgver"
    sed -i 's/missing_libs=\"libcurl /missing_libs=\"/g' bootstrap.sh
    sed -i 's/missing_libs=\".*/missing_libs=\"vjson sha2 sha3 mongoose\"/g' bootstrap.sh
}

build() {
	cd "$srcdir/$pkgname-$pkgver"
    mkdir -p build
    cd build
    #cmake -DBUILD_SERVER=OFF -DBUILD_RECEIVER=OFF -DBUILD_LIBCVMFS_CACHE=OFF -DBUILD_LIBCVMFS=OFF ../
    cmake -C "${srcdir}/settings.cmake" ../

    make
}

package() {
	cd "$srcdir/$pkgname-$pkgver/build"
	make DESTDIR="$pkgdir/" install
    sed -e "s/\/etc\/auto.master/\/etc\/autofs\/auto.master/g" -i $pkgdir/usr/bin/cvmfs_config
    install -Dm644 "${srcdir}/${pkgname}-${pkgver}/COPYING" "${pkgdir}/usr/share/licenses/cvmfs/COPYING"

    echo "CVMFS_REPOSITORIES=atlas.cern.ch,atlas-condb.cern.ch,grid.cern.ch,sft.cern.ch,lhcb.cern.ch,lhcbdev.cern.ch" > $pkgdir/etc/cvmfs/default.local
    echo "CVMFS_HTTP_PROXY=DIRECT" >> $pkgdir/etc/cvmfs/default.local
}
