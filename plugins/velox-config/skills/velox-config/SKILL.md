---
name: velox-config
description: Create, explain, review, and troubleshoot Velox configuration. Use for questions about Velox Flows, actions, definitions, scripts, mappings, schedules, or customer-specific configuration, and for generating importable Velox .CFG files. Always use VeloxEDI/velox-ai-info as the shared Velox knowledge source when GitHub is available. Optionally use one explicitly designated customer configuration repository as read-only context. Never modify GitHub; generated configuration must be returned as new .CFG file(s) for the user to import into Velox.
---

# Velox Config

Use this skill to understand existing Velox configuration and create new importable Velox configuration.

## Core rules

1. Treat `VeloxEDI/velox-ai-info` as the shared source of truth for current Velox documentation, conventions, Code Library functions, and examples. Search it when the connected GitHub app is available and the task depends on Velox-specific behaviour.
2. A customer configuration repository is optional. If the user designates one, use only that repository for customer-specific configuration context.
3. Treat every customer repository as read-only. Never create, update, commit, push, branch, open a PR, or otherwise modify it.
4. Never search or use another customer's repository while working in a customer context.
5. If no customer repository is available, create configuration from the user's requirements plus shared Velox knowledge and the bundled CFG format reference.
6. Return newly created configuration as one or more `.CFG` files for the user to import into Velox. Do not present a GitHub write as the delivery mechanism.
7. Do not invent unsupported Velox properties, classes, enums, scripting functions, or import semantics. Verify against `velox-ai-info`, the customer's existing config, or the bundled example before using them.

## Workflow

### Understand or explain existing configuration

1. Identify the customer repository if one is explicitly designated.
2. Search the customer repository for the relevant Flow, action, module, script, definition, FIDS, endpoint, field name, or other concrete identifier.
3. Search `VeloxEDI/velox-ai-info` for the Velox concepts/functions needed to interpret what was found.
4. Explain the configuration using customer evidence for "what this customer does" and shared Velox knowledge for "how Velox works".
5. Call out uncertainty when the repository does not contain enough evidence.

### Create new configuration

1. Determine the requested integration behaviour, inputs, outputs, scheduling/triggering, mappings, scripts, connections, and error/logging requirements from the conversation.
2. Search `VeloxEDI/velox-ai-info` for relevant implementation guidance and examples.
3. If a customer repository is designated, inspect similar existing customer configuration and preserve compatible naming, folder structure, conventions, connection references, and reusable patterns where appropriate.
4. Build a new standalone Delphi text-stream `.CFG` import file. Use new identifiers where required rather than silently reusing unrelated customer object identities.
5. Validate the generated text structurally against `references/cfg-format.md` and, where useful, `references/example-order-import.cfg`.
6. Deliver the `.CFG` file to the user with a short description of what it creates and any assumptions that materially affect import or runtime behaviour.

## CFG generation rules

Read `references/cfg-format.md` before generating or materially editing a `.CFG` file.

Use `references/example-order-import.cfg` only as a structural/reference example. Do not blindly copy customer-specific names, GUIDs, folders, connections, schemas, mappings, scripts, or version metadata from it.

A Velox `.CFG` file is Delphi text-stream syntax, not JSON/XML/YAML. Preserve Delphi streaming syntax exactly, including:

- `object <instance>: <class>` / `end` nesting
- property assignment with `=`
- quoted Delphi strings
- multiline `.Strings = (` lists
- sets such as `[fdMonday, fdTuesday]`
- enum values such as `fatMap`, `fstMonitor`, or `feUTF8`
- floating-point date/time values where required by the exported format
- GUID/FIDS values in braces
- Delphi string concatenation and character literals such as `#13#10` when present

Do not convert streamed values into another serialization format.

## Customer isolation

When a customer repository is in scope, keep the effective knowledge boundary to:

- `VeloxEDI/velox-ai-info`
- the explicitly designated customer repository
- files the user directly provides in the current customer context

Do not use other accessible private repositories as examples unless the user explicitly asks for a cross-customer comparison and confirms that use is appropriate.

## GitHub behaviour

Use GitHub only for retrieval/search/read operations in this skill. Even if write-capable GitHub tools are available, do not invoke them for Velox configuration changes.

If the user asks to create configuration, create the `.CFG` artifact in the conversation instead.

## Output quality

For explanations, be concrete: name the relevant Flow/actions/definitions/scripts and describe the data path in execution order.

For generated configuration, prefer a complete importable `.CFG` artifact over disconnected snippets. State assumptions separately rather than embedding uncertainty silently into the configuration.
