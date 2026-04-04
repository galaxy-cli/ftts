# ftts
A minimalist CLI wrapper that piped text to the Festival Text-to-Speech engine from files, strings, or your clipboard.

### Prerequisites
    Festival: The core TTS engine
    xsel: Required for clipboard support (`-c` flag)

### Installation
```
chmod +x ftts
mv ftts ~/.local/bin/  # Or anywhere in your $PATH
```

### Usage
```
ftts -s "Hello world`          # Speak a string directly
ftts -f notes.txt              # Speak content of a file
ftts -c                        # Speak current clipboard content
```

### Options
| Option | Argument | Description |
| :--- | :---: | :--- |
| `-h, --help` | None | Show this help message |
| `-c, --clipboard` | None | Speak text from the clipboard |
| `-f, --file` | `FILE` | Speak content from a plaintext file |
| `-s, --speak` | `TEXT` | Speak a quoted string directly |