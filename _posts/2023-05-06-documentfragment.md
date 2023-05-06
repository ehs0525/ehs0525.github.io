---
layout: single
title: "[JavaScript] DocumentFragment"
categories: javascript
tag: [documentFragment, fragment]
author_profile: false
search: true
use_math: false
sidebar:
  nav: "counts"
---



자바스크립트로 DOM을 만들고 추가하는 일반적인 방법은 document.createElement()로 생성하고 appendChild()를 이용해 바로 등록을 하는 방법입니다. 이때 DocumentFragment 노드를 사용하면 오직 메모리상에만 존재하는 경량화된 DOM을 만들 수 있습니다.

```html
.
.
.
<body>
    <script>
    	const docFrag = document.createDocumentFragment();
        for(let i = 0; i < 10; i++) {
            let divEl = document.createElement("div");
            divEl.innerText = "Item " + i;
            // document.body.appendChild(divEl);
            docFrag.appendChild(divEl);
        }
        document.body.appendChild(docFrag);
    </script>
</body>
.
.
.
```

***div 요소를 하나 만들어서 노드 트리를 만드는 것과 어떤 차이가 있을까?***

# DocumentFragment의 특징

1. DocumentFragment를 DOM에 추가한다고 해도 DocumentFragment 노드는 등록되지 않고 그 자식 노드들만 추가됩니다.
2. DocumentFragment를 DOM에 추가하면 DocumentFragment 노드의 자식 요소들은 더이상 메모리상에 존재하지 않습니다.

또한 이러한 특징 때문에 요소를 여러 개의 각기 다른 부모 요소에 추가할 때 더욱 깔끔한 코딩이 가능합니다.

```html
.
.
.
<body>
	<div class="container"></div>
	<div class="container"></div>
    <div class="container"></div>
    <script>
        const frag = document.createDocumentFragment();
        
    	//let elements = [];
        for(let i = 0; i < 100; i++) {
            const el = document.createElement('div');
            //elements.push(el);
            frag.appendChild(el);
        }
        
        const containers = document.querySelectorAll(".container");
        for(let i = 0; i < containers.length; i++) {
            //for(let j = 0; j < elements.length; j++) {
            //	containers[i].appendChild(elements[j].cloneNode(true));
            //}
            containers[i].appendChild(frag.cloneNode(true));
        }
        
        console.log(frag.childNodes);	// NodeList(100) [div, ..., div]
    </script>
</body>
.
.
.
```

2번 특징에서 DocumentFragment의 자식 노드를 유지시키고 싶다면 cloneNode를 통해 복제하는 방법이 있습니다.  
