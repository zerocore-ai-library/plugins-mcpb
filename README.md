# Plugins MCP Server

Plugin registry search and resolution for AI agents. Search and resolve agents, personas, commands, tools, snippets, and packs from local storage or the plugin registry.

## Setup

### Using tool CLI

Install the CLI from https://github.com/zerocore-ai/tool-cli

```bash
# Install from tool.store
tool install library/plugins
```

```bash
# View available tools
tool info library/plugins
```

```bash
# Search for plugins
tool call library/plugins -m search -p query="code review"
```

```bash
# Search for a specific type
tool call library/plugins -m search -p query="assistant" -p plugin_type=agent
```

```bash
# Resolve a plugin reference
tool call library/plugins -m resolve -p reference="radical/genesis" -p plugin_type=agent
```

## Tools

### `search`

Search the plugin registry for agents, personas, commands, tools, snippets, and packs.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `query` | string | Yes | Search query text (full-text search on name, description, keywords) |
| `plugin_type` | string | No | Filter by type: `agent`, `persona`, `command`, `tool`, `snippet`, `pack` |
| `limit` | integer | No | Max results (1-100, default: 20) |

**Output:**
| Field | Type | Description |
|-------|------|-------------|
| `results` | array | Search results with `namespace`, `name`, `reference`, `description`, `plugin_type`, `latest_version`, `total_downloads`, `star_count` |
| `count` | integer | Number of results returned |

### `resolve`

Resolve a plugin reference to its manifest and content. Supports local plugins and registry fallback.

**Input:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `reference` | string | Yes | Plugin reference: `[namespace/]name[@version]` (e.g., "genesis", "radical/genesis", "radical/genesis@1.0.0") |
| `plugin_type` | string | Yes | Type: `agent`, `persona`, `command`, `tool`, `snippet` |

**Output:**
| Field | Type | Description |
|-------|------|-------------|
| `found` | boolean | Whether the plugin was successfully resolved |
| `namespace` | string | Resolved namespace (null for unnamespaced local plugins) |
| `name` | string | Plugin name |
| `version` | string | Resolved version (semver string, null if unversioned) |
| `source` | string | Where resolved from: `local` or `registry` |
| `path` | string | Filesystem path (only for local source) |
| `manifest` | object | Parsed manifest content |
| `content` | string | Raw content body (for spec-based plugins, excludes frontmatter) |

## License

Apache-2.0
