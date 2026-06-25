# keystroke-sounds-web

Public website and auto-update files for Keystroke Sounds.

## Hosting your own custom sound source

Keystroke Sounds can load sound packs from any **source URL** — a JSON file
that lists the packs you want to share. The official source is:

```
https://edomor.github.io/keystroke-sounds-web/downloads/index.json
```

You can host your own anywhere that serves a public file over HTTPS: GitHub
Pages, an S3 bucket, your own web server, a gist's raw URL, etc. Anyone can
then point the app at your source URL and download your packs.

### 1. The source file (`index.json`)

The source is a single JSON file with a list of categories, each containing a
list of packs:

```json
{
  "categories": [
    {
      "name": "My Category",
      "packs": [
        {
          "id": "my_pack",
          "name": "My Pack",
          "description": "A short description shown in the app.",
          "author": "Your Name",
          "url": "https://example.com/packs/my_pack.zip"
        }
      ]
    }
  ]
}
```

**Pack fields**

| Field         | Required | Description                                                        |
|---------------|----------|--------------------------------------------------------------------|
| `id`          | yes      | Unique identifier for the pack (lowercase, no spaces).             |
| `name`        | yes      | Display name shown in the app.                                     |
| `description` | yes      | Short text shown under the name.                                   |
| `author`      | yes      | Credited author.                                                   |
| `url`         | yes      | Direct HTTPS link to the pack's `.zip` file (see below).          |

You can have as many categories and packs as you like. The `url` can point
anywhere — it does not have to live next to the `index.json`.

### 2. The pack itself (a `.zip` file)

Each pack is a `.zip` containing your `.wav` sound files plus a `pack.json`
manifest that maps keys to sounds:

```
my_pack.zip
├── pack.json
├── default_0.wav
├── default_1.wav
├── space_0.wav
├── enter_0.wav
├── backspace_0.wav
├── tab_0.wav
└── modifier_0.wav
```

**`pack.json` example**

```json
{
  "name": "My Pack",
  "description": "A short description shown in the app.",
  "author": "Your Name",
  "builtin": false,
  "selection": "by_key",
  "sounds": {
    "default": ["default_0.wav", "default_1.wav"],
    "space": ["space_0.wav"],
    "enter": ["enter_0.wav"],
    "backspace": ["backspace_0.wav"],
    "tab": ["tab_0.wav"],
    "modifier": ["modifier_0.wav"]
  },
  "key_overrides": {}
}
```

**`pack.json` fields**

| Field           | Description                                                                                  |
|-----------------|----------------------------------------------------------------------------------------------|
| `name`          | Pack name.                                                                                    |
| `description`   | Short description.                                                                            |
| `author`        | Credited author.                                                                              |
| `builtin`       | Keep this `false` for custom packs.                                                           |
| `selection`     | How sounds are chosen. `by_key` gives each key its own sound from the pool.                  |
| `sounds`        | Maps a group to the list of `.wav` filenames (relative to the zip root) used for that group. |
| `key_overrides` | Optional map of a specific key to its own list of `.wav` files. Leave as `{}` if unused.     |

The `sounds` groups: `default` is the pool used for normal keys, and
`space`, `enter`, `backspace`, `tab` and `modifier` cover those special keys.
Reference each `.wav` by its filename inside the zip.

### 3. Publish and share

1. Upload your `.zip` pack(s) to a public HTTPS location.
2. Create your `index.json` pointing the `url` of each pack at those zips.
3. Host the `index.json` at a public HTTPS URL.
4. Share that URL — in the app, load it from a link to install your packs.

> Tip: this whole repository is a working example. The `downloads/` folder
> holds the `index.json` source and every pack `.zip`, served via GitHub Pages.
