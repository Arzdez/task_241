# Дз 1

## 1. Установка пакетов

Установить пакеты `samba`, `bind`, `bind-utils` и питон-модуль `python3-module-requests`.

Установка всех пакетов:

```bash
sudo apt-get install samba bind bind-utils python3-module-requests
```

Вывод терминала:

```text 
Reading Package Lists... Done
Building Dependency Tree... Done
The following extra packages will be installed:
  bind-control  glibc-gconv-modules  glibc-nss         glibc-pthread  libavahi  libcrypto3  libicu78     libjemalloc2  libldb      libsasl2-3  libtalloc  libtevent  libwbclient  openldap-common  samba-common-client  samba-dcerpc  tdb-utils
  glibc-core    glibc-locales        glibc-preinstall  glibc-utils    libbind   libcups     libjansson4  libldap2      libnghttp2  libssl3     libtdb     libuv      logrotate    samba-common     samba-common-libs    samba-libs
The following packages will be upgraded
  glibc-core  glibc-gconv-modules  glibc-locales  glibc-nss  glibc-preinstall  glibc-pthread  glibc-utils  libcrypto3  libssl3  python3-module-requests
The following NEW packages will be installed:
  bind  bind-control  bind-utils  libavahi  libbind  libcups  libicu78  libjansson4  libjemalloc2  libldap2  libldb  libnghttp2  libsasl2-3  libtalloc  libtdb  libtevent  libuv  libwbclient  logrotate  openldap-common  samba  samba-common  samba-common-client  samba-common-libs  samba-dcerpc  samba-libs  tdb-utils
10 upgraded, 27 newly installed, 0 removed and 172 not upgraded.
Need to get 42.1MB of archives.
After unpacking 82.9MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ftp.altlinux.org Sisyphus/noarch/classic bind-control 1.3-alt1:sisyphus+252433.440.3.1@1591341818 [3431B]
Get:2 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-preinstall 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [287kB]
Get:3 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-core 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [1776kB]
Get:4 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-pthread 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [63.7kB]
Get:5 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-locales 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [14.7MB]
Get:6 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-nss 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [74.3kB]
Get:7 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-gconv-modules 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [1155kB]
Get:8 http://ftp.altlinux.org Sisyphus/x86_64/classic glibc-utils 6:2.43.0.35.fe0ccc-alt1:sisyphus+433030.200.4.1@1789537105 [443kB]
Get:9 http://ftp.altlinux.org Sisyphus/x86_64/classic libssl3 3.5.8-alt1:sisyphus+430919.100.1.1@1787772100 [386kB]
Get:10 http://ftp.altlinux.org Sisyphus/x86_64/classic libcrypto3 3.5.8-alt1:sisyphus+430919.100.1.1@1787772100 [2038kB]
Get:11 http://ftp.altlinux.org Sisyphus/x86_64/classic libjemalloc2 5.3.0-alt2.1:sisyphus+401281.100.1.1@1764331556 [216kB]
Get:12 http://ftp.altlinux.org Sisyphus/x86_64/classic libnghttp2 1.70.0-alt1:sisyphus+428572.100.1.1@1786002363 [63.7kB]
Get:13 http://ftp.altlinux.org Sisyphus/x86_64/classic libuv 1.52.1-alt2:sisyphus+414633.100.1.1@1775679992 [78.1kB]
Get:14 http://ftp.altlinux.org Sisyphus/x86_64/classic libbind 9.18.50-alt1:sisyphus+421671.100.2.1@1781709446 [1201kB]
Get:15 http://ftp.altlinux.org Sisyphus/x86_64/classic bind-utils 9.18.50-alt1:sisyphus+421671.100.2.1@1781709446 [185kB]
Get:16 http://ftp.altlinux.org Sisyphus/x86_64/classic libavahi 0.8-alt5:sisyphus+418729.400.3.1@1779276975 [242kB]
Get:17 http://ftp.altlinux.org Sisyphus/x86_64/classic libcups 2.4.19-alt3:sisyphus+433976.100.1.1@1790152204 [277kB]
Get:18 http://ftp.altlinux.org Sisyphus/x86_64/classic libicu78 1:7.8.3-alt1:sisyphus+427160.100.1.1@1785153798 [10.0MB]
Get:19 http://ftp.altlinux.org Sisyphus/x86_64/classic libjansson4 2.15.1-alt1:sisyphus+423684.100.1.1@1782991828 [57.4kB]
Get:20 http://ftp.altlinux.org Sisyphus/x86_64/classic libsasl2-3 2.1.28-alt6:sisyphus+433745.100.1.1@1789985808 [199kB]
Get:21 http://ftp.altlinux.org Sisyphus/noarch/classic openldap-common 2.6.13-alt1:sisyphus+411252.300.5.1@1773745238 [60.7kB]
Get:22 http://ftp.altlinux.org Sisyphus/x86_64/classic libldap2 2.6.13-alt1:sisyphus+411252.300.5.1@1773745238 [208kB]
Get:23 http://ftp.altlinux.org Sisyphus/x86_64/classic libtdb 1.4.13-alt1.1:sisyphus+426462.700.10.2@1785773015 [48.9kB]
Get:24 http://ftp.altlinux.org Sisyphus/x86_64/classic libtalloc 1:2.4.3-alt1.1:sisyphus+426462.600.10.2@1785772955 [25.5kB]
Get:25 http://ftp.altlinux.org Sisyphus/x86_64/classic libtevent 0.16.2-alt1.1:sisyphus+426462.1000.10.2@1785773061 [40.5kB]
Get:26 http://ftp.altlinux.org Sisyphus/x86_64/classic libldb 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [218kB]
Get:27 http://ftp.altlinux.org Sisyphus/x86_64/classic libwbclient 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [127kB]
Get:28 http://ftp.altlinux.org Sisyphus/noarch/classic python3-module-requests 2.34.2-alt1:sisyphus+418161.200.3.1@1779090568 [148kB]
Get:29 http://ftp.altlinux.org Sisyphus/noarch/classic samba-common-client 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [211kB]
Get:30 http://ftp.altlinux.org Sisyphus/x86_64/classic samba-common-libs 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [4133kB]
Get:31 http://ftp.altlinux.org Sisyphus/x86_64/classic samba-libs 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [1211kB]
Get:32 http://ftp.altlinux.org Sisyphus/x86_64/classic samba-dcerpc 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [730kB]
Get:33 http://ftp.altlinux.org Sisyphus/x86_64/classic tdb-utils 1.4.13-alt1.1:sisyphus+426462.700.10.2@1785773015 [26.8kB]
Get:34 http://ftp.altlinux.org Sisyphus/x86_64/classic bind 9.18.50-alt1:sisyphus+421671.100.2.1@1781709446 [392kB]
Get:35 http://ftp.altlinux.org Sisyphus/x86_64/classic logrotate 3.20.1-alt2:sisyphus+321776.100.2.1@1684960309 [61.9kB]
Get:36 http://ftp.altlinux.org Sisyphus/noarch/classic samba-common 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [196kB]
Get:37 http://ftp.altlinux.org Sisyphus/x86_64/classic samba 4.22.11-alt3:sisyphus+431109.200.2.1@1787956896 [792kB]
Fetched 42.1MB in 1s (26.1MB/s)
Committing changes...
Preparing...                                                                                                                                                                                                                                                    #################################################################################################### [100%]
Updating / installing...
 1: samba-common-client-4.22.11-alt3                                                                                                                                                                                                                            #################################################################################################### [  2%]
 2: openldap-common-2.6.13-alt1                                                                                                                                                                                                                                 #################################################################################################### [  4%]
 3: glibc-preinstall-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                    #################################################################################################### [  6%]
 4: glibc-core-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                          warning: /etc/nsswitch.conf created as /etc/nsswitch.conf.rpmnew
#################################################################################################### [  9%]
 5: libcrypto3-3.5.8-alt1                                                                                                                                                                                                                                       #################################################################################################### [ 11%]
 6: libtalloc-1:2.4.3-alt1.1                                                                                                                                                                                                                                    #################################################################################################### [ 13%]
 7: libtevent-0.16.2-alt1.1                                                                                                                                                                                                                                     #################################################################################################### [ 15%]
 8: libtdb-1.4.13-alt1.1                                                                                                                                                                                                                                        #################################################################################################### [ 17%]
 9: glibc-pthread-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                       #################################################################################################### [ 19%]
10: libssl3-3.5.8-alt1                                                                                                                                                                                                                                          #################################################################################################### [ 21%]
11: libjemalloc2-5.3.0-alt2.1                                                                                                                                                                                                                                   #################################################################################################### [ 23%]
12: libnghttp2-1.70.0-alt1                                                                                                                                                                                                                                      #################################################################################################### [ 26%]
13: libavahi-0.8-alt5                                                                                                                                                                                                                                           #################################################################################################### [ 28%]
14: libjansson4-2.15.1-alt1                                                                                                                                                                                                                                     #################################################################################################### [ 30%]
15: libcups-2.4.19-alt3                                                                                                                                                                                                                                         #################################################################################################### [ 32%]
16: libldb-4.22.11-alt3                                                                                                                                                                                                                                         #################################################################################################### [ 34%]
17: tdb-utils-1.4.13-alt1.1                                                                                                                                                                                                                                     #################################################################################################### [ 36%]
18: libsasl2-3-2.1.28-alt6                                                                                                                                                                                                                                      #################################################################################################### [ 38%]
19: libldap2-2.6.13-alt1                                                                                                                                                                                                                                        #################################################################################################### [ 40%]
20: libuv-1.52.1-alt2                                                                                                                                                                                                                                           #################################################################################################### [ 43%]
21: libbind-9.18.50-alt1                                                                                                                                                                                                                                        #################################################################################################### [ 45%]
22: libicu78-1:7.8.3-alt1                                                                                                                                                                                                                                       #################################################################################################### [ 47%]
23: libwbclient-4.22.11-alt3                                                                                                                                                                                                                                    #################################################################################################### [ 49%]
24: samba-common-libs-4.22.11-alt3                                                                                                                                                                                                                              #################################################################################################### [ 51%]
25: samba-libs-4.22.11-alt3                                                                                                                                                                                                                                     #################################################################################################### [ 53%]
26: samba-dcerpc-4.22.11-alt3                                                                                                                                                                                                                                   #################################################################################################### [ 55%]
27: logrotate-3.20.1-alt2                                                                                                                                                                                                                                       #################################################################################################### [ 57%]
28: samba-common-4.22.11-alt3                                                                                                                                                                                                                                   #################################################################################################### [ 60%]
29: bind-control-1.3-alt1                                                                                                                                                                                                                                       #################################################################################################### [ 62%]
30: bind-9.18.50-alt1                                                                                                                                                                                                                                           #################################################################################################### [ 64%]
31: samba-4.22.11-alt3                                                                                                                                                                                                                                          #################################################################################################### [ 66%]
32: bind-utils-9.18.50-alt1                                                                                                                                                                                                                                     #################################################################################################### [ 68%]
33: glibc-locales-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                       #################################################################################################### [ 70%]
34: glibc-nss-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                           #################################################################################################### [ 72%]
35: glibc-gconv-modules-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                 #################################################################################################### [ 74%]
36: glibc-utils-6:2.43.0.35.fe0ccc-alt1                                                                                                                                                                                                                         #################################################################################################### [ 77%]
37: python3-module-requests-2.34.2-alt1                                                                                                                                                                                                                         #################################################################################################### [ 79%]
Cleaning up / removing...
38: libssl3-3.3.3-alt1                                                                                                                                                                                                                                          #################################################################################################### [ 81%]
39: libcrypto3-3.3.3-alt1                                                                                                                                                                                                                                       #################################################################################################### [ 83%]
40: glibc-utils-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                        #################################################################################################### [ 85%]
41: glibc-gconv-modules-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                #################################################################################################### [ 87%]
42: glibc-nss-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                          #################################################################################################### [ 89%]
43: glibc-locales-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                      #################################################################################################### [ 91%]
44: glibc-pthread-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                      #################################################################################################### [ 94%]
45: glibc-core-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                         #################################################################################################### [ 96%]
46: glibc-preinstall-6:2.40.0.142.2eb180-alt1                                                                                                                                                                                                                   #################################################################################################### [ 98%]
47: python3-module-requests-2.32.5-alt1                                                                                                                                                                                                                         #################################################################################################### [100%]
Done.
```

