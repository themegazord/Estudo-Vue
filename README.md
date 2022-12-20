# Vue.JS

* Framework Javascript
* Código fonte aberto
* Criação de interfaces de usuários (front-end)
* Aplicações de página única (SPA)

## Vantagens

* Manipulação simplificada do DOM
* Reatividade e Sincronia de estado
* Divisão de interfaces em componentes
* Leve
* Comunidade Ativa


## options

```html
<body>
  <div id='app'>

  </div>
</body>
```


```js
  options = {
    el: '#app',
    data: {},
    methods: {}
  };

  new Vue(options);
```

* `el` -> Para selecionar um elemento com o vue, dentro do objeto options, vai passar ou `#` ou `.`, dependendo se for, respectivamente, um `id` ou uma `classe`
* `data` -> Espera um objeto como parametro que vai conter as variaveis/atributos relativos do template
* `methods` -> Vai armazenar todos os metodos/funções da template

### Um outro jeito de usar o Vue

Em vez de criar um objeto `options` podemos passar esse objeto puro diretamente como parametro, no instanciamento do objeto Vue.

```js
  new Vue({
    el: '#app',
    data: {
      
    }
  });
```

### Capturando dados da propriedade `data`

Como o `new Vue()` é uma classe, podemos utilizar de chamadas de POO para conseguirmos acessar dados com o metodo `this`.

```js
new Vue({
  el: '#app',
  data: {
    n1: 10,
    n2: 5
  },
  methods: {
    somar() {
      return this.n1 + this.n2;
    }
  }
})
```

Só teremos um problema nessa situação quando formos utilizar `arrows functions` dentro de `methods`, visto que, o contexto léxico fará com que `this` dentro da `arrow fn` aponte para a classe `Window`

## Diretiva V-Bind

A diretiva `v-bind` é utilizada para vincular valores nas propriedades dos elementos HTML.

```html
<body>
  <div id="app">
    <a v-bind:href="site">Site</a>
  <div>
</body>
```

```js
new Vue({
  el: '#app',
  data:{
    site: 'google.com.br'
  },
  methods:{},
})
```

No caso do código acima vamos estar vinculando o valor da variavel `site`, da propriedade `data` a propriedade `href` do elemento `a` do html. Assim, toda vez que o valor de `site` for alterado, o valor do `href` também será;

### Sintaxe Sugar V-Bind

O `v-bind` contém o _sintaxe sugar_ `:`, ou seja, ficaria da seguinte forma:


```html
<body>
  <div id="app">
    <a :href="site">Site</a>
  <div>
</body>
```

```js
new Vue({
  el: '#app',
  data:{
    site: 'google.com.br'
  },
  methods:{},
})

```

## Diretiva `v-on`

A diretiva `v-on` é responsavel por escutar os eventos em elementos HTML. Permite que você execute uma determinada ação quando um evento ocorre:

```html
  <div id="app">
    <input type="text" :value="count">
    <button v-on:click="add()">+</button>
    <button @click="sub()">-</button>
  </div>
```

```js
  new Vue({
        el: '#app',
        data: {
          count: 0
        },
        methods: {
          add() {
            console.log()
            this.count++
          },
          sub() {
            if (this.count == 0) return;
            this.count--
          }
        }
      });
```

O código acima vai incrementar ou decrementar a variavel `count` contida na propriedade `data` toda vez que ou o botão com valor `+` ou `-` for clicado, chamando as funções `add()` e `sub()` respectivamente. Observasse que, tem um tratamento na função `sub()` que verifica se o valor de `count` é zero, para não deixar decrementar mais.

O _sintaxe sugar_ do v-on é o `@`.

## Diretiva `v-if`, `v-else-if` e `v-else`

Essas diretivas são o equivalente operadores condicionais `if...else if...else`, fazendo com que certos elementos sejam alterados ou não.

