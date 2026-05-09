---
name: seemd
description: Use when the user asks for a shareable web URL for a local markdown file. Triggers on phrases like "share this markdown", "share this doc", "give me a link for this", "send this as a URL", "publish this", "make a public page for this", and Korean equivalents like "공유해줘", "이 README 공유", "링크로 보내줘". Especially when the user names a `.md` file path together with one of those phrases. Runs `npx @tentagrip/seemd <path>`, returns a slug URL on https://seemd.xyz. Linked `.md` files are uploaded together and cross-document links rewritten to slug URLs. Documents auto-expire after 3 days.
user-invocable: true
allowed-tools: Bash
argument-hint: "<path-to-md-file>"
---

# seemd — share markdown via URL

## What it does

Runs the `seemd` CLI to upload a markdown file (and every relative `.md` it links to) and returns a public URL on https://seemd.xyz. Uploaded documents auto-expire 3 days after creation.

## How to invoke

```bash
npx @tentagrip/seemd <path-to-md-file>
```

The **last line of stdout is the share URL**. Pass it to the user verbatim. `stderr` carries progress (`Uploading N linked documents alongside...`); surface the count in your reply.

## What the CLI does for you

- Walks markdown links in the source file (and recursively in those linked files)
- Uploads every reachable relative `.md` / `.markdown` file
- Rewrites cross-document links to slug URLs so navigation works on the shared page
- Preserves anchors: `./X.md#section` becomes `https://seemd.xyz/<slug>#section`
- Skips images, absolute URLs, non-md targets
- Tolerates link cycles (the back-edge link stays as-is and 404s on click)

## Gotchas

### 1. Don't paraphrase the URL
Slug URLs are 21-char nanoids. Output them verbatim. "Roughly that URL is..." or any rewording breaks the link.

### 2. Ambiguous file path
If the user says "share this doc" without naming a file, ask which one before uploading. A wrong upload is live for 3 days and produces a useless URL.

### 3. Broken relative links
Linked-but-missing files print `Warning: linked file not found: ...` to stderr and stay as the original path (404 on click). This is expected — surface a one-line summary of which links are broken so the user can fix the source.

### 4. Cycles
Mutual references (`A → B → A`) are not an error. The back-edge target's link is left untouched, so clicking it 404s. If the user cares, mention which link is in a cycle.

### 5. Large link graphs
Docs with 20+ linked files can take tens of seconds. Pass through the stderr progress messages so the user knows it's not stuck.

### 6. Expiry
Documents disappear after 3 days. If the user expects permanence, mention this up-front.