---

## 2. Для каждого установленного пакета вывести: 2.1) версию; 2.2) список файлов, принадлежащих пакету; 2.3) зависимости пакета.

### Для samba:

Версия:

```bash
rpm -qi samba
```
Вывод терминала: 

```text
Name        : samba
Version     : 4.22.11
Release     : alt3
DistTag     : sisyphus+431109.200.2.1
Architecture: x86_64
Install Date: Sun 27 Sep 2026 08:22:44 PM UTC
Group       : System/Servers
Size        : 3195632
License     : GPLv3+ and LGPLv3+
Signature   : RSA/SHA512, Fri 28 Aug 2026 10:42:58 PM UTC, Key ID ff979dedda2773bb
Source RPM  : samba-4.22.11-alt3.src.rpm
Build Date  : Fri 28 Aug 2026 10:41:36 PM UTC
Build Host  : sin-sisyphus.hasher.altlinux.org
Relocations : (not relocatable)
Packager    : Evgeny Sinelnikov (Sin) <sin@altlinux.org>
Vendor      : ALT Linux Team
URL         : http://www.samba.org/
Summary     : The Samba4 CIFS and AD client and server suite
Description :
Samba is the standard Windows interoperability suite of programs for Linux and Unix.
```
Таким образом, версия установленного пакета - 4.22.11

