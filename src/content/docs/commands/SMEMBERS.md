---
title: SMEMBERS
description: Documentation for the DiceDB command SMEMBERS
---
<!-- description in 2 to 3 sentences, following is an example -->
The `SMEMBERS` command in DiceDB is used to retrieve all the members of a set stored at a specified key. Sets in DiceDB are unordered collections of unique strings. This command is useful for obtaining the entire set of elements for further processing or inspection.

## Syntax

```bash
SMEMBERS key
```
<!-- If the command have subcommands please mention but do not consider them as arguments -->
<!-- please mention them in subcommands section and create their individual documents -->

## Parameters
<!-- please add all parameters, small description, type and required, see example for SET command-->
| Parameter | Description                                                               | Type    | Required |
|-----------|---------------------------------------------------------------------------|---------|----------|
| `key`     |  The key of the set from which you want to retrieve all members. This parameter is required and must be a valid string.                                            | String  | Yes      |


## Return values
<!-- add all scenarios, see below example for SET -->

| Condition                                      | Return Value                                      |
|------------------------------------------------|---------------------------------------------------|
| if key exists                 | array of all the members in the set stored at the specified key                                             |
| if key does not exist       | empty array                                            |


## Behaviour
<!-- How does the command execute goes here, kind of explaining the underlying algorithm -->
<!-- see below example for SET command -->
<!-- Please modify for the command by going through the code -->
1. DiceDB checks if the key exists.
1. If the key exists and is of type set, DiceDB retrieves all the members of the set.
1. If the key does not exist, DiceDB returns an empty array.
1. If the key exists but is not of type set, an error is returned.

## Errors
<!-- sample errors, please update for commands-->
<!-- please add all the errors here -->
<!-- incase of a dynamic error message, feel free to use variable names -->

1. `Wrong type of value or key`:

   - Error Message: `(error) WRONGTYPE Operation against a key holding the wrong kind of value`
   - Occurs when attempting to use the command on a key that contains a non-string value.


## Example Usage

### Retrieving Members from an Existing Set
<!-- examples here are for set, please update them for the command -->

```bash
127.0.0.1:7379> SADD myset "apple" "banana" "cherry"
127.0.0.1:7379> SMEMBERS myset
1) "apple"
2) "banana"
3) "cherry"
```
<!-- Please use detailed scenarios and edges cases if possible -->
###  Retrieving Members from a Non-Existent Set



```bash
127.0.0.1:7379> SMEMBERS nonexistentset
(empty array)
```

### Error Case - Key Exists but is Not a Set

```bash
127.0.0.1:7379> SET foo bar PX 10000
OK
```

### Setting only if key does not exist

Setting a key `foo` only if it does not already exist

```bash
127.0.0.1:7379> SET mystring "hello"
127.0.0.1:7379> SMEMBERS mystring
(error) WRONGTYPE Operation against a key holding the wrong kind of value
```

<!-- Optional -->
## Best Practices
<!-- below example from Keys command -->
- Always ensure that the key you are querying with `SMEMBERS` is of type set to avoid type errors.
- Use `EXISTS` command to check if a key exists before using `SMEMBERS` if you are unsure about the key's existence.
- For very large sets, prefer using `SSCAN` to avoid performance issues.

  
<!-- Optional -->
## Alternatives
<!-- below example from keys command -->
- `SCAN`: The `SCAN` command is a cursor-based iterator that allows you to incrementally iterate over the keyspace without blocking the server. It is a more efficient alternative to `KEYS` for large datasets.

<!-- Optional -->
## Notes
<!-- below example from json.get command -->
- The order of elements returned by `SMEMBERS` is not guaranteed to be consistent. Sets in DiceDB are unordered collections, so the order of elements may vary.
- For large sets, consider using the `SSCAN` command to iterate over the set incrementally to avoid blocking the DiceDB server for a long time.
- 
By following this documentation, you should be able to effectively use the `SMEMBERS` command in DiceDB to retrieve all members of a set.

