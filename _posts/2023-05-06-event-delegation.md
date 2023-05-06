---
layout: single
title: "[JavaScript] 이벤트 위임"
categories: javascript
tag: [event, delegate, delegation]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---

요소에 이벤트를 등록하는 일반적인 방법은 addEventListener()를 이용하는 방법입니다. 하지만 수십, 수백 개의 요소에 이벤트를 등록하고 싶다고 해서 요소마다 일일이 addEventListner()를 연결할 수는 없는 노릇입니다. 이벤트 흐름을 잘 이용한다면 단 1개의 이벤트 리스너로 수많은 요소의 이벤트를 처리할 수 있도록 만들 수 있습니다.

만약 이벤트 리스너가 div 요소에 있고 사용자가 부모 요소 div의 자식인 <button> 태그를 클릭했다면 브라우저는 이벤트가 발생한 button 태그를 찾기 시작할 것이고 이벤트 캡처링과 버블링을 통해 button 태그의 부모 요소인 div의 이벤트 리스너를 실행시킬 것입니다.

이때 **event 객체에는 DOM에서 일어나는 이벤트의 정보가 들어있습니다**. event.currentTarget은 이벤트가 등록된 요소를 가르킵니다. 이는 이벤트 리스너 안의 this가 참조하는 대상과 같습니다. 그리고 이벤트가 최초에 발생한 요소는 event.target에 참조됩니다.

```html
.
.
.
<body>
    <div>
        <button type="button">button</button>
    </div>
    
    <script>
    	const divEl = document.querySelector("div");
        divEl.addEventListener('click', function(event) {
            console.log(event.currentTarget);	// 버튼 클릭시 <div>...</div>
            console.log(event.target);	// 버튼 클릭시 <button>...</button>
            console.log(this);	// 버튼 클릭시 <div>...</div>
        })
    </script>
</body>
.
.
.
```



# 이벤트 위임

```html
.
.
.
<body>
    <div class="parent">
        <button type="button">Generate Item</button>
        <ul>
            <li>Item added</li>
        </ul>
    </div>
    
    <script>
    	const parent = document.querySelector(".parent");
        
        parent.addEventListener('click', function(event) {
            if(event.target.tagName.toLowerCase() === "button") {
                const item = document.createElement('li');
                item.innerText = "Item added";
                parent.querySelector('ul').appendChild(item);
            }
            
            if(event.target.tagName.toLowerCase() === "li") {
                console.log("HIT!");
            }
        })
    </script>
</body>
.
.
.
```

 부모 요소에 이벤트를 등록하여 마치 자식 요소들에게 이벤트가 등록된 것처럼 코드를 짤 수 있다. 즉, 이벤트를 발생시키고 싶은 요소를 이벤트 리스너가 설치된 부모 요소의 자식으로 배치한다면 그 요소가 몇 개든 상관없이 이벤트를 등록할 수 있습니다. 또한, 요소가 동적으로 생성되어 계속 추가되어도 같은 기능을 유지합니다.

 이렇게 **이벤트 흐름을 활용하여 단일 이벤트 리스너가 여러 개의 이벤트 대상을 처리할 수 있게하는 프로그래밍**을 이벤트 위임(event delegation)이라고 합니다.
