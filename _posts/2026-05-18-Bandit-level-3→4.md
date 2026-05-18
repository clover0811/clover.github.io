---
title: "Bandit Level 3→4 풀이"
date: 2026-05-18
categories: [Wargame, Bandit]
tags: [Bandit, OverTheWire, Wargame, Linux, SSH]
---

## 문제 설명

<img width="450" height="200" alt="image" src="https://github.com/user-attachments/assets/c5a90064-0588-40a6-9d80-73a8e0033b58" />

이번 문제는 이전과 다르게 홈 디렉터리 안이 아니라 `inhere`디렉터리 안 숨겨진 파일에 비밀번호가 있다고 나와있다.

## 풀이 과정

<img width="330" height="95" alt="image" src="https://github.com/user-attachments/assets/c500328c-dcbd-4540-8de5-f546d1a862b9" />

`ls`명령어를 통해 홈 디렉터리 안에 `inhere`디렉터리가 있는 것을 확인했다.

`cd`명령어를 이용해서 `inhere`디렉터리로 이동했다.

`inhere`디렉터리로 이동한 후 `ls`명령어를 입력했다.

<img width="330" height="60" alt="image" src="https://github.com/user-attachments/assets/fe3c3347-99d8-414e-be89-6936273dd31b" />

...? 아무것도 안 나옴..

문제 설명을 보니 `inhere`디렉터리 안에 숨겨진 파일이 있다고 하는데.. 그걸 찾으면 되는 것 같다.

숨겨진 파일을 찾기 위해서 `ls`명령어의 옵션 중 하나인 `-a`을 이용했다.

<img width="380" height="70" alt="image" src="https://github.com/user-attachments/assets/25fda14f-49b8-48a3-8d0e-68e2d19901a7" />

`ls -a`명령어를 입력하니 숨겨진 파일들이 있는 것을 확인했다.

더 자세히 보기 위해서 `-a`옵션과 `-l`옵션을 이용했다.

<img width="700" height="120" alt="image" src="https://github.com/user-attachments/assets/34ef7d27-f79f-4ef4-91ba-cd4fa3dcd86d" />

`ls -al`명령어로 자세히 보니 `bandit4`로 가기 위한 파일이 `...Hiding-From-You`라는 것을 알 수 있었다.

<img width="550" height="80" alt="image" src="https://github.com/user-attachments/assets/3b1e49e7-803c-48a2-9b79-0f48a762d449" />

`cat`명령어로 파일을 열어 비밀번호를 찾아냈다.

이 문제에서 숨겨진 파일들을 보면 공통적으로 파일 이름 앞에 `.`이 붙은 것을 확인할 수 있는데, 파일 앞에 `.`이 있으면 파일을 숨길 수 있다는 특징을 알게 되었다.

## 핵심 내용 정리

| 개념 | 내용 |
|---|---|
| `cd` | 디렉터리를 이동할 때 사용하는 명령어다. |
| `ls -a` | 숨김 파일을 포함한 모든 파일을 출력한다. |
| `ls -l` | 파일의 권한, 소유자, 크기, 수정 시간 등 상세 정보를 출력한다. |
| `ls -al` | `-a`와 `-l`을 함께 사용한 형태로, 숨김 파일까지 포함해 자세히 확인할 수 있다. |
| 숨김 파일 | 리눅스에서는 파일 이름 앞에 `.`이 붙으면 숨김 파일로 처리된다. |
