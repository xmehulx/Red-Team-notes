# #proxychains
Routes all traffic coming from any command-line tool to any proxy we specify.

>We can only perform a `full TCP connect scan` over proxychains because proxychains cannot understand partial packets. If we send partial packets like half connect scans, it will return incorrect results. 
>Also, `host-alive` checks may not work against Windows targets because the Windows Defender firewall blocks ICMP requests (traditional pings) by default.