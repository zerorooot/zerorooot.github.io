---
title: 强制apt使用ipv4
tags:
  - linux
  - apt
date: 2020-10-08 20:34:54
description: echo 'Acquire::ForceIPv4 "true";' | sudo tee /etc/apt/apt.conf.d/99force-ipv4
---
echo 'Acquire::ForceIPv4 "true";' | sudo tee /etc/apt/apt.conf.d/99force-ipv4
