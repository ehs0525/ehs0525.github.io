---
layout: single
title: "[JavaScript] BOM & DOM & Node"
categories: javascript
tag: [BOM, DOM, node]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

# 1. BOM (Browser Object Model)

웹 브라우저의 다양한 기능을 객체처럼 다루는 모델

BOM에는 window, screen, location, navigator 등 다양한 객체가 있습니다.

## window 객체

- Global Context (전역 공간)이자 브라우저 창을 나타내는 객체입니다.
- 전역 변수나 전역 함수의 경우 window의 프로퍼티처럼 작동하게 됩니다.
- 중요 프로퍼티 : innerWidth, innerHeight, screenX, screenY, scrollBy(), scrollTo()

## screen 객체

- 사용자 환경의 디스플레이(모니터) 정보를 가지는 객체입니다.
- 중요 프로퍼티 : availHeight, availWidth, width, height, orientation

## location 객체

- 사용자가 보고 있는 페이지의 URL을 다루는 객체입니다.
- 중요 프로퍼티 : href(페이지간 이동 가능), reload, replace(현재 페이지를 교체, 히스토리가 삭제됨)

## navigator 객체

- 웹 브라우저 및 브라우저 환경정보를 가지는 객체입니다.
- 중요 프로퍼티 : userAgent(브라우저를 접속한 OS를 확인)



# 2. DOM (Document Object Model)

자바스크립트 Node 객체의 계층화된 트리

DOM에는 document, element, text, comment 등 다양한 노드가 있습니다.

## document 노드

- 웹 페이지마다 존재하는 객체입니다. 웹 페이지 안의 모든 컨텐츠를 다루는 시작점입니다.
- 중요 프로퍼티 : title, url, doctype, documentElement, head, body, getElementById, createElement, querySelector, readyState

## element 노드

- 웹 페이지 안의 각 html 태그 요소들을 의미합니다.
- 중요 프로퍼티 : querySelector(전체가 아닌 element 안의 다른 element를 찾는 데 한정됨), classList, dataset, id, innerHTML, parentNode, nextSibling, previousSibling
