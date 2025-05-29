# 🤖 WhatsApp AI Chatbot

Bot WhatsApp pintar dengan integrasi AI menggunakan **Baileys** (WhatsApp Web API), **Express.js**, dan **Socket.IO** untuk komunikasi masa nyata.

## ✨ Ciri-ciri Utama

- 🔄 **Multi AI Provider** - Menyokong OpenAI, OpenRouter, dan Google Gemini
- 📱 **Integrasi WhatsApp** - Menggunakan @whiskeysockets/baileys (tanpa Puppeteer)
- 🌐 **Papan Pemuka Masa Nyata** - WebSocket dengan Socket.IO untuk pemantauan langsung
- 🛡️ **Sistem Senarai Hitam Pintar** - Penapis kata terlarang dengan fail konfigurasi
- 💬 **Ingatan Perbualan** - Menyimpan konteks perbualan setiap pengguna
- ⚙️ **System Prompt Dinamik** - Edit prompt AI secara masa nyata
- 📊 **Pemantauan Mesej Langsung** - Lihat semua mesej masuk/keluar
- 🔒 **Pengurusan Sesi** - Simpan sesi WhatsApp secara automatik
- 🎨 **Antara Muka Yang Cantik** - Papan pemuka web yang moden dan responsif
- 📁 **Seni Bina Modular** - Kod yang teratur dengan baik

## 🛠️ Teknologi Digunakan

- **Backend**: Node.js + Express.js
- **WhatsApp API**: @whiskeysockets/baileys v6.7.17
- **AI Providers**: 
  - OpenAI API (GPT-3.5/GPT-4)
  - OpenRouter (Claude, Llama, dll)
  - Google Gemini
- **Komunikasi Masa Nyata**: Socket.IO v4.7.4
- **Pengurus Pakej**: PNPM
- **Frontend**: Vanilla HTML/CSS/JS
- **Keselamatan**: DOMPurify untuk perlindungan XSS

## 📋 Keperluan

- **Node.js** v16+ (Disyorkan: v18 atau v20)
- **PNPM** (Pengurus pakej yang digunakan)
- **Kunci API** dari salah satu provider:
  - Kunci API OpenAI
  - Kunci API OpenRouter
  - Kunci API Google Gemini
- **WhatsApp** di telefon pintar untuk imbas kod QR

## 🚀 Pemasangan

### 1. Clone Repository

```bash
git clone https://github.com/xhanafix/wa-aichatbot-bm.git
cd wa-aichatbot-bm
```

### 2. Pasang Dependencies

```bash
# Pasang PNPM jika belum ada
npm install -g pnpm

# Pasang dependencies
pnpm install
```

### 3. Konfigurasi Persekitaran

Salin fail `.env.example` ke `.env`:

```bash
cp .env.example .env
```

Edit fail `.env` dengan konfigurasi anda:

```env
# Konfigurasi Server
PORT=3002

# Pemilihan AI Provider (openai/openrouter/gemini)
AI_PROVIDER=openai

# Konfigurasi OpenAI
OPENAI_API_KEY=sk-proj-your-actual-openai-api-key

# Konfigurasi OpenRouter  
OPENROUTER_API_KEY=sk-or-your-actual-openrouter-api-key
OPENROUTER_MODEL_NAME=anthropic/claude-3-haiku

# Konfigurasi Google Gemini
GEMINI_API_KEY=your-actual-gemini-api-key

# System Prompt (akan dibuat secara automatik di config/system-prompt.txt)
SYSTEM_PROMPT=Anda adalah pembantu AI WhatsApp yang mesra dan membantu.

# Laluan Sesi WhatsApp
SESSION_FILE_PATH=./session

# Kata-kata Senarai Hitam (akan dibuat secara automatik di config/blacklist-words.txt)
BLACKLIST_WORDS=spam,promosi,iklan,judi,togel,casino
```

> ⚠️ **PENTING**: Jangan sekali-kali commit fail `.env` ke Git! Fail `.gitignore` telah dikonfigurasi untuk melindungi data sensitif.

### 4. Jalankan Server

```bash
# Mod pembangunan (dengan auto-restart menggunakan nodemon)
pnpm dev

# Mod pengeluaran
pnpm start
```

Server akan berjalan di `http://localhost:3002` (atau mengikut PORT di `.env`)

## 📱 Cara Penggunaan

### 1. Akses Papan Pemuka Web

Buka pelayar dan lawati:
```
http://localhost:3002
```

### 2. Menghubungkan WhatsApp

1. ✅ Klik butang **"🚀 Start Bot"** di papan pemuka
2. 📱 Kod QR akan muncul di papan pemuka web 
3. 📲 Imbas kod QR dengan WhatsApp di telefon anda:
   - Buka WhatsApp → Menu (⋮) → **Peranti Tertaut** → **Tautkan Peranti**
4. ✅ Bot akan aktif selepas berjaya dihubungkan

### 3. Konfigurasi AI System Prompt

