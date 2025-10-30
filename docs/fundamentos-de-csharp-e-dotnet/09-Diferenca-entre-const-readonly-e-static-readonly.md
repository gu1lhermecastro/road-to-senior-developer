# 9. Diferença entre `const`, `readonly` e `static readonly`.

🔹 Em C#, você pode declarar campos que **não mudam de valor** com `const`, `readonly` ou `static readonly`.<br>
Apesar de parecerem parecidos, eles têm propósitos e **comportamentos bem diferentes**.

### Quando usar cada um:
| Situação                                                                                   | Melhor escolha    |
| ------------------------------------------------------------------------------------------ | ----------------- |
| Valor fixo universal, conhecido em tempo de compilação (ex: PI, nome da app, porta padrão) | `const`           |
| Valor imutável, mas definido em tempo de execução (ex: nome recebido no construtor)        | `readonly`        |
| Valor global, imutável e definido em runtime (ex: configuração carregada no startup)       | `static readonly` |

### Comparativo direto:
| Característica                         | `const`              | `readonly`             | `static readonly`                 |
| -------------------------------------- | -------------------- | ---------------------- | --------------------------------- |
| Escopo                                 | Estático (implícito) | Por instância          | Por tipo (classe)                 |
| Tempo de atribuição                    | Compilação           | Execução (construtor)  | Execução (carregamento da classe) |
| Pode mudar após construção?            | ❌ Nunca              | ❌ Apenas no construtor | ❌ Nunca                           |
| Pode ser valor calculado?              | ❌ Não                | ✅ Sim                  | ✅ Sim                             |
| Aceita tipos complexos?                | ❌ Não                | ✅ Sim                  | ✅ Sim                             |
| Substituído pelo compilador no código? | ✅ Sim                | ❌ Não                  | ❌ Não                             |

## 🧩 Exemplo prático:

```csharp
public class Configuracao
{
    public const string AppName = "MeuSistema"; // valor fixo

    public readonly string Ambiente; // depende do construtor

    public static readonly DateTime DataInicio = DateTime.Now; // inicializado em runtime

    public Configuracao(string ambiente)
    {
        Ambiente = ambiente;
    }
}
```
Uso:
```csharp
var cfg = new Configuracao("Produção");
Console.WriteLine($"{Configuracao.AppName} - {cfg.Ambiente} - {Configuracao.DataInicio}");
```

## ⚙️ `const`

#### Definição:
- Representa um valor constante em tempo de compilação.
- É implicitamente static (pertence ao tipo, não à instância).
- Só pode ser atribuído com valores literais conhecidos em compile-time.

Exemplo:
```csharp
public class Config
{
    public const double Pi = 3.14159;
    public const string AppName = "MeuApp";
}
```
Uso:
```csharp
Console.WriteLine(Config.Pi);       // 3.14159
Console.WriteLine(Config.AppName);  // "MeuApp"
```

#### Limitações:
- Só aceita tipos primitivos (int, double, bool, string, etc.) ou enum.
- Não pode receber resultado de métodos.
- Não pode mudar em runtime.
- Se uma DLL expõe um const e você recompila só uma parte do código, o valor antigo pode ficar “gravado” em quem o consome (pois o compilador substitui o valor literalmente no código).

Exemplo:
```csharp
// Lib A
public const int Version = 1;

// Lib B usa
Console.WriteLine(LibA.Version); // 1

// Se Lib A muda pra 2 e recompila
// Lib B continua mostrando 1 até ser recompilada também!
```

## ⚙️ `readonly`

#### Definição:

- O valor é definido em tempo de execução — mas só pode ser atribuído:
  - na declaração, ou
  - no construtor da classe.
- Pode conter qualquer tipo, inclusive objetos complexos.
- É útil quando o valor depende de lógica ou parâmetros recebidos no construtor.

Exemplo:
```csharp
public class Pessoa
{
    public readonly string Nome;

    public Pessoa(string nome)
    {
        Nome = nome; // ok
    }
}
```
Uso:
```csharp
var p = new Pessoa("Ana");
// p.Nome = "João"; // ❌ erro — readonly não pode ser modificado
```


## ⚙️ `static readonly`

#### Definição:
- Combina as características de `static` e `readonly`.
- O valor é único para a classe inteira, mas pode ser inicializado em tempo de execução (diferente de `const`).
- Pode chamar métodos, ler arquivos de configuração, etc.

Exemplo:
```csharp
public class Config
{
    public static readonly DateTime DataInicial = DateTime.Now;
}
```
Uso:
```csharp
Console.WriteLine(Config.DataInicial);
```
Aqui, **DataInicial** será inicializada **uma vez só**, quando a classe for carregada, e não poderá ser alterada depois.