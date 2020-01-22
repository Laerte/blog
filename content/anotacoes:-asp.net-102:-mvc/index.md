---
title: "Anotações: ASP.NET 102: MVC"
description: "Hoje resolvi assistir esse curso de MVC da Tuts+, nesse post compartilhei o que eu achei de mais relevante. A forma que o conteúdo …"
date: "2014-10-30T00:23:40.611Z"
categories: []
published: true
canonical_link: https://medium.com/@Laerte/anotacoes-asp-net-102-mvc-951b8e14393a
redirect_from:
  - /anotacoes-asp-net-102-mvc-951b8e14393a
---

Hoje resolvi assistir esse [curso de MVC da Tuts+](http://code.tutsplus.com/courses/aspnet-102-mvc), nesse post compartilhei o que eu achei de mais relevante. A forma que o conteúdo é abordado é bem simplista logo qualquer pessoa consegue entender facilmente.

1.  Rotas não são Case sensitive. (Se você tem duas Actions com mesmo nome ex: abc & aBc irá resultar num erro de ambiguidade.)
2.  Rotas personalizadas devem ficar acima das rotas normais, vulgo comuns.
3.  Para renderizar um texto que contém HTML use o **@Html.Raw()**

Para criar um bloco de código na View basta usar, nele você pode ter HTML, C# tudo junto e misturado.

```
@{

 // Código

}
```

Para renderizar um @Laerte (Igual o Twitter, você deve escrever duas vezes o arroba: @@Laerte, resultado: @Laerte, pois se não o Razor acha que é um comando).

Comentários no Razor: **@\*\*@** atalho VS: Ctrl + K + C, ao contrário de um comentário HTML, o comentário do razor remove totalmente o conteúdo da página quando renderizado.

A “**Master Page**” do Razor é a \_Layout que fica dentro da pasta **Shared**.

Para incorporar ela numa View utilize:

```
@{

  Layout = “~/Shared/_Layout”; 

}
```

Para criar uma tag _<a>_ de link use o helper

```
@Html.ActionLink(“Nome”,”Action”, “Controller”)
```

#### Seções

Quando criamos uma Master Page o principal objetivo é não duplicar código, mas nem todos os elementos de uma Master Page vão ser usadas nas páginas que a herdam.

Exemplo: A Master Page contém um Side Bar mas na página de Contato não é necessário popular essa _div_. Quando isso ocorre na página \_Layout dividimos as divs em Seções:

```
<div id="sidebar">@RenderSection(“Nome da Seção”, true)</div>
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

Quando estamos implementando um código para o editar algum objeto e verificamos que o Id não existe podemos retornar uma página de 404.

```
public ActionResult Editar(int? id)
{
  if(!valor.HasValue)
   return new HttpNotFoundResult(); // Retorna 404 se o Id for NULL
}
```

Falei do ActionLink um pouco mais acima, mas também é possível criar links a partir de rotas.

```
@Html.RouteLink(“Nome Link”, “Rota”, new { action = “Editar”, id = 1 } 

@Url.RouteUrl(“Rota”, new { action = “Editar”, id = 2 }) // Usado no retorn dos Controllers.
```

Também é possível saber se a requisição foi feita por **Ajax**, usando a função:

```
if(Request.IsAjaxRequest())
{
  return PartialView(); //Retorna uma PartialView que é basicamente a View se o _ViewStart apenas o conteúdo da view.
}
```

Quando a **Action** tem sobrecarga é necessário especificar se é GET ou POST.

```
[HttpGet]
public ActionResult Editar(int id)
{

}

[HttpPost]
public ActionResult Editar(Pessoa model)
{

}
```

Nesse exemplo no GET ele carrega o objeto, e no POST ele salva.

Quando a View é tipada temos que usar o Helper:

```
@Html.HiddenFor(m => m.Id) 
```

Para guardar informações que não são visíveis para o usuário mas são essenciais para sabermos qual objeto estamos manipulando.

Para redirecionar uma View podemos utilizar: **RedirectResult** e **RedirectToAction**.

Algumas validações que podem ser usadas nas Models:

```
[Display(Name = “Campo)] // Como o nome da propriedade será exibida nas validações e nas views (Labels, etc).

Required(ErrorMessage = “{0} é obrigatório”)] // Torna a propriedade obrigatória, o {0} é o nome da propriedade se for setado o Display ele pega o valor do Display.

//[StringLength(20, ErrorMessage = “{0} não pode ter mais que {1} caracteres.”)]
//[MaxLength(20, ErrorMessage = “{0} não pode ter mais que {1} caracteres.”)] // Os dois métodos realizam a mesma tarefa, a diferença é que o StringLength é espefico para String.

[MinLength(10, ErrorMessage = “{0} deve ter mais que {1} caracteres.”)] // Quantidade mínima de caracteres que a propriedade deve ter

public string Campo {get; set;}
```

Nas Views:

```
@Html.ValidationMessageFor(m => m.campo) // Exibe do lado do campo
@Html.ValidationSummary() // Mostra os erros numa lista
```

Para verificar se uma View é válida no Controller usamos **ModelState.IsValid**, como ele retorna se a Model é válida devemos apenas negar ela:

```
if(!ModelState.IsValid){
   // Código
}
```

#### Criando uma validação personalizada

1.  Crie uma classe que herde da classe **ValidationAttribute:**

```
public class Teste : ValidationAttribute
{
     public string Valor { get; set; }
     public Teste(string valor)
     {
       this.Valor = valor;
     }
}
```

3\. Sobrescreva o método **IsValid()**;

```
public override bool IsValid(object value)
{
  // Validação
}
```

Depois é só usar na Model:

```
[Teste(“Parametro”), ErrorMessage = “{0} não é válido.”]
public string Campo { get; set; }
```

Quando terminar de assistir as vídeo-aulas termino esse post.
