---
layout: single
title: "[JavaScript] 웹 페이지 렌더링"
categories: javascript
tag: [web, page, browser, rendering]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

# 웹 브라우저의 기본적인 구조

![image-20230505002140775]({{site.url}}/images/2023-05-05-browser-rendering/image-20230505002140775.png){: .align-center}

- 사용자 인터페이스 : 주소창, 뒤로가기/앞으로가기 버튼, 즐겨찾기 버튼, 새로고침 버튼 등 웹 페이지에서 보이는 부분 외의 영역
- 브라우저 엔진 : 사용자 인터페이스와 렌더링 엔진을 중개
- **렌더링 엔진** : HTML, CSS, JavaScript 코드를 사용자에게 렌더링해주는 역할
- 통신 : 이미지, 스타일시트 등을 다운받는데 사용됨
- JS 엔진 : 자바스크립트를 해석
- UI 백엔드 : OS에 의존하는 UI 형태(checkbox 등)를 그림
- 자료 저장소 : 쿠키 등과 같이 지속적으로 저장되어야할 자료를 저장하는 웹 데이터베이스



# 렌더링 엔진의 작동 순서

## 1. 파싱

**HTML을 파싱하여 DOM으로 변환합니다.**

- 오타 혹은 잘못된 문법을 사용했을 경우 예외처리를 합니다.

- <link>, <img> 와 같은 태그를 만나면 리소스를 다운로드합니다.

- <script> 태그를 만나면 DOM 파싱을 중단하고 자바스크립트를 해석합니다.

## 2. 스타일 계산

**CSS를 파싱하여 CSSOM으로 변환합니다.**

- CSSOM 정보를 통해 DOM 노드에 대한 스타일을 결정합니다.
- 결정된 스타일은 크롬 개발자 도구의 computed 항목에서 확인 가능합니다.

## 3. 레이아웃

**레이아웃 트리(렌더 트리)를 생성합니다.**

* DOM과 계산된 스타일을 따라가며 요소의 크기나 좌표와 같은 정보를 담은 레이아웃 트리를 생성합니다.
* 화면에 표현되는 정보만 트리에 담기게 됩니다. (display: none X, visibility: hidden O, 가상요소 O)

## 4. 페인트

**레이아웃 트리(렌더 트리)가 생성되면 이 트리를 따라 페인트 기록이 생성됩니다.**

- 페인트 기록에는 요소를 렌더링하는 순서가 저장됩니다. 그리고 지금까지의 정보를 바탕으로 한 페이지를 여러 개의 레이어로 나눈 뒤 그 위에 텍스트, 색, 이미지, 보더, 그림자 등의 모든 시각적 부분을 그리는 작업이 진행됩니다.

## 5. 컴포지팅

**각각의 레이어를 스크린에 픽셀로 표현하고(레스터링) 나누었던 레이어들을 합성해 페이지를 그립니다. 이를 컴포지팅이라고 합니다.**
