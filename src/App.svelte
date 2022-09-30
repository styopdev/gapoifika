<script>
	export let movies;
	let question, timeout, answer = '', interval;

	function getAnswer() {
		let randomIndex = Math.floor(Math.random() * movies.length);
		interval && clearInterval(interval);
		question = movies[randomIndex];
		const questionParts = question.split(' ');

		if (questionParts.length <= 1) {
			return getAnswer();
		}

		for (let i = 0; i < questionParts.length; i++) {
			if (isFinite(questionParts[i])) {
				answer = '';
				return getAnswer();
			}
			answer += questionParts[i].substr(0, 1).toUpperCase();
			answer += questionParts[i].substr(1, 1);
		}

		document.getElementById('area').innerHTML = answer;
		document.getElementById('result').innerHTML = '';
		document.getElementById('result').className = '';
		document.getElementById('form').reset();
		answer = '';

		timeout = 10;
		document.getElementById('show-answer').disabled = true;

		interval = setInterval(() => {
			document.getElementById('show-answer').innerHTML = 'Показать ответ (' + timeout + ')';
			timeout--;
			if (!timeout) {
				document.getElementById('show-answer').disabled = false;
				document.getElementById('show-answer').innerHTML = 'Показать ответ';
				clearInterval(interval);
			}
		}, 1000);
	}

	function showAnswer() {
		document.getElementById('area').innerHTML = question;
	}

	function checkAnswer(event) {
		const answ = (event.target[0].value || '').replace('/', '').replace('-', '').replace(':', '');

		if (question.toLowerCase().trim() == answ.toLowerCase().trim()) {
			document.getElementById('result').innerHTML = 'Да, это правильный ответ :)';
			document.getElementById('result').className = 'green';
		} else {
			document.getElementById('result').innerHTML = 'Чуть-чуть не так :(';
			document.getElementById('result').className = 'red';
		}
	}
	window.onload = getAnswer;
</script>

<main>
	<h1>ГаПоИФиКа</h1>
	<div class="content">
		<div id="area" class="area">

		</div>
		<div id="result" class="red green">
	
		</div>
	</div>
	
	<button class="refresh" on:click={getAnswer}>Обновить</button>
	<button id ="show-answer" class="refresh" on:click={showAnswer}>Показать ответ</button>
	
	<form id='form' class="check-gapo-form" on:submit|preventDefault={checkAnswer}>
		<input type="text">
		<input type="submit" value="Проверить">
	</form>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
	
	.area {
		border: 3px solid #ff3e00;
		color: #ff3e00;
		width: 500px;
		height: 100px;
		font-size: 60px;
		line-height: 100px;
		overflow-x: auto;
        overflow-y: hidden;
		white-space: nowrap;
		
	}

	.content {
		width: 500px;
		margin: 0 auto;
	}

	#result {
		height: 20px;
		font-size: 16px;
	}

	.check-gapo-form {
		margin-top: 20px;
	}

	.refresh {
		margin-top: 20px;
	}
	#result.green {
		color: rgb(30, 204, 30);
	}
	#result.red {
		color: rgb(196, 15, 15);
	}
</style>