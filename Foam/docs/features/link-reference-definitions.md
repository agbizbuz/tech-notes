# Link Reference Definitions

When you use `[[wikilinks]]`, the [foam-vscode](https://github.com/foambubble/foam/tree/main/packages/foam-vscode) extension can automatically generate [Markdown Link Reference Definitions](https://spec.commonmark.org/0.29/#link-reference-definitions) at the bottom of the file. This is not needed to navigate your workspace with foam-vscode, but is useful for files to remain compatible with various Markdown tools (e.g. parsers, static site generators, VS code plugins etc), which don't support `[[wikilinks]]`.

## Example

The following example:

```md
- [[wikilinks]]
- [[github-pages]]
```

...generates the following link reference definitions to the bottom of the file:

```md
[wikilinks]: wikilinks 'Wikilinks'
[github-pages]: github-pages 'GitHub Pages'
```

You can open the [raw markdown](https://foambubble.github.io/foam/user/features/link-reference-definitions.md) to see them at the bottom of this file

## Specification

The three components of a link reference definition are `[link-label]: link-target "Link Title"`

- **link label:** The link text to match in the surrounding markdown document. This matches the inner bracket of the double-bracketed `[[wikilink]]` notation
- **link destination** The target of the matched link
  - By default we generate links without extension. This can be overridden, see [Configuration](#configuration) below
- **"Link Title"** Optional title for link (The Foam template has a snippet of JavaScript to replace this on the website at runtime)

## Configuration

You can choose to generate link reference definitions with or without file extensions, depending on the target, or to disable the generation altogether. As a rule of thumb:

- Links with file extensions work better with standard markdown-based tools, such as GitHub web UI.
- Links without file extensions work better with certain web publishing tools that treat links as literal urls and don't transform them automatically, such as the standard GitHub pages installation.

By default, Foam generates links without file extensions for legacy reasons, but this may change in future versions.

You can override this setting in your Foam workspace's `settings.json`:

- `"foam.edit.linkReferenceDefinitions": "withoutExtensions"` (default)
- `"foam.edit.linkReferenceDefinitions": "withExtensions"`
- `"foam.edit.linkReferenceDefinitions": "off"`

### Ignoring files

Sometimes, you may want to ignore certain files or folders, so that Foam doesn't generate link reference definitions to them.

There are three options for excluding files from your Foam project:

1. `files.exclude` (from VSCode) will prevent the folder from showing in the file explorer.

   > "Configure glob patterns for excluding files and folders. For example, the file explorer decides which files and folders to show or hide based on this setting. Refer to the Search: Exclude setting to define search-specific excludes."

2. `files.watcherExclude` (from VSCode) prevents VSCode from constantly monitoring files for changes.

   > "Configure paths or glob patterns to exclude from file watching. Paths or basic glob patterns that are relative (for example `build/output` or `*.js`) will be resolved to an absolute path using the currently opened workspace. Complex glob patterns must match on absolute paths (i.e. prefix with `**/` or the full path and suffix with `/**` to match files within a path) to match properly (for example `**/build/output/**` or `/Users/name/workspaces/project/build/output/**`). When you experience the file watcher process consuming a lot of CPU, make sure to exclude large folders that are of less interest (such as build output folders)."

3. `foam.files.ignore` (from Foam) ignores files from being added to the Foam graph.

   > "Specifies the list of globs that will be ignored by Foam (e.g. they will not be considered when creating the graph). To ignore the all the content of a given folder, use `<folderName>/**/*`" (requires reloading VSCode to take effect).

For instance, if you're using a local instance of [Jekyll](https://jekyllrb.com/), you may find that it writes copies of each `.md` file into a `_site` directory, which may lead to Foam generating references to them instead of the original source notes.

You can ignore the `_site` directory by adding any of the following settings to your `.vscode/settings.json` file:

```json
  "files.exclude": {
    "**/_site": true
  },
  "files.watcherExclude": {
    "**/_site": true
  },
  "foam.files.ignore": [
    "_site/**/*"
  ]
```

After changing the setting in your workspace, you can run the [[workspace-janitor]] command to convert all existing definitions.

See [[link-reference-definition-improvements]] for further discussion on current problems and potential solutions.

