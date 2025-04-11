<script>
	import { fade } from 'svelte/transition';
  
	let questions = [
	  { 
		question: "Jaká planeta je největší ve Sluneční soustavě?", 
		options: ["Venuše", "Jupiter", "Neptun", "Saturn"], 
		correct: 1 
	  },
	  { 
		question: "Která planeta je ke Slunci nejblíže?", 
		options: ["Venuše", "Země", "Merkur", "Mars"], 
		correct: 2 
	  },
	  { 
		question: "Jak se jmenuje naše galaxie?", 
		options: ["Mléčná dráha", "Andromeda", "Orion", "Velká mlhovina"], 
		correct: 0 
	  },
	  { 
		question: "Z kolika planet se skládá naše Sluneční soustava?", 
		options: ["7", "8", "9", "10"], 
		correct: 1 
	  },
		  { 
		question: "Která planeta je známá díky svým prstencům?", 
		options: ["Venuše", "Saturn", "Uran", "Neptun"], 
		correct: 1 
	  },
	];
  
	let currentQuestion = 0;
	let score = 0;
	let answered = false;
	let selectedAnswer = null;
	let timeLeft = 10;
	let timer;
  
	function startTimer() {
	  clearInterval(timer);
	  timeLeft = 15;
	  timer = setInterval(() => {
		if (timeLeft > 0) {
		  timeLeft--;
		} else {
		  clearInterval(timer);
		  nextQuestion();
		}
	  }, 1000);
	}
  
	function selectAnswer(index) {
	  if (!answered) {
		answered = true;
		clearInterval(timer);
		selectedAnswer = index;
		if (index === questions[currentQuestion].correct) {
		  score++;
		}
		setTimeout(nextQuestion, 1500);
	  }
	}
  
	function nextQuestion() {
	  if (currentQuestion < questions.length - 1) {
		currentQuestion++;
		answered = false;
		selectedAnswer = null;
		startTimer();
	  } else {
		currentQuestion++;
		clearInterval(timer);
	  }
	}
  
	function restartQuiz() {
	  currentQuestion = 0;
	  score = 0;
	  answered = false;
	  selectedAnswer = null;
	  startTimer();
	}
  
	startTimer();
  </script>
  
  
  <h1>Příklad kvíz</h1>
  <p>Tento kvíz je zaměřen na Sluneční soustavu, je zde 5 otázek, kde je vždy pouze jedna z odpovědí správně. Pokud se odpoví správně tak odpověď zezelená, pokud se odpoví špatně odpověď zčervená a zároveň se zobrazí správná odpověď zeleně. Je zde zobrazeno aktuální skóre počtu bodů které se aktualizuje podle odpovědí na otázky. Zároveň je zde nastaven čas na odpověď a to 15 sekund pokud se neodpoví na otázku včas tak se otázka přeskočí a kvíz pokračuje. Ke konci kvízu se zobrazí celkové skóre spolu s tlačítkem hrát znovu.</p>
  <hr>
  
  <main>
	{#if currentQuestion < questions.length}
		  <h1>Kvíz</h1>
	  <div transition:fade>
		<h2>{questions[currentQuestion].question}</h2>
				<p>Aktuální skóre: {score} / {questions.length}</p>
		<ul>
		  {#each questions[currentQuestion].options as option, i}
			<li>
			  <button on:click={() => selectAnswer(i)}
				class:selected={selectedAnswer === i}
				class:correct={answered && i === questions[currentQuestion].correct}
				class:wrong={answered && selectedAnswer === i && i !== questions[currentQuestion].correct}>
				{option}
			  </button>
			</li>
		  {/each}
		</ul>
			  <p>Čas na odpověď: {timeLeft} s</p>
	  </div>
	{:else}
	  <div transition:fade>
			  <h1>Výsledek:</h1>
		<h2>Tvoje skóre: {score} z {questions.length} bodů</h2>
		<button on:click={restartQuiz}>Hrát znovu</button>
	  </div>
	{/if}
  </main>
  
  
  
  <style>
	h1, p  {
	  text-align: center;
	}
	main {
	  text-align: center;
	  font-family: Arial, sans-serif;
	}
	ul {
	  list-style: none;
	  padding: 0;
	}
	button {
	  display: block;
	  margin: 10px auto;
	  padding: 10px 20px;
	  font-size: 18px;
	  border: none;
	  cursor: pointer;
	}
	  button:hover {
	  font-weight: bold;
	}
	.selected {
	  background-color: lightgray;
	}
	.correct {
	  background-color: green;
	  color: white;
	}
	.wrong {
	  background-color: red;
	  color: white;
	}
  </style>
  