# TTS WebSocket Service

A real-time Text-to-Speech (TTS) WebSocket service that provides streaming audio synthesis with character-level timing alignment. Built using the Kokoro TTS engine.

## Features

- **Real-time streaming**: Processes text as it arrives and streams audio back immediately
- **Mathematical notation support**: Handles LaTeX math expressions (e.g., `\(a^2 + b^2 = c^2\)`)
- **Character-level alignment**: Provides precise timing for each character in the synthesized speech
- **Smart text chunking**: Automatically splits text at sentence boundaries for optimal synthesis
- **WebSocket protocol**: Compatible with streaming interfaces

## Architecture

### Core Components

1. **WebSocketTTSService**: Main service handling client connections and audio generation
2. **MathNotationProcessor**: Converts LaTeX mathematical expressions to speakable text
3. **Audio Processing Pipeline**: Handles format conversion, resampling, and encoding
4. **Alignment Generator**: Creates character-level timing information using heuristic weighting

### Protocol Flow

```
Client -> Server: { "text": " ", "flush": false }           # Initial handshake
Client -> Server: { "text": "Hello world", "flush": false }  # Text chunks
Server -> Client: { "audio": "base64...", "alignment": {...} } # Audio response
Client -> Server: { "text": "", "flush": true }             # Flush remaining
Client -> Server: { "text": "", "flush": false }            # Disconnect
```

## Installation

### Prerequisites

```bash
# Install Kokoro TTS (example - adjust based on actual installation method)
pip install kokoro-tts

# Install Python dependencies
pip install asyncio websockets numpy soundfile
```

### Required Dependencies

- `asyncio` - Asynchronous WebSocket handling
- `websockets` - WebSocket server implementation
- `numpy` - Audio data processing
- `soundfile` - Audio file format handling
- `kokoro` - TTS engine (Kokoro TTS)

## Usage

1. Open `tts_test_client.html` in a web browser
2. Run all the cells in the Python notebook
3. Connect the client (HTML) to `ws://localhost:8765`
4. Type text to synthesize (supports LaTeX math)
5. Audio chunks stream back with real-time captions

## Performance Considerations

### Audio Processing Optimizations

- **Format agnostic input**: Safely handles various Kokoro TTS output formats
- **Efficient resampling**: Linear interpolation for sample rate conversion
- **Memory management**: Processes audio in chunks to minimize memory usage
- **Error resilience**: Continues operation even if individual chunks fail

### Text Processing Optimizations

- **Smart chunking**: Splits text at natural boundaries (sentences, phrases)
- **Streaming synthesis**: Generates audio for partial text before complete input
- **Unicode normalization**: Handles various text encodings consistently
