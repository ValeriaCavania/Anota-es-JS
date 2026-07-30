#Functions#
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

