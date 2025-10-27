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