Cписок файлов, принадлежащих пакету:

```bash
rpm -ql samba
```
Вывод терминала:

```text
/etc/alternatives/packages.d/samba-mit
/etc/rc.d/init.d/nmb
/etc/rc.d/init.d/smb
/etc/samba/smbusers
/usr/lib/samba/samba-bgqd
/usr/lib/systemd/system/nmb.service
/usr/lib/systemd/system/samba-bgqd.service
/usr/lib/systemd/system/smb.service
/usr/lib64/samba/auth
/usr/lib64/samba/auth/unix.so
/usr/lib64/samba/sbin/eventlogadm
/usr/lib64/samba/sbin/nmbd
/usr/lib64/samba/sbin/smbd
/usr/lib64/samba/vfs
/usr/lib64/samba/vfs/acl_tdb.so
/usr/lib64/samba/vfs/acl_xattr.so
/usr/lib64/samba/vfs/aio_fork.so
/usr/lib64/samba/vfs/aio_pthread.so
/usr/lib64/samba/vfs/audit.so
/usr/lib64/samba/vfs/btrfs.so
/usr/lib64/samba/vfs/cap.so
/usr/lib64/samba/vfs/catia.so
/usr/lib64/samba/vfs/commit.so
/usr/lib64/samba/vfs/crossrename.so
/usr/lib64/samba/vfs/default_quota.so
/usr/lib64/samba/vfs/dirsort.so
/usr/lib64/samba/vfs/expand_msdfs.so
/usr/lib64/samba/vfs/extd_audit.so
/usr/lib64/samba/vfs/fake_perms.so
/usr/lib64/samba/vfs/fileid.so
/usr/lib64/samba/vfs/fruit.so
/usr/lib64/samba/vfs/full_audit.so
/usr/lib64/samba/vfs/glusterfs_fuse.so
/usr/lib64/samba/vfs/gpfs.so
/usr/lib64/samba/vfs/linux_xfs_sgid.so
/usr/lib64/samba/vfs/media_harmony.so
/usr/lib64/samba/vfs/offline.so
/usr/lib64/samba/vfs/posix_eadb.so
/usr/lib64/samba/vfs/preopen.so
/usr/lib64/samba/vfs/readahead.so
/usr/lib64/samba/vfs/readonly.so
/usr/lib64/samba/vfs/recycle.so
/usr/lib64/samba/vfs/shadow_copy.so
/usr/lib64/samba/vfs/shadow_copy2.so
/usr/lib64/samba/vfs/shell_snap.so
/usr/lib64/samba/vfs/streams_depot.so
/usr/lib64/samba/vfs/streams_xattr.so
/usr/lib64/samba/vfs/syncops.so
/usr/lib64/samba/vfs/time_audit.so
/usr/lib64/samba/vfs/unityed_media.so
/usr/lib64/samba/vfs/virusfilter.so
/usr/lib64/samba/vfs/widelinks.so
/usr/lib64/samba/vfs/worm.so
/usr/lib64/samba/vfs/xattr_tdb.so
/usr/share/doc/samba-4.22.11
/usr/share/doc/samba-4.22.11/COPYING
/usr/share/doc/samba-4.22.11/LDAP
/usr/share/doc/samba-4.22.11/LDAP/README
/usr/share/doc/samba-4.22.11/LDAP/get_next_oid
/usr/share/doc/samba-4.22.11/LDAP/ol-schema-migrate.pl
/usr/share/doc/samba-4.22.11/LDAP/samba-nds.schema
/usr/share/doc/samba-4.22.11/LDAP/samba-schema-FDS.ldif
/usr/share/doc/samba-4.22.11/LDAP/samba-schema-netscapeds5.x.README
/usr/share/doc/samba-4.22.11/LDAP/samba-schema.IBMSecureWay
/usr/share/doc/samba-4.22.11/LDAP/samba.ldif
/usr/share/doc/samba-4.22.11/LDAP/samba.schema
/usr/share/doc/samba-4.22.11/LDAP/samba.schema.at.IBM-DS
/usr/share/doc/samba-4.22.11/LDAP/samba.schema.oc.IBM-DS
/usr/share/doc/samba-4.22.11/README.downgrade
/usr/share/doc/samba-4.22.11/README.md
/usr/share/doc/samba-4.22.11/WHATSNEW.txt
/usr/share/doc/samba-4.22.11/autofs
/usr/share/doc/samba-4.22.11/autofs/auto.smb
/usr/share/doc/samba-4.22.11/misc
/usr/share/doc/samba-4.22.11/misc/adssearch.pl
/usr/share/doc/samba-4.22.11/misc/check_multiple_LDAP_entries.pl
/usr/share/doc/samba-4.22.11/misc/cldap.pl
/usr/share/doc/samba-4.22.11/misc/extra_smbstatus
/usr/share/doc/samba-4.22.11/misc/wall.perl
/usr/share/doc/samba-4.22.11/printer-accounting
/usr/share/doc/samba-4.22.11/printer-accounting/README
/usr/share/doc/samba-4.22.11/printer-accounting/acct-all
/usr/share/doc/samba-4.22.11/printer-accounting/acct-sum
/usr/share/doc/samba-4.22.11/printer-accounting/hp5-redir
/usr/share/doc/samba-4.22.11/printer-accounting/lp-acct
/usr/share/doc/samba-4.22.11/printer-accounting/printcap
/usr/share/doc/samba-4.22.11/printing
/usr/share/doc/samba-4.22.11/printing/VampireDriversFunctions
/usr/share/doc/samba-4.22.11/printing/prtpub.c
/usr/share/doc/samba-4.22.11/printing/readme.prtpub
/usr/share/doc/samba-4.22.11/printing/smbprint.sysv
/var/lib/samba/drivers
```
Тут лежат исполняемые файлы пакета и другие нужные файлы

