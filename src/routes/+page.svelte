<script>
	// Custom Components/Routes
	import Counter from './Counter.svelte';
	import Thing from './Thing.svelte';

	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';

	import { createEventDispatcher } from 'svelte';
	import { onMount } from 'svelte';
    import { construct_svelte_component } from 'svelte/internal';

	const dispatch = createEventDispatcher();
	function sayHello() {
		// dispatch('message', {
		// 	text: 'Hello!'
		// });
		let randomNum = Math.floor(Math.random() * 1000);;
		alert(randomNum);
	}

	function handleMessage(event) {
		alert(event.detail.text);
	}
	let user = { loggedIn: false };

	function toggle() {
		user.loggedIn = !user.loggedIn;
	}
	let cats = [
		{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
		{ id: 'z_AbfPXTKms', name: 'Maru' },
		{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
	];
	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' },
	];

	function handleClick() {
		things = things.slice(1);
	}

	/*-----Globals to start game with initial values-----*/
	const modes = ['EASY', 'HARD'];
	const circles = ['circle', 'circle', 'circle', 'circle', 'circle', 'circle'];
	let colors = [1];
	let color_display;


	export function handleCircleClick(event) {
        // event.currentTarget.nextElementSibling.classList.toggle('close')
		console.log(colors);
    }
	let numCircles = 6;
	let pickedColor = null;

	let modeSelected = { easy: false };

	function toggleMode(e) {
		e.target.id == "EASY" ? numCircles = 3 : numCircles = 6;
		reset(numCircles);
	}

	/*----Picks random color from colors array----*/
	function pickColor(number){
	    let random = Math.floor(Math.random() * number);
	    return colors[random];
	}

	/*----Generates either 6 or 3 colors based on difficulty selected----*/
	function generateRandomColors(num){
		let arr = [];
		for(let i = 0; i < num; i++) {
			arr.push(randomColor())
		}
		return arr;
	}

	/*----Resets colors, picks a color from color array, sets colors to circles----*/
	function reset(number){
		// if(number === 3) {
		// 	//hide hard mode circles
		// 	hardModeCircles.style.display = 'none'
		// }

		colors = generateRandomColors(number);		
		let pickedColor = pickColor(number);
		color_display !== null ? color_display.textContent = pickedColor : null;
		// messageDisplay !== null ? messageDisplay.textContent = '' : null;
		// change colors of circles
		// if(circles.length > 0) {
		// 	for(let i = 0; i < number; i++){
		// 		console.log('CIRCLE:', circles[i]);
		// 		circles[i].style.display = 'block';
		// 		circles[i].style.background = colors[i];
		// }
		// }

		console.log('reset');
		console.log(colors)
	}

	export function init(){
		// setupCircles();
		reset(numCircles);
	}

	/*----Function to apply click handler onto each circle in game area----*/
	function setupCircles(){
		console.log('SETUP CIRCLES')
		// for(let i = 0; i < circles.length; i++){
		//     circles[i].addEventListener('click', function() {
		//         let clickedColor = this.style.background;
		//         if (clickedColor === pickedColor) {
		//             messageDisplay.textContent = 'Correct';
		//             resetButton.textContent = 'Play Again?';
		//             changeColors(clickedColor);
		//             h1.style.background = clickedColor;
		//         } else {
		//             this.style.background = '#232323';
		//             messageDisplay.textContent = 'Try again';
		//         }
		//     });
		// }
	}

	/*-----Click Handler for reset button-----*/
	// resetButton.addEventListener('click', function(){
	//     reset();
	// });

	/*----Sets correct colors to each circle----*/
	// function changeColors(color){
	//     for(let i = 0; i < circles.length; i++){
	//         circles[i].style.background = color;
	//     }
	// }


	/*----Picks 3 random colors (r, g, b)----*/
	function randomColor(){
		let r = Math.floor(Math.random() * 256);
		let g = Math.floor(Math.random() * 256);
		let b = Math.floor(Math.random() * 256);
		return "rgb(" + r + ", " + g + ", " + b + ")";
	}

	onMount(() => {
		/*-----Initialize Game-----*/
		init();

		/*-----Selectors to change elements on DOM-----*/
		// let circles = document.getElementsByClassName('circle');
		// let colorDisplay = document.getElementById('colorDisplay');
		// let messageDisplay = document.querySelector('#message');
		// let hardModeCircles = document.getElementsByClassName('hard-mode-circle');
		// let h1 = document.querySelector('h1');
		// let resetButton = document.querySelector('#reset');
		// let modeButtons = document.querySelectorAll('.mode');

	});
</script>

<svelte:head>
	<title>RGB Color Game - JY</title>
	<meta name="description" content="RBG Guessing Game" />
</svelte:head>

<section>
	<h1>The Great 
		<br>
		<span bind:this={color_display} id="colorDisplay">RBG</span> 
		<br>Color Game</h1>

		<div id='stripe'>
			<button id='reset'>New Colors</button>
			<span id="message"></span>
			<button class={numCircles == 3 ? `selected` : ``} id="EASY" on:click={toggleMode}>EASY</button>
			<button class={numCircles == 6 ? `selected` : ``} id='HARD' on:click={toggleMode}>HARD</button>
			<!-- Attempt to rewrite later 
			{#each modes as mode, j}
			<button class={numCircles == 3 ? `selected` : ``} id={mode} on:click={toggleMode}>{mode}</button>
			{/each} -->
		</div>
	
		<div id="container">
			{#each colors as color, i}

			<div
				style='background-color: {color}'
				class="circle" 
				id={i.toString()} 
				on:click={handleCircleClick} 
				on:keyup={() => console.log('keyup')}
				>
			</div>
		
			{/each}
		</div>
	<!-- <button on:click={sayHello}>Click for a random number</button> -->
                                
	<!-- {#if user.loggedIn}
		<button on:click={toggle}>
		Log out
		</button>
	{:else}
		<button on:click={toggle}>
			Log in
		</button>
	{/if}

	<button on:click={handleClick}>
		Remove first thing
	</button>
	
	{#each things as thing (thing.id) }
		<Thing name={thing.name}/>
	{/each}

	<h1>The Famous Cats of YouTube</h1>

	<ul>
		{#each cats as { id, name }, i}
			<li><a target="_blank" href="https://www.youtube.com/watch?v={id}" rel="noreferrer">
				{i + 1}: {name}
			</a></li>
		{/each}
	</ul>
	<h1>
		<span class="welcome">
			<picture>
				<source srcset={welcome} type="image/webp" />
				<img src={welcome_fallback} alt="Welcome" />
			</picture>
		</span>

		to your new<br />SvelteKit app
	</h1>

	<h2>
		try editing <strong>src/routes/+page.svelte</strong>
	</h2>

	<Counter />  -->
</section>

<style>

.circle {
	caret-color: transparent;
    width: 30%;
    background: white;
    padding-bottom: 30%;
    border-radius: 50%;
    float: left;
    margin: 1.66%;
    transition: background 0.6s;
    --webkit-transition: background 0.6s;
    --moz-transition: background 0.6s;
}

#container {
    max-width: 600px;
    margin: 20px auto;
}

h1 {
    color: white;
    line-height: 1.1;
    text-align: center;
    background-color: steelblue;
    margin: 0;
    font-weight: normal;
    text-transform: uppercase;
    padding: 20px 0;

}


#stripe {
    background: white;
    height: 30px;
    text-align: center;
    color: black;
    }

.selected {
    background: steelblue;
    color: white;

}

#colorDisplay {
    font-size: 200%;
}
 
button {
    border: none;
    background: none;
    text-transform: uppercase;
    height: 100%;
    font-weight: 700;
    color: steelblue;
    letter-spacing: 1px;
    font-size: inherit;
    transition: all 0.3s;
    --webkit-transition: all 0.3s;
    --moz-transition: all 0.3s;
    outline: none;
}

button:hover {
    color: white;
    background: steelblue;
}

#message {
    width: 20%;
    display: inline-block;
    font-weight: 500;
}
</style>
