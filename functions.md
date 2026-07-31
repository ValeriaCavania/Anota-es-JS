# Functions #

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


## CALL( ) ##
Para entender o call, partirei do exemplo do objeto carro:
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
Como visto, se executar descricaoCarro() o this será undefined. A função sabe o que precisa fazer que é imprimir marca e ano, mas ela não sabe de quem imprimir. É como se perguntasse: "Preciso imprimir marca/ano, mas de qual objeto?".
**Solução: call()**
O call diz: "Use ESTE objeto como this". 
Sintaxe: funcao.call(objeto) --> descricaoCarro.call(carro);
Traduzindo a linha de cima: Execute descricaoCarro, mas considere que o this será o objeto carro. Então this = carro.
Importante: Call() não altera a função no sentido dela virar função de carro, ela apenas foi executada uma vez usando determinado objeto.
Exemplo:
```
const pessoa = {
  nome: "Valéria",
  idade: 27
};
```
E uma função separada:
```
function apresentar(){
  console.log(Meu nome é ${this.nome} e tenho ${this.idade} anos);
}
```
Se fizer: apresentar() não funciona, porque this === undefined. 
Agora: apresentar.call(pessoa), o resultado será: Meu nome é Valéria e tenho 27 anos.
Agora outro exemplo para provar que a função é reutilizável com outro objeto:
```
const aluno = {
  nome: "Joao",
  idade: 20,
};
apresentar.call(aluno);
```
O resultado será: Meu nome é Joao e tenho 20 anos.
Vantagem: A mesma função serviu para objetos diferentes.

Seguindo o exemplo da aula:
```
descricaoCarro.call(carro);
```
É possível entender que o JS fez a função "olhar" para o objeto carro. Toda FUNÇÃO possui o método call(), nesse caso, está usando o método call na função descricaoCarro. Isso não significa que "carro chamou a função". E sim que: "A função usou o seu método call() para definir quem será o this". Portanto, a função usa o método call para definir quem será o this apenas durante a execução, ou seja, executar descricaoCarro() utilizando o objeto livro como CONTEXTO de this.

O call() pode receber mais argumentos - Sintaxe completa:
```
funcao.call(this, arg1, arg2, arg3...)
```
Exemplo:
```
const Pessoa = {
  nome: "Valéria"
};
function apresentar(cidade, nome) {
  console.log(`${this.nome} mora em ${this.cidade} e tem ${idade} anos`);
}
```
A função possui dois parâmetros: cidade e idade. Então:
```
apresentar.call(pessoa, "São Paulo", 28);
```
O this será pessoa, definido pelo call(), o JS enxerga:
this.nome = Valéria
cidade = São Paulo
idade = 27
**Regra: No call o primeiro argumento sempre será o this**
**Quando o this = null:**
```
function somar(a+b){
  console.log(a+b);
}
somar.call(null, 10, 20);
```
This = null porque essa função não utiliza o this.

## APPLY( ) ##
Sintaxe:
```
funcao.apply(this, [arg1, arg2, arg3];
```
Exemplo:
```
Math.max.apply(null, [10,5,30,8];
```
No call os argumentos vem separado por (,) e no apply os argumentos vem dentro de um array.
Exemplo:
```
const numeros = [23,369,21,54,63];
Math.max(numeros);
```
O retorno será Nan, porque Math.max espera receber assim:
Math.max(23,369,21,54,63) e não assim Math.max([23,369,21,54,63]) já que ele ve o array todo como o primeiro argumento e não cinco argumentos diferentes.
Então ao fazer: 
```
Math.max.apply(null, numeros);
```
O JS vê: Math.max(23,369,21,54,63);
**O que o JS faz por baixo dos panos quando usa-se apply?**
Faz o mesmo que o operador spreed:
```
const numeros = [23,369,21,54,63];
Math.max(...numeros);
```
Transforma o array numeros em 23, 369, 21, 54, 63. Essa declaração tem o mesmo efeito que Math.max.apply(null, numeros).
Portanto o apply faz o mesmo que o call, mas recebe os argumentos em um array.

## BIND( ) ##
Cria uma nova função que quando for executada vai chamar a função original usando o this.
```
function descricaoCarro(){
  console.log(this.marca);
}
```
descricaoCarro.call(carro); --> This = carro, executa a função e acabou.

```
const novaFuncao = descricaoCarro.bind(carro);
```
Nesse caso, nada é executado, apenas criou-se uma nova função, ou seja, em "novaFuncao" não se guarda um objeto e sim uma nova função.
Portanto, o JS pensa: "Não vou executar agora, criei uma nova função que quando for chamada usará "carro" como contexto de this.
```
novaFuncao();
```
Executa, agora this = carro.

O bind vai resolver esse problema: "Quero guardar determinada função em uma variável, mas sem perder o this":
```
const executar = cachorro.latir;
executar();
```
executar() retorna undefined, porque o this é o objeto global window.
Agora, usando bind():
```
const selecionar = document.querySelectorAll.bind(document);
```
