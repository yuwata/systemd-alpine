# Maintainer: Yu Watanabe <watanabe.yu+github@gmail.com>
pkgname=systemd
pkgver=259
pkgrel=0
pkgdesc="System and Service Manager"
url="https://systemd.io"
arch="all"
license="LGPL-2.1-or-later AND MIT AND GPL-2.0-or-later"

builddir=$srcdir/build

makedepends="
        acl
        acl-dev
        agetty
        alpine-sdk
        audit-dev
        bash
        bash-completion-dev
        bpftool
        build-base
        bzip2-dev
        coreutils
        coreutils-env
        cryptsetup-dev
        curl-dev
        dbus
        dbus-dev
        elfutils-dev
        gettext-dev
        glib-dev
        gnutls-dev
        gperf
        grep
        iproute2
        iptables-dev
        kbd
        kexec-tools
        kmod
        kmod-dev
        libapparmor-dev
        libarchive-dev
        libbpf-dev
        libcap-dev
        libcap-utils
        libfido2-dev
        libgcrypt-dev
        libidn2-dev
        libmicrohttpd-dev
        libpwquality-dev
        libqrencode-dev
        libseccomp-dev
        libselinux-dev
        libxkbcommon-dev
        linux-pam-dev
        lz4-dev
        meson
        openssl-dev
        p11-kit-dev
        pcre2-dev
        polkit-dev
        py3-elftools
        py3-jinja2
        py3-pefile
        py3-pytest
        py3-lxml
        quota-tools
        rsync
        sed
        sfdisk
        tpm2-tss-dev
        tpm2-tss-esys
        tpm2-tss-rc
        tpm2-tss-tcti-device
        tzdata
        util-linux-dev
        util-linux-login
        util-linux-misc
        valgrind-dev
        xen-dev
        zlib-dev
        zstd-dev
"

# For some reasons shell completion packages cannot be installed,
# and installation failed with the following:
# ======
# (209/224) Installing systemd-bash-completion (258_rc4-r132)
# ERROR: systemd-bash-completion-258_rc4-r132: package mentioned in index not found (try 'apk update')
# (219/224) Installing systemd-zsh-completion (258_rc4-r132)
# ERROR: systemd-zsh-completion-258_rc4-r132: package mentioned in index not found (try 'apk update')
# ======
# Maybe a bug in 'apk index' command?
# When it is fixed, please add the following two into $subpackages:
#        $pkgname-bash-completion
#        $pkgname-zsh-completion
subpackages="
        $pkgname-boot
        $pkgname-container
        $pkgname-dbg
        $pkgname-dev
        $pkgname-doc
        $pkgname-journal-remote
        $pkgname-libs
        $pkgname-lang
        $pkgname-tests
        $pkgname-ukify
"

build() {
        meson setup \
              --werror \
              -Dlibc=musl \
              -Dinstall-tests=true \
              $builddir $srcdir
        meson compile -C $builddir
}

check() {
        meson test --print-errorlogs -C $builddir
}

package() {
        depends="
                $pkgname-libs
                !openrc
                acl-libs
                agetty
                audit-libs
                bash
                coreutils
                cryptsetup-libs
                dbus
                grep
                kbd
                kmod
                kmod-libs
                less
                libarchive
                libblkid
                libbpf
                libcap2
                libcrypto3
                libdw
                libelf
                libfdisk
                libidn2
                libmount
                linux-pam
                libpwquality
                libqrencode
                libseccomp
                libselinux
                libssl3
                libxkbcommon
                mount
                pcre2
                umount
                musl-utils
                util-linux-login
        "

        provides="
                elogind
                eudev
                eudev-libs
                resolvconf
                udev
        "
        provider_priority=100

        DESTDIR="$pkgdir" meson install -C $builddir
}

dev() {
        provides="
                eudev-dev
                udev-dev
        "

        provider_priority=100

        default_dev
}

boot() {
        amove usr/lib/systemd/boot/efi/addonx64.efi.stub
        amove usr/lib/systemd/boot/efi/linuxx64.efi.stub
        amove usr/lib/systemd/boot/efi/systemd-bootx64.efi
}

