---
title: "Atribuição de varíavel em uma condição"
description: "Implementando uma funcionalidade num sistema que ao pressionar determinadas teclas realiza uma validação e que se a tecla UP for…"
date: "2016-03-28T21:55:21.435Z"
categories: []
published: true
canonical_link: https://medium.com/@Laerte/atribui%C3%A7%C3%A3o-de-var%C3%ADavel-em-uma-condi%C3%A7%C3%A3o-c6e787cea269
redirect_from:
  - /atribuição-de-varíavel-em-uma-condição-c6e787cea269
---

Implementando uma funcionalidade num sistema que ao pressionar determinadas teclas realiza uma validação e que se a tecla UP for pressionada realiza outra validação, resolvi usar **atribuição em uma condição** para poupar código, acabei descobrindo um bug que explico como resolvi nesse post.

#### Mapa das teclas:

```
    Número || Tecla
	9  || TAB
	13 || ENTER
	38 || UP KEY
	40 || DOWN KEY
```

#### script.js

```javascript
var isUPKey = false;

function validacao (num) {
	var keyCode = num; 
	if(keyCode == 13 || keyCode == 40 || (isUPKey = keyCode == 38)) {
		// código omitido
	}

	console.log(isUPKey);
}
```

-   Interação #1: validacao(9) _// false_
-   Interação #2: validacao(38) _// true_
-   Interação #3: validacao(40) _// true_

Por que a variável isUPKey está verdadeira? Sendo que o keyCode é 40? Porque deixei a atribuição e a condição no final.

Na segunda iteração isUPKey era verdadeira, já na #3 iteração a segunda condição atendeu ao esperado logo o interpretador não verifica a última a condição. Pois estamos utilizando o operador || (OU) (se encontra um verdadeiro não verifica o restante). Podemos resolver isso de duas formas:

-   Usando o operador && que força todas as condições serem verificadas.
-   Mover a condição pro começo.

Nesse cenário a primeira solução não se adequa, pois não consigo verificar mais de uma tecla por vez.

```javascript
function validacao () {
	if((isUPKey = keyCode == 38) || keyCode == 13 || keyCode == 40) {
		// código omitido
	}

	console.log(isUPKey);
}

validacao(40); // false
```
