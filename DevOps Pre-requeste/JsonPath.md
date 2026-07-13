# JSONPath — Short Notes

## What is JSONPath?

JSONPath is a query language used to select and extract data from JSON documents. It is similar to how SQL selects data from database tables.

## JSON Structure

* `{}` represents an **object/dictionary** containing key-value pairs.
* `[]` represents an **array/list** containing multiple elements.
* Array indexes start from `0`.

Example:

```json
{
  "car": {
    "color": "blue",
    "price": "$20,000"
  }
}
```

## Important JSONPath Symbols

| Symbol  | Meaning                                   | Example           |
| ------- | ----------------------------------------- | ----------------- |
| `$`     | Root of the JSON document                 | `$.car`           |
| `.`     | Access a child property                   | `$.car.price`     |
| `[n]`   | Select an array element by index          | `$.wheels[0]`     |
| `[a,b]` | Select multiple array elements            | `$[0,3]`          |
| `*`     | Select all properties/elements            | `$.car.wheels[*]` |
| `[?()]` | Filter array elements                     | `$[?(@ > 40)]`    |
| `@`     | Current element being checked by a filter | `@.location`      |

## Common Queries

### Select a property

```text
$.property1
```

### Select a nested property

```text
$.car.price
```

### Select an array element

```text
$.car.wheels[2]
```

This selects the third wheel because indexing starts at `0`.

### Select a property from an array element

```text
$.car.wheels[2].model
```

### Select multiple array elements

For this root array:

```json
["car", "bus", "truck", "bike"]
```

Use:

```text
$[0,3]
```

Result:

```json
[
  "car",
  "bike"
]
```

### Select values from every array element

```text
$.car.wheels[*].model
```

### Filter objects by a property

```text
$.car.wheels[?(@.location == "rear-right")].model
```

This checks each wheel and returns the model whose location is `rear-right`.

### Filter numbers

```text
$[?(@ > 40)]
```

Common comparison operators:

| Operator | Meaning                  |
| -------- | ------------------------ |
| `==`     | Equal to                 |
| `!=`     | Not equal to             |
| `>`      | Greater than             |
| `<`      | Less than                |
| `>=`     | Greater than or equal to |
| `<=`     | Less than or equal to    |

## Using JSONPath with `jpath`

General command:

```bash
cat file.json | jpath '<jsonpath-query>'
```

Examples:

```bash
cat q1.json | jpath '$.property1'
cat q7.json | jpath '$.car.wheels[2].model'
cat q13.json | jpath '$[0,3]'
```

The KodeKloud `jpath` command displays matches inside an array, even when only one value matches.

## Saving a Query in a Shell Script

When running a query directly, keep it inside single quotes so Bash does not expand `$`:

```bash
cat q13.json | jpath '$[0,3]'
```

When this whole command is inside double quotes, escape `$`:

```bash
echo "cat q13.json | jpath '\$[0,3]'" > /root/answer13.sh
```

Without the backslash, Bash interprets `$[0,3]` as arithmetic and changes it to `3`.

Test the script:

```bash
bash /root/answer13.sh
```

## Quick Method for Solving Questions

1. Identify whether the root is an object `{}` or array `[]`.
2. Start the query with `$`.
3. Follow object keys using `.key`.
4. Select array items using `[index]`, `[a,b]`, or `[*]`.
5. Use `[?()]` when an item should be selected by its value instead of its position.
6. Add the final property you want, such as `.model` or `.price`.

> Note: Some advanced JSONPath features can differ slightly between tools. Test the query using the same implementation used by your lab.

## Reference

* [KodeKloud: JSON and JSONPath lesson](https://notes.kodekloud.com/docs/DevOps-Pre-Requisite-Course/General-Pre-Requisites/JSON-JSON-Path/page)