container() {
        depends="$pkgname"

        amove usr/bin/importctl
        amove usr/bin/machinectl
        amove usr/bin/portablectl
        amove usr/bin/systemd-dissect
        amove usr/bin/systemd-nspawn
        amove usr/bin/systemd-vmspawn
        amove usr/lib/systemd/import-pubring.pgp
        amove usr/lib/systemd/system-generators/systemd-import-generator
        amove usr/lib/systemd/system/dbus-org.freedesktop.import1.service
        amove usr/lib/systemd/system/dbus-org.freedesktop.machine1.service
        amove usr/lib/systemd/system/machine.slice
        amove usr/lib/systemd/system/machines.target
        amove usr/lib/systemd/system/machines.target.wants
        amove usr/lib/systemd/system/remote-fs.target.wants/var-lib-machines.mount
        amove usr/lib/systemd/system/sockets.target.wants/systemd-importd.socket
        amove usr/lib/systemd/system/sockets.target.wants/systemd-machined.socket
        amove usr/lib/systemd/system/systemd-importd.service
        amove usr/lib/systemd/system/systemd-importd.socket
        amove usr/lib/systemd/system/systemd-machined.service
        amove usr/lib/systemd/system/systemd-machined.socket
        amove usr/lib/systemd/system/systemd-mountfsd.service
        amove usr/lib/systemd/system/systemd-mountfsd.socket
        amove usr/lib/systemd/system/systemd-nspawn@.service
        amove usr/lib/systemd/system/systemd-nsresourced.service
        amove usr/lib/systemd/system/systemd-nsresourced.socket
        amove usr/lib/systemd/system/systemd-vmspawn@.service
        amove usr/lib/systemd/system/var-lib-machines.mount
        amove usr/lib/systemd/systemd-export
        amove usr/lib/systemd/systemd-import
        amove usr/lib/systemd/systemd-import-fs
        amove usr/lib/systemd/systemd-importd
        amove usr/lib/systemd/systemd-machined
        amove usr/lib/systemd/systemd-mountfsd
        amove usr/lib/systemd/systemd-mountwork
        amove usr/lib/systemd/systemd-nsresourced
        amove usr/lib/systemd/systemd-nsresourcework
        amove usr/lib/systemd/systemd-pull
        amove usr/lib/systemd/user/machine.slice
        amove usr/lib/systemd/user/machines.target
        amove usr/lib/systemd/user/systemd-nspawn@.service
        amove usr/lib/systemd/user/systemd-vmspawn@.service
        amove usr/lib/tmpfiles.d/systemd-nspawn.conf
        amove usr/sbin/mount.ddi
        amove usr/share/dbus-1/interfaces/org.freedesktop.import1.Manager.xml
        amove usr/share/dbus-1/interfaces/org.freedesktop.import1.Transfer.xml
        amove usr/share/dbus-1/interfaces/org.freedesktop.machine1.Image.xml
        amove usr/share/dbus-1/interfaces/org.freedesktop.machine1.Machine.xml
        amove usr/share/dbus-1/interfaces/org.freedesktop.machine1.Manager.xml
        amove usr/share/dbus-1/system-services/org.freedesktop.import1.service
        amove usr/share/dbus-1/system-services/org.freedesktop.machine1.service
        amove usr/share/dbus-1/system.d/org.freedesktop.import1.conf
        amove usr/share/dbus-1/system.d/org.freedesktop.machine1.conf
        amove usr/share/polkit-1/actions/org.freedesktop.import1.policy
        amove usr/share/polkit-1/actions/org.freedesktop.machine1.policy
}

remote() {
        depends="$pkgname"

        amove etc/systemd/journal-remote.conf
        amove etc/systemd/journal-upload.conf
        amove usr/lib/systemd/system/systemd-journal-gatewayd.service
        amove usr/lib/systemd/system/systemd-journal-gatewayd.socket
        amove usr/lib/systemd/system/systemd-journal-remote.service
        amove usr/lib/systemd/system/systemd-journal-remote.socket
        amove usr/lib/systemd/system/systemd-journal-upload.service
        amove usr/lib/systemd/systemd-journal-gatewayd
        amove usr/lib/systemd/systemd-journal-remote
        amove usr/lib/systemd/systemd-journal-upload
        amove usr/lib/sysusers.d/systemd-remote.conf
        amove usr/share/systemd/gatewayd
        amove var/log/journal/remote
}

libs() {
       amove usr/lib/libsystemd.so.0
       amove usr/lib/libsystemd.so.0.41.0
       amove usr/lib/libudev.so.1
       amove usr/lib/libudev.so.1.7.11
}

tests() {
        depends="
                $pkgname
                python3
                util-linux-misc
        "

        amove usr/lib/systemd/tests
}

ukify() {
        depends="
                py3-cryptography
                py3-pefile
                py3-zstandard
                python3
        "

        amove usr/bin/ukify
        amove usr/lib/kernel/install.d/60-ukify.install
        amove usr/lib/systemd/ukify
}
