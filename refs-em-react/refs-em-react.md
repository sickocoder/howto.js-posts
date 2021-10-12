---
title: 'Referências em React'
date: '12 de Agosto, 2021'
preview: 'Uma ponte entre a virtual DOM e a native DOM'
coverImage: 'some_url'
authors: 'Jose Tone'
---

Num fluxo de dados normal, a única forma de componentes filhos interagirem com seus componentes filhos é fazendo recurso das props. Para modificar um aspecto/comportamento em um componente filho (a partir do pai) é necessário re-renderizar o mesmo com novas props. No entando, embora o React tenha evoluído muito ao longo do tempo e seu processo de renderização seja bem mais performático agora, há alguns casos em que é imperial modificar(as vezes acessar o estado) de um componente sem ter que re-renderizá-lo\*. O componente a ser modificado pode ser um React Component ou um DOM Element. Para estes casos o uso de referências ajuda (e muito)!

### O que é referência?

Referência é um recurso do React que possibilita acessar (referenciar) um elemento do DOM ou um elemento React. É normalmente usada para em casos onde queremos alterar o valor de um componente filho sem ter que re-renderizar (alterando as props) o componente pai.

### Okay, já entendi o conceito! Mas como é que crio uma?

Atualmente existem duas formas de criar referências e elas estão directamente as duas formas de criar componentes em React (classes e funções), então, vamos para cada uma delas:

**Componente em classe**

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

**Componente em funções (usando o hook)**

```jsx
const MyComponent: FC = () => {
  const myRef = useRef();

  return <div ref={myRef} />;
};
```

Como pode ver, ambas as abordagens são relativamente simples de implementar e com lógicas fáceis de se entender.

### Vamos ao código!

Como diz o famoso ditado (que eu acabei de inventar 😅), não vive o Dev de "Bla Bla Bla" mas do seu código , então vamos a um exemplo de vida real para tornar tudo mais claro!

> Disclaimer: o código abaixo usa a convenção de criação de componentes usando funções e consequentemente faz o uso dos poderosos hooks.

```jsx
const InputComponent = (props) => {
  return <input ref={props.inputRef} />;
};

const Example: FC = () => {
  const nameRef = useRef();

  useEffect(() => {
    setTimeout(() => {
      if (nameRef && nameRef.current) nameRef.current.value = 'Gabriela';
    }, 2000);
  }, []);

  return <InputComponent inputRef={nameRef} />;
};
```

No exemplo acima o valor (value) do input é atualizado pelo componente pai sem que seja necessário uma re-renderização. Não é incrível?

### Agora já sei usar Refs em React 🥷🏽

Até aqui aprendemos sobre o que são referências em React e como definir e utilizar, agora somos ninjas!! Referências são muito poderosas e quando bem usadas podem aumentar de forma massiva a performance dos sites em React, muitas libs famosas como react-use-form usam-nas para evitar a renderização desnecessária dos inputs enquanto o usuário os preenche! Sabendo de tudo isso espero que cries coisas fantásticas usando referências! Seja Feliz!

Obrigado por ler este artigo ❤️ espero que tenha sido útil para ti.
