---
tags: grub, linux
toc: true
---

## 1. recovering GRUB in Ubuntu

```
sudo grub-install /dev/XXX
```

## 2. XXX애 들어가는 디스크name 찾는법
*  ```sudo fdisk -l```로 Boot 옵션에 체크되어 있는 디스크를 찾기 
or
* ```ls -l /dev/disk/by-label/```로 Ubuntu가 위치한 디스크 name 찾기

## 주의할점 
disk의 파티션 넘버는 무시할 것, 예를 들어 /dev/sda1, /dev/sda2 등이 있으면 sudo grub-install /dev/sda만 치면 됩니당


## 명심할점
author by Master Park (HJ)