Зависимости пакета:

```bash
rpm -q --requires samba
```
Вывод:

```text
tdb-utils
/bin/sh
/bin/kill
/bin/sh
/etc/alternatives/packages.d
/etc/rc.d/init.d
/etc/rc.d/init.d(SourceIfNotEmpty)
/etc/rc.d/init.d(is_yes)
/etc/rc.d/init.d(msg_reloading)
/etc/rc.d/init.d(msg_starting)
/etc/rc.d/init.d(msg_stopping)
/etc/rc.d/init.d(msg_usage)
/etc/rc.d/init.d(start_daemon)
/etc/rc.d/init.d(status)
/etc/rc.d/init.d(stop_daemon)
/etc/rc.d/init.d/functions
libtalloc.so.2()(64bit) >= set:kfUrxAnXaOTGSosxKXSVLwqsa78TkZlQPFQ3zWTz56hOa1A89EoGyRnNddviHp
libtdb.so.1()(64bit) >= set:kfcmIl2Mp8l89Vmmgka3Qj9iMmbXrBcQZGqx8qAusrTkE3
libtevent.so.0()(64bit) >= set:lg3zbkGQzCQjO2u7MamWvXFSrWMOgJ5zp8O5NKSBPC4Cq6VjlRVbScXfixQ
/usr/sbin/post_service
/usr/sbin/preun_service
samba-common = 4.22.11-alt3:sisyphus+431109.200.2.1
samba-dcerpc = 4.22.11-alt3:sisyphus+431109.200.2.1
samba-common = 4.22.11-alt3:sisyphus+431109.200.2.1
rpmlib(PayloadIsLzma)
```
### Для bind:

Версия:

```bash
rpm -qi bind
```
Вывод терминала: 

```text
Name        : bind
Version     : 9.18.50
Release     : alt1
DistTag     : sisyphus+421671.100.2.1
Architecture: x86_64
Install Date: Sun 27 Sep 2026 08:22:42 PM UTC
Group       : System/Servers
Size        : 1365098
License     : MPL-2.0
Signature   : RSA/SHA512, Wed 17 Jun 2026 03:17:34 PM UTC, Key ID ff979dedda2773bb
Source RPM  : bind-9.18.50-alt1.src.rpm
Build Date  : Wed 17 Jun 2026 03:17:26 PM UTC
Build Host  : slev-sisyphus.hasher.altlinux.org
Relocations : (not relocatable)
Packager    : Stanislav Levin <slev@altlinux.org>
Vendor      : ALT Linux Team
URL         : https://www.isc.org/bind/
Summary     : ISC BIND - DNS server
Description :
The Berkeley Internet Name Domain (BIND) implements an Internet domain
name server.  BIND is the most widely-used name server software on the
Internet, and is supported by the Internet Software Consortium (ISC).

This package provides the 9.18.50 server and related
configuration files.
```
Таким образом, версия установленного пакета - 9.18.50

Cписок файлов, принадлежащих пакету

```bash
rpm -ql bind
```
Вывод терминала:

