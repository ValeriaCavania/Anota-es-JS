**Object**: Todo Objeto é criado com o construtor Object e por isso herda as propriedades e métodos do seu prototype.
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
O JS procura esse método dentro de honda, mas não existe, então procura dentro do protótipo, que existe e executa, esse é o mecanismo de herança.

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
