## Object ##
Todo Objeto é criado com o construtor Object e por isso herda as propriedades e métodos do seu prototype.
Em JavaScript praticamente tudo é um objeto.
String, array, function, date, regexp são objetos e todos foram criados com o construtor Object.
Exemplo: Object -> Array -> frutas = ["Banana", "Uva"];
## Object(): ##
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
Hoje é mais comum escrever de acordo com o segundo exemplo, mas o primeiro mostra que todos os objetos são criados pelo construtor Object.

## 1)Object.create() ##
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
O JS procura essa propriedade acelerar em honda, mas não existe, então procura dentro do protótipo, que existe e executa, esse é o mecanismo de herança. Em outras palavras, o JS faz uma busca nessa sequência:
honda.acelerar() --> existe acelerar em honda? --> não --> existe no protótipo? --> sim --> executa a função. Portanto, quando uma propriedade não existe no objeto o JS sobe para o protótipo.

## 2) Init() ##
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

## 3)Object.assign() ##
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
Ao declarar: 
```
const cachorro = Object.create(animal);
```
O método falar pertence ao objeto animal com create e cachorro apenas "herda", se alterar o método falar() de animal e como cachorro consulta o método pelo protótipo, cachorro passa a usar a nova versão do método.
Ao declarar:
```
Object.assign(cachorro, animal);
```
O método falar pertence ao próprio cachorro, porque foi feita uma cópia para ele, isto é, apontem para a mesma função, e se caso alterar o método falar de animal, cachorro continua com a referência antiga.
Fazendo uma analogia, o create é o livro emprestado da biblioteca, quando necessário consulta o livro, mas ele pertence a biblioteca. Já o assign é o livro que tirou cópia da biblioteca, se a biblioteca alterar o livro, a cópia não altera, continua igual.
O assing é útil quando se tem muito objetos e todos precisam da mesma função, ao invés de escrever as funções em todos os objetos, cria-se apenas um objeto com as funções e depois copia para quem precisar.

**Outra Conexão**:
A partir de Array.prototype.forEach o carros.forEach (carro sendo um array) vem do Array.prototype, isso é exatamento o comportamento do Object.create(), quando se declara: const honda = Object.create(carro) o objeto carro passa a exercer um papel semelhante ao _Array.prototype_ que fornece métodos para os objetos que tem como protótipo. Já o object.assign() não cria essa relação de herança, ele apenas copia propriedade de um objeto para outro.

**Outra Conexão**:
Supondo o código:
```
const carro = {
  acelerar() {
    return "acelerou";
  }
};
const honda = Object.create(carro);
console.log(honda.hasOwnProperty("acelerar"));
```
O resultado do console, será false, porque honda não possui esse método acelerar. Existe apenas dentro do protótipo, ou seja: O JavaScript procura a propriedade acelerar em honda, mas não existe, então procura dentro do protótipo, que existe e executa, esse é o mecanismo de herança.

```
const honda = {};
Object.assign(honda, carro);
console.log(honda.hasOwnProperty("acelerar"));
```
O resultado do console nesse caso será true, porque honda agora possui a propriedade acelerar por conta do assign que copia as propriedades de carro para dentro de honda.

## 4) Object.defineProperty() / Object.definePorperties() ##
Quando se cria um objeto como no exemplo:
```
const carro = {
  rodas: 4
};
```
Internamente, no JS o objeto carro possui outras informações, além da propriedade rodas, como por exemplo:
+ rodas 
+ ├── value: 4 - Valor da propriedade. 
+ ├── writable: true - Permite alterar o valor da propriedade (carros.rodas = 6), se fosse false, então não seria possível alterar. 
+ ├── configurable: true - Permite deletar (delete.carro.rodas), mas se fosse false, não seria possível deletar, além disso impede que redefina os descritores posterioremnte. 
+ └── enumerable: true - Se carro fosse: 

```
const carro = {
  marca: "Honda",
  ano:2024
};
Object.keys(carro);
```
O resultado da última linha seria: ["marca","ano"] porque enumerable é true para marca e ano, mas se fosse false, então a última linha retornaria apenas ["ano"]. A propriedade marca continuaria existindo, ela só fica "escondida" para métodos que percorrem propriedades enumeráveis.
Ao declarar:
```
Object.defineProperties(motos, {
  rodas: {
    value: 2,
    configurable: false,
    writable: true,
    enumerable: true
  }
});
```
Isso significa: Recria a propriedade "rodas", mas agora controlando suas configurações internas. No exemplo, configurable false não permite apagar, writable true pode alterar o valor e enumerable true aparece no object.keys.
A frase "Todas as propriedades do objeto são mutávies" acontece para objetos criados normalmente (writable = true) e funciona porque o Object.defineProperty existe justamente para mudar esse comportamento padrão.
**Teste**:
```
Object.defineProperty(carro, "rodas",{
  value: 4,
  writable: false
});
carro.rodas = 8;
```
A última linha não vai aplicar, porque a configuração do Object.defineProperty define o writable como false. O valor continuará sendo 4. 
O enumerable e configurable não mantem o valor padrão true, nesse caso todas as configurações que não forem informadas passam a ser false por padrão quando foi craida com defineProperty().


