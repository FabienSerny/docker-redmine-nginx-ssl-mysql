# Docker compose for Redmine with nginx, MySQL and Let's Encrypt (SSL)
Docker compose to get a Redmine working with Nginx, MySQL and https (with let's encrypt)

## Installation

Once you will have pulled this repo, just do the following :

1) Edit the "docker-compose.yml" file
- Replace all occurrences of "www.yourdomain.com" by your domain
- Replace all occurrences of "your@mail.com" by your mail
- Replace all occurrences of "your_mysql_password" by the MySQL password of your choice
- Set all SMTP variables correctly "SMTP_DOMAIN", "SMTP_HOST", "SMTP_PORT", "SMTP_USER", "SMTP_PASS"
In my case I use Mandrill. If you don't want to use specific SMTP configuration, just delete all variables beginning with SMTP

2. Just do a "docker-compose up" in the directory


3. You're done!

Database, plugins and files directory are volumes, so they will be persistent.


## Extra configuration

If you want to upload big files in attachment (more than 1MO), just do the following.

        nano /etc/nginx/conf.d/default.conf

You should see these lines at the end of your files

        location / {
                proxy_pass http://www.yourdomain.com;
        }

Just add "client_max_body_size 25m;" in it

        location / {
                proxy_pass http://www.yourdomain.com;
                client_max_body_size 25m;
        }

Then reload your nginx onfiguration with the command :

        /usr/sbin/nginx -s reload

Since the file "/etc/nginx/conf.d" is a volume, you won't have to do it again.

# Big thanks for their work :
- JRCS, who develop an image to run very easily let's encrypt : https://hub.docker.com/r/jrcs/letsencrypt-nginx-proxy-companion/
- jwilder/docker-gen, who develop docker-gen and its nginx configuration template : https://hub.docker.com/r/jwilder/docker-gen/