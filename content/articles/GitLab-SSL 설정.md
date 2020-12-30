---
title: Gitlab SSL 설정
description: Gitlab SSL 설정
renderimg: /blog/gitlab_512.png
img: /blog/landscape_1920.jpg
alt: Gitlab SSL 설정
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - GitLab
  - web development
---

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

## 참고

- [Lunight](https://lunightstory.tistory.com/6)