## 5) Get e Set ##
Exemplo:
```
const pessoa = {
  nome: "Valéria",
  get saudacao(){
    return `Ola, ${this.nome}`;
  }
};
console.log(pessoa.saudacao);
```
Percebe-se que não foi chamada a função (não foi escrito assim: pessoa.saudacao() e mesmo assim a função foi executada. --> isso é o get.
Exemplo:
```
const pessoa = {
  _idade: 27,
  set idade(valor){
    this._idade = valor;
  }
};
pessoa.idade = 30;
```
Nesse caso o JS não criou simplesmente idade = 30, na verdade fez: set idade(30). Por de baixo dos panos foi executado uma função get e set nesses dois casos.
Exemplo:
```
Object.defineProperties(carros, {
  rodas: {
    enumerable: true,

    get() {
      return this._rodas;
    },

    set(valor) {
      this._rodas = valor * 4 + " Total Rodas";
    },
  },
});
carros.rodas = 2;
```
Ao declarar a última linha, o JS transforma essa atribuição em uma chamada setter: set(2) e executa:
```
this._rodas = 2 * 4 + " Total Rodas";
this._rodas = "8 Total Rodas;
```
Percebe-se então que o valor armazenado não é 2, mas 8 Total Rodas. 
Depois o Get:
```
console.log(carros.rodas);
```
O JS executa o get() que retorna this._rodas, ou seja, "8 Total Rodas".
**Por que getter usa _rodas?**
Se o código fosse assim:
```
get(){
  return this.rodas;
}
```
Seguindo o raciocínio: Ler rodas --> executa o get --> get tenta ler rodas --> executa o get de novo --> o get tenta ler rodas --> ∞. Isso entraria em uma recursão infinita, por isso usa-se _rodas para armazenar o valor "real".


## 6) Object.getOwnPropertyDescriptors() ##
Esse método serve para verificar as configurações (value, writable, configurable e enumerable) de um objeto.
```
const carro = {
  rodas: 4
};
```
O resultado:
```
{
  rodas: {
    value: 4,
    writable: true,
    enumerable: true,
    configurable: tue
  }
}
```
Ou seja, defineProperties() configura as propriedades e getOwnPropertyDescriptors() consulta como elas estão configuradas.

## 7) Object.keys(), values() e entries() ##
```
const camaro = {
  marca: "Camaro",
  ano: 2018
};
```
**Object.keys()** Quer as chaves: ["marca", "ano"];
**Object.values()** Quer os valores: ["Camaro", 2018];
**Object.entries()** Quer os dois juntos:
```
[
  ["marca","camaro"], ["ano", 2018]
]
```
É importante relembrar que o Object.keys() as propriedades enumeráveis desse objeto. Por isso métodos que estão no protótipo não aparecem. Por exemplo:
```
Object.keys(Array);
```
retorna [], porque mesmo que Array tenha propriedades como: length, name, prototype, isArray, from, of... essas propriedades não são numeráveis. 

## 8)Object.getOwnPropertyNames() ##
Retorna todas as propriedades sejam enumeráveis ou não. Diferente do Object.keys() que só retorna as enumeráveis.


## 9)Object.getPrototypeOf() ##
Responde: Qual é o protótipo desse objeto?
Exemplo:
```
const frutas = ["Banana","Pera"];
Object.getPrototypeOf(frutas);
```
Retorna: Array.prototype, pois esse é o objeto imediatamente acima de frutas na cadeia de protótipos.
Frutas --> Array.prototype --> Object.prototype --> null

## 10)Object.is() ##
Objetos são comparados pela referência e não pelo conteúdo.
```
const frutas1 = ["Banana", "Pêra"];
const frutas2 = ["Banana", "Pêra"];
```
Apesar de terem exatamente o mesmo conteúdo:
```
Objecy.is(frutas1, frutas2);
```
Retorna false, porque são dois objetos diferentes.
Agora:
```
const novaFrutas = frutas1;
```
O array frutas1 e novaFruta apontam para o mesmo array. Então: 
```
Object.is(frutas1, novaFruta);
```
Retorna true.

## 11) freeze, seal e preventExtensions ##
São 3 formas de proteção/modificam o estado.
```
const filme = {
  obra: "Crepusculo",
  ano: 2008
};
```
**Object.preventExtensions()** significa que não se pode mais adicionar propriedades novas, mas ainda pode alterar e deletar as existentes.
**Object.seal()** significa que não pode adicionar nem deletar propriedades, mas pode alterar as existentes.
**Object.freeze()** significa que não pode adicionar, deletar e nem alterar propriedades, sendo o mais restritivo dos três.
Agora os métodos que consultam o estado.
```
Object.isFrozen(objeto);
Object.isSealed(objeto);
Object.isExtensible(objeto);
```


_________________________________________________________________________________
