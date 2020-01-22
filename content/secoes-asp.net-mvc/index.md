---
title: "Seções — ASP.NET MVC"
description: "Quando criamos uma Master Page o principal objetivo é não duplicar código, mas nem todos os elementos de uma Master Page vão ser usadas nas…"
date: "2014-11-01T13:02:26.702Z"
categories: []
published: true
canonical_link: https://medium.com/@Laerte/secoes-asp-net-mvc-f1035c332929
redirect_from:
  - /secoes-asp-net-mvc-f1035c332929
---

Quando criamos uma Master Page o principal objetivo é não duplicar código, mas nem todos os elementos de uma Master Page vão ser usadas nas páginas que a herdam.

Exemplo: A Master Page contém um Side Bar mas na página de Contato não é necessário popular essa _div._ Quando isso ocorre na página \_Layout dividimos as divs em Seções:

```
<div id=”sidebar”>@RenderSection(“Nome da Seção”, true)</div>
```

O segundo parâmetro é opcional e o padrão é verdadeiro, ele diz se é obrigatório a view que herda implementar aquela seção.

O código pra implementar a seção na view é:

```
@section NomeDaSecao{

// contéudo

}
```

Quando divimos o conteúdo em seções raramente precisamos usar o o **@RenderBody**, mas é possível usa-lo da mesma forma. Ele irá renderizar qualquer contéudo que não esteja dentro das seções no lugar onde foi inserido na \_Layout.

Para verificar se a seção foi definida, na \_Layout fazemos a seguinte condição

```
@if(IsSectionDefined(“Nome”){

<div id=”sidebar”>@RenderSection(“Nome da Seção”, false)</div>

}
```

Assim a div só será renderizada se a seção for definida.

Para não precisar ficar escrevendo em toda view o link para a MasterPage (\_Layout), podemos colocar na \_ViewStart, todos os códigos que todas as views vão ter em comum.
