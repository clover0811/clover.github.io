---
title: "Bandit Level 2→3 풀이"
date: 2026-05-18
categories: [Wargame, Bandit]
tags: [Bandit, OverTheWire, Wargame, Linux, SSH]
---

## 문제 설명

<img width="450" height="200" alt="image" src="https://github.com/user-attachments/assets/772fb765-fe25-4b79-9d60-52477f8e4bd3" />

이번 문제는 홈 디렉터리에 있는 `--spaces in this filename--`파일에 비밀번호가 저장되어 있고, `bandit3`에 로그인 하는 것이 목표다.

## 문제 풀이

<img width="580" height="150" alt="image" src="https://github.com/user-attachments/assets/1480f477-04a9-42a6-8308-44d4b149af72" />

이전 문제와 같이 파일 이름에 `-`문자가 있는 걸 보아하니 경로 설정을 통해 문제를 풀어주면 될 것 같았다.

하지만 풀리지 않았고.. 내용을 자세히 보니 `cat`명령어가 4번 실행되는 것이 보였다.

이를 통해 공백이 생기면 하나의 파일이 아니라 여러 개의 파일로 인식될 수 있다고 생각했다.

이 공백 문제를 해결하기 위해 찾아보니 명령어에서 띄어쓰기는 옵션, 인자를 구분하기 위한 특수문자로 사용한다고 나와있고, `""`로 감싸주면 파일을 묶어서 처리할 수 있다고 한다.

`""`을 사용하여 명령어를 입력했고, 다음과 같이 문제를 해결할 수 있었다.

<img width="650" height="65" alt="image" src="https://github.com/user-attachments/assets/85582c0c-89a1-48c9-b0af-f94c93615bcb" />

또 다른 방법으로는 이스케이프 문자를 사용하여 해결하는 방법이다. (코딩 생각하면 편함.)

<img width="650" height="65" alt="image" src="https://github.com/user-attachments/assets/78b01613-f564-4224-b470-98c52eb7806b" />

공백도 하나의 특수문자로 볼 수 있기 때문에 공백 앞에 \를 붙여 문자로 인식시켰다.

## 핵심 내용 정리

| 개념 | 설명 |
|---|---|
| 공백 포함 파일 | 파일명에 공백이 있으면 쉘이 이를 하나의 파일명이 아니라 여러 개의 인자로 나누어 인식할 수 있다. |
| `""` 사용 | 파일명을 `""`로 감싸면 공백까지 포함해서 하나의 파일명으로 처리할 수 있다. |
| 이스케이프 문자 | `\`를 사용하면 바로 뒤에 오는 특수문자를 문자 그대로 인식시킬 수 있다. |