```text
/etc/bind
/etc/bind.keys
/etc/named.conf
/etc/rc.d/init.d/bind
/etc/rndc.conf
/etc/sysconfig/bind
/etc/syslog.d/bind
/run/named
/usr/bin/arpaname
/usr/bin/dnssec-cds
/usr/bin/dnssec-dsfromkey
/usr/bin/dnssec-importkey
/usr/bin/dnssec-keyfromlabel
/usr/bin/dnssec-keygen
/usr/bin/dnssec-revoke
/usr/bin/dnssec-settime
/usr/bin/dnssec-signzone
/usr/bin/dnssec-verify
/usr/bin/named-checkconf
/usr/bin/named-checkzone
/usr/bin/named-compilezone
/usr/bin/named-journalprint
/usr/bin/named-rrchecker
/usr/bin/nsec3hash
/usr/lib/rpm/bind-restart.filetrigger
/usr/lib/systemd/system/bind.service
/usr/lib/tmpfiles.d/bind.conf
/usr/lib64/bind
/usr/lib64/bind/filter-a.so
/usr/lib64/bind/filter-aaaa.so
/usr/lib64/named
/usr/sbin/ddns-confgen
/usr/sbin/named
/usr/sbin/rndc
/usr/sbin/rndc-confgen
/usr/sbin/tsig-keygen
/usr/share/doc/bind-9.18.50
/usr/share/doc/bind-9.18.50/README.ALT
/usr/share/doc/bind-9.18.50/README.md
/usr/share/man/man1/arpaname.1.xz
/usr/share/man/man1/dnssec-cds.1.xz
/usr/share/man/man1/dnssec-dsfromkey.1.xz
/usr/share/man/man1/dnssec-importkey.1.xz
/usr/share/man/man1/dnssec-keyfromlabel.1.xz
/usr/share/man/man1/dnssec-keygen.1.xz
/usr/share/man/man1/dnssec-revoke.1.xz
/usr/share/man/man1/dnssec-settime.1.xz
/usr/share/man/man1/dnssec-signzone.1.xz
/usr/share/man/man1/dnssec-verify.1.xz
/usr/share/man/man1/named-checkconf.1.xz
/usr/share/man/man1/named-checkzone.1.xz
/usr/share/man/man1/named-compilezone.1.xz
/usr/share/man/man1/named-journalprint.1.xz
/usr/share/man/man1/named-rrchecker.1.xz
/usr/share/man/man1/nsec3hash.1.xz
/usr/share/man/man5/named.conf.5.xz
/usr/share/man/man5/rndc.conf.5.xz
/usr/share/man/man8/ddns-confgen.8.xz
/usr/share/man/man8/filter-a.8.xz
/usr/share/man/man8/filter-aaaa.8.xz
/usr/share/man/man8/named.8.xz
/usr/share/man/man8/rndc-confgen.8.xz
/usr/share/man/man8/rndc.8.xz
/usr/share/man/man8/tsig-keygen.8.xz
/var/lib/bind
/var/lib/bind/dev
/var/lib/bind/dev/log
/var/lib/bind/etc
/var/lib/bind/etc/bind
/var/lib/bind/etc/bind.keys
/var/lib/bind/etc/local.conf
/var/lib/bind/etc/named.conf
/var/lib/bind/etc/options.conf
/var/lib/bind/etc/rfc1912.conf
/var/lib/bind/etc/rfc1918.conf
/var/lib/bind/etc/rndc.conf
/var/lib/bind/etc/rndc.key
/var/lib/bind/etc/zone
/var/lib/bind/session
/var/lib/bind/var
/var/lib/bind/var/log
/var/lib/bind/var/log/named
/var/lib/bind/var/run
/var/lib/bind/var/run/named
/var/lib/bind/zone
/var/lib/bind/zone/127.in-addr.arpa
/var/lib/bind/zone/empty
/var/lib/bind/zone/localdomain
/var/lib/bind/zone/localhost
/var/lib/bind/zone/slave
/var/log/named
```
Тут лежат исполняемые файлы пакета и другие нужные файлы

Зависимости пакета:

```bash
rpm -q --requires bind
```
Вывод:

```text
bind-control >= 1.3
/bin/sh
/bin/sh
/etc/rc.d/init.d
/etc/rc.d/init.d(SourceIfNotEmpty)
/etc/rc.d/init.d(action)
/etc/rc.d/init.d(failure)
/etc/rc.d/init.d(is_yes)
/etc/rc.d/init.d(msg_reloading)
/etc/rc.d/init.d(msg_usage)
/etc/rc.d/init.d(start_daemon)
/etc/rc.d/init.d(status)
/etc/rc.d/init.d(stop_daemon)
/etc/rc.d/init.d(success)
/etc/rc.d/init.d/functions
/lib64/ld-linux-x86-64.so.2
coreutils
grep
libcap.so.2()(64bit) >= set:kgzEnqa243uKaZoevXMvRH1
libcap.so.2(LIBCAP_1.10)(64bit)
libcap.so.2(LIBCAP_2.21)(64bit)
libcrypto.so.3()(64bit) >= set:qpdXyh
libjemalloc.so.2()(64bit) >= set:jgo00eJYaC870
libjson-c.so.5()(64bit) >= set:lhaJLNNHpDhhnQpG3JrrIq7k7gBMslQ
libnghttp2.so.14()(64bit) >= set:lk6bf0
rpmlib(PosttransFiletriggers)
service
/etc/rc.d/init.d/bind
/sbin/chkconfig
/usr/sbin/control-dump
/usr/sbin/groupadd
/usr/sbin/useradd
coreutils
/usr/sbin/control
/usr/sbin/control-restore
chkconfig
grep
service
sed
/usr/sbin/preun_service
libbind = 9.18.50-alt1:sisyphus+421671.100.2.1
rpmlib(PayloadIsLzma)
[student@student-04 ~]$ rpm -qi bind-utils
Name        : bind-utils
Version     : 9.18.50
Release     : alt1
DistTag     : sisyphus+421671.100.2.1
Architecture: x86_64
Install Date: Sun 27 Sep 2026 08:22:47 PM UTC
Group       : Networking/Other
Size        : 602572
License     : MPL-2.0
Signature   : RSA/SHA512, Wed 17 Jun 2026 03:17:33 PM UTC, Key ID ff979dedda2773bb
Source RPM  : bind-9.18.50-alt1.src.rpm
Build Date  : Wed 17 Jun 2026 03:17:26 PM UTC
Build Host  : slev-sisyphus.hasher.altlinux.org
Relocations : (not relocatable)
Packager    : Stanislav Levin <slev@altlinux.org>
Vendor      : ALT Linux Team
URL         : https://www.isc.org/bind/
Summary     : Utilities provided by ISC BIND
Description :
This package contains various utilities related to DNS that are derived
from the BIND 9.18.50 source tree, including dig, host,
nslookup and nsupdate.
```

### Для bind-utils:

Версия:

```bash
rpm -qi bind-utils
```
Вывод терминала: 

```text
Name        : bind-utils
Version     : 9.18.50
Release     : alt1
DistTag     : sisyphus+421671.100.2.1
Architecture: x86_64
Install Date: Sun 27 Sep 2026 08:22:47 PM UTC
Group       : Networking/Other
Size        : 602572
License     : MPL-2.0
Signature   : RSA/SHA512, Wed 17 Jun 2026 03:17:33 PM UTC, Key ID ff979dedda2773bb
Source RPM  : bind-9.18.50-alt1.src.rpm
Build Date  : Wed 17 Jun 2026 03:17:26 PM UTC
Build Host  : slev-sisyphus.hasher.altlinux.org
Relocations : (not relocatable)
Packager    : Stanislav Levin <slev@altlinux.org>
Vendor      : ALT Linux Team
URL         : https://www.isc.org/bind/
Summary     : Utilities provided by ISC BIND
Description :
This package contains various utilities related to DNS that are derived
from the BIND 9.18.50 source tree, including dig, host,
nslookup and nsupdate.
```
Таким образом, версия установленного пакета - 9.18.50