Di papan pemuka web:
1. 🔽 Tatal ke bahagian **"🧠 AI Configuration"**
2. ✏️ Edit system prompt mengikut keperluan (atau gunakan lalai)
3. 💾 Klik **"Update Prompt"** untuk menyimpan

### 4. Pemantauan Masa Nyata

- 💬 Bahagian **"Live Messages"** memaparkan mesej masuk/keluar secara masa nyata
- 📊 Lihat statistik perbualan dan status bot
- 🔄 Mulakan semula bot jika diperlukan tanpa segar semula halaman

## 🔧 API Endpoints

### 📱 API Routes WhatsApp (`/api/whatsapp`)

| Method | Endpoint | Penerangan |
|--------|----------|-------------|
| `GET` | `/api/whatsapp/status` | Periksa status bot WhatsApp |
| `POST` | `/api/whatsapp/start` | Mulakan klien WhatsApp |
| `POST` | `/api/whatsapp/stop` | Hentikan klien WhatsApp |
| `POST` | `/api/whatsapp/send` | Hantar mesej secara manual |
| `GET` | `/api/whatsapp/chats` | Dapatkan senarai chat aktif |

### 🤖 API Routes AI (`/api/ai`)

| Method | Endpoint | Penerangan |
|--------|----------|-------------|
| `GET` | `/api/ai/prompt` | Dapatkan system prompt semasa |
| `PUT` | `/api/ai/prompt` | Kemaskini system prompt |
| `POST` | `/api/ai/generate` | Jana respons AI (ujian) |
| `DELETE` | `/api/ai/conversation/:userId` | Padam sejarah perbualan pengguna |
| `GET` | `/api/ai/stats` | Statistik perbualan |

### 🌐 WebSocket Events (Socket.IO)

#### Client → Server Events
```javascript
socket.emit('start-whatsapp');  // Mulakan klien WhatsApp
socket.emit('stop-whatsapp');   // Hentikan klien WhatsApp
```

#### Server → Client Events
```javascript
socket.on('qr-code', (qr) => {});                    // Kod QR untuk imbas
socket.on('whatsapp-ready', () => {});               // Bot sedia
socket.on('whatsapp-authenticated', () => {});       // Pengesahan berjaya  
socket.on('whatsapp-disconnected', () => {});        // Bot terputus
socket.on('message-received', (message) => {});      // Mesej masuk
socket.on('ai-response', (response) => {});          // Respons AI dihantar
socket.on('error', (error) => {});                   // Event ralat
```

## 🙏 Kredit

