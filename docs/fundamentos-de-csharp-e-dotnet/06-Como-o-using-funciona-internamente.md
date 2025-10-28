# 6. Como o `using` funciona internamente?

🔹 O `using` serve para **garantir que objetos que consomem recursos não gerenciados** (como arquivos, conexões de banco, streams, sockets etc.) sejam **liberados automaticamente**, mesmo que ocorra uma exceção.

🔹 Ele garante que o `Dispose()` **será chamado** no final do bloco — com ou sem exceção.

🔹 Nem todos os recursos que seu código usa são “gerenciados” pelo CLR.
Alguns são **recursos externos**, como:

- Arquivos no sistema operacional;
- Conexões de rede;
- Ponteiros nativos;
- Handles de janelas (UI);
- Conexões de banco de dados.

Esses recursos não são liberados pelo Garbage Collector, então **você precisa liberar manualmente** — e o `using` faz isso de forma segura.

Ele funciona **em cima da interface** `IDisposable`, que define o método:

```csharp
public interface IDisposable
{
    void Dispose();
}
```

## 🧩 Exemplo prático COM `using`
Vamos criar uma classe `Relatorio` que abre um arquivo e precisa fechá-lo corretamente:
```csharp
using System;
using System.IO;

class Relatorio : IDisposable
{
    private StreamWriter _arquivo;

    public Relatorio(string caminho)
    {
        _arquivo = new StreamWriter(caminho);
        Console.WriteLine("Arquivo aberto.");
    }

    public void Escrever(string texto)
    {
        _arquivo.WriteLine(texto);
    }

    // Método obrigatório do IDisposable
    public void Dispose()
    {
        // Libera o recurso gerenciado
        if (_arquivo != null)
        {
            _arquivo.Close();
            _arquivo = null;
        }

        Console.WriteLine("Arquivo fechado (Dispose chamado).");
    }
}
```
E o uso:
```csharp
using (var relatorio = new Relatorio("dados.txt"))
{
    relatorio.Escrever("Linha 1");
}
```
Saída:
```csharp
Arquivo aberto.
Arquivo fechado (Dispose chamado).
```

## 🧩 Exemplo prático SEM `using`
```csharp
void SalvarDados()
{
    var arquivo = new StreamWriter("dados.txt");
    arquivo.WriteLine("Teste");
    // Esqueceu o Dispose()!
}
```
#### 🔴 Problemas:
- O arquivo pode continuar bloqueado (outros processos não conseguem acessá-lo);
- Pode causar vazamento de handle de arquivo;
- O conteúdo pode não ser gravado completamente (por causa do buffer).
#### ✅ Solução:
```csharp
using (var arquivo = new StreamWriter("dados.txt"))
{
    arquivo.WriteLine("Teste");
}
```

## ⚙️ O que acontece internamente
Quando você escreve:
```csharp
using (var arquivo = new StreamWriter("dados.txt"))
{
    arquivo.WriteLine("Olá, mundo!");
}
```
O compilador **traduz automaticamente** isso para algo como:
```csharp
StreamWriter arquivo = new StreamWriter("dados.txt");
try
{
    arquivo.WriteLine("Olá, mundo!");
}
finally
{
    if (arquivo != null)
        ((IDisposable)arquivo).Dispose();
}
```
## ⚙️ 6. E se o objeto tiver um `DisposeAsync()`?
Com o **C# 8**, surgiu o `IAsyncDisposable`, usado com `await using`:
```csharp
await using var conn = new SqlConnection(connectionString);
await conn.OpenAsync();
// ...
```
Aqui, o compilador gera um código que chama `await conn.DisposeAsync()`, **permitindo a liberação assíncrona** de recursos (como conexões ou streams de rede).