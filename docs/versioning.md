# Versioning

This document describes how to version prompts, agents, skills, and workflows.

---

## 1. Version Format

We use **simplified semantic versioning** — `<major>.<minor>` — embedded in both the **filename** and the **metadata header**.

| Component | Increment When |
|---|---|
| Major (`v2`, `v3`) | Breaking change — different behaviour, new required variables, or incompatible output format |
| Minor (`v1.1`, `v1.2`) | Backward-compatible improvement — better wording, new optional variable, minor fix |

---

## 2. Version in Filenames

Include the version as the last segment of the file name before the extension:

```
customer-support-handle-refund-v1.md       # Major version 1
customer-support-handle-refund-v1.1.md     # Minor update to v1
customer-support-handle-refund-v2.md       # Breaking change, new major version
```

Keep **all versions** in the repository — do not overwrite old versions. Mark superseded versions as `status: deprecated` in their metadata.

---

## 3. Version in Metadata

```yaml
version: "1.1"
status: production    # Change to deprecated when superseded
```

---

## 4. When to Create a New Version

**Create a new major version (`v2`) when:**
- The prompt's required input variables change (added, removed, or renamed).
- The output format changes in a way that breaks existing consumers.
- The fundamental approach or persona changes significantly.

**Create a new minor version (`v1.1`) when:**
- You improve wording without changing behaviour.
- You add an optional variable with a sensible default.
- You fix a minor bug (e.g., correcting a typo in an instruction).

**Edit in-place (no version bump) for:**
- Fixing typos in comments or notes sections.
- Updating metadata fields only (e.g., adding a tag).

---

## 5. Deprecating a Version

When a new version supersedes an old one:

1. Update the old file's metadata:
   ```yaml
   status: deprecated
   superseded_by: customer-support-handle-refund-v2
   ```
2. Do **not** delete the old file — keep it for reference and backward compatibility.
3. Log the deprecation in [`changelog/CHANGELOG.md`](../changelog/CHANGELOG.md).

---

## 6. Agent, Skill, and Workflow Versioning

The same rules apply. Version is tracked in the `version` field of the YAML config:

```yaml
agent:
  id: customer-support-agent
  version: "1.1"
```

For agents and workflows, create a new config file (e.g., `agent-config-v2.yaml`) for major versions and update `agent-config.yaml` to point at the latest.
