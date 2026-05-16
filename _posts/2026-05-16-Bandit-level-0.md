---
title: "Bandit Level 0"
date: 2026-05-16
categories: [Wargame, Bandit]
tags: [Bandit, OverTheWire, Wargame, Linux, SSH]
---

## 문제 설명

<img width="450" height="200" alt="image" src="https://github.com/user-attachments/assets/2f7e04c6-24b9-4b27-ab92-e6ed2e28c77c" />

이번 문제의 목표는 SSH를 사용해 Bandit 서버에 접속하는 것이다.

접속에 필요한 정보는 다음과 같다.

| 항목 | 값 |
|---|---|
| Host | `bandit.labs.overthewire.org` |
| Port | `2220` |
| Username | `bandit0` |
| Password | `bandit0` |

위 정보를 이용해 로그인하면 Level 0을 완료할 수 있다.

## 풀이 과정

<img width="550" height="600" alt="image" src="https://github.com/user-attachments/assets/56b149ce-83e6-45a0-beb7-7fa4cad35ffc" />

문제에서 제공된 정보를 바탕으로 SSH 접속을 시도했다.

`ssh bandit0@bandit.labs.overthewire.org -p 2220`

명령어를 실행하면 비밀번호를 입력하라는 창이 나오는데

`password: bandit0`

비밀번호로 bandit0을 입력하면 Bandit 서버에 로그인 할 수 있다.

정상적으로 로그인 되면 다음과 같이 프롬포트가 변경된다.

`bandit0@bandit:~$`

이를 통해 Level 0 서버 접속에 성공했음을 확인할 수 있다.
