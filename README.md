## CaptivePortal `Code Ctrl+E (Windows/Linux)`
# Перенаправление Captive Portal
dpkg -l | grep -E "hostapd|dnsmasq|nginx|php|mariadb|python3|pip"
grep "Install:" /var/log/apt/history.log | tail -n 10        

apt list --installed | grep nftables   
libnftables1/stable,now 1.0.6-2+deb12u2 amd64 [installed]   
nftables/stable,now 1.0.6-2+deb12u2 amd64 [installed]   

nft list ruleset

nano /etc/nftables.conf  
nft -c -f /etc/nftables.conf  
nft -f /etc/nftables.conf  

systemctl enable nftables        
systemctl start nftables        

nano /etc/dnsmasq.conf        
dnsmasq --test        
dnsmasq: syntax check OK.        
systemctl restart dnsmasq        
systemctl status dnsmasq        

sudo mkdir -p /etc/systemd/system/dnsmasq.service.d/        
sudo nano /etc/systemd/system/dnsmasq.service.d/override.conf        
[Unit]        
After=network-online.target        
Wants=network-online.target        

sudo systemctl daemon-reload        
sudo systemctl restart dnsmasq        
sudo systemctl status dnsmasq        
journalctl -u dnsmasq -n 20        
dig google.com @127.0.0.1        

dmesg | grep wlp2s0        
sudo nano /etc/hostapd/hostapd.conf        
sudo truncate -s 0 /var/log/syslog        
sudo truncate -s 0 /var/log/hostapd.log        
sudo truncate -s 0 /var/log/messages        
sudo truncate -s 0 /var/log/kern.log        
sudo systemctl restart hostapd        
sudo systemctl status hostapd        
sudo tail -f /var/log/syslog        
sudo systemctl is-enabled hostapd        
sudo systemctl is-enabled dnsmasq        
sudo ifconfig wlp2s0 up        
sudo tail -f /var/log/syslog        
sudo systemctl status nginx        
ls -l /var/www/captive_portal/        
sudo nano /etc/nginx/sites-available/default        
nginx -v        
php -v        
cat /etc/os-release        
uname -r        
sudo iw dev

nft list ruleset        
GNU nano 7.2 /etc/nftables.conf         
```python
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
    set authorized_macs {
        type ether_addr
        elements = { 1c:cc:00:4d:91:03, d6:08:00:5c:54:78, dc:a9:71:00:d0:b7, e8:11:0:8e:60:d0 }
    }

    chain input {
        type filter hook input priority filter; policy accept;
        udp dport 53 accept
        udp dport { 67, 68 } accept
    }

    chain forward {
        type filter hook forward priority filter; policy drop;
        ether saddr @authorized_macs accept
        iifname "wlp2s0" accept
        oifname "wlp2s0" accept
    }

    chain output {
        type filter hook output priority filter; policy accept;
    }
}

table ip nat {
    chain prerouting {
        type nat hook prerouting priority filter; policy accept;
        ip daddr 192.168.1.1 tcp dport 80 redirect to :80
    }

    chain postrouting {
        type nat hook postrouting priority srcnat; policy accept;
        oifname "enp3s0" masquerade
    }
}
```
