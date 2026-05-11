---
title: "PNG, ZIP 파일 구조 이해하기"
date: 2026-05-11
categories: [Digital Forensics]
tags: [PNG, ZIP, File Structure, Digital Forensics]
---

## 미션 선정 이유

PNG, ZIP 파일 구조 이해와 디지털포렌식 CTF문제 맛보기

## 수행 과정

1. PNG 파일 구조 이해하기
2. ZIP 파일 구조 이해하기
3. 이해한 내용을 바탕으로 문제 풀어보기

---

# 1. PNG 파일 구조

## 1-1. PNG 파일의 전체 구조

PNG 파일은 크게 다음과 같은 구조로 이루어져 있다.

    [PNG Signature]
    [Chunk 1]
    [Chunk 2]
    [Chunk 3]
    ...

PNG 파일은 먼저 고정된 시그니처로 시작하고, 이후 여러 개의 chunk가 순서대로 이어진다.

---

## 1-2. PNG Signature

PNG 파일은 항상 다음 8바이트로 시작한다.

    89 50 4E 47 0D 0A 1A 0A

이 중 `50 4E 47`은 ASCII 문자로 `PNG`를 의미한다.

    89 50 4E 47 0D 0A 1A 0A
       P  N  G

이 값은 PNG 파일임을 식별하는 역할을 한다.  
따라서 파일이 정상적으로 열리지 않을 때, 맨 앞부분의 시그니처가 손상되어 있는지 확인해볼 수 있다.

---

## 1-3. PNG Chunk 구조

PNG 파일은 여러 개의 chunk로 구성되며, 각 chunk는 다음과 같은 구조를 가진다.

    [Length 4바이트]
    [Chunk Type 4바이트]
    [Chunk Data 가변 길이]
    [CRC 4바이트]

예를 들어 다음과 같은 값이 있다고 가정하면,

    00 00 00 0D 49 48 44 52 ...

각 부분은 다음과 같이 해석이 가능하다.

| 구역 | 의미 |
| --- | --- |
| `00 00 00 0D` | 데이터 길이 13바이트 |
| `49 48 44 52` | chunk 타입 `IHDR` |
| 이후 데이터 | 실제 chunk 데이터 |
| 마지막 4바이트 | CRC 값 |

---

## 1-4. 주요 PNG Chunk

PNG에서 가장 기본적으로 알아야 할 chunk는 `IHDR`, `IDAT`, `IEND`이다.

### IHDR

`IHDR`은 PNG 파일의 첫 번째 이며chunk, 이미지의 기본 정보를 담고 있다.

- 이미지의 가로 길이
- 이미지의 세로 길이
- 색상 형식
- 비트 깊이

### IDAT

`IDAT`은 실제 이미지 데이터가 저장되는 chunk이다.  
하나의 PNG 파일 안에 `IDAT` chunk가 여러 개 존재할 수도 있다.

### IEND

`IEND`는 PNG 파일의 끝을 의미하는 chunk이다.  
정상적인 PNG 파일이라면 보통 `IEND` chunk 이후에는 추가 데이터가 없어야 한다.

`IEND` chunk는 보통 다음과 같은 형태로 끝난다.

    00 00 00 00 49 45 4E 44 AE 42 60 82

---

## 1-5. PNG 파일 단순화

    89 50 4E 47 0D 0A 1A 0A     ← PNG Signature

    00 00 00 0D
    49 48 44 52                 ← IHDR
    ...                         ← 이미지 정보
    CRC

    ...
    49 44 41 54                 ← IDAT
    ...                         ← 실제 이미지 데이터
    CRC

    00 00 00 00
    49 45 4E 44                 ← IEND
    AE 42 60 82                 ← CRC

---

## 1-6. PNG 파일 분석 시 확인할 점

### 1. 시그니처가 정상적인지 확인하기

    89 50 4E 47 0D 0A 1A 0A

파일이 열리지 않는다면 이 값이 손상되어 있는지 확인할 수 있다.

### 2. 주요 Chunk가 정상적으로 존재하는지 확인하기

    IHDR → IDAT → IEND

기본적인 chunk 순서가 올바른지 확인한다.

### 3. IEND 이후에 추가 데이터가 있는지 확인하기

정상적인 PNG 파일은 `IEND`에서 끝나야 한다.  
만약 `IEND` 이후에도 다른 데이터가 남아 있다면, 다른 파일이 추가로 숨겨져 있을 가능성이 있다.

예를 들어 다음과 같은 값이 이어진다면,

    ... 49 45 4E 44 AE 42 60 82 50 4B 03 04 ...

`50 4B 03 04`는 ZIP 파일에서 자주 보이는 시그니처이므로, PNG 뒤에 ZIP 파일이 숨겨져 있을 가능성을 의심할 수 있다.

---

# 2. ZIP 파일 구조

## 2-1. ZIP 파일의 전체 구조

ZIP 파일은 여러 개의 파일을 하나로 묶는 압축 파일 형식이다.  
기본 구조를 단순화하면 다음과 같다.

    [Local File Header + File Data]
    [Local File Header + File Data]
    ...
    [Central Directory]
    [End of Central Directory]

