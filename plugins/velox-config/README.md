# Velox Config Plugin

Velox Config helps users understand, review, troubleshoot, and create Velox configuration.

## Included

- `skills/velox-config/` - the Velox Config skill.
- `.app.json` - references ChatGPT's GitHub connector.
- `.codex-plugin/plugin.json` - native plugin metadata.

## GitHub usage

The GitHub connection is used as a read-only knowledge source by this skill:

- `VeloxEDI/velox-ai-info` supplies shared Velox product knowledge.
- A user may optionally designate one customer configuration repository as customer-specific context.
- The skill must never create commits, branches, pull requests, or modify files in GitHub.
- Newly generated Velox configuration is returned as `.CFG` files for manual import into Velox.

Each user connects their own GitHub account and retains their own repository permissions.
