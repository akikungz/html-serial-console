# 🖥️ Web Serial Console

A **zero-dependency, single-file** browser-based serial terminal built with vanilla HTML, CSS, JavaScript, and an inline vendored **xterm.js** engine. Connect to any serial device directly from your browser using the [Web Serial API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API) — no extensions, no installs, no build steps.

> **Works completely offline.** Just open `index.html` in a supported browser and start communicating with your hardware.

---

## ✨ Features

### 🔌 Serial Communication
- **One-click connect/disconnect** — browser-native serial port picker with remembered port support
- **USB chip & device identification** — automatically parses USB VID/PID and identifies common chips (CP2102, CH340, FTDI, ESP32, Arduino, RP2040, STM32) with a live header badge
- **Asynchronous FIFO write queue** — rapid typing, macro bursts, and large clipboard pastes queue smoothly without stream writer lock contention
- **Safe port teardown** — drains and aborts write queue, cleanly releases reader/writer stream locks, safely closes the port with driver timeout protection, and prevents reconnect race conditions
- **Resilient stream error recovery** — automatically handles non-fatal framing errors, parity errors, and buffer overruns (common during microcontroller reboot/bootloader cycles) without abruptly killing the connection
- **Modern Web Serial API features** — handles `'connect'` and `'disconnect'` hotplug events, supports `port.forget()` to revoke device permissions, and offers auto-reconnect via `navigator.serial.getPorts()`
- **Configurable baud rates** — 9600, 19200, 38400, 57600, 115200, 230400, 460800, 921600, or any custom value up to 4,000,000
- **Full serial parameters** — data bits (7/8), stop bits (1/2), parity (none/even/odd), flow control (none/hardware RTS/CTS)
- **Configurable buffer size** for high-throughput devices

### 📟 xterm.js Terminal Emulator Integration
- **Industrial-grade terminal** — powered by vendored **xterm.js** and **FitAddon** (auto-adapts to window and container resizing)
- **Staircase effect prevention** — `convertEol: true` handles incoming linefeeds cleanly
- **Full SGR support** — 16-color, 256-color, and 24-bit TrueColor (RGB) foreground & background
- **Text attributes** — bold, dim, italic, underline, inverse
- **Control character handling** — CR, LF, CRLF, backspace, tab
- **CSI sequences** — clear screen (`\e[2J`), erase line (`\e[K`), cursor movement
- **Blinking cursor** with focus/blur visual states
- **Configurable scrollback** buffer (default 10,000 lines)

### 💬 Command Input Bar & History
- **Dedicated command input** — bottom input bar for structured command dispatch (AT commands, G-code, CLI menus)
- **Command history** — recall previous commands with `↑` and `↓` arrow keys, persisted in `localStorage`
- **Dual transmission modes**:
  - **ASCII mode** — sends text strings with configurable line endings (`CR`, `LF`, `CRLF`, or `None`)
  - **HEX mode** — parses raw hexadecimal byte sequences (e.g. `01 03 00 00 00 02 C4 0B` or `0xAA, 0x55`) and transmits binary frames
- **Line ending override** — switch line endings per command or inherit global settings

### 🔧 Hardware Signal & Pin Control
- **DTR / RTS toggle** — manually control Data Terminal Ready and Request To Send pins
- **ESP32 auto-reset** — automated RTS/DTR pulse sequence for one-click ESP32/ESP8266 reset into application
- **Arduino reset** — dedicated DTR pulse for standard Arduino (Uno/Nano/Mega/Pro Mini) reset
- **Hardware input indicators** — live status indicators for CTS (Clear To Send) and DSR (Data Set Ready) pins
- **Break signal** — send serial break to the connected device

### 📁 File Streaming / Send File
- **Stream files to serial port** — send text scripts, binary firmware payloads, or G-code files
- **Transfer progress modal** — live progress bar, byte counter, transfer speed (KB/s), and instant cancel button
- **Chunked async backpressure** — sends in paced 256-byte chunks through the FIFO write queue

### ⚡ Quick Macros
- **Customizable macro buttons** — define frequently-used commands as one-click chips
- **Escape sequence support** — macros can include `\r`, `\n`, `\t`, `\e`, or raw hex bytes like `\x03` (Ctrl+C)
- **Fault-tolerant storage** — macros saved to `localStorage` wrapped with safe `try/catch` fallback
- **Default set included** — `?`, `help`, `status`, `reboot`, `AT`

### 🔍 Hex Inspector
- **Strict 16-byte row alignment** — residual bytes are buffered so every completed row is aligned to 16 bytes with standard hex offsets (`0x000000`, `0x000010`, ...)
- **Pending row preview** — partial byte chunks (< 16 bytes) render immediately in a pending row without waiting for a full block
- **Decoupled high-performance engine** — byte ingestion is decoupled from DOM rendering using throttled `requestAnimationFrame` and batched row pruning
- **Hex dump export** — download formatted hex dump text with offsets, hex bytes, and ASCII representation

### 📋 Terminal Options & Persistence
- **Timestamps** — optional `[HH:MM:SS.mmm]` prefix on each new line
- **Auto-scroll** — automatically follow latest output (toggle on/off)
- **Local echo** — echo typed characters and sent commands locally to terminal
- **Newline mode** — configurable Enter key behavior: CR (`\r`), LF (`\n`), or CRLF (`\r\n`)
- **Backspace mode** — configurable: BS (`0x08`) or DEL (`0x7F`)
- **Full settings persistence** — all serial settings and toggles are saved across browser refreshes in `localStorage`

