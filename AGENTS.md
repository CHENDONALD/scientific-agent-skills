# Repository Guidance

- Every `SKILL.md` must include a skill version in YAML frontmatter under the Agent Skills spec-compatible `metadata` mapping:
  ```yaml
  metadata:
    version: "1.0"
  ```
- When updating an existing skill, increment its `metadata.version` in the same change. Use quoted numeric strings and advance the visible version sequence (for example, `1.0` to `1.1`). Use a new major version only for a breaking or substantial redesign.
