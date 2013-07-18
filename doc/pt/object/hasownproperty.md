## `hasOwnProperty`

Com a finalidade de aferir se um objeto possue uma propriedade definida *em si* mesmo e **não**
de algum lugar de sua [cadeia de protótipos](#object.prototype), é necessário utilizar
o método  `hasOwnProperty` que todos os objetos herdam de `Object.prototype`.

> **Nota:** **Não** é suficiente verificar se uma propriedade é `undefined`.
> A propriedade continuará existindo, e apenas o seu valor é que passará a ser definido como
> `undefined`.


`hasOwnProperty` é a única coisa em JavaScript que lida com as propriedades e não atravessar 
a cadeia de protótipos.

    // Envenenando o Object.prototype
    Object.prototype.bar = 1; 
    var foo = { goo: undefined };
    
    foo.bar; // 1
    'bar' in foo; // true

    foo.hasOwnProperty('bar'); // false
    foo.hasOwnProperty('goo'); // true

Apenas `hasOwnProperty` vai dar o resultado correto e esperado, o que é 
essencial quando há iteração sobre as propriedades de qualquer objeto. Não há 
outra maneira de excluir as propriedades que não são definidas no objeto em si, 
mas em algum lugar em sua cadeia de protótipos.

### `hasOwnProperty` como uma propriedade

JavaScript **não** protege o nome da propriedade `hasOwnProperty`, assim, existe a possibilidade
de que um objeto pode ter uma propriedade com este nome, é necessário usar 
um `hasOwnProperty` externo para obter resultados corretos.

    var foo = {
        hasOwnProperty: function() {
            return false;
        },
        bar: 'Here be dragons'
    };

    foo.hasOwnProperty('bar'); // sempre devolverá false

    // utilizando call empreste o método hasOwnProperty de outro objeto 
    // passando 'foo' como objeto contexto para checar a propriedade 'bar'
    ({}).hasOwnProperty.call(foo, 'bar'); // true

### Conclusão

Usar `hasOwnProperty` é o único método confiável para verificar a existência de uma propriedade em um objeto. 
Recomenda-se o uso de `hasOwnProperty` sempre que fizer uso de estruturas de repetição, como [loops `for in`](#object.forinloop), isto evitará que ocorram erros com propriedades provenientes da extensão de [protótipos](#object.prototype)
nativos.