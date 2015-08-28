# sniproxy-netflix

You will need to set up [sniproxy](https://github.com/dlundquist/sniproxy)

For that you will need to install udns first. 

```
sudo apt-get install autotools-dev cdbs debhelper dh-autoreconf dpkg-dev gettext libev-dev libpcre3-dev libudns-dev pkg-config
mkdir udns_packaging
cd udns_packaging
wget http://archive.ubuntu.com/ubuntu/pool/universe/u/udns/udns_0.4-1.dsc
wget http://archive.ubuntu.com/ubuntu/pool/universe/u/udns/udns_0.4.orig.tar.gz
wget http://archive.ubuntu.com/ubuntu/pool/universe/u/udns/udns_0.4-1.debian.tar.gz
tar xfz udns_0.4.orig.tar.gz
cd udns-0.4/
tar xfz ../udns_0.4-1.debian.tar.gz
dpkg-buildpackage
cd ..
sudo dpkg -i libudns-dev_0.4-1_amd64.deb libudns0_0.4-1_amd64.deb
```

Setting up sniproxy,

```
cd ..
mkdir sniproxy
cd sniproxy
git clone https://github.com/dlundquist/sniproxy.git
./autogen.sh && ./configure && make check
```

You should see: # PASS: 18 in green and no skips/fails/errors.

```
sudo make install
dpkg-buildpackage
sudo dpkg -i ../sniproxy_***_amd64.deb
```

SniProxy is installed.

Configure SNIProxy.

```
sudo vi /etc/sniproxy.conf

# sniproxy configuration file
# lines that start with # are comments
# lines with only white space are ignored

user daemon

# PID file
pidfile /var/run/sniproxy.pid

error_log {
    # Log to the daemon syslog facility
    #syslog daemon

    # Alternatively we could log to file
    filename /var/log/sniproxy/sniproxy.log

    # Control the verbosity of the log
    priority notice
}

access_log {
    filename /var/log/sniproxy/access.log
}

listen 80 {
    proto http
    bad_requests log
}

listen 443 {
    proto tls
    bad_requests log
}

table {
    hulu\.com *
    netflix(\.com|\.net) *
    go\.com *
    (nbc|nbcuni)\.com *
    (mtv|mtvnservices)\.com *
    theplatform\.com *
    a248\.e\.akamai\.net *
    video\.dl\.playstation\.net *
    pandora\.com *
    crackle\.com *
    vevo\.com *
    crunchyroll\.com *
    dramafever\.com *
    optimizely\.com *
    uplynk\.com *
    maxmind\.com *
    ipinfo\.io *
    brightcove\.com *
    iheart\.com *
    logotv\.com *
    pbs\.org *
    warnerbros\.com *
    cwtv\.com *
    smithsonianchannel\.com *
    southpark\.cc\.com *
    spike\.com *
    vh1\.com *
    fxnetworks\.com *
    ipad-streaming\.cbs\.com *
    tve_nbc-vh\.akamaihd\.net *
    sni-vh\.akamaihd\.net *
    unicornmedia\.com *
    hbogo\.com *
    hbonow\.com *
    amazon\.com *
    dramafevertoken-a\.akamaihd\.net *
    disney\.com *
    disneyjunior\.com *
}

sudo service sniproxy status
sudo service sniproxy stop
sudo service sniproxy start
```
