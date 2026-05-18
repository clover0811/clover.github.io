---
title: "Bandit Level 1→2 풀이"
date: 2026-05-18
categories: [Wargame, Bandit]
tags: [Bandit, OverTheWire, Wargame, Linux, SSH]
---

## 문제 설명

<img width="500" height="240" alt="image" src="https://github.com/user-attachments/assets/cff86108-9a55-4461-8cdc-fa0bc16b3c9e" />

이번 문제는 홈 디렉터리에 있는 `-` 라는 파일에 비밀번호가 저장되어 있고, bandit2에 로그인 하는 것이 목표다.

## 풀이 과정

<img width="480" height="150" alt="image" src="https://github.com/user-attachments/assets/b78d7fb7-20aa-4cee-9766-9069b452df22" />

`ls`명령어로 홈 디렉터리 안에 `-`라는 파일이 있는 것을 확인했고, 이전 단계와 같이 `cat`명령어를 통해 `-`파일을 열어보았다.

그런데 파일이 열리지 않는다.. `cat`명령어가 뭔가 잘못된 건 아닐까..?

이전 문제에서 정리한 글을 보면 `cat`명령어는 파일의 내용을 출력하는 명령어라는 걸 알 수 있다.

하지만 파일의 내용이 출력되지 않는 걸 보고 다른 명령어가 있는지 알아보고, `cat`명령어에 대해서도 문제점이 뭔지 자세히 알아보았다.

알아본 결과 `cat`명령어는 표준 출력 형식으로 파일의 내용을 보여주는 명령어인데, `-`라는 문자는 표준 입력 형식으로 바뀌기 때문에 입력하는 값이 출력이 되는 것이었다.

즉 `-`파일을 파일이 아닌 문자로 인식해서 생긴 문제였다.

<img width="650" height="170" alt="image" src="https://github.com/user-attachments/assets/a5fb639d-4070-4c8e-b317-26a18e2eaa61" />

그렇다면 이 문제를 해결할 방법은 없는걸까...? (No)

해결 방법은 `-`문자를 문자가 아닌 파일로 읽을 수 있도록 해줘야한다.

이때 중요한 것이 바로 절대 경로와 상대 경로다.

절대 경로는 최상위 루트(/)부터 전체 경로를 표현하는 것을 의미하고, 상대 경로는 현재 작업 디렉터리(.)를 기준으로 경로를 표현하는 것을 의미한다.

이를 이용하여 문제를 풀어보면

<img width="500" height="80" alt="image" src="https://github.com/user-attachments/assets/2f808567-41fe-4213-ba31-dc44293560b1" />
<p align="center">
  <sub>절대 경로를 사용하여 `-` 파일을 읽은 결과</sub>
</p>

<img width="500" height="80" alt="image" src="https://github.com/user-attachments/assets/9cbcbece-5509-44a0-9668-579960364573" />
<p align="center">
  <sub>절대 경로를 사용하여 - 파일을 읽은 결과</sub>
</p>

절대 경로와 상대 경로를 통해 `-`을 문자가 아닌 파일로 읽게 해서 다음과 같이 문제를 해결할 수 있다.

## 핵심 내용 정리

| 개념 | 설명 |
|---|---|
| 표준 입력 | 프로그램이 입력을 받는 기본 통로이며, 파일 디스크립터 번호는 `0`이다. |
| 표준 출력 | 프로그램의 실행 결과가 출력되는 기본 통로이며, 파일 디스크립터 번호는 `1`이다. |
| 표준 에러 | 오류 메시지가 출력되는 기본 통로이며, 파일 디스크립터 번호는 `2`이다. |
| 절대 경로 | 최상위 디렉터리 `/`부터 시작하는 전체 경로이다. |
| 상대 경로 | 현재 작업 디렉터리 `.`를 기준으로 표현하는 경로이다. |
