---
title: 'Arrays como nunca!'
date: '8 de Agosto 2021'
preview: 'Um monte de funções interessantes que facilitam o uso e manipulação de Arrays em JS'
coverImage: 'some_url'
authors: 'Jose Tone'
label: Javascript
---

Arrays são sem medo de errar parte fundamental e uma das estruturas de dados mais usadas na programação, deste os tempos remotos até aos dias de hoje! Sua principal função é o agrupamento de dados semelhantes (ou não) para fácil uso e manipulação, mas nem sempre foi assim antigamente (e dependo da linguagem) a maior parte dos algoritmos de manipulação dos mesmos (que se dizem comuns agora) era implementados manualmente pelo programador em seu projecto. Felizmente, com o passar do tempo, alguns algoritmos que se consideram vitais são passados directamente a linguagem, trazendo assim um ganho de tempo para os programadores/desenvolvedores que precisem lidar com arrays. Hoje vamos ver um monte de funções interessantes que facilitam a nossa vida (JS/TS Devs).

> Disclaimer! Para tornar a leitura mais simples vamos de agora em diante considerar que sempre que no atual artigo se mencionar função do Tipo **PHOC** está a se a querer referenciar à uma função que receba três parâmetros, sendo o primeiro um item/elemento, o segundo um index (do tipo inteiro) e o terceiro um array.

```jsx
function PHOC(element, index, array) {
  // pode retornar algo
}
```

Para os nossos exemplos vamos considerar como fonte de dados o seguinte array:

```jsx
const people = [
  {
    name: 'Jose',
    age: 21,
  },
  {
    name: 'Jossano',
    age: 11,
  },
];
```

Agora estamos prontos! Vamos às funções!!!

### ⚡️ Map

A função map tem o objectivo de transformar os elementos de um array, recebe uma função do tipo **PHOC** como parâmetro que é executada para cada elemento/item do array tendo que retornar um outro item (ou o mesmo, é o Dev quem manda!). Esta função retorna um novo array, mantendo o anterior intacto.

```jsx
const result = people.map(function (element, index, array) {
  return `${element.name} tem ${element.age} anos`;
});

console.log(result); // ['Jose tem 21 anos', 'Jossano tem 11 anos']
```

**Dica**: experimente fazer console.log das constantes element, index e array para um melhor entendimento de como tudo funciona.

### ⚡️ Filter

A função filter (como seu nome diz em português: filtro) tem o objectivo de filtrar os elementos de uma array, recebe também uma função do tipo **PHOC** como parâmetro que é executada para cada elemento do array tendo que retornar um valor boleano\* (que será usado para definir quais elementos serão filtrados). Esta função retorna um novo array, mantendo o anterior intacto.

```jsx
const result = people.filter(function (element, index, array) {
  return element.age >= 18;
});

console.log(result); // [{ name: 'Jose', age: 21 }]
```

O código acima filtra todos as pessoas (dentro do array "people") que tenham idade igual ou superior a 18.

### ⚡️ Reduce

A função reduce é usada quando precisamos fazer uma operação (no array) lembrando dos valores anteriores. Ele faz "alguma coisa" em cada elemento e mantém um registro dos cálculos em uma variável do acumulador e quando não há mais elementos restantes, ele retorna o acumulador (o acumulador pode ser literalmente qualquer coisa, mas há que se ter cuidado com as tipagens).

A própria função Reduce leva duas entradas: (a) a função redutora ou callback; (b) um ponto de partida opcional ou valor inicial.

A função redutora ou o retorno de chamada aceita 4 argumentos: acumulador, elemento atual, índice e o array completo.

Caso o valor inicial for fornecido, o acumulador na primeira iteração será igual ao valor inicial e o elemento atual será igual ao primeiro elemento do array. Caso contrário, o acumulador seria igual ao primeiro elemento do array e o elemento atual seria igual ao segundo elemento do array.

Confuso? Vamos a um exemplo:

Imagine que queiramos de alguma forma saber o somatório de todas as idades no array (não me perguntes porquê 😅), nesse caso o reducer nos facilitaria a vida.

```jsx
const totalIdades = people.reduce((acumulador, elementoAtual) => {
  return acumulador + elementoAtual.age;
}, 0);
console.log(totalIdades); // 32
```

**Dica**: no exemplo acima a função redutora/callback está implementada com forma de arrow function.

### ⚡️ ForEach

A função forEach é parecida a um estrutura de repetição "for", ela recebe uma função do tipo **PHOC** (que não retorna nada) que é executada para cada elemento do array. Vejamos um exemplo:

```jsx
people.forEach((element) => {
  console.log(element.name);
});
// Jose
// Jossano
```

**Dica**: em javascript não somos obrigados usar todos os parâmetros definidos numa função, sabemos que no exemplo a função é do tipo **PHOC**, logo recebe três parâmetros, mas só usamos um.

### ⚡️ Some

A função some (recebe um parâmetro do tipo **PHOC**) é usada para verificar se pelo menos um elemento dentro do array obedece a uma dada condição, retorna true se sim e false noutro caso. Definição meio abstrata né? Um exemplo resolve:

Imagine que queiramos saber se pelo menos um dos elementos do array "people" contêm 11 anos.

```jsx
const alguemTem11Anos = people.some((element) => element.age === 11);
console.log(alguemTem11Anos); // true
```

### ⚡️ Every

Ao contrário da função some, a função every verifica se todos os elementos do array obedecem a uma certa condição, retorna true se sim e false noutro caso. Agora acho que já foi bem mais fácil de entender, mas um exemplo nunca é de mais 😉:

Imagine que queiramos saber se todos os elementos do array "people" têm menos de 18 anos.

```jsx
const todosSaoMenores = people.every((element) => element.age < 18);
console.log(todosSaoMenores); // false
```

### ⚡️ Find

A função find recebe um callback do tipo **PHOC** e tem o objectivo de filtrar um array retornando o primeiro elemento encontrado que cumpra com a condição imposta. Analisemos o exemplo abaixo.

```jsx
const person = people.find((element) => element.name === 'Jossano');
console.log(person); // { name: 'Jossano', age: 11 }
```

### ⚡️ Sort

Esta função é usada para ordenar um array, porém há se ter algum cuidado a usá-la uma vez que não é imutável (não cria uma nova instância do array, modifica o atual). Ela recebe como parâmetro uma função que define o critério de ordenação. A ordenação padrão é ascendente.

A função que passamos como parâmetro recebe por sua vez dois parâmetros: o elemento actual e o próximo elemento e deve retornar um valor boleano\*. Eu sei, eu sei, bué confuso, mas o exemplo está aqui para nos acudir: vamos lá ordenar o nosso array de forma crescente (baseado na idade).

```jsx
const arrayAOrdenar = people; // não queremos estragar nosso array primário

arrayAOrdenar.sort((atual, proximo) => atual.age < proximo.age);
console.log(arrayAOrdenar);

/*
[
	{
		name: 'Jossano',
		age: 11,
	},
	{
		name: 'Jose',
		age: 21,
	},
]
*/
```

Dica: no exemplo acima, o critério de ordenação for a idade (age).

### Agora és um ninja! 🥷🏽

Embora existem ainda algumas outras funções do género, com tudo o que temos até aqui já nos podemos considerar ninjas quando o assunto é arrays em Javascript/Typescript, agora podemos sem medo abraçar os arrays e usufruir de todas as vantagens e funções engenhosas que ele acarreta. Seja feliz!

Obrigado por ler este artigo ❤️ espero que tenha sido útil para ti.
