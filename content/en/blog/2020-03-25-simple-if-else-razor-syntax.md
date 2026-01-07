---
date: "2020-03-25T00:00:00Z"
summary: 'About error: Encountered end tag ''tr'' with no matching start tag'
categories:
- developer
tags:
- lessons-learned
- asp-net
- asp-net-core
title: Simple If/Else Razor Syntax
---

I was looking for solving this error:

`Encountered end tag "tr" with no matching start tag. Are your start/end tags properly balanced?`

and StackOverflow helped me, again.

[Simple If/Else Razor Syntax](https://stackoverflow.com/questions/9710431/simple-if-else-razor-syntax)

I'm trying to do a simple If/Else within a foreach with this code:

```csharp
@{
var count = 0;
    foreach (var item in Model)
    {
        if (count++ % 2 == 0)
        {
            @:<tr class="alt-row">
        } else { 
            @:<tr>
        }
            <td>
                @Html.DisplayFor(modelItem => item.Title)
            </td>
            <td>
                @Html.Truncate(item.Details, 75)
            </td>
            <td>
                @Html.ActionLink("Edit", "Edit", new { id=item.ProjectId }) |
                @Html.ActionLink("Details", "Details", new { id = item.ProjectId }) |
                @Html.ActionLink("Delete", "Delete", new { id=item.ProjectId })
            </td>
        </tr>
    }
}
```

I get a parse error `Encountered end tag "tr" with no matching start tag. Are your start/end tags properly balanced?`. Seems like the if statement doesn't wanna' work.

**Answer**

Just use this for the closing tag:

```csharp
  @:</tr>
```

And leave your if/else as is.

> Seems like the if statement doesn't wanna' work.

It works fine. You're working in 2 language-spaces here, it seems only proper not to split open/close sandwiches over the border.

