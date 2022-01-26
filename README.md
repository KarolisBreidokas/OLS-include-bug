This is repo to display and document an include bug with pre-compiled OpenLiteSpeed binary


# Steps to reproduce

## Install OpenLiteSpeed from precompiled libraries
This guide is tested on RHEL8 compatable OS (Rocky Linux 8.4).

```bash
wget https://openlitespeed.org/packages/openlitespeed-1.7.14.tgz 
tar -zxvf openlitespeed-*.tgz
cd openlitespeed
./install.sh
```

## Clone this repository to /usr/local/lsws/conf/bugreport

```bash
git clone https://github.com/KarolisBreidokas/OLS-include-bug.git /usr/local/lsws/conf/bugreport
```

## Include vhosts.conf to /usr/local/lsws/conf/httpd_config.conf

Add `include /usr/local/lsws/conf/bugreport/config/vhosts.conf` at the end of /usr/local/lsws/conf/httpd_config.conf

## Start OpenLiteSpeed server 

```bash
/usr/local/lsws/bin/lswsctrl start
```

# Expected behaviour 

```
# curl a.tld:8088 a.tld:8088/shared/ b.tld:8088 b.tld:8088/shared/
Hello from a.tld document root
Hello from shared directory
Hello from b.tld document root
Hello from shared directory
```

This is also current behaviour if OpenLiteSpeed is installed using Repository

# Current behaviour

```
# curl a.tld:8088 a.tld:8088/shared/ b.tld:8088 b.tld:8088/shared/
Hello from a.tld document root
Hello from shared directory
Hello from b.tld document root
Oh no. We are trying to access shared directory in b.tld
```

