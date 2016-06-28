---
layout: post
category: Git
title: Git Command
tagline: by TinyBread
tags: [Git]
---
<!--more-->

  

# Git Command

### 환경설정

### 기본적인 명령어

### Branch & Tag


### log

### 원격저장소

- ** git clone 저장소주소 폴더명 ** <br>
원격저장소를 복제하여 저장소를 생성합니다. 폴더명을 생략가능합니다.

- git fetch <br>
원격저장소의 변경사항 가져와서 원격브랜치를 갱신합니다.
 
- git pull <br>
git fetch에서 하는 원격저장소의 변경사항을 가져와서 지역브랙치에 합치는 작업을 한꺼번에 합니다. 파라미터로 풀링할 원격저장소와 반영할 지역브랜치를 줄 수 있습니다.

- git push <br>
파라미터를 주지 않으면 origin 저장소에 푸싱하며 현재 지역브랜치와 같은 이름의 브랜치에 푸싱합니다. --dry-run 옵션을 사용하면 푸싱된 변경사항을 확인할 수 있습니다. 로컬에서 tag를 달았을 경우에 기본적으로 푸싱하지 않기 때문에 git push origin 태그명이나 모든 태그를 올리기 위해서 git push origin --tags를 사용해야 합니다.

- git remote add 이름 저장소주소 <br>
새로운 원격 저장소를 추가합니다.

- it remote <br>
추가한 원격저장소의 목록을 확인할 수 있습니다.

- git remote show 이름 <br>
해당 원격저장소의 정보를 볼 수 있습니다.

- git remote rm 이름 <br>
원격저장소를 제거합니다.

### 참조
- [https://blog.outsider.ne.kr/572](https://blog.outsider.ne.kr/572)
