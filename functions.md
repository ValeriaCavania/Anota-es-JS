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


**THIS:**Significa "quem está executando essa função nesse momento". Exemplo:
```
const pessoa = {
  nome: "Valéria",
  falar: function(){
    console.log(this.nome);
  }
};
pessoa.falar();
```
Quem executou a função foi pessoa, então quem chamou a função foi o objeto pessoa, logo o this será pessoa. Quando o JS chegar em pessoa.falar() ele pensa "quem chamou essa função?" e terá como resposta "pessoa", então this = pessoa, é o mesmo que "console.log(pessoa.nome)".

*função separada do objeto*
```
const carro = {
  marca: "Ford",
  ano: 2018
};
```
```
function descricaoCarro(){
  console.log(this.marca + " " + this.ano);
}
```
Essa função não pertence a carro, ela está sozinha, por isso this não será carro, quando executada a função o retorno é **undefined**.
Agora se fosse assim:
```
const carro = {
  marca: "Ford",
  ano: 2018

  function descricaoCarro(){
    console.log(this.marca + " " + this.ano);
  }
};
```
Se executar carro.descricaoCarro() quem chamou foi carro, então this = carro e a função passa a enxergar this.marca resultando: Ford 2018.
A grande sacada é que no primeiro caso descricaoCarro() quem chamou? Ninguém, então this != carro. No segundo caso carro.descricaoCarro() quem chamou? carro, então this = carro.
No primeiro caso a função usa this.marca, ela não sabe automaticamente que o objeto é carro, porque a função foi criada separada e portanto não se cria essa relação, se tornam coisas independentes. 
*E se guardasse carro.descricaoCarro() em outra variável?*
```
const outrVariavel = carro.descricaoCarro();
console.log(outraVariavel);
```
Quem chamou? foi "outraVariavel" e não mais carro.descricaoCarro(), portanto mudou o this e o resultado é undefined, logo o this depende de COMO a função foi chamada e não ONDE foi criada.
Em vez de pensar que a "Função pertence ao Objeto", o certo é: Na hora de executar, qual objeto está na frente do ponto (.)? 
pessoa.falar() -> this = pessoa; 
carro.acelerar() -> this = carro;
falar(); -> nesse caso o this não será um objeto como pessoa ou carro. Deve-se sempre pensar, quem colocou o ponto (.) antes da função?

*Exercício:*
```
const cachorro = {
  nome: "Rex",
  latir(){
    console.log(this.nome);
  }
}
cachorro.latir(); --> this = cachorro
```
```
const executar = cachorro.latir;
executar(); --> this != cachorro, this = undefined
```
*Por que dá undefined?*
```
function descricaoCarro(){
  console.log(this.marca + " " + this.ano);
}
descricaoCarro();
```
Dependendo do ambiente, o this pode ser o objeto global window, nesse caso, this === window e como window não possui marca e ano, então:
window.marca --> Undefined;
window.ano --> Undefined.

**Regra Geral**
objeto.metodo() --> this é o objeto.
função() (chamada sozinha) --> this é undefined.