Cписок файлов, принадлежащих пакету

```bash
rpm -ql bind-utils
```
Вывод терминала:

```text
/usr/bin/delv
/usr/bin/dig
/usr/bin/host
/usr/bin/mdig
/usr/bin/nslookup
/usr/bin/nsupdate
/usr/share/man/man1/delv.1.xz
/usr/share/man/man1/dig.1.xz
/usr/share/man/man1/host.1.xz
/usr/share/man/man1/mdig.1.xz
/usr/share/man/man1/nslookup.1.xz
/usr/share/man/man1/nsupdate.1.xz
```
Тут лежат исполняемые файлы пакета и другие нужные файлы

Зависимости пакета:

```bash
rpm -q --requires bind-utils
```
Вывод:

```text
/lib64/ld-linux-x86-64.so.2
libidn2.so.0()(64bit) >= set:ifaF4MxNtNuGyt21
libidn2.so.0(IDN2_0.0.0)(64bit)
libkrb5.so.3()(64bit) >= set:nk6RMYKtexZFP7MfOcRD55Qc
libbind = 9.18.50-alt1:sisyphus+421671.100.2.1
rpmlib(PayloadIsLzma)
```
### Для python3-module-requests(я выбрал requests):

Версия:

```bash
rpm -qi python3-module-requests
```
Вывод терминала: 

```text
Name        : python3-module-requests
Version     : 2.34.2
Release     : alt1
DistTag     : sisyphus+418161.200.3.1
Architecture: noarch
Install Date: Sun 27 Sep 2026 08:23:06 PM UTC
Group       : Development/Python3
Size        : 752888
License     : Apache-2.0
Signature   : RSA/SHA512, Mon 18 May 2026 07:49:29 AM UTC, Key ID ff979dedda2773bb
Source RPM  : python3-module-requests-2.34.2-alt1.src.rpm
Build Date  : Mon 18 May 2026 07:49:28 AM UTC
Build Host  : slev-sisyphus.hasher.altlinux.org
Relocations : (not relocatable)
Packager    : Stanislav Levin <slev@altlinux.org>
Vendor      : ALT Linux Team
URL         : https://pypi.org/project/requests/
Summary     : HTTP library, written in Python, for human beings
Description :
Requests allows you to send HTTP/1.1 requests extremely easily. There's no need
to manually add query strings to your URLs, or to form-encode your POST data.
Keep-alive and HTTP connection pooling are 100% automatic, thanks to urllib3.
```
Таким образом, версия установленного пакета - 2.34.2

Cписок файлов, принадлежащих пакету

```bash
rpm -ql python3-module-requests
```
Вывод терминала:

```text
/usr/lib/python3/site-packages/requests
/usr/lib/python3/site-packages/requests-2.34.2.dist-info
/usr/lib/python3/site-packages/requests-2.34.2.dist-info/METADATA
/usr/lib/python3/site-packages/requests/__init__.py
/usr/lib/python3/site-packages/requests/__pycache__
/usr/lib/python3/site-packages/requests/__pycache__/__init__.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/__init__.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/__init__.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/__version__.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/__version__.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/__version__.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/_internal_utils.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/_internal_utils.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/_internal_utils.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/_types.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/_types.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/_types.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/adapters.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/adapters.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/adapters.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/api.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/api.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/api.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/auth.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/auth.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/auth.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/certs.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/certs.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/certs.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/compat.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/compat.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/compat.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/cookies.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/cookies.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/cookies.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/exceptions.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/exceptions.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/exceptions.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/help.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/help.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/help.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/hooks.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/hooks.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/hooks.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/models.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/models.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/models.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/packages.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/packages.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/packages.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/sessions.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/sessions.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/sessions.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/status_codes.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/status_codes.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/status_codes.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/structures.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/structures.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/structures.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__pycache__/utils.cpython-313.opt-1.pyc
/usr/lib/python3/site-packages/requests/__pycache__/utils.cpython-313.opt-2.pyc
/usr/lib/python3/site-packages/requests/__pycache__/utils.cpython-313.pyc
/usr/lib/python3/site-packages/requests/__version__.py
/usr/lib/python3/site-packages/requests/_internal_utils.py
/usr/lib/python3/site-packages/requests/_types.py
/usr/lib/python3/site-packages/requests/adapters.py
/usr/lib/python3/site-packages/requests/api.py
/usr/lib/python3/site-packages/requests/auth.py
/usr/lib/python3/site-packages/requests/certs.py
/usr/lib/python3/site-packages/requests/compat.py
/usr/lib/python3/site-packages/requests/cookies.py
/usr/lib/python3/site-packages/requests/exceptions.py
/usr/lib/python3/site-packages/requests/help.py
/usr/lib/python3/site-packages/requests/hooks.py
/usr/lib/python3/site-packages/requests/models.py
/usr/lib/python3/site-packages/requests/packages.py
/usr/lib/python3/site-packages/requests/py.typed
/usr/lib/python3/site-packages/requests/sessions.py
/usr/lib/python3/site-packages/requests/status_codes.py
/usr/lib/python3/site-packages/requests/structures.py
/usr/lib/python3/site-packages/requests/utils.py
```
Тут лежат исполняемые файлы пакета и другие нужные файлы

Зависимости пакета:

```bash
rpm -q --requires python3-module-requests
```
Вывод:

```text
python3-module-charset-normalizer
python3-module-idna
python3-module-urllib3
/usr/lib/python3/site-packages
rpmlib(PayloadIsLzma)
```

---

# 3.Проверить службы, относящиеся к установленным пакетам. Если нужная служба не запущена — запустить её и добавить в автозагрузку. 3.1) Можно ли включить службу и добавить её в автозагрузку одной командой?

