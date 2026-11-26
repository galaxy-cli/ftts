# ftts

A minimalist CLI text-to-speech wrapper around Festival (`text2wave`) that reads from files, strings, the clipboard, or `stdin` with natural clause, sentence, and paragraph pacing.

### Prerequisites

- **festival** (provides `text2wave`): Core speech synthesis engine
- **aplay** (`alsa-utils`): Default sentence-by-sentence playback engine
- **xsel**: Required for clipboard input (`-c` / `--clipboard`)

You do not need to install everything up front—`ftts` checks the dependencies needed for the mode you run and offers to install missing packages via `apt` when prompted. To install all dependencies ahead of time:

```bash
sudo apt update && sudo apt install festival alsa-utils xsel
```

### Installation

Give the script execution permissions and place it in a directory on your `PATH`:

```bash
chmod +x ftts
mv ftts ~/.local/bin/
```

### Usage

```bash
ftts -s "Hello world"                  # Speak a quoted string
ftts --say="Hello world"               # Long option with '=' syntax
ftts -f notes.txt                      # Speak a plaintext file
ftts -c                                # Speak current clipboard text
echo "Hello from pipe" | ftts          # Read from stdin when piped
ftts -c -m                             # Synthesize, then play via mpv
```

> [!TIP]
> **Reading PDFs or Rich Documents:** PDFs are not read natively by `-f`. You can extract text to `stdin` with a tool like `pdftotext` (`pdftotext doc.pdf - | ftts`) or copy text to the clipboard and run `ftts -c`.

### Pacing & Pauses

Instead of breaking blindly at a fixed character count, `ftts` splits text along natural boundaries and inserts brief silence pauses into the audio stream:

- **Clause breaks (`~90 ms`):** Light breath on commas, semicolons, colons, or dashes in longer chunks.
- **Sentence/chunk breaks (`~180 ms`):** Clear pause at sentence endings while avoiding false splits on decimals like `3.14`.
- **Paragraph & divider breaks (`~420 ms` in MP3/`mpv` mode, `+220 ms` extra beat in `aplay` mode):** Longer pause across blank lines or horizontal rules (`---`, `===`, `***`).

### Default Playback vs. `-m` (`mpv`)

- **Default (`aplay`):** Synthesizes and plays chunks one by one with low startup latency. If a chunk fails synthesis, `ftts` logs a warning and skips that chunk without aborting the run.
- **Clipboard, string, or `stdin` input:** Builds a slug from the opening words plus a timestamp—for example, `~/Documents/the-quick-brown-fox_20260607-153012.mp3`.

If a target filename already exists, `ftts` appends `_2`, `_3`, and so on rather than overwriting existing files.

### Options

| Option | Argument | Description |
| :--- | :---: | :--- |
| `-c, --clipboard` | None | Read text from the system clipboard |
| `-f, --file=FILE` | `FILE` | Read text from a plaintext file |
| `-s, --say=TEXT` | `TEXT` | Speak a quoted text string |
| `-h, --help` | None | Display help message and exit |
| `-v, --version` | None | Output version information and exit |