Projek ini adalah fork dan penambahbaikan dari [wa-aichatbot](https://github.com/paijoe29/wa-aichatbot) oleh [@paijoe29](https://github.com/paijoe29). Terima kasih atas kerja keras dan sumbangan asal!

## 📁 Struktur Projek

```
wa-aichatbot/
├── 📄 server.js                    # Server utama dengan Socket.IO
├── 📋 package.json                 # Dependencies dan scripts
├── 🔧 pnpm-lock.yaml               # Fail kunci PNPM
├── 🔧 pnpm-workspace.yaml          # Konfigurasi ruang kerja PNPM
├── 🔒 .env                         # Pembolehubah persekitaran (PERIBADI)
├── 📖 README.md                    # Dokumentasi utama
├── 🔒 README_SECURITY.md           # Panduan keselamatan
├── 🛡️ .gitignore                   # Fail git ignore
│
├── 📁 config/
│   ├── 🤖 system-prompt.txt        # System prompt AI (dijana automatik)
│   └── 🚫 blacklist-words.txt      # Kata-kata senarai hitam (dijana automatik)
│
├── 📁 services/
│   ├── 🤖 aiService.js             # Perkhidmatan utama AI (multi-provider)
│   ├── 🔄 openaiService.js         # Perkhidmatan khusus OpenAI (legacy)
│   └── 📱 whatsappService.js       # Perkhidmatan WhatsApp dengan Baileys
│
├── 📁 routes/
│   ├── 📱 whatsapp.js              # API routes WhatsApp
│   └── 🤖 ai.js                    # API routes AI
│
├── 📁 public/
│   ├── 🌐 index.html               # Antara muka papan pemuka web
│   ├── 🎨 style.css                # Gaya papan pemuka
│   ├── 📁 js/
│   │   └── ⚡ main.js               # JavaScript papan pemuka
│   └── 🗂️ index_backup.html        # Fail sandaran
│
└── 📁 session/                     # Data sesi WhatsApp (PERIBADI)
    ├── 🔑 creds.json               # Kredensial WhatsApp
    ├── 🔐 app-state-sync-*.json    # Kunci penyegerakan keadaan
    ├── 🔐 pre-key-*.json           # Kunci pra-penyulitan
    └── 📱 session-*.json           # Data sesi setiap peranti
```

> ⚠️ **Folder `session/` dan fail `.env` mengandungi data sensitif** dan tidak akan di-upload ke Git (dilindungi oleh `.gitignore`)

## ⚙️ Konfigurasi AI Providers

### 🔑 Mendapatkan Kunci API

#### Pilihan A: OpenAI (Disyorkan)
1. 🌐 Lawati [OpenAI Platform](https://platform.openai.com/api-keys)
2. ➕ Buat kunci API baru
3. 📝 Tetapkan `AI_PROVIDER=openai` dan `OPENAI_API_KEY=sk-proj-...`
4. 💰 Pastikan akaun mempunyai kredit/bil

#### Pilihan B: OpenRouter (Multi-Model)
1. 🌐 Lawati [OpenRouter](https://openrouter.ai/)
2. 📝 Daftar dan dapatkan kunci API
3. 📝 Tetapkan `AI_PROVIDER=openrouter` dan `OPENROUTER_API_KEY=sk-or-...`
4. 🤖 Pilih model di `OPENROUTER_MODEL_NAME` (contoh: `anthropic/claude-3-haiku`)

#### Pilihan C: Google Gemini
1. 🌐 Lawati [Google AI Studio](https://aistudio.google.com/)
2. 🔑 Dapatkan kunci API untuk Gemini
3. 📝 Tetapkan `AI_PROVIDER=gemini` dan `GEMINI_API_KEY=...`

### 🎯 Khasiat System Prompt

#### Melalui Papan Pemuka Web (Disyorkan) ✅
1. 🌐 Buka `http://localhost:3002`
2. 🔽 Scroll ke bahagian **"🧠 AI Configuration"**
3. ✏️ Edit textarea dan klik **"💾 Update Prompt"**

#### Via File 📁
Edit file `config/system-prompt.txt` dan restart server

#### Via Environment Variable 🔧
Set `SYSTEM_PROMPT` di file `.env`

### 🚫 Konfigurasi Blacklist

#### Via File (Automatic) 📁
- File `config/blacklist-words.txt` dibuat otomatis dari `BLACKLIST_WORDS` di `.env`
- Format: kata dipisah koma (spam,iklan,judi,dll)

#### Via Environment Variable 🔧
Edit `BLACKLIST_WORDS` di file `.env`

## 🔧 Troubleshooting

### ❌ QR Code Tidak Muncul
```bash
# Cek apakah server berjalan di port yang benar
lsof -i :3002  # Linux/Mac
netstat -ano | findstr :3002  # Windows

# Solusi:
1. ✅ Pastikan server berjalan di port yang benar
2. 🌐 Cek console browser untuk error
3. 🔄 Restart server dan coba lagi
4. 🔒 Pastikan firewall tidak memblokir port
```

### ❌ Bot Tidak Merespon Pesan
```bash
# Debug steps:
1. ✅ Pastikan API key valid dan memiliki credit
2. 📋 Cek logs di terminal untuk error
3. 🤖 Periksa system prompt sudah dikonfigurasi
4. 🚫 Cek apakah pesan terkena blacklist filter
5. 📱 Pastikan WhatsApp masih terkoneksi
```

### ❌ Error "Cannot find module"
```bash
# Install ulang dependencies
rm -rf node_modules pnpm-lock.yaml  # Linux/Mac
rmdir /s node_modules & del pnpm-lock.yaml  # Windows

pnpm install
```

### ❌ Error Permission Denied (Termux/Linux)
```bash
# Pastikan Node.js dan PNPM terinstall dengan benar
pkg update && pkg upgrade  # Termux
pkg install nodejs-lts     # Termux

# Install PNPM global
npm install -g pnpm
```

### ❌ Baileys Connection Error
```bash
# Jika mengalami masalah koneksi WhatsApp:
1. 🗑️ Hapus folder session/
2. 🔄 Restart server
3. 📱 Scan QR code ulang
4. ⏳ Tunggu beberapa menit sebelum retry
```

### ❌ Socket.IO Connection Failed
```bash
# Jika dashboard tidak real-time:
1. 🌐 Refresh browser
2. 🔍 Cek browser console untuk error
3. 🚫 Disable ad-blocker jika ada
4. 🔄 Restart server
```

## 📊 Features Detail

### 🤖 AI Multi-Provider Support
- **OpenAI**: GPT-3.5-turbo, GPT-4, GPT-4-turbo
- **OpenRouter**: Claude, Llama, Mixtral, dll (50+ models)
- **Google Gemini**: Gemini Pro, Gemini Pro Vision
- **Auto-fallback**: Jika satu provider error, bisa switch otomatis

### 📱 WhatsApp Integration 
- **Baileys v6.7.17**: No Puppeteer, lebih stabil
- **Multi-device**: Support WhatsApp Web multi-device
- **Session persistent**: Auto-save login session
- **QR Code**: Tampil di dashboard web dan terminal

### 🛡️ Security & Filtering
- **Blacklist system**: Filter kata-kata terlarang
- **Rate limiting**: Prevent spam
- **XSS Protection**: DOMPurify untuk input sanitization
- **Environment protection**: `.env` dan `session/` tidak ter-upload ke Git

### 💬 Conversation Management
- **Memory per user**: Context tersimpan per nomor WhatsApp
- **History management**: Clear conversation via API
- **Message logging**: All messages logged for debugging
