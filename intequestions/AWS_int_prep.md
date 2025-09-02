## what is different bw a name vs cname records.

The A record points a name to a specific IP. If you want blog.dnsimple.com to point to the server 185.31.17.133 you’ll configure:

blog.dnsimple.com.     A        185.31.17.133

CNAME Record (Canonical Name Record)

Purpose: Maps a domain/subdomain → another domain name (not an IP)   EG:  app.example.com → myapp-alb-1234.elb.amazonaws.com
