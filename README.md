# 🎤 Offline Multi-Language Audio Transcription System

A real-time audio transcription application that captures audio from your microphone and transcribes it offline using Vosk speech recognition models. Supports English, Spanish, and Hindi with a web-based dashboard for viewing, filtering, and exporting transcriptions.

## 🛠️ Tech Stack

### Backend
- **Python 3.x** - Core programming language
- **Flask** - Web framework for the dashboard
- **Vosk** - Offline speech recognition engine
- **SQLite3** - Database for storing transcriptions
- **sounddevice** - Audio capture from microphone
- **wave** - Audio file processing

### Frontend
- **HTML5** - Web interface structure
- **CSS3** - Styling and layout
- **Vanilla JavaScript** - Dynamic content updates and AJAX calls

### Key Libraries
- `flask` - Web server and routing
- `vosk` - Speech-to-text recognition
- `sounddevice` - Real-time audio input
- `sqlite3` - Built-in Python database
- `threading` - Background audio processing
- `queue` - Thread-safe audio data handling

## 📋 Prerequisites

Before running the application, ensure you have:

1. **Python 3.7+** installed
2. **pip** package manager
3. **Microphone** access on your system
4. **Internet connection** (only for initial model downloads)

## 🚀 Installation & Setup

### Step 1: Install Python Dependencies

```cmd
pip install flask vosk sounddevice
```

### Step 2: Download Vosk Language Models

Download the following models from [Vosk Models](https://alphacephei.com/vosk/models):

1. **English**: `vosk-model-small-en-us-0.15`
2. **Spanish**: `vosk-model-small-es-0.42`
3. **Hindi**: `vosk-model-small-hi-0.22`

Extract each model folder to the project root directory. Your structure should look like:

```
project-root/
├── vosk-model-small-en-us-0.15/
├── vosk-model-small-es-0.42/
├── vosk-model-small-hi-0.22/
├── app.py
├── transcriber.py
└── ...
```

### Step 3: Verify Directory Structure

The application will automatically create:
- `transcriptions.db` - SQLite database
- `audio_clips/` - Directory for saved audio files

## ▶️ Running the Application

### Start the Server

```cmd
python app.py
```

The application will:
1. Initialize the SQLite database
2. Start the background audio transcriber
3. Launch the Flask web server on `http://0.0.0.0:5000`

### Access the Dashboard

Open your web browser and navigate to:

```
http://localhost:5000
```

## 🎯 Features

- **Real-time Transcription**: Captures and transcribes audio continuously
- **Multi-language Support**: Simultaneous recognition for English, Spanish, and Hindi
- **Audio Playback**: Each transcription is saved with its audio clip
- **Language Filtering**: Filter transcriptions by language
- **Export Options**: Download transcriptions as TXT or CSV
- **Auto-refresh**: Dashboard updates every 2 seconds
- **Persistent Storage**: All transcriptions saved to SQLite database

## 📁 Project Structure

```
.
├── app.py                  # Flask web application and routes
├── transcriber.py          # Audio capture and transcription engine
├── templates/
│   └── index.html         # Web dashboard interface
├── audio_clips/           # Saved audio files (auto-created)
├── transcriptions.db      # SQLite database (auto-created)
├── vosk-model-small-*/    # Language models (download required)
└── arch/                  # Archived versions
```

## 🔌 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Main dashboard page |
| `/data?lang={lang}` | GET | Get transcriptions (JSON) |
| `/download/txt` | GET | Download all transcripts as TXT |
| `/download/csv` | GET | Download all transcripts as CSV |
| `/audio_clips/<filename>` | GET | Stream audio file |

### Query Parameters

- `lang` - Filter by language: `en`, `es`, `hi`, or `all` (default: `all`)

## 🗄️ Database Schema

```sql
CREATE TABLE transcripts (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT,
    language TEXT,
    text TEXT,
    audio_file TEXT
);
```

## ⚙️ Configuration

### Audio Settings
- Sample Rate: 16000 Hz
- Channels: Mono (1)
- Block Size: 8000 frames
- Format: 16-bit PCM

### Model Paths
Edit `MODEL_PATHS` in `transcriber.py` to change model locations:

```python
MODEL_PATHS = {
    "en": "vosk-model-small-en-us-0.15",
    "es": "vosk-model-small-es-0.42",
    "hi": "vosk-model-small-hi-0.22"
}
```

## 🐛 Troubleshooting

### Model Not Found Error
```
FileNotFoundError: Download model for {lang} from: https://alphacephei.com/vosk/models
```
**Solution**: Download and extract the required Vosk models to the project root.

### Audio Device Error
**Solution**: Ensure your microphone is connected and accessible. Check system audio permissions.

### Port Already in Use
**Solution**: Change the port in `app.py`:
```python
app.run(host="0.0.0.0", port=5001, debug=True)
```

## 📝 Notes

- The application runs in debug mode by default
- Transcriptions are stored permanently in the database
- Audio files are saved in WAV format
- The system checks all three languages simultaneously for each audio chunk

## 🔒 Security Considerations

- The server binds to `0.0.0.0` (all interfaces) - restrict this in production
- No authentication is implemented - add security layers for production use
- Audio files are stored unencrypted

## 📄 License

This project is for educational and development purposes.
