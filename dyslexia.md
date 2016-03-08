
<h3>Симулятор дизлексии с помощью JavaScript'a</h3>
<p>Моя подруга, которая имеет дизлексию объяснила мне, как она читает. Она может читать, но это требует очень большой концентрации внимания и буквы как будто прыгают с места на место.</p>
<p>Тут я вспонил, что читал о <a href="https://en.wikipedia.org/wiki/Typoglycemia">typoglycemia</a>. Возможно ли сделать это прямо на сайте с помощью Javascript'а? Да, конечно.</p>
<p>Хотите сохранить ссылку на эту страницу или сами что-то поправить? <a href="https://github.com/geon/geon.github.com/blob/master/_posts/2016-03-03-dsxyliea.md">Fork it</a> на github (оригинал на английском), <a href="https://github.com/crea7or/dyslexia-js">а тут эта версия</a> на русском.</p>


<p>*Дислекси́я (др.-греч. δυσ- — приставка, означающая нарушение + λέξις — речь) — избирательное нарушение способности к овладению навыком чтения и письма при сохранении общей способности к обучению. Исторически так сложилось, что в западных странах в понятие Дислексия включают все проблемы, связанные с письменной речью:</p>
<ul>
<li>проблемы с овладением навыком чтения;</li>
<li>проблемы с овладением навыком письма;</li>
<li>проблемы с грамотностью;</li>
<li>проблемы с овладением математикой;</li>
<li>проблемы, связанные с нарушением моторики и координации;</li>
<li>проблемы с поддержанием внимания.</li>
</ul>
<p>Отечественная логопедия рассматривает все эти проблемы по отдельности, не связывая их между собой, как: дислексия, дисграфия, дисорфография, дискалькулия, диспраксия, СДВ(Г).</p>
<p>Существует ещё один вид дислексии — Dyslexia litteris (латынь — дислексия букв). Проявляется при наборе текста. Выявляется как смена местами рядом стоящих букв. Систематически проявление отследить невозможно, появляется только в словах, состоящих из более чем четырёх букв.</p>
</p>
<p>*Источник: <a href="https://ru.wikipedia.org/wiki/%D0%94%D0%B8%D1%81%D0%BB%D0%B5%D0%BA%D1%81%D0%B8%D1%8F">Wikipedia</a>.</p>

<p>Оригинал страницы на английском и автор оригинального JavaScript'а - <a href="http://geon.github.io/programming/2016/03/03/dsxyliea">geon.github.com</a></p>

<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/2.0.3/jquery.min.js"></script>
<script type="text/javascript">

"use strict";

$(function(){

	var getTextNodesIn = function(el) {
	    return $(el).find(":not(iframe,script)").addBack().contents().filter(function() {
	        return this.nodeType == 3;
	    });
	};

	// var textNodes = getTextNodesIn($("p, h1, h2, h3"));
	var textNodes = getTextNodesIn($("*"));



	function isLetter(char) {
		return /^[\d]$/.test(char);
	}


	var wordsInTextNodes = [];
	for (var i = 0; i < textNodes.length; i++) {
		var node = textNodes[i];

		var words = []

		//var re = /\w+/g;
		//fix to match russian language letters
		var re = /[a-zA-Zа-яА-Я]+/g;
		var match;
		while ((match = re.exec(node.nodeValue)) != null) {

			var word = match[0];
			var position = match.index;

			words.push({
				length: word.length,
				position: position
			});
		}

		wordsInTextNodes[i] = words;
	};


	function messUpWords () {

		for (var i = 0; i < textNodes.length; i++) {

			var node = textNodes[i];

			for (var j = 0; j < wordsInTextNodes[i].length; j++) {

				// Only change a tenth of the words each round.
				if (Math.random() > 1/10) {

					continue;
				}

				var wordMeta = wordsInTextNodes[i][j];

				var word = node.nodeValue.slice(wordMeta.position, wordMeta.position + wordMeta.length);
				var before = node.nodeValue.slice(0, wordMeta.position);
				var after  = node.nodeValue.slice(wordMeta.position + wordMeta.length);

				node.nodeValue = before + messUpWord(word) + after;
			};
		};
	}

	function messUpWord (word) {

		if (word.length < 3) {

			return word;
		}

		return word[0] + messUpMessyPart(word.slice(1, -1)) + word[word.length - 1];
	}

	function messUpMessyPart (messyPart) {

		if (messyPart.length < 2) {

			return messyPart;
		}

		var a, b;
		while (!(a < b)) {

			a = getRandomInt(0, messyPart.length - 1);
			b = getRandomInt(0, messyPart.length - 1);
		}

		return messyPart.slice(0, a) + messyPart[b] + messyPart.slice(a+1, b) + messyPart[a] + messyPart.slice(b+1);
	}

	// From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random
	function getRandomInt(min, max) {
		
		return Math.floor(Math.random() * (max - min + 1) + min);
	}


	setInterval(messUpWords, 50);
});


</script>

