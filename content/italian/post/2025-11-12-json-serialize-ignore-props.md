---
date: "2025-11-12"
title: JSON Serialize ignorare proprietà a runtime
summary: Come escludere a runtime una o più proprietà dall'istanza di una classe serializzata in formato json
categories:
- developer
tags:
- lessons-learned
---

In questo post descrivo come escludere a runtime una o più proprietà di un oggetto dalla serializzazione in formato json utilizzando la libreria `System.Text.Json`.

Consideriamo un oggetto di tipo Book così definito:

```csharp

public class Book
{
    public int Id { get; set; }
    public string? Title { get; set; }
    public int? Pages { get; set; }
    public string? Author { get; set; }
    public string Language { get; set; }
    public int PubYear { get; set; }
}

```

Questo il codice per serializzare in formato json un'istanza della classe Book:

```csharp

var book = new Book
{
    Id = 1,
    Title = "Pensieri lenti e veloci",
    Pages = 684,
    Author = "Daniel Kahneman",
    Language = "Italiano",
    PubYear = 2020
};

var json = JsonSerializer.Serialize(book,
    new JsonSerializerOptions { WriteIndented = true }
);

Console.WriteLine(json);

```

Questo sarà il risultato dell'oggetto in formato json.

```json
{
    "Id": 1,
    "Title": "Pensieri lenti e veloci",
    "Pages": 684,
    "Author": "Daniel Kahneman",
    "Language": "Italiano",
    "PubYear": 2020
}
```



Se si vuole ignorare una proprietà di un oggetto dalla serializzazione si può utilizzare l'attributo `[JsonIgnore]` su ogni proprietà da escludere.

```csharp

public class Book
{
    [JsonIgnore]
    public int Id { get; set; }
    public string? Title { get; set; }
    public int? Pages { get; set; }
    public string? Author { get; set; }
    public string Language { get; set; }
    public int PubYear { get; set; }
}

```

In questo modo ogni volta che un oggetto di quel tipo verrà serializzato, il contenuto della proprietà con attributo `[JsonIgnore]` non verrà incluso nel risultato.


```csharp

var book = new Book
{
    Id = 1,
    Title = "Pensieri lenti e veloci",
    Pages = 684,
    Author = "Daniel Kahneman",
    Language = "Italiano",
    PubYear = 2020
};

var json = JsonSerializer.Serialize(book,
    new JsonSerializerOptions { WriteIndented = true }
);

Console.WriteLine(json);

```

Questo sarà il risultato dell'oggetto in formato json con la proprietà `Id` che è stata ignorata dalla serializzazione.

```json
{
    "Title": "Pensieri lenti e veloci",
    "Pages": 684,
    "Author": "Daniel Kahneman",
    "Language": "Italiano",
    "PubYear": 2020
}
```


L'utilizzo dell'attributo `[JsonIgnore]` è un'operazione eseguita a design time perchè si conosce a priori quali proprietà andranno serializzate e quali no.

Ma se si vuole ignorare una proprietà a runtime, nonostante la proprietà non abbia l'attributo `[JsonIgnore]` allora è necessario scegliere un percorso diverso.

```csharp

public class JsonIgnoreProperties
{
    private readonly List<string> _ignoredNames = new List<string>();
    private readonly Type _type;

    public JsonIgnoreProperties(Type objType)
    {
        _type = objType;
    }

    public void IgnoreProperty(string name)
    {
        _ignoredNames.Add(name);
    }

    public void ModifyTypeInfo(JsonTypeInfo ti)
    {
        if (ti.Type != _type)
        {
            return;
        }

        JsonPropertyInfo[] props = ti.Properties.Where(
            (pi) => !_ignoredNames.Contains(pi.Name)
        ).ToArray();

        ti.Properties.Clear();

        foreach (var pi in props)
        {
            ti.Properties.Add(pi);
        }
    }
}

```

A questo punto la classe `JsonIgnoreProperties` va instanziata ed utilizzata in questo modo:

```csharp

var book = new Book
{
    Id = 1,
    Title = "Pensieri lenti e veloci",
    Pages = 684,
    Author = "Daniel Kahneman",
    Language = "Italiano",
    PubYear = 2020
};

var modifier = new JsonIgnoreProperties(typeof(Book));
modifier.IgnoreProperty("Language");

JsonSerializerOptions options = new()
{
    TypeInfoResolver = new DefaultJsonTypeInfoResolver()
    {
        Modifiers = { modifier.ModifyTypeInfo }
    },
    WriteIndented = true
};

var json = JsonSerializer.Serialize(book, options);
Console.WriteLine(json);

```

Ecco il risultato in formato json con le seguenti proprietà escluse dalla serializzazione:
- `Id` esclusa grazie all'attributo `[JsonIgnore]`
- `Language` esclusa grazie alla classe `JsonIgnoreProperties`

```json
{
    "Title": "Pensieri lenti e veloci",
    "Pages": 684,
    "Author": "Daniel Kahneman",
    "PubYear": 2020
}
```




