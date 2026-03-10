# KafkaLens Plugin Index

The official public plugin index for [KafkaLens](https://github.com/fatichar/KafkaLens).

KafkaLens downloads this index to discover and install community plugins. The index is served as a static file via GitHub Pages.

**Index URL:** `https://fatichar.github.io/kafkalens-plugin-index/plugins.json`

---

## 📦 Available Plugins

### 🎨 Themes

| Plugin | Description | Themes |
|--------|-------------|---------|
| **[Nature Themes Collection](https://github.com/fatichar/kafkalens-themes)** | Beautiful nature-inspired themes | Bright, Forest, Gray, Ocean, Purple |

---

## Repository Structure

```
/
├── plugins.json       # Plugin index consumed by KafkaLens
├── icons/             # Optional plugin icons (PNG, ~64×64)
└── README.md          # This file
```

---

## Enabling GitHub Pages

To serve the index, enable GitHub Pages on this repository:

1. Go to **Settings → Pages**
2. Under **Source**, select **Deploy from a branch**
3. Choose branch **`main`** and folder **`/ (root)`**
4. Click **Save**

The index will then be publicly available at:

```
https://fatichar.github.io/kafkalens-plugin-index/plugins.json
```

---

## plugins.json Format

The index is a single JSON file with the following structure:

```json
{
  "name": "KafkaLens Public Plugin Repository",
  "description": "Brief description of this repository.",
  "homepage": "https://github.com/fatichar/kafkalens-plugin-index",
  "plugins": [
    {
      "id": "com.example.kafkalens.myplugin",
      "name": "My Plugin",
      "description": "What this plugin does.",
      "author": "Your Name",
      "homepage": "https://github.com/example/my-plugin",
      "versions": [
        {
          "version": "1.1.0",
          "kafkalensVersion": ">=1.6.0",
          "downloadUrl": "https://github.com/example/my-plugin/releases/download/v1.1.0/myplugin-1.1.0.zip",
          "icon": "icons/myplugin.png",
          "changelog": "Bug fixes and performance improvements."
        },
        {
          "version": "1.0.0",
          "kafkalensVersion": ">=1.5.0",
          "downloadUrl": "https://github.com/example/my-plugin/releases/download/v1.0.0/myplugin-1.0.0.zip",
          "icon": "icons/myplugin.png",
          "changelog": "Initial release."
        }
      ]
    }
  ]
}
```

### Field Reference

| Field | Required | Description |
|---|---|---|
| `id` | Yes | Unique reverse-domain plugin ID (see [Plugin ID Rules](#plugin-id-rules)) |
| `name` | Yes | Human-readable plugin name |
| `description` | Yes | Short description of what the plugin does |
| `author` | Yes | Author name or organization |
| `homepage` | Yes | URL to the plugin's source repository |
| `versions` | Yes | List of available versions, newest first |

**Per-version fields:**

| Field | Required | Description |
|---|---|---|
| `version` | Yes | Semantic version string (e.g. `1.2.0`) |
| `kafkalensVersion` | Yes | Version constraint for KafkaLens compatibility (e.g. `>=1.5.0`) |
| `downloadUrl` | Yes | Direct URL to the `.zip` release asset |
| `icon` | No | Relative path in this repo (`icons/myplugin.png`) or external URL |
| `changelog` | No | Short description of changes in this version |

### Version Ordering

Versions **must** be listed newest to oldest. KafkaLens installs the first version whose `kafkalensVersion` constraint is satisfied.

---

## Plugin ID Rules

Plugin IDs use **reverse-domain notation** to ensure global uniqueness.

**Valid examples:**
```
com.example.kafkalens.jsonformatter
io.confluent.kafkalens.avroviewer
org.company.kafkalens.tools
```

**Avoid generic IDs:**
```
jsonformatter          ← too short, likely to conflict
kafkalens.plugin.json  ← missing domain prefix
```

Use your own domain or GitHub username as the prefix:
```
com.yourdomain.kafkalens.pluginname
io.github.yourusername.kafkalens.pluginname
```

---

## Plugin Package Format

Plugins are distributed as `.zip` files attached to GitHub Releases.

**Asset naming convention:**
```
pluginname-1.0.0.zip
```

**ZIP contents:**

```
plugin.dll         # Required — the compiled plugin assembly
plugin.json        # Optional — plugin metadata override
icon.png           # Optional — plugin icon
README.md          # Optional — end-user documentation
```

The `plugin.dll` must contain the `KafkaLensPlugin` assembly metadata attribute. Refer to the [KafkaLens plugin development guide](https://github.com/fatichar/KafkaLens) for details.

---

## Icons

Icons can be stored in one of two ways:

**1. In this repository** (preferred for long-term stability):
```
icons/myplugin.png
```
Reference it in `plugins.json` as:
```json
"icon": "icons/myplugin.png"
```

**2. External URL** (hosted elsewhere):
```json
"icon": "https://raw.githubusercontent.com/example/my-plugin/main/icon.png"
```

Icon guidelines:
- Format: **PNG**
- Size: **64×64 pixels** (recommended)
- Keep file sizes small

---

## Publishing a Plugin

Follow these steps to list your plugin in this index:

### 1. Create your plugin repository

Set up a public GitHub repository for your plugin. Include:
- Source code
- A `README.md` explaining what your plugin does
- Build instructions

### 2. Build the plugin DLL

Compile your plugin targeting the KafkaLens plugin interface. The output must be a `.dll` containing the `KafkaLensPlugin` assembly metadata attribute.

### 3. Package as a ZIP

Create a `.zip` file containing at minimum:

```
myplugin-1.0.0.zip
└── plugin.dll
```

Name the archive `pluginname-version.zip` (e.g. `jsonformatter-1.0.0.zip`).

### 4. Publish a GitHub Release

In your plugin repository:

1. Tag your commit: `git tag v1.0.0 && git push --tags`
2. Create a GitHub Release for the tag
3. Attach the `.zip` file as a release asset
4. Note the direct download URL — it will look like:
   ```
   https://github.com/example/myplugin/releases/download/v1.0.0/myplugin-1.0.0.zip
   ```

### 5. Submit a PR to this repository

Fork this repository and edit `plugins.json`:

**For a new plugin**, add an entry to the `plugins` array:

```json
{
  "id": "com.yourname.kafkalens.myplugin",
  "name": "My Plugin",
  "description": "What your plugin does.",
  "author": "Your Name",
  "homepage": "https://github.com/yourname/myplugin",
  "versions": [
    {
      "version": "1.0.0",
      "kafkalensVersion": ">=1.5.0",
      "downloadUrl": "https://github.com/yourname/myplugin/releases/download/v1.0.0/myplugin-1.0.0.zip",
      "icon": "icons/myplugin.png",
      "changelog": "Initial release."
    }
  ]
}
```

**For a new release of an existing plugin**, prepend a new entry to the `versions` array (newest first):

```json
"versions": [
  {
    "version": "1.1.0",
    "kafkalensVersion": ">=1.6.0",
    "downloadUrl": "https://github.com/yourname/myplugin/releases/download/v1.1.0/myplugin-1.1.0.zip",
    "icon": "icons/myplugin.png",
    "changelog": "Added support for X."
  },
  {
    "version": "1.0.0",
    "kafkalensVersion": ">=1.5.0",
    "downloadUrl": "https://github.com/yourname/myplugin/releases/download/v1.0.0/myplugin-1.0.0.zip",
    "icon": "icons/myplugin.png",
    "changelog": "Initial release."
  }
]
```

Open the PR with the title format: `Add plugin: My Plugin` or `Update plugin: My Plugin v1.1.0`.

### PR Checklist

Before submitting your PR, confirm:

- [ ] Plugin ID uses reverse-domain format
- [ ] `downloadUrl` points to a publicly accessible `.zip` asset
- [ ] Versions are listed newest → oldest
- [ ] `kafkalensVersion` constraint is set correctly
- [ ] Icon file is included in `icons/` (if hosted here)
- [ ] `plugins.json` is valid JSON (validate with [jsonlint.com](https://jsonlint.com))

---

## Maintaining This Index

This repository is intentionally kept simple and static:

- No build tools or generators
- No CI/CD automation
- No backend dependencies

All changes are made by editing `plugins.json` directly via pull request. Maintainers review PRs to verify plugin IDs are unique, download URLs are reachable, and JSON is valid before merging.