Для проверки использовалась команда:

```bash
systemctl list-unit-files | grep -E 'smb|nmb|named'
```

Результат:

```text
nmb.service                                  disabled        disabled
smb.service                                  disabled        disabled
systemd-hostnamed.service                    static          -
systemd-hostnamed.socket                     static          -
```

Для bind сначала проверил `named`, но такой службы в системе не оказалось:

```bash
systemctl status named --no-pager
```

```text
Unit named.service could not be found.
```
Потом проверил samba через smb:
```bash
systemctl status smb --no-pager
```
```text
○ smb.service - Samba SMB Daemon
     Loaded: loaded (/usr/lib/systemd/system/smb.service; disabled; preset: disabled)
     Active: inactive (dead)
       Docs: man:smbd(8)
             man:samba(7)
             man:smb.conf(5)
```
Она оказалась загружена, но неактивна как и в первом тесте с `smb|nmb|named`

После этого нашлись службы связанные с bind:

```bash
systemctl list-unit-files | grep -Ei 'bind|dns|named'
```

Результат:

```text
bind.service                                 disabled        disabled
systemd-hostnamed.service                    static          -
systemd-hostnamed.socket                     static          -
rpcbind.target                               static          -
```

Таким образом, служба bind в системе называется `bind.service`.



# Состояние служб до запуска

## Samba SMB

Команда(из прошлого пункта):

```bash
systemctl status smb --no-pager
```

Результат:

```text
○ smb.service - Samba SMB Daemon
     Loaded: loaded (/usr/lib/systemd/system/smb.service; disabled; preset: disabled)
     Active: inactive (dead)
       Docs: man:smbd(8)
             man:samba(7)
             man:smb.conf(5)
```

Служба `smb` была неактивна.

## Samba NMB

Команда:

```bash
systemctl status nmb --no-pager
```

Результат:

```text
○ nmb.service - Samba NMB Daemon
     Loaded: loaded (/usr/lib/systemd/system/nmb.service; disabled; preset: disabled)
     Active: inactive (dead)
       Docs: man:nmbd(8)
             man:samba(7)
             man:smb.conf(5)
```

Служба `nmb` тоже была неактивна.

## 3.1. Для запуска службы и одновременного добавления её в автозагрузку надо использовать команду:

```bash
sudo systemctl enable --now 'служба'
```
Поэтому для служб:

```bash
sudo systemctl enable --now smb
```

```bash
sudo systemctl enable --now nmb
```
```bash
sudo systemctl enable --now bind 
```

Команда `systemctl enable --now` позволяет одновременно:

1. запустить службу прямо сейчас;
2. включить её автоматический запуск при загрузке ОС.

## Состояние служб после запуска

### Для bind

Команда:

```bash
systemctl status bind --no-pager
```

Результат:

```text
● bind.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/bind.service; enabled; preset: disabled)
     Active: active (running) since Sun 2026-09-27 20:48:17 UTC; 2min 13s ago
 Invocation: d621fbebbd924492bb34aa832595bb1d
      Tasks: 5 (limit: 1728)
     Memory: 30.1M (peak: 31.8M)
        CPU: 145ms
     CGroup: /system.slice/bind.service
             └─25132 /usr/sbin/named -u named

Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './DNSKEY/IN': 2001:500:1::53#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './NS/IN': 2001:500:1::53#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './DNSKEY/IN': 2001:503:c27::2:30#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './NS/IN': 2001:503:c27::2:30#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './DNSKEY/IN': 2001:500:2f::f#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './DNSKEY/IN': 2001:500:1::53#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './DNSKEY/IN': 2001:500:a8::e#53
Sep 27 20:48:17 student-04 named[25132]: network unreachable resolving './DNSKEY/IN': 2001:500:2::c#53
Sep 27 20:48:18 student-04 named[25132]: managed-keys-zone: Initializing automatic trust anchor management for zone '.'; DNSKEY ID 20326 is now trusted, waiving the normal 30-day waiting period.
Sep 27 20:48:18 student-04 named[25132]: managed-keys-zone: Initializing automatic trust anchor management for zone '.'; DNSKEY ID 38696 is now trusted, waiving the normal 30-day waiting period.
```

Служба bind успешно запущена и включена в автозагрузку.

## Samba SMB

Команда:

```bash
systemctl status smb --no-pager
```

Результат:

```text
● smb.service - Samba SMB Daemon
     Loaded: loaded (/usr/lib/systemd/system/smb.service; enabled; preset: disabled)
     Active: active (running) since Sun 2026-09-27 20:48:47 UTC; 2min 7s ago
 Invocation: fc468c8cf37b4a7e8c00a78dd1ad900c
       Docs: man:smbd(8)
             man:samba(7)
             man:smb.conf(5)
   Main PID: 25232 (smbd)
     Status: "smbd: ready to serve connections..."
      Tasks: 3 (limit: 1728)
     Memory: 40.6M (peak: 40.6M)
        CPU: 103ms
     CGroup: /system.slice/smb.service
             ├─25232 /usr/sbin/smbd --foreground --no-process-group
             ├─25235 /usr/sbin/smbd --foreground --no-process-group
             └─25236 /usr/sbin/smbd --foreground --no-process-group

Sep 27 20:48:46 student-04 systemd[1]: Starting smb.service - Samba SMB Daemon...
Sep 27 20:48:47 student-04 systemd[1]: Started smb.service - Samba SMB Daemon.
```

Служба smb успешно запущена и включена в автозагрузку.

## Samba NMB

Команда:

```bash
systemctl status nmb --no-pager
```

Результат:

