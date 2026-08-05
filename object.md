##Object##
Todo Objeto é criado com o construtor Object e por isso herda as propriedades e métodos do seu prototype.
Em JavaScript praticamente tudo é um objeto.
String, array, function, date, regexp são objetos e todos foram criados com o construtor Object.
Exemplo: Object -> Array -> frutas = ["Banana", "Uva"];
1)Object():
```
const pessoa = new Object({
  nome: "Valéria";
})
```
O trecho de código acima equivale a: 
```
const pessoa = {
  nome: "Valéria";
}
```
Hoje é mais comum escrever de acordo com o segundo exemplo, mas o primeiro mora que todos os objetos são criados pelo construtor Object.

2)Object.create():
```
const carro = {
  acelerar() {},
  buzinar() {},
}
```
```
const honda = Object.create(carro);
```
Baseado nos dois trechos acima, pode-se inicialmente pensar que honda copiou carro, mas na verdade honda não possui os métodos  de carro. Ao fazer:
```
honda.acelerar();
```
O JS procura esse método dentro de honda, mas não existe, então procura dentro do protótipo, que existe e executa, esse é o mecanismo de herança. Em outras palavras, o JS faz uma busca nessa sequência:
honda.acelerar() --> existe acelerar em honda? --> não --> existe no protótipo? --> sim --> executa a função. Portanto, quando uma propriedade não existe no objeto o JS sobe para o protótipo.

3) Init():
```
init(valor){
  this.marca = valor;
  return this;
}
```
Quem é o this? Depende de quem chamou. Por exemplo? honda.init("Honda"), então this === honda, logo:
this.marca = "Honda" vira honda.marca = "Honda", em seguida: return this = return honda.
Por isso funciona:
```
const ferrari = Object.create(carro).init(ferrari);
```
Porque Object.create() -> gera objeto -> init() -> retorna o próprio objeto -> atribui na variável.

4)Object.assign():
```
const FuncaoAutomovel ={
  acelerar(){
    return "Acelerou";
  },
  buzinar(){
    return "Buzinou";
  }
}

const motot = {
  rodas: 2,
  capacete: true
}

Object.assign(moto, funcaoAutomovel);
```
O que o assign fez foi uma copia! É como se "Literalmente" executasse isso:
moto.acelerar = funcaoAutomovel.acelerar;
moto.buzinar = funcaoAutomovel.buzinar;
E depois do assign, o objeto virou:
```
const moto = {
  rodas: 2,
  capacete: true,
  acelerar(){},
  buzinar(){}
}
```
Agora essas funções buzinar() e acelerar() pertencem ao objeto moto.
Comparando com o create x assign:
const honda = Object.create(carro); ---> As funções continuam em carro;
Object.assign(moto, funcaoAutomovel); ---> As funções foram copiada para dentro de moto ou seja, moto e funcaoAutomovel apontam para a mesma função existente.

**Teste**
```
const animal = {
  falar() {
    return "Oi";
  }
}
```
Ao declarar: const cachorro = Object.create(animal); O método falar pertence ao objeto animal com create e cachorro apenas herda.
Ao declarar: Object.assign(cachorro, animal);;  O método falar pertence ao próprio cachorro, porque foi feita uma cópia para ele, isto é, apontem para a mesma função, e se caso alterar o método falar de animal, cachorro continua com a referência antiga.
Fazendo uma analogia, o create é o livro emprestado da biblioteca, quando necessário consulta o livro, mas ele pertence a biblioteca. Já o assign é o livro que tirou cópia da biblioteca, se a biblioteca alterar o livro, a cópia não altera, continua igual.
O assing é útil quando se tem muito objetos e todos precisam da mesma função, ao invés de escrever as funções em todos os objetos, cria-se apenas um objeto com as funções e depois copia para quem precisar.
