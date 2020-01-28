---
title: "OnClientClick “disparando” antes do ValidationGroup"
description: "Criando uma página de cadastro que usa validações via Javascript observei o seguinte problema: As validações padrões do ASP (ex…"
date: "2015-03-10T21:10:29.167Z"
categories: []
published: true
canonical_link: https://medium.com/@Laerte/onclientclick-disparando-antes-do-validationgroup-aa0f6851f755
redirect_from:
  - /onclientclick-disparando-antes-do-validationgroup-aa0f6851f755
---

Criando uma página de cadastro que usa validações via Javascript observei o seguinte problema: As validações padrões do ASP (ex: RequiredFieldValidator) não estavam funcionando, a ação do “OnClientClick” executava sem verificar se os validadores do ASP, pesquisando um pouco no **StackOverflow** encontrei a solução:

```csharp
if(Page_ClientValidate()) {
	return Funcao('param')
}
```

Coloquei esse código no OnClientClick, que faz o seguinte: Verifica se os validadores da página retornam true (estão corretamente preenchidos) e dentro dele chama minha função que valida os campos via Javascript, caso o retorno de ambos for verdadeiro o ASP executa a ação “OnClick” do Codebehind.

**Referência:**  
[OnClientClick fired before ValidationGroup](http://stackoverflow.com/questions/9718698/onclientclick-fired-before-validationgroup/9718953#9718953)
