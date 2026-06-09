---
title: "Bandit Level 4→5"
date: 2026-05-19
categories: [Wargame, Bandit]
tags: [Bandit, OverTheWire, Wargame, Linux, SSH]
published: false
---

## 문제 설명

<img width="500" height="250" alt="image" src="https://github.com/user-attachments/assets/9574ab74-577a-421c-b4e7-2b8f23c1eee4" />

이번 문제에서는 비밀번호가 `inhere`디렉터리에서 사람이 읽을 수 있는 유일한 파일에 저장되어 있다고 한다.

팁으로 터미널이 어지럽혀지면 `reset`명령어를 사용하라고 나와있다.

## 풀이 과정

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/b6d7ce08-9471-46b1-9268-98879b4fe78c" />

`inhere`디렉터리에서 `ls`명령어를 이용하여 무슨 파일이 있는지 확인해보았다.

안에 파일이 10개가 있는 것을 확인했고, 일단 `-file00`부터 읽어보았다.

<img width="580" height="100" alt="image" src="https://github.com/user-attachments/assets/ea9d6b28-b019-4bb4-b5e5-b9081eff7d42" />

`-file00`파일을 읽어보니 사람이 읽을 수 없는 문자들이 출력되는걸 확인할 수 있었다.

왜 그런지 확인해보기 위해 `file`명령어를 이용하여 파일 타입을 확인해보았다.

<img width="480" height="70" alt="image" src="https://github.com/user-attachments/assets/1e89c0e7-2181-4631-9533-8259706aca4a" />

확인해보니 `-file00`파일은 `data`타입이란 걸 알 수 있었다.

`file`명령어를 이용하여 10개의 파일 타입을 하나하나 찾아볼 수도 있지만, 파일이 많으면 많을수록 하나하나 입력하는 건 매우 힘들 것이다.

여기서는 와일드카드(*)라는 문자를 통해 모든 파일을 `file`명령어를 이용하여 한번에 출력할 수 있다.

`file ./*`명령어를 사용하여 모든 파일들의 타입을 출력해보았다.

<img width="420" height="285" alt="image" src="https://github.com/user-attachments/assets/4862bd27-5784-4c01-92c1-fb94021f9e41" />

모든 파일들의 타입을 출력해보니 `-file07`파일에서 `data`가 `ASCII text`라고 쓰여져 있는 걸 볼 수 있다.

여기서 `ASCII code`는 사람이 읽을 수 있는 숫자나 알파벳 컴퓨터로 표현한 코드체계이며, `text`는 우리가 알고 있는 텍스트를 말한다.

그렇다면 `ASCII text`는 사람이 읽을 수 있는 숫자나 알파벳을 컴퓨터로 표현한 코드체계로 이루어진 `text`인 것이라고 이해할 수 있다.

따라서 문제에서 사람이 읽을 수 있는 유일한 파일은 `-file07`이 된다는 것을 알 수 있다.

<img width="460" height="75" alt="image" src="https://github.com/user-attachments/assets/af89fe5d-41d2-4b64-bd0a-37806f840352" />

`cat`명령어를 사용해 `-file07`파일을 확인하여 비밀번호를 찾아냈다.

문제설명에서 팁으로 주어진 `reset`명령어는 사용하지 않았지만 직접 사용해보면 무엇을 의미하는지 알 수 있을 것이다.

## 핵심 내용 정리

| 개념 | 내용 |
|---|---|
| `file` | 파일의 실제 타입을 확인하는 명령어다. |
| `ASCII text` | 사람이 읽을 수 있는 텍스트 파일이다. |
| `*` | 여러 파일을 한 번에 지정할 수 있는 와일드카드 문자이다. |
| `reset` | 터미널 출력이 깨졌을 때 상태를 초기화할 수 있다. |
