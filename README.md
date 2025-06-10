sudo ln -s /etc/nginx/sites-available/superset /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