```text
● nmb.service - Samba NMB Daemon
     Loaded: loaded (/usr/lib/systemd/system/nmb.service; enabled; preset: disabled)
     Active: active (running) since Sun 2026-09-27 20:50:14 UTC; 1min 1s ago
 Invocation: 86b2ea758cd94b9b8f93f64272efb1e5
       Docs: man:nmbd(8)
             man:samba(7)
             man:smb.conf(5)
   Main PID: 25333 (nmbd)
     Status: "nmbd: ready to serve connections..."
      Tasks: 1 (limit: 1728)
     Memory: 3.1M (peak: 3.4M)
        CPU: 61ms
     CGroup: /system.slice/nmb.service
             └─25333 /usr/sbin/nmbd --foreground --no-process-group

Sep 27 20:50:14 student-04 systemd[1]: Starting nmb.service - Samba NMB Daemon...
Sep 27 20:50:14 student-04 systemd[1]: Started nmb.service - Samba NMB Daemon.
```

Служба `nmb` также успешно запущена и включена в автозагрузку.

---

# 4. Конфигурационные файлы

В основной конфигурационные файлы находятся в каталоге `/etc`

## Конфигурация Samba

для Samba:

```text
/etc/samba/smb.conf
```

Для просмотра начала файла использовал команду:

```bash
sudo head -20 /etc/samba/smb.conf
```

Результат:

```text
# See smb.conf.example for a more detailed config file or
# read the smb.conf manpage.
# Run 'testparm' to verify the config is correct after
# you modified it.
#
# Note:
# SMB1 is disabled by default. This means clients without support for SMB2 or
# SMB3 are no longer able to connect to smbd (by default).

[global]
        workgroup = SAMBA
        security = user

        passdb backend = tdbsam

        printing = cups
        printcap name = cups
        load printers = yes
        cups options = raw
```

Для проверки корректности конфига была установлена утилита testparm.

Команда:

```bash
testparm -s
```

Результат содержал:

```text
Load smb config files from /etc/samba/smb.conf
Loaded services file OK.
Weak crypto is allowed by GnuTLS (e.g. NTLM as a compatibility fallback)

Server role: ROLE_STANDALONE

# Global parameters
[global]
        printcap name = cups
        security = USER
        workgroup = SAMBA
        idmap config * : backend = tdb
        cups options = raw
        include = /etc/samba/usershares.conf


[homes]
        browseable = No
        comment = Home Directories
        inherit acls = Yes
        read only = No
        valid users = %S %D%w%S


[printers]
        browseable = No
        comment = All Printers
        create mask = 0600
        path = /var/tmp
        printable = Yes


[print$]
        comment = Printer Drivers
        create mask = 0664
        directory mask = 0775
        force group = @printadmin
        path = /var/lib/samba/drivers
        write list = @printadmin root
```

Таким образом, конфигурационный файл Samba прочитан и проверен.

---

## Конфигурация BIND

В составе пакета BIND присутствуют следующие конфигурационные файлы:

```text
/etc/bind
/etc/bind.keys
/etc/named.conf
/etc/rndc.conf
/etc/sysconfig/bind
/etc/syslog.d/bind
```

Основным конфигурационным файлом bind является:

```text
/etc/named.conf
```

---

## Конфигурационные файлы bind-utils и Requests

Для `bind-utils` и `python3-module-requests` отдельные файлы в каталоге `/etc` пакетами не устанавливаются.

Проверка выполнялась командами:

```bash
rpm -ql bind-utils | grep '^/etc/'
```

и:

```bash
rpm -ql python3-module-requests | grep '^/etc/'
```

Вывода не было.

---

# 5. Для одной из служб: 5.1) показать её текущий статус; 5.2) показать последние записи из журнала; 5.3) определить, под каким пользователем она работает.

Я выбрал службу `smb.service`.

## Текущий статус службы

Команда:

```bash
systemctl status smb --no-pager
```

Результат:

```text
● smb.service - Samba SMB Daemon
     Loaded: loaded (/usr/lib/systemd/system/smb.service; enabled; preset: disabled)
     Active: active (running) since Sun 2026-09-27 20:48:47 UTC; 10min ago
 Invocation: fc468c8cf37b4a7e8c00a78dd1ad900c
       Docs: man:smbd(8)
             man:samba(7)
             man:smb.conf(5)
   Main PID: 25232 (smbd)
     Status: "smbd: ready to serve connections..."
      Tasks: 3 (limit: 1728)
     Memory: 40.6M (peak: 40.6M)
        CPU: 105ms
     CGroup: /system.slice/smb.service
             ├─25232 /usr/sbin/smbd --foreground --no-process-group
             ├─25235 /usr/sbin/smbd --foreground --no-process-group
             └─25236 /usr/sbin/smbd --foreground --no-process-group

Sep 27 20:48:46 student-04 systemd[1]: Starting smb.service - Samba SMB Daemon...
Sep 27 20:48:47 student-04 systemd[1]: Started smb.service - Samba SMB Daemon.
```

Таким образом, служба smb находится в состоянии `active (running)` и включена в автозагрузку.

---

## Последние записи журнала

Для просмотра последних 10 записей журнала использовал команду:

```bash
journalctl -u smb -n 10 --no-pager
```

Результат:

```text
Sep 27 20:48:46 student-04 systemd[1]: Starting smb.service - Samba SMB Daemon...
Sep 27 20:48:47 student-04 systemd[1]: Started smb.service - Samba SMB Daemon.
```

По журналу видно, что служба была запущена.

---

## Пользователь, от имени которого работает служба

Для определения пользователя, от имени которого работают процессы Samba, использовал команду:

```bash
ps -eo user,pid,comm | grep '[s]mbd'
```

Результат:

```text
root       25232 smbd
root       25235 smbd-notifyd
root       25236 smbd-cleanupd
```

Основной PID службы был определён командой:

```bash
systemctl show smb -p MainPID
```

Результат:

```text
MainPID=25232
```

После этого была выполнена проверка:

```bash
ps -p 25232 -o user,pid,comm
```

Результат:

```text
USER         PID COMMAND
root       25232 smbd
```

Таким образом, основной процесс `smbd` работает от имени пользователя root.

---

# 10. Вывод

Установка пакета, запуск службы и включение автозапуска являются разными операциями. Установка пакета добавляет программное обеспечение и необходимые файлы в систему. Запуск службы активирует её работу в текущем сеансе системы. Включение автозапуска указывает системе автоматически запускать службу при загрузке ОС.