```html
    Nota: <input type="text" id="inputNota" @blur="setNota()">
    <p v-if="nota == ''">Digite a nota do aluno</p>
    <p v-else-if="nota >= 7 && nota <= 10" class="green">Aluno aprovado</p>
    <p v-else-if="nota >= 5 && nota < 7" class="yellow">Aluno em Recuperação</p>
    <p v-else-if="nota >=0 && nota < 5" class="red">Aluno reprovado</p>
    <p v-else>A nota informada é inválida</p>
```

```js
    new Vue({
      el: '#app',
      data: {
        nota: 0,
      },
      methods: {
        setNota() {
          this.nota = inputNota.value
        }
      }
    });
```

No código acima, quando o input perder o foco, ele vai fazer com que a função `setNota()` seja ativada, pegando a informação do input pelo seu `id.value` e inserindo na variavel `nota` do metodo `data`. Quando isso ocorrer, as diretivas vao verificar qual é o valor da variavel `nota` e será definido qual será mostrado em tela. 

## Diretiva `v-show`

A diretiva `v-show` é semelhante a diretiva `v-if` com a diferença de que o componente que porta o `v-show` é carregado no html porém, com o atributo `style="display=none"`, sendo que o `v-if` carrega o atributo html apenas que o fator `if` for true.

```html
  <div id="app">
    <p @mouseover="exibirTextAjuda = true" @mouseout="exibirTextAjuda = false">Exemplo tooltip</p>
    <div v-show="exibirTextAjuda">
      <h3>Titulo texto ajuda</h3>
      <p>Conteudo texto ajuda</p>
    </div>
  </div>
```

```js
    new Vue({
      el: '#app',
      data: {
        exibirTextAjuda: false
      },
      methods: {}
    });
```

O código acima, a diretiva `v-show` espera um fator booleano, que é definido com a variavel `exibirTextAjuda` que começa como `false`. Usando os eventos `@mouseover` e `@mouseout` definimos diretamente o valor dessa variavel como `true` ou `false` respectivamente. Fazendo com que o atributo `style="display=none"` seja inserido ou retirado da `div`

## Diretiva `v-html`

Essa diretiva faz o mesmo papel do `innerHTML` do DOM.

```html
  <div id="app">

    <div v-html="elementosHTML">

    </div>

  </div>
```

```js
    new Vue({
      el: '#app',
      data: {
        elementosHTML: '<p><b>Site:</b></p><a target="_blank" href="https://www.google.com.br">Google</a>'
      },
      methods: {}
    });
```

Caso haja qualquer informação dentro da div que contém a diretiva `v-html` preenchida, os valores serão subsitituidas pelos valores que estão sendo passados para o `v-html`

## Diretiva `v-text`

Essa diretiva funciona da mesma forma que `innerText`

## Diretiva `v-once`

Essa diretiva faz com que o valor do elemento HTML seja o mesmo que foi setado quando foi carregado

## Diretiva `v-for`

Essa diretiva faz o papel de operador de _looping_ dentro do Vue

```html
    <ul>
      <li v-for="(curso, key) in cursos" v-text="key+1 + '-' + curso"></li>
    </ul>
```

```js
new Vue({
      el: '#app',
      data: {
        cursos: ['Laravel', 'Web Completo', 'Banco de Dados', 'SOLID', 'Angular']
      },
      methods: {}
    });
```

O script acima, vai iterar sobre a variavel `cursos`, inserindo o tanto o `value` quando `key` na `li` da `ul`.

O mesmo pode ser feito com objetos

```html
    <table border="1px">
      <thead>
        <tr>
          <th>ID</th>
          <th>Titulo</th>
          <th>Descrição</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="({titulo, descricao}, key) in cursos">
          <td>{{key}}</td>
          <td>{{titulo}}</td>
          <td>{{descricao}}</td>
        </tr>
      </tbody>
    </table>
```

```js
new Vue({
      el: '#app',
      data: {
        cursos: {
          1000: {
            titulo: 'Laravel',
            descricao: 'Domine o framework PHP mais poderoso do mundo'
          },
          1001: {
            titulo: 'Banco de Dados',
            descricao: "Aprenda os principais BD's do mercado"
          },


        }
      },
      methods: {}
    });
```