앞부분에는 실제 파일 데이터가 저장되고, 뒷부분에는 ZIP 안에 어떤 파일들이 들어 있는지 정리한 정보가 저장된다.

---

## 2-2. ZIP에서 자주 보는 시그니처

ZIP 파일을 분석할 때 자주 확인하는 값은 다음과 같다.

| 구분 | 시그니처 | 의미 |
| --- | --- | --- |
| Local File Header | `50 4B 03 04` | 개별 파일의 시작 |
| Central Directory Header | `50 4B 01 02` | 파일 목록 정보 |
| End of Central Directory | `50 4B 05 06` | ZIP 파일의 끝 |

---

## 2-3. Local File Header

`Local File Header`는 ZIP 내부에 들어 있는 각 파일 앞에 존재한다.

    50 4B 03 04

이 뒤에는 해당 파일의 이름, 압축 방식, 크기, CRC 값 등이 저장된다.

즉, 이 부분은 다음과 같은 의미를 가진다.

> 이제부터 ZIP 안의 한 파일이 시작되며, 이 파일의 기본 정보는 다음과 같다.

---

## 2-4. Central Directory

`Central Directory`는 ZIP 파일의 목차 역할을 한다.

    50 4B 01 02

이 영역에는 ZIP 안에 어떤 파일들이 들어 있는지, 각 파일이 어디에 위치하는지와 같은 정보가 정리되어 있다.

ZIP 프로그램은 압축 파일을 열 때 단순히 앞에서부터 하나씩 읽는 것이 아니라, 파일 끝부분에 있는 `Central Directory`를 참고해 전체 파일 목록을 확인한다.

---

## 2-5. End of Central Directory

`End of Central Directory`는 ZIP 파일의 끝을 나타낸다.

    50 4B 05 06

이 부분에는 다음과 같은 정보가 들어 있다.

- ZIP 안에 들어 있는 파일 개수
- Central Directory의 크기
- Central Directory의 시작 위치
- 주석 길이

즉, ZIP 파일 전체 구조를 마무리하는 요약 정보라고 볼 수 있다.

---

## 2-6. ZIP 파일을 단순화해서 보면

    50 4B 03 04
    [파일 1의 정보]
    [파일 1의 실제 데이터]

    50 4B 03 04
    [파일 2의 정보]
    [파일 2의 실제 데이터]

    50 4B 01 02
    [파일 목록 정보]

    50 4B 05 06
    [ZIP 파일 끝]

---

## 2-7. ZIP 파일 분석 시 확인할 점

### 1. ZIP 시그니처가 존재하는지 확인하기

    50 4B 03 04

파일 확장자가 ZIP이 아니더라도, 내부에서 이 값이 발견되면 ZIP 데이터가 포함되어 있을 가능성이 있다.

### 2. 파일 끝부분의 구조 확인하기

ZIP 파일은 보통 끝부분에 `Central Directory`와 `End of Central Directory`가 존재한다.  
따라서 앞부분만 보는 것이 아니라 파일의 뒤쪽 구조도 함께 확인해야 한다.

### 3. 다른 파일 내부에 ZIP이 숨겨져 있는지 확인하기

포렌식 문제에서는 PNG와 같은 정상 파일 뒤에 ZIP 파일을 이어 붙여 숨기는 경우가 있다.  
이때 PNG의 `IEND` 이후에 `50 4B 03 04`가 보인다면, 뒤쪽 데이터를 분리해 ZIP으로 복구할 수 있다.

---

# 3. PNG와 ZIP 비교

| 항목 | PNG | ZIP |
| --- | --- | --- |
| 파일 시작 | `89 50 4E 47 0D 0A 1A 0A` | `50 4B 03 04` |
| 기본 구성 단위 | Chunk | File Entry |
| 주요 구조 | `IHDR`, `IDAT`, `IEND` | Local File Header, Central Directory, EOCD |
| 끝을 나타내는 구조 | `IEND` | `50 4B 05 06` |
| 포렌식 문제에서 자주 보는 포인트 | IEND 뒤 추가 데이터, 헤더 손상, 이미지 크기 조작 | 숨겨진 ZIP, 중첩 압축, 시그니처 탐색 |

---

# 정리

PNG 파일은 고정된 시그니처와 여러 개의 chunk로 구성되어 있으며, `IHDR`, `IDAT`, `IEND`가 핵심적인 역할을 한다.  
특히 `IEND`는 PNG 데이터의 끝을 의미하므로, 그 이후에 다른 데이터가 존재한다면 추가로 숨겨진 파일이 있는지 확인해볼 수 있다.

ZIP 파일은 개별 파일 정보를 담는 `Local File Header`, 파일 목록을 관리하는 `Central Directory`, 전체 구조의 끝을 나타내는 `End of Central Directory`로 구성된다.  
ZIP은 파일 목록 정보가 뒤쪽에 저장된다는 특징이 있으므로, 분석할 때는 파일 앞부분뿐만 아니라 끝부분도 함께 확인해야 한다.
