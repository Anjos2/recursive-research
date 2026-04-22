# Privacy Policy

**Plugin:** recursive-research
**Author:** Joseph Huayhualla ([@Anjos2](https://github.com/Anjos2))
**Last updated:** April 2026

## TL;DR

`recursive-research` does **not** collect, store, transmit, or share any user data. It runs entirely within your local Claude Code instance. The plugin author has no servers, no telemetry, and no visibility into anything you do with the plugin.

---

## What the plugin does

- Reads local files **you explicitly provide** when you select `local` or `mixed` mode
- Performs web searches using Claude Code's built-in tools and/or MCPs **already configured in your agent**
- Writes research checkpoints and artifacts to a local `memoria/investigaciones/` folder inside your working project
- Operates entirely on your machine — no requests are sent to any server controlled by the plugin author

## What the plugin does NOT do

- No telemetry, analytics, crash reports, or usage tracking
- No data sent to the plugin author or any third party controlled by the author
- No account, login, API key, or authentication required by the plugin itself
- No background processes, no persistent connections, no "call home"
- No automatic updates or version checks initiated by the plugin

---

## Third-party tools

When you have MCPs installed (e.g., Firecrawl, Context7) or use Claude Code's built-in search/fetch tools, the plugin invokes them on your behalf to carry out the research. Queries dispatched to those tools are subject to each tool's own privacy policy:

- **Firecrawl** — see Firecrawl's privacy policy at the Firecrawl project's repository/site
- **Context7** — see Upstash/Context7's privacy policy
- **Claude Code WebSearch / WebFetch** — see [Anthropic's Privacy Policy](https://www.anthropic.com/legal/privacy)
- **Any other MCP you configure** — see its own terms

The plugin does not modify, proxy, intercept, log, or retain those requests. It only requests them based on the search strategy derived from your research seed and the source-tiering scheme.

---

## Local data

All research outputs (`memoria/investigaciones/<slug>/*.md`) live on your machine, inside your project directory. You are responsible for how you store, back up, share, or delete them. The plugin author has no access to them.

If you use the `local` or `mixed` mode and provide paths to sensitive documents, those documents remain on your machine — the plugin reads them to inform the research but does not copy them anywhere off your disk.

---

## Your rights

Since the plugin does not collect any personal data, there is nothing for the author to access, delete, export, or modify on your behalf. You control every file.

If you want to remove the plugin and all its outputs:

```
# Remove plugin installation
rm -rf ~/.claude/plugins/recursive-research

# Remove research outputs (per project)
rm -rf <your-project>/memoria/investigaciones
```

---

## Changes to this policy

Any changes to this policy will be committed to this file in the repository. You can inspect the full history with:

```
git log PRIVACY.md
```

Material changes will also be announced in the release notes of the version that introduces them.

---

## Contact

For privacy-related questions or concerns, open a [GitHub issue](https://github.com/Anjos2/recursive-research/issues).
