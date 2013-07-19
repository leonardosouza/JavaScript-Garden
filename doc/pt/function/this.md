## Como funciona o `this`

Javascript tem um conceito diferente sobre a utilização do nome especial `this` se comparado a maioria das
linguagens de programação. Existem exatamente **cinco** diferentes formas de visualizar o valor
de `this` quando utiliza-se o mesmo na linguagem.

### O escopo global (Global Scope)

    this;

Quando se utiliza `this` no contexto global, ele simplesmente refere-se ao objeto *global*.

### Chamando uma função

    foo();

Aqui `this` refere-se ao objeto *global*.

> **Nota ES5:** Em modo estrito (strict mode), global **não** existe mais.
> `this` terá o valor `undefined` neste caso.

### Chamando um método

    test.foo(); 

Neste exemplo `this` refere-se a `test`.

### Chamando um  construtor

    new foo(); 

Chamar uma função precedendo-a pela palavra chave `new` atua como um 
[construtor](#function.constructors). Dentro da função, `this` refere-se ao `Objeto` *recém criado*.


### Definição explícita de `this`

    function foo(a, b, c) {}
                          
    var bar = {};
    foo.apply(bar, [1, 2, 3]); // array que se apilará
    foo.call(bar, 1, 2, 3); // resultados a = 1, b = 2, c = 3

Quando utlizamos os métodos `call` e `apply` de `Function.prototype`, o valor de
`this` dentro da função chamada se ajustará **explicitamente** ao primeiro argumento (objeto contexto)
correspondente a chamada da função.

Como resultado, no exemplo acima, a função `foo` será emprestada pelos métodos `call` e `apply`, fazendo
com que o this aponte para `bar`, já que este é definido como texto para execução da função.

> **Nota:** `this` **não pode** ser usado para referir-se a um objeto dentro de um `Objeto`
> literal. Assim `var obj = {me: this}` **não** dará nenhum resultado em `me` referindo-se a 
> `obj`, desta forma `this` apenas será obtido por um dos cincos casos enumerados anteriormente.

### Erros comuns

Embora a maioria destes casos faça sentido, o primeiro pode ser considerado como um erro
de concepção da linguagem, neste caso **nunca** terá uma utilidade prática.

    Foo.method = function() {
        function test() {
            // this é definido como um objeto global
        }
        test();
    }

Um erro comum é que `this` dentro de `test` faça referência a `Foo`, enquanto de fato isso **não existe**. 

Com a finalidade de acessar `Foo` de dentro de `test` é necessário criar uma variável local 
dentro de `method` para referir-se a `Foo`.

    Foo.method = function() {
        var that = this;
        function test() {
            // Usar that estende this aqui
        }
        test();
    }

`that` é um termo comumente utilizado para referenciar `this` para um contexto externo. 
Neste caso ele utiliza o recurso conhecido como [closures](#function.closures) para
passar `this` como valor da variável `that`.

### Atribuição de métodos


Outra coisa que **não** funciona em Javascript são os `alias`, que é a atribuição de um 
método para uma variável.

    var test = someObject.methodTest;
    test();


Devido ao primeiro caso, `test` agora age como uma simples chamada de função, portanto, 
`this` neste contexto não estará atribuído a `someObject`.

Enquanto a ligação tardia de `this` pode parecer a princípio uma má idéia, na verdade 
é o que permite-nos trabalhar com a chamada [herança prototípica](#object.prototype).

    function Foo() {}
    Foo.prototype.method = function() {};

    function Bar() {}
    Bar.prototype = Foo.prototype;

    new Bar().method();

Quando os `métodos` são chamados a partir de uma instância de `Bar`, `this` apontará para o 
objeto gerado a partir da desta instância.