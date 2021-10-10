---
title: 'Entendendo o conceito de Curry em JS'
date: 'August 8 2021'
preview: 'Uma forma mais sofisticada de lidar com funções com múltiplos parâmetros'
coverImage: 'some_url'
authors: 'Jose Tone'
---

Se você é um desenvolvedor Javascript (aposto que é 😅) e já estudou os principais conceitos da linguagem, provavelmente sabe algo sobre Closure (se não, definitivamente deve estudar). Closure é um dos conceitos/características mais importantes do Javascript, nos dá (desenvolvedores) poder ilimitado, um de seus presentes é a possibilidade de termos **Programação Funcional** em nossa linguagem. Programação funcional é um grande tópico, mas hoje focaremos apenas em Currying.

---

### Então, o que é Curry?

Imagina uma função que receba dois parâmetros (x e y) e retorna x + y:

```jsx
const soma = (x, y) => x + y;
```

se chamarmos a função com apenas um parâmetro (isto é permitido a não ser que se use Typescript) como:

```jsx
console.log(soma(4));
```

Recebemos na console **NaN (Not a Number)**. Isto porque o segundo parâmetro não está sendo passado e para estes casos (falta de parâmetros) o Javascript passa undefined como padrão, neste caso teríamos executado com o código acima **4 + undefined**, o que definitivamente **não resulta em um número (NaN).** Mas e se houvesse uma maneira de evitar o NaN? Se a função soma nunca realizasse a operação até ter todos os parâmetros de que precisa bem definidos? E se fizéssemos com que a função soma retornasse outra(s) funções que preservassem o seu contexto e pedissem o(os) parâmetros restantes sempre que algum parâmetro estiver em falta? Imagina o poder que teríamos (talvez não seja muito poder para uma função que só soma dois números 😅), imagina se aplicássemos o mesmo conceito para todas as funções que quisermos? Isto é Curry! E não te preocupes se ainda não pegaste a ideia, tens logo abaixo:

> Currying é um processo em programação funcional onde se pode transformar uma função com múltiplos parâmetros em uma sequência de funções encadeadas que preservam o seu contexto e requerem o(os) próximo(s) parâmetro(s).

### Hora de implementar a nossa função Curry 🤩

Agora que já sabemos o conceito de Curry é hora de saber como implementar a nossa versão:

```jsx
const curry = function (fn) {
  const arity = fn.length;
  return function f1(...args) {
    if (args.length >= arity) return fn(...args);
    else
      return function curried(...otherArgs) {
        return f1(...args.concat(otherArgs));
      };
  };
};
```

Como pode ver, não é assim tão complexo (se entendes o conceito de Closure). Com isto podemos agora tornar a nossa função passada (soma) em carried. Teremos algo mais ou menos como:

```jsx
const soma = (x, y) => x + y;
const currySoma = curry(soma);
```

Para os que preferem a forma mais curta:

```jsx
const currySoma = curry((x, y) => x + y);
```

Agora podemos chamar a função como quisermos! (Leve em conta que fazer currying numa função não previne tipos de parâmetros, apenas a forma como são passados).

```jsx
// 1
console.log(currySoma(1, 2));
// 2
console.log(currySoma(1)(2));
// 3
console.log(currySoma(1));
```

Vamos entender as três formas (de chamar a função) no código acima:

1. retorna 3 na console porque todos os parâmetros esperados são passados, desse jeito a função currySoma é executada normalmente.
2. A primeira a função é chamada apenas com um parâmetro fazendo com que seja retornada outra função que espera o segundo parâmetro e este é passado logo em seguida, por isso neste caso é também retornado 3 na console.
3. Apenas um parâmetro é passado (para uma função curried que espera dois), o que faz com que seja retornada uma outra função (que possui o contexto da função original) e que espera o segundo parâmetro (para assim realizar a soma), deste modo é retornado na console a definição de uma função anónima que obedece as condições acima citas.

### Um exemplo da vida real

Imagina que tenhamos um vetor de livros como:

```jsx
const livros = [
  {
    titulo: 'The fundamentals of modern JS',
    paginas: 304,
  },
  {
    titulo: 'Learn to cook',
    paginas: 304,
  },
  {
    titulo: 'Sapiens',
    paginas: 443,
  },
];
```

Se quisermos filtrar todos os livros com 304 páginas faríamos algo mais ou menos assim:

```jsx
const possuiPaginas = (numPaginas, obj) => obj.paginas === numPaginas;
const livrosFiltrados = livros.filter((livro) => possuiPaginas(304, livro));
```

Não há nada de errado com a implementação acima mas eu acho que esta é uma das situações onde podíamos tirar vantagem de currying de formas a termos um código mais limpo e _"mais sofisticado"_. Vejamos como curry pode nos ajudar nisto:

```jsx
const possuiPaginas = curry((numPaginas, obj) => obj.paginas === pages);
```

Agora podes refactorar o nosso filtro para parecer mais bonito e fácil de ler:

```jsx
const livrosFiltrados = books.filter(possuiPaginas(304));
```

### Porquê e como funciona?

Como a função possuiPaginas agora é um Curry, quando a chamamos passando apenas um parâmetro, ela retorna outra função que lembra do contexto (com o parâmetro passado anteriormente) e espera que o parâmetro ausente seja passado e a função de filter lida com isso por nós. É lindo 😍.

Se ainda for confuso para você, não se preocupe, significa que você está aprendendo, recomendo tentar executar este exemplo em seu computador, modificar e talvez criar seus próprios exemplos.

**Espero poder ajudar e até o próximo post. Obrigado! ❤️**
