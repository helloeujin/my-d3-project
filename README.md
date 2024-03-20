# my-d3-project
<br />

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

<br />

## 2. **SVG 캔버스 설정**

시각화를 만들기 위해 웹 페이지의 `<body>`에 SVG 요소를 추가합니다. 너비와 높이를 속성으로 정의합니다. 이 안에서 모든 그래픽 요소들이 그려집니다.

```jsx
const svg = d3.select("body")
  .append("svg")       
  .attr("width", 500)  
  .attr("height", 500) 
```

<br />

### 선택자 사용하기

- `d3.select` 또는 `d3.selectAll` 함수를 사용하여 DOM 내의 요소 선택할 수 있습니다.
    - `d3.select('circle')` → circle이 태그인 요소
    - `d3.select('.circle')` → 클래스이름이 circle인 요소
    - `d3.select('#circle')` → 아이디가 circle인 요소


<br />

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

<br />

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

<img width="508" alt="Screenshot 2024-01-10 at 3 04 10 PM" src="https://github.com/helloeujin/my-d3-project/assets/2341775/988855b5-e7fc-40cf-8f06-47b3d1efe22b">

출처: 작가

<br />

## 4. **인터랙션 및 전환 추가**

### 인터랙션 추가

D3는 `.on()` 함수를 사용하여 DOM 요소에 이벤트를 추가합니다. 이를 통해 사용자의 상호작용에 따라 동적으로 반응할 수 있습니다.

- `mouseover` → 마우스가 원 위에 올라갈 때 이벤트가 발생합니다
- `mouseout` → 마우스가 원에서 벗어날 때 이벤트가 발생합니다

  <br />

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


<br />

## 완성된 파일 살펴보기

다음은 완성된 `main.js` 파일입니다.

```jsx
import * as d3 from "d3";

// SVG 캔버스 생성 및 설정
const svg = d3
  .select("body")
  .append("svg")
  .attr("width", 500)
  .attr("height", 500);

// 객체 배열 데이터
const data = [
  { x: 50, y: 50, radius: 40 },
  { x: 150, y: 150, radius: 30 },
  { x: 250, y: 250, radius: 50 },
  { x: 350, y: 350, radius: 20 },
  { x: 450, y: 450, radius: 40 },
];

// 데이터에 기반한 원 생성
const circles = svg
  .selectAll("circle")
  .data(data)
  .enter()
  .append("circle")
  .attr("cx", (d) => d.x) // 객체의 x 속성을 원의 x 좌표로 사용
  .attr("cy", 250) // 객체의 y 속성을 원의 y 좌표로 사용
  .attr("r", (d) => d.radius) // 객체의 radius 속성을 원의 반지름으로 사용
	.attr("fill", "blue") // 원의 색상 설정
  .attr("stroke", "black")
  .attr("stroke-width", 3);

// 마우스 오버 이벤트: 원의 크기와 색상을 변경
circles.on("mouseover", function () {
  d3.select(this)
    .transition()
    .duration(500)
    .attr("r", (d) => d.radius * 2);
});

// 마우스 아웃 이벤트: 원을 원래 크기와 색상으로 복원
circles.on("mouseout", function () {
  d3.select(this)
    .transition()
    .duration(500)
    .attr("r", (d) => d.radius);
});
```

<br />

## JSON 테스트해보기
```
[
  {
    "fruit": "사과",
    "quantity": 20,
    "details": {
      "color": "red",
      "price": 1.2
    }
  },
  {
    "fruit": "바나나",
    "quantity": 40,
    "details": {
      "color": "yellow",
      "price": 0.8
    }
  },
  {
    "fruit": "체리",
    "quantity": 60,
    "details": {
      "color": "red",
      "price": 2.5
    }
  },
  {
    "fruit": "딸기",
    "quantity": 80,
    "details": {
      "color": "red",
      "price": 1.8
    }
  },
  {
    "fruit": "포도",
    "quantity": 100,
    "details": {
      "color": "purple",
      "price": 2.0
    }
  }
]
```
