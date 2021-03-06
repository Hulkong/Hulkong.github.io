---
title: GitLab 백업 및 복원
description: GitLab 백업 및 복원
renderimg: /blog/gitlab_512.png
img: /blog/landscape_1920.jpg
alt: GitLab 백업 및 복원
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - GitLab
  - web development
---

## GitLab의 모든 설정은 /etc/gitlab/gitlab.rb에서 관리

## backup

```bash
sudo gitlab-rake gitlab:backup:create
```

- 생성된 파일은 /var/opt/gitlab/backups에 저장
- gitlab.rb와 gitlab-secrets.json은 포함 안됨
- 백업 경로 변경

```bash
# vi /etc/gitlab/gitlab.rb
gitlab_rails['backup_path'] = '백업 디렉토리'
```

## restore

```bash
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
sudo gitlab-ctl status

# 백업 파일이 하나일 경우
sudo gitlab-rake gitlab:backup:restore

# 백업 파일이 여러개 있을 경우
sudo gitlab-rake gitlab:backup:restore BACKUP='FILENAME'

# Gitlab Service 시작
sudo gitlab-ctl restart
sudo gitlab-rake gitlab:check SANITIZE=true
```

- 백업 당시 GitLab과 현재 GitLab버전이 같아야 함

## 자동백업

```bash
# crontab -e
# 매월 매일 02:00에 백업
0 2 * * * /opt/gitlab/bin/gitlab-rake gitlab:backup:create
```

<br/>

# 참고

- [팬도라](https://judo0179.tistory.com/50)
- [yjunyoung](https://yjunyoung.tistory.com/entry/9-Gitlab-%EC%88%98%EB%8F%99%EC%9E%90%EB%8F%99-%EB%B0%B1%EC%97%85-%EC%84%A4%EC%A0%95)
