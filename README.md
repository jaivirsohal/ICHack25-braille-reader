# ICHack25-braille-reader

## Demo & More info

https://devpost.com/software/braille-reader-with-teaching-features

## Installation

Before running the project, install the required dependencies using pip:

```sh
pip install -r requirements.txt
```

Create a file named `.env` by copying the contents of `.env.template` and replacing the placeholders accordingly.

## Structure

The `pipeline` directory contains 5 `.py` files:
1. `image_converter.py` - processes a photo of Braille writing to return a string of Braille Unicode characters. Based on [Angelina Braille Reader](https://angelina-reader.ru/).
2. `braille_converter.py` - translates a string of Braille characters to English. Built with [Claude](https://claude.ai/).
3. `generate_voice_eleven.py` - reads a string to create a speech audio file. Built with [ElevenLabs](https://elevenlabs.io/) (Tokens are limited, so use sparingly)
4. `generate_voice_gtts.py` - reads a string to create a speech audio file. Lower quality than `3.`. Uses [Google TTS](https://cloud.google.com/text-to-speech)
5. `main.py` - Combines `1.`-`4.` to read a picture of Braille out loud (accepts an image, outputs an audio file).

The `project` directory contains files for the Django backend server.

`samples` contains pictures of Braille writing that can be used for various testing and demo purposes.

`output` contains the resultant audio file and any intermediate I/O.

## Usage

Follow these steps to get the Braille Reader with Teaching Features up and running:

### 1. Clone the repository

```bash
git clone https://github.com/your-username/ICHack25-braille-reader.git
cd ICHack25-braille-reader
```

### 2. Create and activate a virtual environment (optional but recommended)

```bash
# On Windows (PowerShell)
python -m venv venv
.\venv\Scripts\Activate.ps1

# On macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure environment variables

1. Copy `.env.template` to `.env`:

   ```bash
   cp .env.template .env
   ```
2. Open `.env` in your editor and set the following variables:

   * `ELEVENLABS_API_KEY`: Your ElevenLabs token (for `generate_voice_eleven.py`).
   * `GOOGLE_API_KEY`: API key for Google TTS (for `generate_voice_gtts.py`).
   * Any other keys or settings as required by the template.

### 5. Run the pipeline from the command line

The `main.py` script ties the entire pipeline together:

```bash
python pipeline/main.py \
  --input_image samples/test_braille.jpg \
  --output_audio output/reading.mp3 \
  [--method {elevenlabs,gtts}] \
  [--verbose]
```

* `--input_image`  : Path to the Braille image to read.
* `--output_audio` : Path where the generated audio file will be saved.
* `--method`       : (Optional) Choose voice generator: `elevenlabs` (default) or `gtts`.
* `--verbose`      : (Optional) Print detailed logs to the console.

Example:

```bash
python pipeline/main.py --input_image samples/page1.png --output_audio output/page1.mp3 --method gtts --verbose
```

### 6. Using individual modules

If you prefer to run each component separately, use the following commands:

* **Convert image to Braille Unicode**:

  ```bash
  python pipeline/image_converter.py --image samples/test_braille.jpg --output output/braille.txt
  ```
* **Translate Braille to text**:

  ```bash
  python pipeline/braille_converter.py --input output/braille.txt --output output/text.txt
  ```
* **Generate speech (ElevenLabs)**:

  ```bash
  python pipeline/generate_voice_eleven.py --text output/text.txt --output output/audio_eleven.mp3
  ```
* **Generate speech (Google TTS)**:

  ```bash
  python pipeline/generate_voice_gtts.py --text output/text.txt --output output/audio_gtts.mp3
  ```

### 7. Frontend and Django server

1. Install Django dependencies:

   ```bash
   pip install -r project/requirements.txt
   ```
2. Run migrations and start the server:

   ```bash
   cd project
   python manage.py migrate
   python manage.py runserver
   ```
3. Access the app at `http://127.0.0.1:8000` in your browser.

### 8. Sample outputs

* Sample Braille Unicode output: `output/braille.txt`
* Sample plain-text translation: `output/text.txt`
* Sample audio files: `output/reading.mp3`, `output/audio_eleven.mp3`, `output/audio_gtts.mp3`

---

*For more details, see the *[*Demo & More Info*](https://devpost.com/software/braille-reader-with-teaching-features)*.*


## Requirements
- Python 3.13 or later
- Required packages (listed in `requirements.txt`, installed via pip)

## License
