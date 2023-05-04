---
layout: single
title: "[JavaScript] 리페인트 & 리플로우"
categories: javascript
tag: [web, page, browser, rendering, reflow, repaint, transform, requestAnimationFrame]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

# Reflow

Reflow는 브라우저에서 HTML 요소들의 레이아웃을 재계산하는 과정을 말합니다.

 브라우저는 HTML, CSS, JavaScript 등의 리소스를 로드하고 해석하여 DOM을 구성하고 렌더 트리를 만듭니다. 이때 각 요소들의 크기와 위치를 계산하여 브라우저 창에 표시될 최종 레이아웃을 생성하는데, 이 과정을 reflow라고 합니다.

 reflow는 상대적으로 높은 비용을 요구하기 때문에, 불필요한 reflow를 줄이고 성능을 개선하기 위해서는 CSS 속성 변경 최소화 등 다양한 방법들을 고려할 수 있습니다.

## Reflow를 발생하게 하는 속성

width, height, padding, margin, float, position 등 레이아웃에 영향을 주는 모든 속성



# Repaint

Repaint는 웹 브라우저가 DOM 요소의 스타일 변경 등으로 인해 화면에 그려진 요소들을 다시 그리는 것을 말합니다.

 예를 들어, 요소의 배경색이 변경되는 경우 해당 요소의 repaint가 발생하여 요소의 색상이 새로운 배경색으로 다시 그려집니다. repaint는 reflow와 다르게 레이아웃 계산이 필요하지 않기 때문에 reflow에 비해 상대적으로 성능 부담이 적습니다.

## Repaint를 발생하게 하는 속성

color, border-radius, background, box-shadow 등 시각적으로 보여지는 모든 속성



리플로우와 리페인트는 렌더링 과정에서 레이아웃 단계와 페인트 단계를 다시(Re) 거치는 과정입니다.{: .notice--primary}

그렇다면 왜 리플로우와 리페인트는 렌더링 속도에 중요한 영향을 미칠까?

**렌더링 과정은 순차적으로 진행됩니다.** 각각의 렌더링 과정들은 반드시 전 단계의 데이터가 필요합니다. 만약 전 단계의 변화가 일어나면 다음 단계에 모두 영향을 미칩니다.

 특정 요소가 60fps로 움직이는 애니메이션을 생각해봅시다. 이때 요소의 레이아웃이 변경되기 때문에 렌더링 엔진은 60장의 프레임에 0.1초 간격으로 리플로우, 리페인트 과정을 수행해야 합니다. 이 과정 중에 0.1초 안으로 리플로우, 리페인트를 끝내지 못하면 우리 눈에는 애니메이션이 버벅거리는 것처럼 보입니다. 결국 reflow, repaint를 해야할 요소가 많을수록 자연스럽지 못한 애니메이션을 그리게 될 것이고 브라우저의 전체적인 성능에 영향을 미치게 됩니다.



# 불필요한 reflow, repaint를 줄이는 방법

## CSS Transform 속성을 사용합니다.

transform을 사용하여 만드는 애니메이션은 CPU 대신 GPU를 사용하여 화면 렌더링을 처리합니다.

CPU는 OS, 브라우저, 기타 프로세스 등으로 인해 이미 자원 소모가 심하다. GPU는 여러 개의 코어가 간단한 작업을 동시에 협업하는데 특화되어 있기 때문에 애니메이션을 빠르게 처리할 수 있습니다.

## requestAnimationFrame 함수를 사용합니다.

requestAnimationFrame 함수는 자바스크립트를 통해 일어나는 애니메이션 정보를 브라우저에 매 프레임마다 미리 알려줍니다. 이는 **자바스크립트 애니메이션이 프레임의 시작 시 실행되도록 보장합니다.**
