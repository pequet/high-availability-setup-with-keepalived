# High Availability Setup with Keepalived and Floating IPs on DigitalOcean Ubuntu 16.04 

## Prerequisites: 

- two Ubuntu 16.04 servers on Digital Ocean (located within the same datacenter and with private networking enabled)
- nginx installed and running (apt-get install nginx)

## Change: 

YOUR_MASTER_PRIVATE_IP to *your master server private IP* in both /etc/keepalived/keepalived.conf  
YOUR_BACKUP_PRIVATE_IP to *your backup server private IP* in both /etc/keepalived/keepalived.conf  
MAKE_UP_AN_AUTH_PASSWORD to *a secret string* in both /etc/keepalived/keepalived.conf (only the first eight characters matter)  
YOUR_DIGITALOCEAN_API_TOKEN to *your DO API token* in both /etc/keepalived/master.sh  
YOUR_FLOATING_IP to *your floating IP* in both /etc/keepalived/master.sh  

## Commands:

```
apt-get install build-essential libssl-dev
cd ~
# Make sure to download the latest version of keepalived on the next line: http://www.keepalived.org/download.html
wget http://www.keepalived.org/software/keepalived-######.tar.gz
tar xzvf keepalived*
cd keepalived*
./configure
make
make install
# nano /etc/init/keepalived.conf
mkdir -p /etc/keepalived
curl http://169.254.169.254/metadata/v1/interfaces/private/0/ipv4/address && echo
ip -4 addr show dev eth1
nano /etc/keepalived/keepalived.conf
# curl -LO /usr/local/bin/assign-ip http://do.co/assign-ip
nano /etc/keepalived/master.sh
chmod +x /etc/keepalived/master.sh
apt-get install python-pip
export LC_ALL=C
pip install --upgrade pip
pip install requests
# nano /etc/systemd/system/keepalived.service
systemctl preset keepalived.service
systemctl start keepalived.service
# service keepalived status
```

## References:

- https://www.digitalocean.com/community/tutorials/how-to-create-a-high-availability-setup-with-heartbeat-and-floating-ips-on-ubuntu-14-04  
- https://www.digitalocean.com/community/questions/keepalived-for-ubuntu-16  

