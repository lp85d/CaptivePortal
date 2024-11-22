## CaptivePortal `Code Ctrl+E (Windows/Linux)`
# Перенаправление Captive Portal
dpkg -l | grep -E "hostapd|dnsmasq|nginx|php|mariadb|python3|pip"

apt list --installed | grep nftables   
libnftables1/stable,now 1.0.6-2+deb12u2 amd64 [installed]   
nftables/stable,now 1.0.6-2+deb12u2 amd64 [installed]   

nft list ruleset

nano /etc/nftables.conf  
nft -c -f /etc/nftables.conf  
nft -f /etc/nftables.conf  
nft list ruleset  
`table inet filter 
        chain input 
                type filter hook input priority filter; policy accept;
        chain forward 
                type filter hook forward priority filter; policy drop;
                iif "wlp2s0" accept
        chain output 
                type filter hook output priority filter; policy accept;
table ip nat 
        chain prerouting 
                type nat hook prerouting priority filter; policy accept;
                iif "wlp2s0" tcp dport 80 dnat to 192.168.1.1:80
        chain postrouting 
                type nat hook postrouting priority srcnat; policy accept;
                masquerade
`
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
