## O loop `for in`

Assim como o operador `in`, o loop `for in` também percorre a cadeia de protótipos quando itera as propriedades de um objeto.

> **Nota:** O loop `for in` **não** se repetirá em qualquer propriedade que
> tenha atributos `enumeráveis` definidos como `false`; por exemplo, a propriedade 
> `length` de um array.
    
    // Envenenando o Object.prototype
    Object.prototype.bar = 1;

    var foo = {moo: 2};
    for(var i in foo) {
        console.log(i); // Imprime as propriedades bar e moo
    }

Já que não é possível modificar o comportamento do loop `for in` em si mesmo, é
necessário filtrar as propriedades internas não desejadas dentro do loop,
para isto utilizamos o método [`hasOwnProperty`](#object.hasownproperty) de
`Object.prototype`.

> **Nota:** Como `for in` sempre percorre por completo a cadeia de protótipos, 
> ele ficará mais lento a cada camada adicional de herença adicionado a um objeto.

### Usando `hasOwnProperty` para filtragem

    // O mesmo objeto foo do código acima
    for(var i in foo) {
        if (foo.hasOwnProperty(i)) {
            console.log(i);
        }
    }

Está versão é a única forma correta de uso. Isto se deve **apenas** ao uso de
`hasOwnProperty` que imprimirá `moo`. Quando `hasOwnProperty` for omitido, o código é
propenso a erros para os casos em que os protótipos nativos - ex. `Object.prototype` -
tenham sido extendidos. 

Um dos frameworks mais usados que implementa essa funcionalidade é o [Prototype][1].
Quando o framework é incluido, o loop `for in` que não utilizar `hasOwnProperty` não poderá
garantir que não será interrompido.

### Conclusão

Recomenda-se usar **sempre**  utilizar `hasOwnProperty`. Pressupostos nunca devem ser feitos sobre o meio ambiente em que o código está sendo executado, ou se os protótipos nativos foram extendidos ou não. 

[1]: http://www.prototypejs.org/

