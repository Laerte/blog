---
title: "Criando uma validação personalizada —  ASP.NET MVC"
description: "Crie uma classe que herde da classe ValidationAttribute:"
date: "2014-11-01T12:59:09.463Z"
categories: []
published: true
canonical_link: https://medium.com/@Laerte/criando-uma-validacao-personalizada-434a2dd599be
redirect_from:
  - /criando-uma-validacao-personalizada-434a2dd599be
---

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
```![Classe personalizada de validação](./asset-1.png)
