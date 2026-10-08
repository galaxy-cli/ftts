# ftts

A minimalist, high-performance CLI wrapper that pipes text to the Festival Text-to-Speech engine from files, strings, or your clipboard with fluid sentence pacing, an optional mpv playback mode, and optional MP3 saving. It can also play an existing MP3 directly.

### Prerequisites

- **Festival**: The core TTS engine
- **aplay**: Default playback engine (ships in the `alsa-utils` package)
- **xsel**: Required for clipboard support (`-c` flag)
- **lame**: Required for MP3 saving (`-o` flag) or playback via `-m`
- **mpv**: Only required if you use `-m`

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
ftts -c -m                             # Convert, then play the result through mpv
ftts -f book.txt -o ~/Documents        # Listen in real-time and save an MP3 copy
ftts -m document.mp3                   # Play an existing MP3 directly via mpv
```

> [!TIP]
> **Reading PDFs & Protected Documents:** To read documents like PDFs or web articles, simply open them in your preferred viewer, press `Ctrl+A` to select the text, `Ctrl+C` to copy it, and run `ftts -c`. This offloads the text extraction to your system for maximum reliability.

### Default playback vs. `-m`

Without `-m`, speech plays through `aplay` sentence-by-sentence as it's synthesized -- audio starts almost immediately, and a bad sentence is skipped without losing the rest.

With `-m`, the entire input is converted into one complete MP3 first, with status messages along the way (`reading text from...`, `converting text to speech...`, `playing via mpv...`), and only then is it handed to `mpv` to play in full. This trades a quieter, lower-latency start for a cleaner, single finished file to play -- there's more of a wait before anything plays on a longer document, but you get clear feedback that it's working the whole time instead of silence.

### Saving audio

`-o` takes a **destination directory**, not a specific filename -- `ftts` names the file for you and drops it in there:
- **File input** reuses the source name: `ftts -f notes.txt -o ~/Documents` → `~/Documents/notes.mp3`
- **Clipboard/string input** has no natural name to borrow, so it's named from a slug of the first few words plus a timestamp: `ftts -s "The quick brown fox" -o ~/Documents` → something like `~/Documents/the-quick-brown-fox_20260607-153012.mp3`

If a file of that name already exists, a numeric suffix (`_2`, `_3`, ...) is added automatically rather than overwriting it. Combining `-o` with `-m` saves and plays the same finished file -- nothing extra is built twice.

`-o` can't be combined with `-m FILE.mp3` direct playback, since there's nothing to synthesize or save in that mode.

### Playing an existing MP3

Pass `-m` together with a bare `.mp3` path (instead of `-c`/`-f`/`-s`) to skip text-to-speech entirely and just play that file through mpv:
```bash
ftts -m document.mp3
```
Without `-m`, a bare file argument like this isn't accepted -- it has to go through `-f` to be read as text, or be given to `-m` to be played as audio.

### Options

| Option | Argument | Description |
| :--- | :---: | :--- |
| `-c, --clipboard` | None | Speak text from the clipboard |
| `-f, --file` | `FILE` | Speak content from a specified plaintext FILE |
| `-s, --say` | `TEXT` | Speak the provided quoted string of TEXT |
| `-m, --mpv` | None | Convert fully, then play through mpv (or, with a bare `.mp3` path, play that file directly) |
| `-o, --output` | `DIR` | Also save an MP3 copy into DIR (auto-named, see above) |
| `--help` | None | Show help message and exit |
| `-v, --version` | None | Output version information and exit |
