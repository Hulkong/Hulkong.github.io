# SSL 설정

```bash
sudo ufw allow https

# sudo vi /etc/gitlab/gitlab.rb
external_url    'https://로 변경'
nginx['redirect_http_to_https'] = true

sudo mkdir -p /etc/gitlab/ssl
sudo chmod 700 /etc/gitlab/ssl
sudo cp hulkong.crt /etc/gitlab/ssl/
sudo cp hulkong.key /etc/gitlab/ssl/
sudo gitlab-ctl reconfigure
```

# 참고
- [Lunight](https://lunightstory.tistory.com/6)