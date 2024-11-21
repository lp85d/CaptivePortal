## CaptivePortal
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
{table inet filter chain input {;
                        type filter hook input priority filter; policy accept;
                }
        
                chain forward {
                        type filter hook forward priority filter; policy drop;
                        iif "wlp2s0" accept
                }
        
                chain output {
                        type filter hook output priority filter; policy accept;
                }
        }
        table ip nat {
                chain prerouting {
                        type nat hook prerouting priority filter; policy accept;
                        iif "wlp2s0" tcp dport 80 dnat to 192.168.1.1:80
                }
        
                chain postrouting {
                        type nat hook postrouting priority srcnat; policy accept;
                        masquerade
                }
        }
}}