O código acima vai iterar sobre o objeto `cursos` e atraves do _destructuring assignment_ vai retornar os valores das chaves, sem ter que trazer todo o objeto.

## Tag `template`

A tag `template` se comporta como elemento pai, porém, não é renderizada no DOM

## Variaveis computadas

As variaveis computadas dentro do Vue são funções que são definidas em um componente Vue e que retornar um Valor. São usadas para calcular um valor a partir de outras propriedades do componente e podem ser usadas em qualquer lugar do template do componente.

```html
  <div id="app">
    <h1>Adicionar pacientes</h1>
    <span>Nome: </span> <input type="text" id="inputNome">
    <span>Idade: </span> <input type="text" id="inputIdade">
    <span>Plano: </span> <input type="text" id="inputPlano">
    <button @click="addPacientes()">Adicionar</button>
    <hr>
    <h4>Último paciente cadastrado</h4>
    <p v-if="pacientes.length > 0">
      {{lastPatient}}
    </p>
    <hr>
    <h4>Pacientes do plano Ouro</h4>
    <p v-for="p in isGold">{{p.nome}}</p>
    <hr>
    <h4>Pacientes cadastrados</h4>
    <p v-for="p in pacientes">{{p.nome}}</p>
  </div>
```

```js
new Vue({
      el: '#app',
      data: {
        pacientes: [],
      },
      methods: {
        addPacientes() {
          this.pacientes.push({
            nome: inputNome.value,
            idade: inputIdade.value,
            plano: inputPlano.value,
          })
          inputNome.value = ''
          inputIdade.value = ''
          inputPlano.value = ''
        }
      },
      computed: {
        lastPatient() {
          let key = this.pacientes.length - 1
          let txt = ''
          txt += 'Paciente: ' + this.pacientes[key].nome + ', '
          txt += 'Idade: ' + this.pacientes[key].idade + ', '
          txt += 'Plano: ' + this.pacientes[key].plano

          return txt
        },
        isGold() {
          return this.pacientes.filter(e => e.plano === 'Ouro')
        }
      }
    });
```

Tanto a variavel `lastPatient` quanto a `isGold` são variaveis computadas, usadas para tirar do template a responsabilidade de gerir regras de negócio.

## Diretiva `v-model`

A diretiva `v-model` é utilizada como atalho para o conjunto de duas diretivas, `v-bind` e `v-on`. Ao usar essa diretiva, criamos uma via de mão dupla, onde em tempo real, alteramos uma varivel do componente Vue com os dados inseridos em um input do HTML

```html
    {{paciente}} <br>
    <input type="text" v-model="paciente">
```

```js
    new Vue({
      el: '#app',
      data: {
        paciente: 'Gustavo de Camargo',
      },
      methods: {},
      computed: {}
    });
```

## Propriedade `watch`

Watch, como o proprio nome já diz, vai ficar observando alguma variavel da propriedade `data` e vai retornar 2 valores, o valor novo inserido nessa variavel e o valor antigo.

```html
  <div id="app">
    <span>Paciente: </span><input type="text" v-model="paciente">
    <br>
    <ul>
      <li v-for="p in lista">{{p.nome}}</li>
    </ul>
  </div>
```

```js
new Vue({
      el: '#app',
      data: {
        pacientes: [
          { nome: 'Gustavo de Camargo Campos', idade: 23 },
          { nome: 'Edma Aparecida de Camargo', idade: 49 },
          { nome: 'Guilherme dos Santos', idade: 19 },
          { nome: 'Aline da Silva Santos', idade: 23 },
          { nome: 'Antonio Marcos Campos', idade: 45 },
        ],
        lista: []
      },
      methods: {},
      computed: {},
      watch: {
        paciente(newValue, oldValue) {
          this.lista = this.pacientes.filter(e => e.nome.match(newValue))
        }
      }
    });
```

O código acima vai funcionar como uma consulta _inline_ sempre retornando um novo array que satisfaça a consulta feita no array existente.