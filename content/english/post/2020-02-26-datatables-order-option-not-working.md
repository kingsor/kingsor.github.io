---
date: "2020-02-26"
summary: When DataTables order option seems not to work
categories:
- developer
tags:
- lessons-learned
title: DataTables Order option not working
---

[Default ordering (sorting)](https://datatables.net/examples/basic_init/table_sorting.html)
Using this parameter you can define which column(s) the order is performed upon, and the ordering direction.

```javascript
$(document).ready(function() {
    $('#example').DataTable( {
        "order": [[ 3, "desc" ]]
    } );
} );
```

Ordering a table by its fourth column in descending order.

I applied this option to the init of my table but is seemed not to work.

Then I found these comments in the [order](https://datatables.net/reference/option/order) option reference:

> If, like me and many other people who have posted on various forums, you are using server side processing but the ordering of your columns is not following your configuration directives, be aware that datatables uses local storage to cache the order directive such that changing the config value will initially have no effect. To see your changes you first need to clear your browser's local storage for the domain being used. (by [losttheplot](https://datatables.net/forums/profile/losttheplot))
> 
> This is true if you have enabled state saving ([stateSave](https://datatables.net/reference/option/stateSave) option). With state saving enabled, the saved state will take priority over the initialisation options (since otherwise the save state wouldn't be restored!). Important point though - thanks for making it. (by [allan](https://datatables.net/forums/profile/allan))
> 

The table I was adding the order option to had also the `stateSave` option. So, after clearing the browser's local storage, the `order` option started working fine.
