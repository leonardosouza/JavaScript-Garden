## Uso de objetos e propriedades

Tudo em JavaScript funciona como um objeto, com as duas únicas excessões de
[`null`](#core.undefined) e [`undefined`](#core.undefined).

    false.toString(); // 'false'
    [1, 2, 3].toString(); // '1,2,3'

    function Foo() {}
    Foo.bar = 1; 
    Foo.bar; // 1

Um equívoco comum é que literais númericos não podem ser utilizadas como objetos. Isso ocorre porque uma falha no parser do JavaScript tenta avaliar a *notação de ponto* como se 
este fosse um elemento da definição de um número de ponto flutuante.

    2.toString(); // lança SyntaxError

Existem várias soluções que podemos utilizar para que os literais numéricos funcionem como
objetos:

     2..toString();  // adicionando um segundo ponto
     2 .toString();  // adicionando um espaço a esquerda do ponto
    (2).toString();  // avaliando-o dentro de parenteses

### Objetos como um tipo de dados


Objetos em JavaScript também podem ser usados como [*Hashmaps*][1]; que consistem basicamente de propriedades nomeadas (chaves) contendo valores associados (mapeados) a estas últimas.

Usando um objeto literal - {} notação - é possível criar um objeto simples. Este novo objeto é [herdado](#object.prototype) de Object.prototype e não tem [propriedades próprias](#object.hasownproperty) definidas.

    var foo = {}; // um novo objeto vazio

    // um novo objeto com a propriedade 'test' contendo o valor 12
    var bar = {test: 12};

### Accesso a propriedades

Podemos acessar as propriedades de um objeto de duas maneiras, seja utilizando a notação de ponto ou a notação de colchetes.

    var foo = {name: 'Kitten'}
    foo.name; // kitten
    foo['name']; // kitten

    var get = 'name';
    foo[get]; // kitten

    foo.1234; // SyntaxError
    foo['1234']; // funciona!

Ambas notações trabalham de forma quase idêntica, com a única diferença sendo que a notação de colchetes permite a configuração dinâmica de propriedades e o uso de nomes de propriedades que poderiam levar a um erro de sintaxe.


### Eliminando propriedades

A única maneira de eliminar uma propriedade de um objeto é utilizado o operador `delete`

A única maneira de remover uma propriedade de um objeto é usar o operador `delete`. Definindo a propriedade como `undefined` ou `null` apenas remove o *valor* associado à propriedade, mas não a *chave*.

    var obj = {
        bar: 1,
        foo: 2,
        baz: 3
    };
    
    obj.bar = undefined;
    obj.foo = null;
    delete obj.baz;

    for(var i in obj) {
        if (obj.hasOwnProperty(i)) {
            console.log(i, '' + obj[i]);
        }
    }


Os resultados do log acima são `bar undefined` e `foo null` - apenas `baz` foi removido, e portanto não aparece no log.


### Avaliação das Chaves

    var test = {
        'case': 'Sou uma palavra chave, devo ser avaliada como uma string',
        delete: 'Também sou uma palavra chave' // lança SyntaxError
    };

As propriedades dos objetos podem ser definidos com carateres simples como strings. Devido ao mal desenho do parser Javascript, o exemplo anterior lançará uma excessão `SyntaxError` para ambientes rodando versões anteriores ao
ECMAScript 5.

Este erro se deve ao fato de `delete` ser uma *palavra reservada* da linguagem, por tanto deve ser definido como uma string literal para garantir que ele será interpretado corretamente pelos parsers Javascript mais velhos.

[1]: http://en.wikipedia.org/wiki/Hashmap

