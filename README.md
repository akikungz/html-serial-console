# 🖥️ Web Serial Console

A **zero-dependency, single-file** browser-based serial terminal built entirely with vanilla HTML, CSS, and JavaScript. Connect to any serial device directly from your browser using the [Web Serial API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API) — no extensions, no installs, no build steps.

> **Works completely offline.** Just open `index.html` in a supported browser and start communicating with your hardware.

---

## ✨ Features

### 🔌 Serial Communication
- **One-click connect/disconnect** — browser-native serial port picker
- **Configurable baud rates** — 9600, 19200, 38400, 57600, 115200, 230400, 460800, 921600, or any custom value
- **Full serial parameters** — data bits (7/8), stop bits (1/2), parity (none/even/odd), flow control (none/hardware RTS/CTS)
- **Configurable buffer size** for high-throughput devices

### 📟 Built-in ANSI Terminal Emulator
- **Full SGR support** — 16-color, 256-color, and 24-bit TrueColor (RGB) foreground & background
- **Text attributes** — bold, dim, italic, underline, inverse
- **Control character handling** — CR, LF, CRLF, backspace, tab
- **CSI sequences** — clear screen (`\e[2J`), erase line (`\e[K`)
- **Blinking cursor** with focus/blur visual states
- **Configurable scrollback** buffer (default 10,000 lines)

### 🔧 Hardware Signal Control
- **DTR / RTS toggle** — manually control Data Terminal Ready and Request To Send pins
- **ESP32 / Arduino reset** — automated DTR/RTS pulse sequence for one-click device reset
- **Break signal** — send serial break to the connected device

### ⚡ Quick Macros
- **Customizable macro buttons** — define frequently-used commands as one-click chips
- **Persistent storage** — macros saved to `localStorage` across sessions
- **Default set included** — `?`, `help`, `status`, `reboot`, `AT`

### 🔍 Hex Inspector
- **Real-time hex dump** view of all incoming data
- **Standard format** — offset | hex bytes | ASCII representation
- **Tab-switchable** between Terminal and Hex Inspector views

### 📋 Terminal Options
- **Timestamps** — optional `[HH:MM:SS.mmm]` prefix on each new line
- **Auto-scroll** — automatically follow latest output (toggle on/off)
- **Local echo** — echo typed characters locally before device response
- **Newline mode** — configurable Enter key behavior: CR (`\r`), LF (`\n`), or CRLF (`\r\n`)
- **Backspace mode** — configurable: BS (`0x08`) or DEL (`0x7F`)

### 💾 Export & Logging
- **One-click log export** — download the entire session as a timestamped `.txt` file
- **RX/TX byte counters** — live statistics displayed in the toolbar

### 🎨 Design
- **Dark theme** with a premium glassmorphism-inspired aesthetic
- **Fully responsive** layout
- **Custom scrollbars** styled for the dark UI
- **Zero external dependencies** — no CDN, no npm, no framework. Pure offline HTML

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

2. **Select baud rate** — choose from the dropdown or pick "Custom..." for any arbitrary value.

3. **Click Connect** — the browser will prompt you to select a serial port.

4. **Start communicating** — click inside the terminal area and type. Keystrokes are sent directly to the device in real-time.

5. **Disconnect** when finished — click the red Disconnect button or simply close the tab.

No server, no build step, no installation required.

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `Ctrl + C` | Send ETX (0x03) — interrupt signal |
| `Ctrl + D` | Send EOT (0x04) — end of transmission |
| `Ctrl + Z` | Send SUB (0x1A) — suspend signal |
| `Ctrl + L` | Clear terminal screen |
| `Tab` | Send tab character (0x09) |
| `Enter` | Send newline (configurable: CR / LF / CRLF) |
| `Backspace` | Send backspace (configurable: BS / DEL) |
| Arrow keys | Send VT100 escape sequences |
| `Home` / `End` | Send cursor home / end sequences |
| `Escape` | Send ESC (0x1B) |
| `Ctrl + V` / Paste | Paste clipboard content to serial port |

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

---

## 🎯 Use Cases

- **Embedded development** — debug ESP32, Arduino, STM32, Raspberry Pi Pico over USB serial
- **AT command interfaces** — communicate with modems, LoRa modules, Bluetooth modules
- **3D printer consoles** — send G-code to Marlin, Klipper, or other firmware
- **Router / switch configuration** — access serial consoles on networking equipment
- **UART debugging** — monitor and interact with any UART-based device
- **Quick serial testing** — no need to install PuTTY, screen, minicom, or any desktop tool

---

## 📁 Project Structure

```
html-serial-console/
├── index.html      # Entire application (HTML + CSS + JS) — single file
└── README.md       # This file
```

The entire application is contained in a single `index.html` file (~66 KB) with:
- **Inline CSS** — complete dark-themed design system with CSS custom properties
- **Inline JavaScript** — full terminal emulator class, serial communication logic, UI event handling
- **No external resources** — works without internet, CDN, or any build tooling

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
- Built with ❤️ as a zero-dependency, portable serial terminal
