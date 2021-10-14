---
title: 'useEffect - O "all in one"'
date: '14 de Outubro, 2021'
preview: 'Um dos hooks mais importantes do React'
coverImage: 'some_url'
authors: 'Jose Tone'
label: 'ReactJS'
---

Se você é das antigas provavelmente lembra de como eram os componentes baseados em classe e como os métodos para tratar o seu ciclo de vida (não faz mal se não lembrar), não havia (e não há) nenhum problema com eles, eles são bem intuitivos e fazem o seu trabalho lindamente! Mas com a introdução dos componentes baseados em funções a coisa ficou relativamente complicada, é bem mais difícil de imitar o comportamento das funções de ciclo de vida de um componente baseado em classe para outro baseado em funções. Assim como toda a gestão de estados dos componentes em classe é feita por meio de hooks, a gestão de side-effects segue a mesma convenção, ela feita pelo hook **useEffect**.

> SideNote: Side-effects são todas as operações que afetam o seu componente e não podem ser feitas durante a renderização. ex: chamadas a api, subscrições...

### Antes de tudo...

É importante que antes tudo recordemos os métodos de ciclo de vida de componentes baseados em classe para assim fazermos uma relação directa entre eles e o useEffect. Seguem abaixo os métodos e suas responsabilidades:

- **componentDidMount**

  Ele é executado depois de o componente ser montado (como o próprio nome já sugere), ou seja, depois que o componente é adicionado à árvore DOM.

  ```jsx
  class Clock extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        date: new Date(),
      };
    }

    componentDidMount() {
      this.timerID = setInterval(() => this.tick(), 1000);
    }

    tick() {
      this.setState({
        date: new Date(),
      });
    }

    render() {
      return (
        <div>
          <h1>Hello, world!</h1>
          <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
      );
    }
  }
  ```

  exemplo tirado da documentação do React em: [https://reactjs.org/docs/state-and-lifecycle.html](https://reactjs.org/docs/state-and-lifecycle.html)

- **componentDidUpdate**

  É executado sempre que o componente é re-renderizado (não é executado na primeira renderização). É um ótimo lugar para trabalhar e operar no DOM quando o componente for atualizado. Por exemplo, você pode fazer chamadas a API. Apenas certifique-se de compará-lo com as props atuais.

  ```jsx
  componentDidUpdate(prevProps) {
  	// Uso típico (não te esqueças de comprar as props)
    if (this.props.userID !== prevProps.userID) {
      this.fetchData(this.props.userID);
    }
  }
  ```

  exemplo tirado da documentação do React em: [https://reactjs.org/docs/react-component.html#componentdidupdate](https://reactjs.org/docs/react-component.html#componentdidupdate)

- **componentWillUnmount**

  Este é o último método do ciclo de vida do componente, é executado quando o componente é removido do DOM, é normalmente usado para limpar todas as tarefas e ou referências de objectos no componente. Aproveitando o exemplo do componentDidMount:

  ```jsx
  class Clock extends React.Component {
    constructor(props) {
      super(props);
      this.state = {
        date: new Date(),
      };
    }

    componentDidMount() {
      this.timerID = setInterval(() => this.tick(), 1000);
    }

    componentWillUnmount() {
      clearInterval(this.timerID);
    }

    tick() {
      this.setState({
        date: new Date(),
      });
    }
    render() {
      return (
        <div>
          <h1>Hello, world!</h1>
          <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
        </div>
      );
    }
  }
  ```

  exemplo tirado da documentação do React em: [https://reactjs.org/docs/state-and-lifecycle.html](https://reactjs.org/docs/state-and-lifecycle.html)

### useEffect - o mágico!

Agora já vimos quais e como são os métodos de ciclo de vida dos componentes React baseados em classe está na hora de apreciarmos a magia do hook useEffect vendo como um só hook faz o trabalho (e muito bem) de três métodos sem complexidade nenhuma:

> Nota: para garantir uma melhor compreensão dos conceitos os exemplos usando useEffect (componentes em funções) serão os mesmo que os já citados outrora acima.

- **useEffect como componentDidMount**

  Para que o useEffect se comporte como o componentDidMount chamamos a função useEffect do React passando um callback síncrono como parâmetro, seguido de um array vazio (chamado de array de dependências).

  ```jsx
  import React from 'react';

  const Clock = () => {
    const [date, setDate] = React.useState(new Date());

    React.useEffect(() => {
      setInterval(() => this.tick(), 1000);
    }, []); // array vazio indica que o useEffect não tem dependências

    const tick = () => {
      setDate(new Date());
    };

    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {date.toLocaleTimeString()}.</h2>
      </div>
    );
  };
  ```

- **useEffect como componentDidUpdate**

  A forma de definir é praticamente idêntica a do exemplo acima (useEffect como componentDidMount) porém desta vez o array de dependências (segundo parâmetro do useEffect) deve conter no mínimo um elemento. É provavelmente dentre todas a forma mais popular de definir o useEffect.

  ```jsx
  import React from 'react';

  const Clock = () => {
    const [date, setDate] = React.useState(new Date());

    React.useEffect(() => {
      // verificação apenas para fins explicativos,
      // pode não fazer sentido em situações reais
      if (date.getFullYear() < 2022) {
        setInterval(() => this.tick(), 1000);
      }
    }, [date]); // array com uma dependência

    const tick = () => {
      setDate(new Date());
    };

    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {date.toLocaleTimeString()}.</h2>
      </div>
    );
  };
  ```

  No exemplo acima o useEffect será executado (além da primeira vez quando o componente renderiza) todas as vezes que o valor da variável "date" (e qualquer outra que esteja no seu array de dependências) mudar, como a "date" é uma variável de estado a sua mutação causa uma nova renderização, fazendo assim com que o useEffect seja executado também antes dela (nova renderização).

- **useEffect como componentWillUnmount**

  Para fazer o useEffect se comportar como o método componentWillUnmount em componentes baseados em classe só precisamos fazé-lo retornar uma função e executar o que queremos dentro dela. Está confuso? Vejamos o exemplo:

  ```jsx
  import React from 'react';

  const Clock = () => {
    const [date, setDate] = React.useState(new Date());

    React.useEffect(() => {
      // capturamos o id retornado pelo setInterval
      // para assim limpar o intervalo no unmount
      const timerID = setInterval(() => this.tick(), 1000);

      // a função retornada será apenas executada
      // quando o componente estiver a ser removido do DOM
      return () => {
        clearInterval(timerID);
      };
    }, []);

    const tick = () => {
      setDate(new Date());
    };

    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {date.toLocaleTimeString()}.</h2>
      </div>
    );
  };
  ```

### E aí?

Agora já conhece a fundo o poder do useEffect, já pode sair por aí criando componentes baseados em funções sem medo e nem complicações ao gerir os seus ciclos de vida! Seja feliz!

Obrigado por ler este artigo ❤️ espero que tenha sido útil para ti.
