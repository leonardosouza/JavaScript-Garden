## Protótipos

Javascript não possue em suas características um sistema clássico de herança, mas utiliza *protótipos* para isto.

Enquanto isso muitas vezes é considerada um dos pontos fracos do JavaScript, o modelo de herança prototípica é de fato mais poderoso que o modelo clássico. Por exemplo, é relativamente trivial construir um modelo clássico baseado em um modelo de protótipo, enquanto o contrário é uma tarefa muito mais difícil.

JavaScript é a única linguagem utilizada, que apresenta herança prototípica, por isso pode-se levar algum tempo para ajustar-se às diferenças entre os dois modelos.

A primeira grande diferença é que a herança em JavaScript usa *cadeias de protótipos* (*prototype chains*).

> **Nota:** Simplesmente usando `Bar.prototype = Foo.prototype` teremos dois objetos 
> que compartilham o **mesmo** protótipo. Portanto, as mudanças que se realizem em um 
> destes objetos afetará o outro objeto, sendo assim, na maioria dos casos não este não é 
> o comportamento desejado pelo desenvolvedor.

    // construtor Foo
    function Foo() {
        this.value = 42;
    }

    // Adiciona um método ao protótipo do construtor Foo
    Foo.prototype = {
        method: function() {}
    }

    // construtor Bar
    function Bar() {}

    // Define o protótipo de Bar como uma nova instância de Foo
    Bar.prototype = new Foo();

    // Adiciona ao protótipo de Bar uma nova propriedade
    Bar.prototype.foo = 'Hello World';

    // Assegura que o construcor seja Bar
    Bar.prototype.constructor = Bar;

    // cria uma nova instância do construtor Bar
    var test = new Bar(); 

    // Resultado da cadeia de protótipos (prototype chain)
    test [instance of Bar]
        Bar.prototype [instance of Foo] 
            { foo: 'Hello World' }
            Foo.prototype
                { method: ... }
                Object.prototype
                    { toString: ... /* etc. */ }

No código anterior, o objeto `test` hedará de `Bar.prototype` e `Foo.prototype`,
por isso ele terá acesso a função `method` que foi definida em `Foo`.

Ele também terá acesso a propriedade `value` da **única** instância de `Foo` 
que compõe seu protótipo. É importante notar que `new Bar()` **não** criará uma nova
instância de `Foo`, mas reutilizará os atributos associados ao seu protótipo, desta forma, 
todas as instâncias de `Bar` compartilharão o **mesmo** `valor` da propriedade.

> **Nota:** **Não** use `Bar.prototype = Foo`, uma vez que não irá apontar para o protótipo
> de `Foo`, mas sim para a função de objeto `Foo`. Assim, a cadeia de protótipos
> vai passar por cima `Function.prototype`e não `Foo.prototype`,
> portanto, o `método` não estará disponível na cadeia de protótipos.


### Busca de propriedades

Ao acessar as propriedades de um objeto, o JavaScript irá percorrer a cadeia de protótipos **para cima** até encontrar uma propriedade com o nome solicitado.

Se atingir o topo da cadeia - ou seja `Object.prototype` - e ainda não encontrou a propriedade especificada, ele retornará o valor [undefined](#core.undefined) em seu lugar.

### A propriedade prototype

Enquanto a propriedade prototype é usado pela linguagem para construir as cadeias de protótipos, é ainda possível atribuir qualquer valor dado para isto. No entanto, os tipos primitivos simplesmente ser ignorados quando atribuídos a prototype.

    function Foo() {}
    Foo.prototype = 1; // não terá efeito

Atribuindo objetos, como mostrado no exemplo anterior, vai funcionar, além de permitir a criação dinâmica de cadeias de protótipos.


### Performance

O tempo de pesquisa para as propriedades que estão no alto da cadeia de protótipos pode ter um impacto negativo no desempenho, e isso pode ser significativo no código onde o desempenho é crítico. Além disso, tentando acessar propriedades inexistentes fará o Javascript atravessar a cadeia de protótipos por completo.

Além disso, quando [iteramos](#object.forinloop) as propriedades de um objeto, cada propriedade existente na cadeia de protótipos será enumerada.

### Extensão de protótipos nativos

Uma característica ruim é normalmente extender `Object.prototype` a qualquer outro protótipo construído.

Esta técnica é conhecida como [monkey patching][1] e quebra o *encapsulamento*. Embora utilizado por frameworks populares como [Prototype][2], ainda não há uma boa razão para adotá-lo ou integrá-lo como tipos de dados ou funcionalidade não-padrão.

A única boa razão para extender um protótipo embutido (built-in) é a portar os recursos mais recentes de motores de JavaScript, como por exemplo o [`Array.forEach`][3].


### Conclusão

É essencial entender o modelo de herança prototípica antes de escrever código complexo que faça uso dela. Além disso, estar ciente da extensão das cadeias de protótipos em seu código e dividi-las, se necessário, para evitar possíveis problemas de desempenho. Outro ponto, é que os protótipos nativos nunca devem ser extendidos a menos que seja por uma questão de compatibilidade com os mais recentes recursos de JavaScript.

[1]: http://en.wikipedia.org/wiki/Monkey_patch
[2]: http://prototypejs.org/
[3]: https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/forEach