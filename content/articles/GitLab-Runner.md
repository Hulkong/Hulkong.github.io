---
title: Gitlab Runner
description: Gitlab Runner
renderimg: /blog/gitlab_512.png
img: /blog/landscape_1920.jpg
alt: Gitlab Runner
author:
  name: Hulkong
  bio: ''
  img: /blog/333x213px-kyh.png
tags:
  - GitLab
  - web development
---

## Install, Register and Start Gitlab Runner

## used in GitLab CI

## used to run jobs & send results back to GitLab

### Step1. Install GitLab-Runner

```bash
brew install gitlab-runner
gitlab-runner --version
```

### Step2. Register GitLab-Runner

```bash
gitlab-runner register
```

### Step3. Start GitLab-Runner

```bash
brew service start gitlab-runner
brew service stop gitlab-runner
```

### Step4. Check runner is activated in the project

참고영상: [Automation Step by Step - Raghav Pal](https://www.youtube.com/watch?v=R8rru9nmZ40&list=PLhW3qG5bs-L8YSnCiyQ-jD8XfHC2W1NL_&index=5)
