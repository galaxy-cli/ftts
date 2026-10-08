# ftts

A minimalist, high-performance CLI wrapper that pipes text to the Festival Text-to-Speech engine from files, strings, or your clipboard with fluid sentence pacing, a choice of playback engine, and optional MP3 saving.

### Prerequisites

- **Festival**: The core TTS engine
- **aplay**: Default playback engine (ships in the `alsa-utils` package)
- **xsel**: Required for clipboard support (`-c` flag)
- **lame**: Required for MP3 saving (`-o` flag) or streaming via `-p mpv`
- **mpv**: Only required if you use `-p mpv` instead of the default player

You don't need to install these up front -- `ftts` checks for whichever ones a given run actually needs and offers to install them via `apt` on the spot. To install everything ahead of time instead:
```bash
sudo apt update && sudo apt install festival alsa-utils xsel lame mpv
```

### Installation

Give the script execution permissions and move it into your local binary directory:

```bash
chmod +x ftts
mv ftts ~/.local/bin/          # Or anywhere else in your $PATH
```

### Usage

```bash
ftts -s "Hello world"                  # Speak a string directly
ftts -f notes.txt                      # Speak content of a plaintext file
ftts -c                                # Speak current clipboard content
ftts -c -p mpv                         # Same, but play through mpv instead of aplay
ftts -f book.txt -o ~/Documents        # Listen in real-time and save an MP3 copy
```

> [!TIP]
> **Reading PDFs & Protected Documents:** To read documents like PDFs or web articles, simply open them in your preferred viewer, press `Ctrl+A` to select the text, `Ctrl+C` to copy it, and run `ftts -c`. This offloads the text extraction to your system for maximum reliability.

### Saving audio

`-o` takes a **destination directory**, not a specific filename -- `ftts` names the file for you and drops it in there:
- **File input** reuses the source name: `ftts -f notes.txt -o ~/Documents` → `~/Documents/notes.mp3`
- **Clipboard/string input** has no natural name to borrow, so it's named from a slug of the first few words plus a timestamp: `ftts -s "The quick brown fox" -o ~/Documents` → something like `~/Documents/the-quick-brown-fox_20260607-153012.mp3`

If a file of that name already exists, a numeric suffix (`_2`, `_3`, ...) is added automatically rather than overwriting it.

### Options

| Option | Argument | Description |
| :--- | :---: | :--- |
| `-c, --clipboard` | None | Speak text from the clipboard |
| `-f, --file` | `FILE` | Speak content from a specified plaintext FILE |
| `-s, --say` | `TEXT` | Speak the provided quoted string of TEXT |
| `-p, --player` | `aplay`/`mpv` | Playback engine to use (default: `aplay`) |
| `-o, --output` | `DIR` | Also save an MP3 copy into DIR (auto-named, see above) |
| `--help` | None | Show help message and exit |
| `-v, --version` | None | Output version information and exit |
