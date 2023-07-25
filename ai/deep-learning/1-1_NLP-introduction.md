# NLP Introduction

<!-- TOC -->

- [NLP Introduction](#nlp-introduction)
    - [NLP 란?](#nlp-%EB%9E%80)
    - [NLP 의 역사](#nlp-%EC%9D%98-%EC%97%AD%EC%82%AC)
    - [텍스트 전처리](#%ED%85%8D%EC%8A%A4%ED%8A%B8-%EC%A0%84%EC%B2%98%EB%A6%AC)
        - [토큰화 Tokenization](#%ED%86%A0%ED%81%B0%ED%99%94-tokenization)
        - [정제 Cleaning](#%EC%A0%95%EC%A0%9C-cleaning)

<!-- /TOC -->

<br>

## NLP 란?

> - 자연어 (Natural Language) : 일상 생활에서 사용하는 보편적 언어
> - 자연어 처리 (Natural Language Processing, NLP) : 컴퓨터가 자연어를 처리하는 일

<br>

- 자연어 처리로 해결할 수 있는 문제
    - 음성 인식
    - 번역
        - Encoding
        - Time Series Modeling
        - Attention Mechanism
        - Self-Attention
    - 요약
    - 분류

<br>

## NLP 의 역사

- 1980 년대
    - Deep Learning 이론이 탄생했으나, 고작 이론에 불과하여 자연어처리에 관해서 다뤄지지 않았음.
- 2000 년대
    - 1D-CNN
    - RNN
- 2010 년대
    - Attention Machanism
    - Transformer

<br>

## 텍스트 전처리

> 자연어를 효과적으로 처리할 수 있도록 전처리 과정을 거친다.

- 토큰화
- 정제 및 추출
- 인코딩

<br>

### 토큰화 (Tokenization)

> 주어진 문장에서 "의미 부여"가 가능한 단위를 찾는다.

- 한국어는 하나의 단어를 다양한 형태로 나타낼 수 있어서 토큰화가 어렵다.

- 종류
    - 표준 토큰화 (Treebank Tokenization)
    - 문장 토큰화

<br>

### 정제 (Cleaning)

> 데이터 사용 목적에 맞추어 노이즈를 제거

- 대문자, 소문자에 따라 의미가 바뀌는 경우가 있으므로 주의
    - `US` vs `us`
- 출현 횟수가 적은 단어의 제거 주의