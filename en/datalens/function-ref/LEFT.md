---
editable: false
sourcePath: en/_api-ref/datalens/function-ref/LEFT.md
---

# LEFT



#### Syntax {#syntax}


```
LEFT( string, number )
```

#### Description {#description}
Returns a string that contains the number of characters specified in `number` from the beginning of the string `string`.

**Argument types:**
- `string` — `String`
- `number` — `Integer`


**Return type**: `String`

#### Example {#examples}

```
LEFT("Computer", 4) = "Comp"
```


#### Data source support {#data-source-support}

`ClickHouse 21.8`, `Files`, `Google Sheets`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.7`, `Oracle Database 12c (12.1)`, `PostgreSQL 9.3`, `Yandex Documents`, `YDB`.
