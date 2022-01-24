## Svelte 입문가이드 강의 듣고 정리

## Svelte 특징분
- 더 적은 코드를 작성 할 수 있다
- 높은 가독성 유지
- 개발 시간 단축
- 쉬운 리팩토링
- 쉬운 디버깅
- 더 작은 번들(SPA 최적화)
- 낮은 러닝커브

### No virtual DOM
- No Diffing
  - virtual dom 비교 할 필요 X virtual dom(비교 -> 갱신)
- No Overhead
  - 오버헤드는 어떤 처리를 위해 들어가는 간접적인 시간이나 메모리등을 말함
- 빠른 성능(퍼포먼스)

### Truly reactive
- Framework-less / VanillaJs
- Only use 'devDependencies'
- 명시적 설계(창의적 작업)

### 단점
- 낮은 성숙도(작은 생태계)
- CDN 미제공
- IE 지원

## 선언적 렌더링
~~~sveltehtml
<script>
	let name = 'world';
	let age = 85
	function assign(){
		name = 'soojuing'
		age = 35
	}
</script>

<h1>Hello {name}!</h1>
<h2 class={age < 85 ? 'active' : '' }>
	{age}
</h2>
<img src="" alt={name} />
<input type="text" bind:value={name} />
<button on:click={assign}>
	Assign
</button>

<style>
	h1 {
		color : red;
	}
	.active {
		color: blue;
	}
</style>
~~~


## 조건문과 반복문
```sveltehtml
<script>
	let name = 'world';
	let toggle = false
</script>

<button on:click={() => {toggle =!toggle}}>
	Toggle
</button>
{#if toggle} 
<h1>Hello {name}!</h1>
{:else} 
  <div>No name!</div>
{/if}
```

```sveltehtml
<script>
	let name = 'Fruits';
	let fruits = ['Apple', 'Bananan', 'Cherry', 'Orange', 'Mango']
	function deleteFruit(){
		fruits=fruits.slice(1)
	}
</script>

<h1>Hello {name}!</h1>
<ul>
	{#each fruits as fruit}
    <li>{fruit}</li>
	{/each}
</ul>
<button on:click={deleteFruit}>
	Eat it
</button>
```


## 이벤트 핸들링
```sveltehtml
<script>
	//import { onMount } from 'svelte'
	let name = 'world';
	let isRed = false
// onMount(()=>{
//		const box = document.querySelector('.box')
//		box.addEventListener('click', () => { isRed = !isRed })
//	})
	function enter(){
		name = 'enter'
	}
	function leave(){
		name = 'leave'
	}
</script>

<h1>Hello {name}!</h1>
<div class="box" 
		 style="background-color: {isRed ? 'red':'orange'};"
		 on:click={ () => { isRed = !isRed }}
	   on:mouseenter={enter}
	   on:mouseleave={leave}>Box!</div>

<style>
	.box{
		width: 300px;
		height: 150px;
		background-color : orange;
	}
</style>
```

```sveltehtml
<script>
	let text = ''
</script>

<h1>
	{text}
</h1>
<input type="text" 
			 value={text}
			 on:input={(e) => {text = e.target.value}} />
<input type="text" bind:value={text} />
<button on:click={() => {text ='Soojung'}}>
	click
</button>
```

## 컴포넌트
```sveltehtml
<script>
	let fruits = ['Apple', 'Banana', 'Cherry', 'Orange', 'Mango']
</script>

<h2>
	Fruits
</h2>
<ul>
	{#each fruits as fruit}
	  <li>{fruit}</li>
	{/each}
</ul>

<h2>
	Fruits reverse
</h2>
<ul>
	<!-- 복사본이 만들어짐
   ...['Apple', 'Banana', 'Cherry', 'Orange', 'Mango']
    'Apple', 'Banana', 'Cherry', 'Orange', 'Mango'
    ['Apple', 'Banana', 'Cherry', 'Orange', 'Mango']
  -->
	{#each [...fruits].reverse() as fruit}
	  <li>{fruit}</li>
	{/each}
</ul>


<h2>
	Fruits slice-2
</h2>
<ul>
	{#each fruits.slice(-2) as fruit}
	  <li>{fruit}</li>
	{/each}
</ul>
```

위의 코드를 아래와 같이 바꿀 수 있음
- App.svelte
```sveltehtml
<script>
	import Fruits from './Fruits.svelte'
	let fruits = ['Apple', 'Banana', 'Cherry', 'Orange', 'Mango']
</script>

<Fruits fruits={fruits} />
<Fruits fruits={fruits} reverse />
<Fruits fruits={fruits} slice='-2'/>
<Fruits fruits={fruits} slice='0, 3'/>
```

- Fruits.svelte
```sveltehtml
<script>
	//Props
	export let fruits
	export let reverse
	export let slice
	
	let computedFruits = []
	let name = ''
	
	if (reverse){
		computedFruits = [...fruits].reverse()
		name = 'reverse'
	}else if (slice) {
		// slice => '-2', '0,3'
		// slice.split(',') => ['-2'], ['0', '3']
		//  ['-2'] => '-2'
		// ['0', '3'] => '0', '3'
	  computedFruits = fruits.slice(...slice.split(','))
		// name = 'slice '+ slice
		name = `slice ${slice}`
	}else{
		computedFruits = fruits
	}
</script>

<h2>
	Fruits {name}
</h2>
<ul>
	{#each computedFruits as fruit}
	  <li>{fruit}</li>
	{/each}
</ul>

```


## 스토어

- App.svelte
```sveltehtml
<script>
	import { storeName } from './store.js'
	import Parent from './Parent.svelte'
	let name = 'world';
	$storeName = name
</script>

<h1>Hello {name}!</h1>
<Parent/>
```

- Parent.svelte
```sveltehtml
<script>
 import Child from './Child.svelte'
</script>
<div>
	Parent
</div>
<Child/>
```

- Child.svelte
- $ 표시를 붙여줘야지 데이터처럼 사용 할 수 있음
- 변수 이름을 지을때 $ 예약어이기 때문에 사용 X 
```sveltehtml
<script>
	import { storeName } from './store.js'
</script>
<div>
	Child {$storeName}
</div>
```

- store.js
- svelte store 가 내장되어 있음
- store 에는 writable, readable, derived, get 함수 들어있음
```js
import { writable } from 'svelte/store'

export let storeName = writable('Soojung')
```

## Todo 예제 만들기
```sveltehtml

```







## 참고
- [svelte 공식홈페이지](https://svelte.dev/)
- [svelte 입문가이드 강의](https://www.inflearn.com/course/%EC%8A%A4%EB%B2%A8%ED%8A%B8-%EC%9E%85%EB%AC%B8-%EA%B0%80%EC%9D%B4%EB%93%9C)