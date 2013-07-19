## As declarações de função e expressões

Funções em JavaScript são objetos de primeira classe. Isso significa que elas podem ser passadas como argumentos de outras funções, ​​como qualquer outro valor. Um uso comum desse recurso é passar uma *função anônima* como `callback` para outra, possivelmente de forma assíncrona.


### A declaração de `função nomeada`

    function foo() {}

A função acima fica [içada](#function.scopes) (hoisting) antes do início da execução do 
programa, assim, está disponível em *todo* o escopo da aplicação definida, 
mesmo antes de ser realmente chamada.

    foo(); // Funciona porque foo foi criado antes que este código seja executado
    function foo() {}

### A expressão de `função anônima`

    var foo = function() {};

Este exemplo define uma função sem nome e anônima a variável `foo`.

    foo; // 'undefined'
    foo(); // Lança TypeError
    var foo = function() {};

Devido ao fato de que `var` é uma declaração que iça o nome da variável `foo` antes da execução real do código começar,
`foo` já está definido quando o script é executado.

Como as atribuições só acontecem em tempo de execução, o valor de `foo` será [undefined](#core.undefined) antes 
do código correspondente ser executado.


### A expressão de `função nomeada`

Outro caso especial é a atribuição das funções citadas.

    var foo = function bar() {
        bar(); // Funciona
    }
    bar(); // ReferenceError

Aqui, a `bar` não se encontra disponível no escopo exterior, uma vez que a única função é 
atribuída a `foo`, no entanto, no interior de `bar` está disponível. Isto é devido ao modo 
de [resolução de nome](#function.scopes) no JavaScript, o nome da 
função está *sempre* disponível no escopo local da própria função.