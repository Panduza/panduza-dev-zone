# Interface Structure

## Topics

All the interfaces support those common topics.

| Topic                                    | QOS | Retain |
| :--------------------------------------- | :-: | :----: |
| \<interface>/atts/info             |  0  | false  |
| \<interface>/atts/\<name> |  0  |  true  |
| \<interface>/cmds/set              |  0  | false  |

Interfaces may have as many \<name> attributes as they want.

## Attributes

Attributes are topics published by drivers to clients.
They serve as information that something changed or happened.

To be notified of such events, a client must subscribe to those attributes.

Each attribute have a name and some fields depending on the interface.

An attribute is represented as a JSON payload, containing its name and an object containing all the fields.

```json
{
    "attribute_name": {
        "field_1": "value",
        "field_2": 0,
        "field_3": { "sub-obj": 0, "sub-obj2": 5 }
    }
}
```

If the attribute payload has only one field, it can be simplified.

```json
{
    "attribute_name": "hello"
}
```

Note: Each field supported by an attribute **must** be present in the payload.
This means an attribute payload is always determinist and free-standing.

### Special attribute 'info'

This attribute is special and must be supported in read-only by all the interfaces.

It is special because it must not be retained.

When '*' is published inside the topic 'pza' then this attribute must be published.


## Commands

In order to pilot an interface, the client must send orders to the driver.

This is possible through commands.

In most cases, commands and attributes are linked entities. They act like read/write operations.
**Meaning a command always has a corresponding attribute.**

**Note: an attribute may be read-only, and therefore has no corresponding command.**

Each supported commands are united through one unique message with the following topic:

`<interface>/cmds/set`

In this topic, the user can send the commands he wants, with the fields he wants:

```json
{
    "command_1": {
        "field_1": "do the change dude !",
        "field_2": "with this setting",
        "field_3": "and why not this one too"
    },
    "command_2": "Do this other command which has only 1 field"
}
```

Since there is only one topic for all commands, **the payload can be partial**.

You can send all commands with all there fields, like in the JSON above.
You can also send only 1 command, with only 1 of its fields.

```json
{
    "command_1": {
        "field_3": "I want to change only field 3 of command 1"
    }
}
```