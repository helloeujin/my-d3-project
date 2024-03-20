# my-d3-project

## 0. D3 설치 및 추가하기

D3.js 라이브러리를 추가하기 위해 다음의 명령어를 실행합니다.

```bash
npm install d3
```

`main.js` 파일 맨 위에 아래와 같이 D3 라이브러리를 추가합니다. 이제 D3 코딩을 할 준비가 되었습니다!

```bash
import * as d3 from "d3";
```
<br />

## 1. 데이터 준비하기

D3에서는 CSV, JSON, XML 같이 다양한 형식의 데이터를 로딩해서 사용할 수 있습니다. 여기에서는 연습을 위해 다음을 `data` 로 사용해봅시다.  

```jsx
const data = [
    { x: 30, y: 30, radius: 20 },
    { x: 70, y: 70, radius: 15 },
    { x: 110, y: 100, radius: 25 },
    { x: 150, y: 30, radius: 10 },
    { x: 190, y: 90, radius: 20 }
];
```

## 2. **SVG 캔버스 설정**

시각화를 만들기 위해 웹 페이지의 `<body>`에 SVG 요소를 추가합니다. 너비와 높이를 속성으로 정의합니다. 이 안에서 모든 그래픽 요소들이 그려집니다.

```jsx
const svg = d3.select("body")
  .append("svg")       
  .attr("width", 500)  
  .attr("height", 500) 
```

### 선택자 사용하기

- `d3.select` 또는 `d3.selectAll` 함수를 사용하여 DOM 내의 요소 선택할 수 있습니다.
    - `d3.select('circle')` → circle이 태그인 요소
    - `d3.select('.circle')` → 클래스이름이 circle인 요소
    - `d3.select('#circle')` → 아이디가 circle인 요소

## 3. **데이터 바인딩**

데이터 바인딩은  `.data()` 함수를 데이터를 DOM 요소에 연결하는 것을 의미합니다. 이로인해 데이터의 변경이 자동으로 DOM에 반영되어, 동적인 웹 시각화를 만들 수 있습니다.

- `svg.selectAll("circle")`: 현재 SVG 내의 모든 원(`circle`)을 선택합니다. 처음 실행 시에는 원이 없으므로, 빈 선택 집합이 반환됩니다.
- `.data()` ****를 사용하여 데이터를 바인딩합니다. 즉, 데이터 배열을 DOM 요소에 연결합니다.
- `.enter()` 는 아직 DOM에 존재하지 않는 데이터 항목에 대해 ‘빈 자리’를 준비합니다. 이는 데이터에 따라 새로운 그래픽 요소들을 나중에 추가할 수 있는 공간을 의미합니다.

```jsx
const circles = svg.selectAll("circle")
	   .data(data)
	   .enter()
```

### 그래픽 요소 **만들기**

- `.append()` 로 새로운 그래픽 요소(예: `circle`)를 생성합니다.
- `.attr()` 로 각 그래픽 요소의 속성(예: 위치, 크기, 색상 등)을 데이터에 따라 설정합니다.

```jsx
circles
	   .append("circle")
	   .attr("cx", d => d.x)  
	   .attr("cy", 250)  
	   .attr("r", d => d.radius) 
	   .attr("fill", "blue"
		 .attr("stroke", "black")
		.attr("stroke-width", 3);
```

완성하면 아래와 같이 그래픽 요소들이 만들어집니다.

![출처: 작가](https://prod-files-secure.s3.us-west-2.amazonaws.com/9bc85f59-fd6d-44a5-a575-1befd5c59eba/3e8d7177-834a-4050-a408-3a0b0323a473/Screenshot_2024-01-10_at_3.04.10_PM.png)

출처: 작가

## 4. **인터랙션 및 전환 추가**

### 인터랙션 추가

D3는 `.on()` 함수를 사용하여 DOM 요소에 이벤트를 추가합니다. 이를 통해 사용자의 상호작용에 따라 동적으로 반응할 수 있습니다.

- `mouseover` → 마우스가 원 위에 올라갈 때 이벤트가 발생합니다
- `mouseout` → 마우스가 원에서 벗어날 때 이벤트가 발생합니다

### **전환(Transition) 추가**

시각화 요소들의 상태 변화를 부드럽게 연출할 수 있는 애니메이션 기능을 제공합니다.

- `.transition()` 를 사용하여 속성 변경이 시간에 따라 부드럽게 적용되도록 합니다.
- `.duration()` 로 전환 지속 시간을 설정합니다.

```jsx
// 마우스 오버 이벤트: 원의 크기와 색상을 변경
circles.on("mouseover", function() {
    d3.select(this)
      .transition()            
      .duration(500)         
      .attr("r", d => d.radius * 2)               
});

// 마우스 아웃 이벤트: 원을 원래 크기와 색상으로 복원
circles.on("mouseout", function() {
    d3.select(this)
      .transition()             
      .duration(500)            
      .attr("r", d => d.radius)              
});
```
