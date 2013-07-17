## Protótipo

Javascript não possue em suas características um sistema clássico de herença, mas utiliza *protótipos* para isto.

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

> **Nota:** **No** utilice `Bar.prototype = Foo`, ya que no apunta al prototipo
> de `Foo`, sino al objeto de la función `Foo`. Así la cadena de prototipo
> cambiará a `Function.prototype` y no a `Foo.prototype`;
> Por lo tanto, el `método` no estará disponible en la cadena de prototipo.

### Búsqueda de propiedades

Cuando se accede a las propiedades de un objeto, JavaScript recorre la cadena de
prototipo hacia **arriba** hasta encontrar la propiedad con el nombre solicitado.

Cuando se llega al final de la cadena - concretamente `Object.prototype` - y aún
no se ha encontrado la propiedad especificada, se retornará un valor
[undefined](#core.undefined) en su lugar.

### La propiedad prototype

Aunque la propiedad prototype es usada por el lenguaje para construir la cadena
de prototipos, es posible asignar **cualquier** valor. Aunque los tipos primitivos 
serán ignorados cuando se asigne en prototype.

    function Foo() {}
    Foo.prototype = 1; // no tendrá efecto

La asignación de objetos, como se muestra en el ejemplo anterior, funcionará, y permitirá
la creación dinámica de cadena de prototipos.

### Rendimiento

El tiempo tomado en la búsqueda de propiedades es alta y la cadena de prototipo puede
presentar un impacto negativo critico en el rendimiento en partes del código. Además, 
si ha tratado de acceder a propiedades que no existen este saltara a la cadena de prototipo.

Además, al recorrer en [iteración](#object.forinloop) las propiedades de un objeto
y **cada** propiedad será encontrada en la cadena de prototipo de manera ennumerada.
 
### Extensión de prototipos nativos

Una mala característica que se suele utilizar para extender `Object.prototype` o cualquier
otro prototipo construido.

Esta técnica es conocida en inglés como [monkey patching][1] ya que *encapsula* lo que se interrumpe en el código.
Si bien es utilizado en frameworks como [Prototype][2], todavía no existen buenas razones para adoptarlo o integrarlo
como tipos de dato o como funcionalidad no estándar.

La **única** buena razón para extender un prototipo es acondicionarlo a nuevas
características en motores de JavaScript; por ejemplo, 
[`Array.forEach`][3].

### En conclusión

Se **debe** entender por completo el módelo de herencia prototipado antes de 
escribir código complejo que lo utlilice. Además, observe la longitud de la
cadena de prototipo y modificala si es necesario para evitar posibles problemas de 
rendimiento. Con relación a los prototipos nativos, estos **nunca** deben ser extendidos a 
menos que sea para mantener la compatibilidad con nuevas características de JavaScript.

[1]: http://en.wikipedia.org/wiki/Monkey_patch
[2]: http://prototypejs.org/
[3]: https://developer.mozilla.org/en/JavaScript/Reference/Global_Objects/Array/forEach

