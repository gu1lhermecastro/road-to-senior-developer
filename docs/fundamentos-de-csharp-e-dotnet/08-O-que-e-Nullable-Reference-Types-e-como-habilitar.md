# 8. O que é _Nullable Reference Types_ e como habilitar?
🔹 Antes do C# 8, **qualquer tipo de referência** (ex: `string`, `object`, `Pessoa`, etc.) podia ser **nulo**, e o compilador **não avisava** quando você esquecia de verificar null:
```csharp
string nome = null;
Console.WriteLine(nome.Length); // 💥 NullReferenceException
```
A partir do **C# 8**, a Microsoft introduziu o recurso **Nullable Reference Types (NRT)** — ele **não muda como o CLR funciona**, mas **muda como o compilador analisa o seu código**.

```csharp
#nullable enable

string nome = null; // ⚠️ Warning: possível atribuição nula
string? sobrenome = null; // Ok

Console.WriteLine(sobrenome.Length); // ⚠️ Warning: possível referência nula
```

## ⚙️ Como habilitar o recurso

Há **três formas** de habilitar o NRT:

### 1. Diretiva no topo do arquivo:
```csharp
#nullable enable
```
- Ativa apenas para **aquele arquivo**.
- Você também pode usar:
  - `#nullable disable` — desativa
  - `#nullable restore` — volta à configuração padrão do projeto
  - `#nullable annotations` e `#nullable warnings` — controle mais granular

### 2. No arquivo .csproj (global no projeto):
```csharp
<PropertyGroup>
  <Nullable>enable</Nullable>
</PropertyGroup>
```
Recomendado — aplica para **todo o código** do projeto.

### 3. No Visual Studio (interface):
- Vá em **Project → Properties → Build → Nullable**
- Selecione **Enable** ou **Warnings**.