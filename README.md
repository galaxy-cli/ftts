# ftts

A minimalist, high-performance CLI wrapper that pipes text to the Festival Text-to-Speech engine from files, strings, or your clipboard with fluid sentence pacing and optional real-time MP3 saving.

### Prerequisites

- **Festival**: The core TTS engine
- **xsel**: Required for clipboard support (`-c` flag)
- **lame**: Required for MP3 conversion (`-o` flag)

You can install these dependencies on Debian/Ubuntu systems via:
```bash
sudo apt update && sudo apt install festival xsel lame
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
ftts -f notes.txt                      # Speak content of a file
ftts -c                                # Speak current clipboard content
ftts -f book.txt -o audiobook.mp3      # Listen in real-time and save to MP3
```

### Options


| Option | Argument | Description |
| :--- | :---: | :--- |
| `-c, --clipboard` | None | Speak text from the clipboard |
| `-f, --file` | `FILE` | Speak content from a plaintext file |
| `-s, --speak` | `TEXT` | Speak a quoted string directly |
| `-o, --output` | `OUT_FILE`| Simultaneously stream audio and save as an MP3 |
| `--help` | None | Show help message and exit |
| `-v, --version` | None | Output version information and exit |