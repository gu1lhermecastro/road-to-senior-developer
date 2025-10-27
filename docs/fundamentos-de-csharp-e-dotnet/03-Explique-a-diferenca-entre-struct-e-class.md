## 3. Explique a diferença entre `struct` e `class`

`struct` → **Value Type**

- O valor é **armazenado diretamente** (não há ponteiro ou referência).
- Atribuir uma `struct` a outra **duplica** os dados.
- `struct` → **não pode herdar**, mas **pode implementar interfaces**.
- `struct` é **excelente para dados pequenos e imutáveis**.

`class` → **Reference Type**

- O valor real está na **heap**.
- A variável armazena **apenas uma referência** (ponteiro) para o objeto.
- Atribuir uma `class` a outra **copia a referência**, não o conteúdo.
- `class` → **pode herdar** de outra classe, usar **virtual/override**, **abstrações**, etc.
- `class` é **melhor para entidades complexas e mutáveis**.

### Exemplo prático comparando ambos:
```csharp
struct Coordenada
{
    public int X, Y;
}

class Pessoa
{
    public string Nome;
}
```

```csharp
var c1 = new Coordenada { X = 1, Y = 2 };
var c2 = c1;
c2.X = 99;
Console.WriteLine(c1.X); // 1

var p1 = new Pessoa { Nome = "Gui" };
var p2 = p1;
p2.Nome = "Maria";
Console.WriteLine(p1.Nome); // "Maria"
```

⚠️ Importante:

- Uma `struct` **pode estar no heap** se fizer parte de um objeto (por exemplo, como campo de uma `class`).
- O que define stack/heap não é apenas o tipo, mas **onde o valor é usado**.