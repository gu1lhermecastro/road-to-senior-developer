# 4. O que é o _**garbage collector**_ e como ele funciona?

🔹 O **Garbage Collector (GC)** é um **mecanismo automático de gerenciamento de memória** do .NET.
Ele é responsável por **liberar o espaço na memória Heap** que não está mais sendo usado por nenhum objeto ativo.


🔹 Quando o GC é executado, ele **começa pela Geração 0** (a mais barata de coletar).
Se ainda faltar memória, ele coleta também a 1 e depois a 2 (essa é mais pesada).

🔹 Depois de liberar o lixo, o GC pode **mover os objetos** restantes para preencher os buracos de memória.
Isso deixa a Heap **contígua** e melhora o desempenho de alocação (menos fragmentação).

Esse processo é chamado de **compaction**.

## 🧩 Exemplo prático

```csharp
void CriarObjetos()
{
    for (int i = 0; i < 10_000; i++)
    {
        var temp = new byte[1024]; // 1 KB
    }

    Console.WriteLine("Objetos criados!");
}
```
- Cada `new byte[1024]` cria um objeto na **Heap**.
- Quando o método termina, **nenhum deles é mais referenciado**.
- Em algum momento, o GC percebe isso e **libera automaticamente** essa memória.

## 🌎 Exemplo de problema real que o GC ajuda a evitar

Imagine um sistema que faz upload de imagens e cria vários objetos temporários em memória.
Sem GC, você precisaria liberar manualmente cada objeto (como `Dispose()` ou `free()` em C).<br>
Um descuido poderia causar **memory leaks**, travamentos e lentidão.

Com o GC, objetos não referenciados são liberados automaticamente, reduzindo muito esse tipo de bug.

## ⚙️ Como ele funciona — visão geral

1. **Você cria objetos** → o .NET aloca esses objetos na **Heap gerenciada**.
2. **O GC** monitora quais objetos ainda têm **referências vivas**.
3. Quando a Heap começa a encher, o GC **pausa as threads** por um breve momento (chamado _Stop-the-world_).
4. Ele então:
    - **Identifica** quais objetos ainda estão sendo usados (alcançáveis);
    - **Libera** os que não estão (sem referência);
    - **Compacta** a memória (opcionalmente) para otimizar o espaço.
5. Depois disso, as threads voltam a rodar normalmente.

Conceito de Generations (Gerações)

## ⚙️ O GC organiza a memória Heap em 3 gerações:

Geração	Descrição	Tamanho típico	O que armazena
| Geração   | Descrição             | Tamanho típico | O que armazena                                   |
| --------- | --------------------- | -------------- | ------------------------------------------------ |
| **Gen 0** | Objetos recém-criados | pequena        | Objetos temporários (curta duração)              |
| **Gen 1** | Intermediária         | média          | Objetos que sobreviveram a algumas coletas       |
| **Gen 2** | Longa duração         | grande         | Objetos “antigos” (ex: caches, singletons, etc.) |

## ⚙️ Como o GC detecta objetos “mortos”

Ele segue um **grafo de acessibilidade** a partir das _roots_ (raízes):

As _roots_ são:
- Variáveis locais em execução;
- Parâmetros de métodos;
- Objetos estáticos (`static`);
- Referências em registradores da CPU.

O GC percorre todas essas referências e **marca os objetos acessíveis**.
Tudo que **não puder ser alcançado** é considerado “lixo” (garbage) e é removido.


> [!WARNING]
> **IMPORTANTE**: o GC não é mágica
> Mesmo com GC, você ainda pode causar problemas, por exemplo:
> - Guardar referências desnecessárias em listas ou caches → impede a coleta.
> - Criar muitos objetos rapidamente → aumenta a frequência do GC.
> - Manter objetos grandes vivos sem necessidade → cresce a Heap e aumenta pausas.
