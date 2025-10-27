## 19. Diferença entre `==` e `.Equals()`
Entender a diferença entre `==` e `.Equals()` é fundamental em C#, pois ela muda completamente dependendo se você está lidando com **Value Types** ou **Reference Types**:

 1. **Value Types** (ex: `int`, `double`, `struct`)
Para tipos de valor, ambos `==` e `.Equals()` comparam o **conteúdo**, porque structs e tipos primitivos **implementam `Equals()`** e **sobrecarga de `==`** para comparar valores.
Exemplo:
```csharp
int a = 10;
int b = 10;

Console.WriteLine(a == b);       // True
Console.WriteLine(a.Equals(b));  // True
```
➡️ Ambos retornam `True`, pois comparam o **valor numérico**.

 2. **Reference Types** (ex: `class`, `object`)
Para classes, o comportamento depende se o tipo **sobrescreve ou não** `Equals()` ou o operador `==`.

Exemplo:
```csharp
class Pessoa
{
    public string Nome;
}

Pessoa p1 = new Pessoa { Nome = "Guilherme" };
Pessoa p2 = new Pessoa { Nome = "Guilherme" };

Console.WriteLine(p1 == p2);       // False
Console.WriteLine(p1.Equals(p2));  // False
```
➡️ Ambos são `False`, pois `p1` e `p2` são **duas instâncias diferentes na memória**.
Mesmo que tenham o mesmo conteúdo, **as referências são distintas**.
