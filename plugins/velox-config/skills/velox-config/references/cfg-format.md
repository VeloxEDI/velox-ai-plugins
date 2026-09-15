# Velox CFG format reference

## Purpose

Velox configuration exports use Delphi text-stream syntax. The file is plain text and is intended to be imported by Velox as configuration, not parsed as JSON, XML, YAML, or INI.

## Structural pattern

A typical export starts with a root streamed object such as:

```text
object TvxConfigTemplate
  FIDS = '{...}'
  ModuleName = '...'
  ...
  object SomeInstance: TSomeClass
    Property = Value
    ...
  end
end
```

Nested streamed objects represent Velox configuration objects and their contained definitions.

## Common value forms

### Strings

```text
ModuleName = 'Order'
```

Escape embedded Delphi string delimiters using normal Delphi text-stream conventions. Long strings may be split and concatenated across lines.

### GUID/FIDS

```text
FIDS = '{56939333-EF81-4C93-8BE0-16D131C75E1E}'
```

Use braces and canonical hyphenated GUID text. Do not reuse an unrelated object's identity when generating a genuinely new object unless Velox semantics specifically require a known sentinel/reference FIDS.

### Booleans

```text
Active = True
```

### Enums

```text
ScheduleType = fstMonitor
ActionType = fatMap
```

Do not quote enum tokens.

### Sets

```text
WeekDays = [fdMonday, fdTuesday, fdWednesday, fdThursday, fdFriday]
```

### String lists / script source

```text
Formula.Strings = (
  'procedure ScriptEvent(var Value: variant);'
  'begin'
  '  ...'
  'end;')
```

These are Delphi streamed string-list entries. Preserve valid quoting and line boundaries.

### Long Delphi strings and character literals

Exports may split long strings with `+` and may include character codes such as `#13#10`.

```text
BlankXML =
  '<?xml version="1.0"?>'#13#10'<Message ...' +
  '...'
```

Preserve this syntax when producing long streamed string values.

### Dates/times

Exported values can use Delphi floating-point date/time representation, for example:

```text
UpdatedDate = 46171.56189511574000000
StartTime = 0.00069444444444444
```

Only emit these properties when supported by the relevant object/pattern; do not fabricate timestamps just to make the file resemble an export.

## Generation guidance

- Prefer patterns verified in `VeloxEDI/velox-ai-info` or an existing customer config.
- Preserve object/class/property names exactly as supported by Velox.
- Keep customer repository access read-only.
- Generate a complete `.CFG` file for import rather than GitHub changes.
- Use fresh object identities for new independently-created objects where required.
- Preserve required cross-references between objects consistently throughout the file.
- Do not assume the bundled example's connection FIDS, folder FIDS, schemas, database definitions, or scripts apply to another customer.

## Bundled example

`example-order-import.cfg` is a real example supplied by the user. It imports a Velox Order XML file into the VxData database and is included to demonstrate real exported stream syntax and object relationships.

It is a structural example, not a universal template.
