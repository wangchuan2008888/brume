DAEMON_USER=root
DAEMON_GROUP=root
DIRECTOR_DAEMON_USER=${DAEMON_USER}
STORAGE_DAEMON_USER=${DAEMON_USER}
FILE_DAEMON_USER=root
STORAGE_DAEMON_GROUP=${DAEMON_GROUP}
WORKING_DIR=/var/lib/bareos
POSTGRESQL_DIR=/usr/pgsql-9.4
mkdir -p /home/cbackup/core
BAREOS_HOME=/home/cbackup/core
PLUGIN_DIR=${BAREOS_HOME}/plugins

./configure   --prefix=$BAREOS_HOME \
  --sbindir=$BAREOS_HOME/sbin \
  --with-sbin-perm=755 \
  --sysconfdir=$BAREOS_HOME/conf/bareos \
  --with-archivedir=$BAREOS_HOME/usrlib/bareos/storage \
  --with-scriptdir=$BAREOS_HOME/usrlib/bareos/scripts \
  --with-plugindir=$BAREOS_HOME/usrlib/bareos/plugins \
  --with-working-dir=$BAREOS_HOME/lib/bareos \
  --with-pid-dir=$BAREOS_HOME/lib/bareos \
  --with-bsrdir=$BAREOS_HOME/lib/bareos \
  --with-logdir=$BAREOS_HOME/log/bareos \
  --with-subsys-dir=/var/lock \
  --with-python \
  --with-plugindir=${PLUGIN_DIR} \
  --enable-smartalloc \
  --disable-conio \
  --disable-rpath \
  --enable-acl \
  --enable-libtool \
  --enable-static \
  --enable-readline \
  --enable-batch-insert \
  --enable-dynamic-cats-backends \
  --enable-xattr \
  --enable-scsi-crypto \
  --enable-ndmp \
  --enable-lmdb \
  --enable-ipv6 \
  --with-postgresql=${POSTGRESQL_DIR}\
  --with-tcp-wrappers \
  --with-openssl \
  --with-dir-user=${DIRECTOR_DAEMON_USER} \
  --with-dir-group=${DAEMON_GROUP} \
  --with-sd-user=${STORAGE_DAEMON_USER} \
  --with-sd-group=${STORAGE_DAEMON_GROUP} \
  --with-fd-user=${FILE_DAEMON_USER} \
  --with-fd-group=${DAEMON_GROUP} \
  --with-dir-password="dirpw" \
  --with-fd-password="fdpw" \
  --with-sd-password="sdpw" \
  --with-mon-dir-password="mondirpw" \
  --with-mon-fd-password="monfdpw" \
  --with-mon-sd-password="monsdpw" \
  --with-basename="localhost" \
  --with-hostname="localhost" \
  --enable-includes=yes \
  --with-glusterfs

make
make install

chmod +x platforms/redhat/bareos-dir
chmod +x platforms/redhat/bareos-sd
chmod +x platforms/redhat/bareos-fd
cp platforms/redhat/bareos-dir /etc/init.d/
cp platforms/redhat/bareos-sd /etc/init.d/
cp platforms/redhat/bareos-fd /etc/init.d/

ldconfig
