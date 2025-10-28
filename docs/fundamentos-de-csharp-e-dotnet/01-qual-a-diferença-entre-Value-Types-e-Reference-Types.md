# 1. Qual a diferença entre _"Value Types"_ e _"Reference Types"_ em C#?

## ⚙️ O que são **Value Types**
Sobre **Value Types**, basicamente a diferença está no tipo de alocação em memória.

Quando falamos de **Value Types**, estamos nos referindo aos tipos primitivos (ex: `int`, `bool`, `struct`); esses são alocados na memória Stack.

A nível de código, quando você passa uma variável **Value Type** para um método, está passando apenas uma cópia do valor. Nesse caso, ao alterar o valor dentro do método, o valor original não é afetado, pois a alteração está restrita ao escopo do método.

## ⚙️ O que são **Reference Types**
Já as variáveis **Reference Types** (ex: `class`, `string`, `object`) possuem uma referência para seus valores, que são alocados na memória Heap.

A nível de código, quando você passa uma variável **Reference Type** para múltiplos métodos, todos os escopos estarão observando a mesma instância.

Caso uma alteração seja feita, todos os métodos estarão lidando com o objeto já alterado.
