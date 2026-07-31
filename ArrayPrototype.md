## Prototype ##
Todo array em JavaScript tem acesso direto aos métodos e propriedades definidos em Array.prototype por meio do mecanismo de herança por protótipo.
```
const carros = ["Ford", "Fiat"];
const frutas = ["Banana", "Uva"];
```
É comum pensar que carro e frutas terão acesso individual as propriedades e métodos de array, como:
carros
 ├── forEach()
 ├── filter()
 ├── map()
 ├── reduce()
 ├── call()
 ├── apply()
 ├── bind()

frutas
 ├── forEach()
 ├── filter()
 ├── map()
 ├── reduce()
 ├── call()
 ├── apply()
 ├── bind()

Quando na verdade, por exemplo, existe apenas uma função forEach() e ela é compartilhada com todos os arrays, ou seja, frutas e carros apontam para a mesma função:
                Array.prototype
          ┌─────────────────────────┐
          │ forEach()               │
          │ filter()                │
          │ map()                   │
          │ reduce()                │
          │ push()                  │
          │ pop()                   │
          └─────────────────────────┘
                 ▲            ▲
                 │            │
              carros       frutas
              
São o mesmo método compartilhado pelo Array.prototype. 
É por isso que a expressão é verdadeira, porque apontam para o mesmo lugar da memória:
```
carros.forEach === frutas.forEach
```
