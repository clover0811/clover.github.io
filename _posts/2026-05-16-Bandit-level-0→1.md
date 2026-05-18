---
title: "Bandit Level 0→1 풀이"
date: 2026-05-16
categories: [Wargame, Bandit]
tags: [Bandit, OverTheWire, Wargame, Linux, SSH]
---

## 문제 설명

<img width="600" height="253" alt="image" src="https://github.com/user-attachments/assets/a47d06cf-a555-4fcb-bff6-eb958dfedebe" />

이번 문제는 홈 디렉터리에 있는 `readme`파일에 비밀번호가 저장되어 있고, 그 비밀번호를 이용하여 `bandit1`에 로그인을 하는 것이 목표다.

## 풀이 과정

<img width="650" height="250" alt="image" src="https://github.com/user-attachments/assets/9d193eca-a5ff-49f5-9893-37e063d483c3" />

`ls`명령어로 홈 디렉터리 안에 `readme`파일이 있는 것을 확인했다.

`cat`명령어를 통해 `readme`파일을 열었고, `bandit1`의 비밀번호를 찾을 수 있었다.

얻은 비밀번호를 통해 `bandit1`계정으로 로그인을 하면 된다.

## 사용한 명령어 정리

| 명령어 | 설명 |
|---|---|
| `ls` | 현재 디렉터리에 있는 파일과 폴더 목록을 확인하는 명령어 |
| `cat` | 파일의 내용을 출력하는 명령어 |
