# 10. O que é `record` e quando usá-lo?

🔹 Ele resolve um problema muito comum: **comparar objetos por valor** e **manter código imutável e limpo**.

🔹 Um `record` é um **tipo de referência (como uma classe)**, mas com **semântica de valor**, ou seja, ele **compara o conteúdo** e **não a referência**:
- **Classe normal**: compara se é o mesmo objeto (referência)
- **Record**: compara se o conteúdo (propriedades) é igual

🔹 O `record` já vem com `Equals()`, `GetHashCode()` e `==` **sobrescritos automaticamente**.<br>
Não precisa escrever nada disso manualmente — o compilador gera tudo pra você.

🔹 Quando usar `record`:
- Você quer representar **dados imutáveis** (DTOs, mensagens, resultados de operações, etc.)
- Precisa de **comparação por valor** automaticamente.
- Quer **copiar objetos com pequenas variações** (`with`).
- Está modelando **entidades sem identidade própria**, como `Endereco`, `ProdutoDTO`, `PagamentoRequest`, etc.

🔹 Quando não usar:
- Você precisa de **mutabilidade intensa** (muitos `set`).
- A **identidade do objeto** importa (ex: entidade de banco de dados com `Id` único).
- Você precisa de **controle total sobre a herança** e comportamento interno.

## 🧩 Exemplo prático

Classe comum:
```csharp
class Pessoa
{
    public string Nome { get; set; }
    public int Idade { get; set; }
}

var p1 = new Pessoa { Nome = "Guilherme", Idade = 30 };
var p2 = new Pessoa { Nome = "Guilherme", Idade = 30 };

Console.WriteLine(p1 == p2);      // ❌ false (referência diferente)
Console.WriteLine(p1.Equals(p2)); // ❌ false
```

Record:
```csharp
record Pessoa(string Nome, int Idade);

var p1 = new Pessoa("Guilherme", 30);
var p2 = new Pessoa("Guilherme", 30);

Console.WriteLine(p1 == p2);      // ✅ true (mesmo conteúdo)
Console.WriteLine(p1.Equals(p2)); // ✅ true
```
Exemplo com `record struct`:
```csharp
record struct Coordenada(int X, int Y);

var c1 = new Coordenada(10, 20);
var c2 = new Coordenada(10, 20);

Console.WriteLine(c1 == c2); // ✅ true (valor igual)
```

Você também pode criar um `record` com corpo completo:
```csharp
public record Produto
{
    public string Nome { get; init; }
    public decimal Preco { get; init; }

    public Produto(string nome, decimal preco)
    {
        Nome = nome;
        Preco = preco;
    }
}
```
O `init` deixa as propriedades imutáveis após a construção:
```csharp
var p = new Produto("Notebook", 5000);
p.Nome = "Outro"; // ❌ Erro — não pode alterar depois de criado
```

## ⚙️ Como ele funciona internamente

Quando você cria um `record`, o compilador gera automaticamente:

- `Equals()` → compara todas as propriedades;
- `GetHashCode()` → baseado nas propriedades;
- `ToString()` → imprime o nome e os valores das propriedades;
- `Deconstruct()` → permite desconstrução;
- `with` → cria cópias imutáveis com mudanças parciais.

## ⚙️ Exemplo de imutabilidade e `with`
```csharp
var pessoa1 = new Pessoa("Guilherme", 30);

// Cria uma cópia com Idade alterada
var pessoa2 = pessoa1 with { Idade = 31 };

Console.WriteLine(pessoa1); // Pessoa { Nome = Guilherme, Idade = 30 }
Console.WriteLine(pessoa2); // Pessoa { Nome = Guilherme, Idade = 31 }
```
O `with` **não altera o objeto original**, ele cria **um novo com as mudanças**.
Isso é ótimo para programação funcional e segurança de dados.