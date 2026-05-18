# ftts

A minimalist CLI wrapper that pipes text to the Festival Text-to-Speech engine from files, strings, or your clipboard with fluid sentence pacing.

### Prerequisites

- **Festival**: The core TTS engine
- **xsel**: Required for clipboard support (`-c` flag)

You can install these on Debian/Ubuntu systems via:
```bash
sudo apt update && sudo apt install festival xsel
```

### Installation

Give the script execution permissions and move it into your local binary directory:

```bash
chmod +x ftts
mv ftts ~/.local/bin/          # Or anywhere else in your $PATH
```

### Usage

```bash
ftts -s "Hello world"          # Speak a string directly
ftts -f notes.txt              # Speak content of a file
ftts -c                        # Speak current clipboard content
```

### Options


| Option | Argument | Description |
| :--- | :---: | :--- |
| `-c, --clipboard` | None | Speak text from the clipboard |
| `-f, --file` | `FILE` | Speak content from a plaintext file |
| `-s, --speak` | `TEXT` | Speak a quoted string directly |
| `--help` | None | Show help message and exit |
| `-v, --version` | None | Output version information and exit |