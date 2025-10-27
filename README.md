# Caminho para Desenvolvedor Sênior


## Tabela sugestiva para uso de IA:

| Habilidade Sênior  | Como a IA Pode Ajudar |
| ------------- |:-------------:|
| Arquitetura & Design | ChatGPT: "Projete a arquitetura de um microsserviço de processamento de pagamentos. Inclua decisões sobre comunicação, resiliência e padrões de projeto." |
| Otimização & Performance | GitHub Copilot: "Reescreva esta consulta LINQ para ser mais eficiente com grandes datasets." ChatGPT: "Qual a diferença de performance entre StringBuilder e concatenação em C# em um loop de 1000 iterações?" |
| Debugging Complexo | Copilot Chat: "Analise este stack trace e sugira as possíveis causas raiz para esta NullReferenceException." |
| Testes de Unidade Robusto | GitHub Copilot: "Gere testes unitários para este método que cubram todos os caminhos de exceção possíveis." |
| Concorrência & Async/Await | ChatGPT: "Explique os perigos de deadlock em código async/await e como um Sênior os evita usando ConfigureAwait(false)." |
| Conhecimento do Ecossistema | Bing Chat: "Quais as melhores práticas atuais para configurar Health Checks em uma API .NET 8?" |

## Aviso Crítico: O Perigo da Dependência
Tornar-se Sênior não é só saber **_como fazer_**, mas saber **_por que fazer_**. A IA é uma ferramenta, não um substituto para o seu julgamento.
* **Não Copie Cego**: Sempre entenda o código que a IA gerou. Pergunte por que ela sugeriu aquela solução.
* **Valide as Respostas**: A IA alucina e comete erros, especialmente em assuntos muito novos ou complexos. Sempre confirme com a documentação oficial.
* **Desenvolva seu Critério**: A habilidade de um Sênior é julgar entre várias soluções boas. Use a IA para gerar opções, mas a decisão final deve ser sua, baseada no contexto do negócio.

## Conclusão
Não existe uma "melhor IA", mas um ecossistema. Comece com:
1. **GitHub Copilot** no seu dia a dia de codificação.
2. **ChatGPT 4** como seu tutor para conceitos avançados e arquitetura.
3. **SonarLint** para treinar seu olhar para a qualidade do código.

Use essas ferramentas de forma ativa e questionadora, e você acelerará dramaticamente sua jornada para se tornar um Desenvolvedor Sênior C# não apenas no título, mas na essência.

## 1. Fundamentos de C# e .NET

### 1.1 Qual a diferença entre _"Value Types"_ e _"Reference Types"_ em C#?
> Sobre **Value Types**, basicamente a diferença está no tipo de alocação em memória.
Quando falamos de **Value Types**, estamos nos referindo aos tipos primitivos (ex: `int`, `bool`, `struct`); esses são alocados na memória Stack.
A nível de código, quando você passa uma variável **Value Type** para um método, está passando apenas uma cópia do valor. Nesse caso, ao alterar o valor dentro do método, o valor original não é afetado, pois a alteração está restrita ao escopo do método.

> Já as variáveis **Reference Types** (ex: `class`, `string`, `object`) possuem uma referência para seus valores, que são alocados na memória Heap.
A nível de código, quando você passa uma variável **Reference Type** para múltiplos métodos, todos os escopos estarão observando a mesma instância. Caso uma alteração seja feita, todos os métodos estarão lidando com o objeto já alterado.

## 5. O que é boxing e unboxing?
**Boxing** e **Unboxing** são processos de **conversão automática entre tipos de valor (value types)** e **tipos de referência (reference types)**.

Eles permitem que valores primitivos (como `int`, `bool`, `double`, `structs`, etc.) sejam **tratados como objetos** quando necessário.
📦 Boxing

> É o processo de **converter um Value Type em Reference Type**, normalmente para `object` ou `System.ValueType`.

🔹 Como funciona:

Quando você faz boxing, o .NET cria uma cópia do valor na heap e referencia essa cópia via um ponteiro (object).

🔹 Exemplo:
```csharp
int x = 42;
object obj = x;  // Boxing

Console.WriteLine(obj);  // 42
```
🔓 Unboxing

> É o processo inverso: converter um Reference Type de volta em um Value Type.

🔹 Exemplo:
```csharp
object obj = 42;   // Boxing
int y = (int)obj;  // Unboxing

Console.WriteLine(y);  // 42
```
🔍 O que acontece internamente:

O CLR verifica se o objeto realmente contém um valor do tipo int.
Se sim, ele copia o valor da heap de volta para a stack.
Se não for compatível, ocorre uma InvalidCastException.

💡 **Evitar Boxing**
A principal forma de evitar é **usar tipos genéricos (`T`)**:

**Sem genéricos (com boxing):**
```csharp
ArrayList list = new ArrayList();
list.Add(42);         // Boxing!
int value = (int)list[0];  // Unboxing
```
**Com genéricos (sem boxing):**
```csharp
List<int> list = new List<int>();
list.Add(42);         // Sem boxing
int value = list[0];  // Sem unboxing
```

⚠️ **Impacto de performance**
**Boxing** e **Unboxing** são custosos porque envolvem:
- **Cópia de dados** entre stack e heap
- **Alocação na heap** (que precisa de GC depois)
- **Conversão de tipo e checagem em tempo de execução**

| Operação     | Direção      | O que acontece                         | Onde fica o dado | Custo |
| ------------ | ------------ | -------------------------------------- | ---------------- | ----- |
| **Boxing**   | Stack → Heap | **Value Type** vira **Reference Type** | Heap             | Alto  |
| **Unboxing** | Heap → Stack | **Reference Type** vira **Value Type** | Stack            | Alto  |

[## 19. Diferença entre `==` e `.Equals()`](docs/fundamentos-de-csharp-e-dotnet/19-diferenca-entre-operator-equal-e-method-equals.md)
