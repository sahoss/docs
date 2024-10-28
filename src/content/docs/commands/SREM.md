---
title: SREM
description: Documentation for the DiceDB command SREM
---
<!-- description in 2 to 3 sentences, following is an example -->
The `SREM` command is used to remove one or more members from a set stored at a specified key. If the specified members are not present in the set, they are ignored. If the key does not exist, it is treated as an empty set and the command returns 0. This command is idempotent, meaning that removing a member that does not exist in the set has no effect.

## Syntax

```bash
SREM key member [member ...]
```
<!-- If the command have subcommands please mention but do not consider them as arguments -->
<!-- please mention them in subcommands section and create their individual documents -->

## Parameters
<!-- please add all parameters, small description, type and required, see example for SET command-->
| Parameter | Description                                                               | Type    | Required |
|-----------|---------------------------------------------------------------------------|---------|----------|
| `key`     |The key of the set from which the members will be removed. This key must be of the set data type.                                            | String  | Yes      |
| `member`   | One or more members to be removed from the set. Multiple members can be specified, separated by spaces.                                          | String  | Yes      |

## Return values
<!-- add all scenarios, see below example for SET -->

| Condition                                      | Return Value                                      |
|------------------------------------------------|---------------------------------------------------|
| The number of members that were removed from the set, not including non-existing members.                   | `Integer reply`                                              |


## Behaviour
<!-- How does the command execute goes here, kind of explaining the underlying algorithm -->
<!-- see below example for SET command -->
<!-- Please modify for the command by going through the code -->
- DiceDB checks if the key exists.
- If the key does not exist, it is treated as an empty set, and the command returns 0.
- If the key exists but is not of the set data type, an error is returned.
- DiceDB attempts to remove the specified members from the set.
- The command returns the number of members that were successfully removed.


## Errors
<!-- sample errors, please update for commands-->
<!-- please add all the errors here -->
<!-- incase of a dynamic error message, feel free to use variable names -->

1. `Wrong type of value or key`:

   - Error Message: `(error) WRONGTYPE Operation against a key holding the wrong kind of value`
   - This error is returned if the key exists but is not of the set data type.

2. `Invalid syntax`:

   - Error Message: `(error) ERR syntax error`
   - This error is returned if the command is not used with the correct number of arguments.

## Example Usage

### Basic Usage
<!-- examples here are for set, please update them for the command -->

 The member "two" is removed from the set `myset`. The command returns 1 because one member was removed.

```bash
127.0.0.1:7379> SADD myset "one" "two" "three"
127.0.0.1:7379> SREM myset "two"
(integer) 1
```
<!-- Please use detailed scenarios and edges cases if possible -->
### Removing multiple members from a set

The members "two" and "three" are removed from the set `myset`. The command returns 2 because two members were removed.

```bash
127.0.0.1:7379> SADD myset "one" "two" "three"
127.0.0.1:7379> SREM myset "two" "three"
(integer) 2
```

### Removing a non-existing member from a set

The member "four" does not exist in the set `myset`. The command returns 0 because no members were removed.

```bash
127.0.0.1:7379> SADD myset "one" "two" "three"
127.0.0.1:7379> SREM myset "four"
(integer) 0
```

### Removing members from a non-existing set

The set `myset` does not exist. The command returns 0 because no members were removed.

```bash
127.0.0.1:7379> SET mykey "value"
127.0.0.1:7379> SREM mykey "one"
(error) WRONGTYPE Operation against a key holding the wrong kind of value

```

### Invalid usage

Trying to set key `foo` with both `EX` and `KEEPTTL` will result in an error

```bash
127.0.0.1:7379> SET foo bar EX 10 KEEPTTL
(error) ERR syntax error
```
<!-- Optional: Used when additional information is to conveyed to users -->
<!-- For example warnings about usage ex: Keys * -->
<!-- OR alternatives of the commands -->
<!-- Or perhaps deprecation warning -->
<!-- anything related to the command which cannot be shared in other sections -->


<!-- Optional -->
## Notes
<!-- below example from json.get command -->
- The SREM command is idempotent. Removing a member that does not exist in the set has no effect and does not produce an error.

- The command can be used to remove multiple members in a single call, which can be more efficient than calling SREM multiple times for individual members.

- If the set becomes empty after the removal of members, the key is automatically deleted from the database.


