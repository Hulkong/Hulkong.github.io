---
title: Gitlab SSH
description: Gitlab SSH
renderimg: /blog/gitlab_512.png
img: /blog/landscape_1920.jpg
alt: Gitlab SSH
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - GitLab
  - web development
---

## How to setup SSH Key

## SSH - Secureed Shell

- Used for authentication
- By setting ssh key you can connect to GitLab server without using username and password each time

### Step1. Run command

- On Mac - run command on terminal
- On Windows - use putty or git bash

```bash
ssh-keygen
```

### Step2. Login to GitLab

Goto account- >Settigs -> SSH Keys

### Step3. Copy contents of id_rsa.pub and Add Key

### Step4. Verify SSH Key

참고영상: [Automation Step by Step - Raghav Pal
](https://www.youtube.com/watch?v=mNtQ55quG9M&list=PLhW3qG5bs-L8YSnCiyQ-jD8XfHC2W1NL_&index=4)