### 💾 Export & Logging
- **Clean log export** — download session logs automatically stripped of ANSI escape sequences for clean text editing
- **Hex dump export** — download inspector view as formatted offset/hex/ASCII text
- **RX/TX byte counters** — live statistics displayed in the toolbar with human-readable units

### 🎨 Design & Accessibility
- **Dark theme** with a premium glassmorphism-inspired aesthetic
- **Fully responsive** layout with mobile and tablet breakpoints
- **Custom scrollbars** styled for the dark UI
- **Zero external dependencies** — vendored inline, no CDN, no npm, no framework. Pure offline HTML

---

## 🚀 Getting Started

### Prerequisites

A browser that supports the **Web Serial API**:

| Browser | Minimum Version | Platform |
|---------|----------------|----------|
| Google Chrome | 89+ | Desktop, Android |
| Microsoft Edge | 89+ | Desktop |
| Opera | 75+ | Desktop |

> ⚠️ **Firefox and Safari do not support the Web Serial API.** The app will display a warning banner if opened in an unsupported browser.

### Usage

1. **Open the file** — simply open `index.html` in a supported browser:
   ```
   # Double-click the file, or:
   open index.html          # macOS
   start index.html         # Windows
   xdg-open index.html      # Linux
   ```

2. **Select baud rate** — choose from the dropdown or pick "Custom..." for any arbitrary value up to 4,000,000.

3. **Click Connect** — the browser will prompt you to select a serial port. Detected chip details (VID/PID) will appear in the header.

4. **Start communicating**:
   - **Type directly** in the terminal canvas for character-by-character interactive shells.
   - **Use the Command Bar** at the bottom to send complete lines or raw hex byte frames with history recall.
   - **Click quick macros** for common operations.
   - **Send a file** to transmit scripts or firmware payloads.

5. **Disconnect** when finished — click the red Disconnect button or simply close the tab.

No server, no build step, no installation required.

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Ctrl + C` (with text selected) | Copy selected terminal text to clipboard |
| `Ctrl + C` (no text selected) | Send ETX (0x03) — interrupt signal to device |
| `Ctrl + Shift + C` | Copy terminal selection to clipboard |
| `Ctrl + Shift + V` | Paste clipboard text to serial port |
| `Ctrl + L` | Clear terminal screen |
| `Ctrl + D` | Send EOT (0x04) — end of transmission |
| `Ctrl + Z` | Send SUB (0x1A) — suspend signal |
| `Tab` | Send tab character (0x09) |
| `Enter` | Send newline (configurable: CR / LF / CRLF) |
| `Backspace` | Send backspace (configurable: BS / DEL) |
| `↑` / `↓` (in Command Bar) | Navigate previous command history |
| `Escape` | Close settings modal or cancel active file transfer |
| Arrow keys (in Terminal) | Send VT100 escape sequences |
| `Home` / `End` (in Terminal) | Send cursor home / end sequences |

---

## 🛠️ Advanced Settings

Click the **⚙️ gear icon** in the header to open the Serial Port Settings modal:

| Setting | Options | Default |
|---------|---------|---------|
| Baud Rate | 1 – 4,000,000 | 115200 |
| Data Bits | 7, 8 | 8 |
| Stop Bits | 1, 2 | 1 |
| Parity | None, Even, Odd | None |
| Flow Control | None, Hardware (RTS/CTS) | None |
| Buffer Size | ≥ 255 bytes | 16384 |
| Newline Mode | CR / CRLF / LF | CR |
| Backspace Mode | BS (0x08) / DEL (0x7F) | BS |
| Auto-Reconnect | Enabled / Disabled | Disabled |
| Forget Port | Revoke permission via `port.forget()` | Available when connected |

---

## 🎯 Use Cases

- **Embedded development** — debug ESP32, Arduino, STM32, Raspberry Pi Pico over USB serial
- **AT command interfaces** — communicate with modems, LoRa modules, Bluetooth modules
- **Binary protocol debugging** — send raw hex packets to Modbus RTU, CAN-bus bridges, sensors
- **3D printer consoles** — send G-code commands and files to Marlin, Klipper, or other firmware
- **Router / switch configuration** — access serial consoles on networking equipment
- **UART debugging** — monitor and interact with any UART-based device
- **Quick serial testing** — zero installs required; works instantly across desktop and Android

---

## 📁 Project Structure

```
html-serial-console/
├── index.html      # Entire application (HTML + CSS + JS) — single file
└── README.md       # This file
```

The entire application is contained in a single `index.html` file (~340 KB) with:
- **Inline xterm.js & FitAddon** — full VT100/ANSI terminal emulation engine vendored inline for 100% offline usage
- **Inline CSS** — complete dark-themed design system with CSS custom properties
- **Inline JavaScript** — async write FIFO queue, decoupled hex inspector, robust port teardown, and UI event handling
- **No external resources** — works completely offline without internet, CDN, or build tooling

---

## 🤝 Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'feat: add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is open source. See the repository for license details.

---

## 🙏 Acknowledgements

- [Web Serial API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API) — the browser API that makes this possible
- [xterm.js](https://xtermjs.org/) — the terminal front-end component for the web
- Built with ❤️ as a zero-dependency, portable serial terminal
