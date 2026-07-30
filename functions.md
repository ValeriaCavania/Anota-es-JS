#Functions#
**Introdução**
```
function Quadrado(lado){
  return lado * lado;
}
const calcular = Quadrado;
console.log(calcular(4));
```
Quadrado() é uma função, mas o nome da função é só um "apelido", porque calcular e Quadrado apontam para a mesma função. Função é um valor, assim como:
```
const nome = "Valéria";
const calcular = Quadrado;
```
O calcular guarda uma função e uma função é um tipo de dado.

***Formas de escrever:***
```
function areaQuadrado(lado){
  retunr lado * lado;
}
```
e
```
const areaQuadrado = new Function( "lado", "return lado * lado");
```
As duas funções fazem a mesma coisa, mas aprender isso significa que uma função é um objeto, assim como existe newDate() que cria um objeto de Date, existe newFunction() que cria um objeto function.
Se é um objeto, todo objeto possui propriedades. Exemplo:
```
cont pessoa = {
  nome: "Valéria",
  idade: 27,
}
```
Tem pessoa.nome e pessoa.idade. Então uma função também tem propriedades. Exemplo:
```
function somar(a,b){
  return a + b;
}
```
Essa function possui somar.nome (retorna uma string, portanto pode-se utilizar toUpperCase()) e somar.length;
E como função é um objeto, ela também possui métodos, como: somar.call(), somar.apply() e somar.bind(). Esses métodos pertencem a função. Isso é a mesma ideia de:
*string possui métodos:*
```
const nome = "Valéria;
nome.toUpperCase();
```
*array possui métodos:*
```
const numeros = [1,2,3,4];
numeros.push(4);
```
Portanto, uma função é um valor e uma função é um objeto por isso ela pode possuir propriedades e métodos.
