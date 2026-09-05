# A demo adblocker

## Files

- `blocklist.txt` - blocklist, one `0.0.0.0 domain` per line.
- `sample.html` - demo page referencing blocked ad domains.
- `adblock.rak` - the blocker script.
- `report.txt` - generated scan report.
- `hosts.blocked` - generated `0.0.0.0 domain` entries, one per unique hit.

## Usage

```
rakc run adblocker/adblock.rak
```

By default it scans the local `sample.html` and writes `report.txt` and
`hosts.blocked`. Two environment variables control the inputs:

- `RAK_ADBLOCK_LIST` - path to the blocklist (default `adblocker/blocklist.txt`)
- `RAK_ADBLOCK_PAGE` - page to scan. A URL (starts with `http`) is fetched
  over the network; otherwise it is read as a local file (default
  `adblocker/sample.html`).

Example - scan a live page:

```
$env:RAK_ADBLOCK_PAGE = "https://example.com/"; rakc run adblocker/adblock.rak
```

## How it works

- Loads the blocklist, handling both `0.0.0.0 domain` and plain `domain`
  lines.
- Extracts script, image, and link URLs from the page, then does a raw
  `src=` scan to catch iframes and anything the tag parser missed.
- Flags any URL that contains a blocklisted domain.
- Writes `report.txt` (per-resource hits) and `hosts.blocked` (deduplicated
  hosts entries you can append to a Pi-hole or hosts file).

Note: the adblocker is a demo.
