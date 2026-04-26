# Çapraz Kontrol — Multi-LLM Cross Code Review

> **Aynı kodu Claude + Gemini + Codex + Qwen ile birden çok AI'ya inceletip birleşik rapor üretir.**
> *Cross-checks the same source file across Claude, Gemini, OpenAI Codex and Qwen CLIs and merges their reviews into one Markdown report.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/node-%E2%89%A518-green)](https://nodejs.org)

---

## 🤔 Neden?

Tek bir LLM yanılır. İki LLM aynı şeyde yanıldıysa **endişelen.** Üç bağımsız LLM aynı bug'a parmak basıyorsa o bug **kesinlikle gerçek**.

Çapraz Kontrol, bir kod dosyasını **eş zamanlı olarak** birden fazla LLM CLI'ına gönderir, her birinin görüşünü ayrı bölümlerde birleştirir, böylece:

- ✅ **Yanlış pozitifleri** filtreleyebilirsiniz (sadece bir AI uyarıyorsa, dikkatli ol)
- ✅ **Kör noktaları** yakalarsınız (Claude görmedi ama Gemini gördü)
- ✅ **Stil/perspektif** farklılıklarını öğrenirsiniz (Codex daha pragmatik, Claude daha akademik)

---

## ✨ Özellikler

- 4 LLM CLI desteği: **Claude Code**, **Gemini CLI**, **OpenAI Codex CLI**, **Qwen** (`llxprt` ile)
- Her CLI seçimli açılıp kapatılabilir (`config.json` → `aktifAIlar`)
- Tek dosya CLI — kurulumdan 30 saniye sonra `ck dosya.js`
- 5 kategori değerlendirme: kod kalitesi, bug, güvenlik, performans, öneri
- Her bulguya kritiklik etiketi (🔴 Kritik / 🟡 Orta / 🟢 Düşük)
- Birleşik Markdown raporu, otomatik `raporlar/` klasörüne kaydedilir
- Türkçe çıktı (uluslararası kullanım için `REVIEW_PROMPT` İngilizceye çevrilebilir)

---

## 💻 Platform Desteği

| Platform | Durum |
|---|---|
| Windows 10/11 | ✅ Tam destek |
| macOS / Linux | ⚠️ Test edilmedi — büyük olasılıkla çalışır (v1.0 `spawnSync` artık `shell: false` ile doğrudan executable çağırıyor; cmd/sh farkı kaldırıldı) |

> Bu projeyi macOS/Linux'ta çalıştırırsanız sonucu issue olarak bildirebilirsiniz. v1.1 ile resmî cross-platform CI eklenecek.

## 🚀 Kurulum

```bash
git clone https://github.com/mustafayilmazart/kesif-capraz-kontrol
cd capraz-kontrol
npm install
npm link        # 'ck' komutu global PATH'e eklenir
```

### Ön Koşullar

`PATH`'inizde aşağıdaki CLI'lardan **istediklerinizin** kurulu olması yeterli:

| CLI | Komut | Lisans |
|---|---|---|
| Claude Code | `claude` | © Anthropic |
| Gemini CLI | `gemini` | © Google |
| OpenAI Codex CLI | `codex` | © OpenAI |
| Qwen (llxprt) | `llxprt` | Apache 2.0 |

`config.json` içinde sadece kuruluları `true` yapın:

```json
{
  "aktifAIlar": { "claude": true, "gemini": true, "codex": false, "qwen": false },
  "envFile": ".env",
  "timeoutMs": 120000,
  "qwenCommand": "llxprt",
  "qwenModel": "qwen3-max"
}
```

### API Anahtarları

```bash
cp .env.example .env
# ANTHROPIC_API_KEY, GEMINI_API_KEY, OPENAI_API_KEY, QWEN_API_KEY
```

---

## 📖 Kullanım

```bash
# Tek dosya
ck path/to/file.js

# Veya
node capraz-kontrol.js path/to/file.js

# Veya npm script
npm run review -- path/to/file.js
```

Sonuç: `raporlar/file_2026-04-21.md` benzeri bir markdown dosyası.

### Örnek Çıktı

```markdown
# Çapraz AI Code Review — auth.js

## 🔵 Claude Code
🔴 Kritik: 12. satırda JWT secret hardcoded — `.env`'e taşı...

## 🟢 Gemini
🔴 Kritik: bcrypt salt rounds 8 — minimum 10 olmalı...

## 🟣 Codex
🟡 Orta: callback hell — async/await refactor önerilir...

## 📊 Birleşik Bulgular
- 2/3 AI: hardcoded secret (KESİN BUG)
- 1/3 AI: bcrypt rounds (ARAŞTIR)
```

---

## ⚠️ Sorumluluğunuz / Disclaimers

**LLM güvenilirliği:** LLM'ler yanılabilir. Bu araç **karar verici değil, fikir verici**dir. Üretim koduna asla AI önerisini körü körüne uygulamayın.

**API key güvenliği:** `.env` dosyanızı **asla commit etmeyin** (`.gitignore`'da zaten var). Bu araç yüklü key'leri **tüm aktif CLI'lara** geçirir; provider izolasyonu istiyorsanız sadece o provider'ı `aktifAIlar`'da `true` yapın. Per-CLI key isolation v1.1 roadmap'inde.

**Komut yürütme:** Bu araç sisteminizde harici LLM CLI'larını çağırır (`spawnSync`). `config.json` dosyanızı **kendi makinenizde tutun**, yetkisiz kişilerin düzenlemesine izin vermeyin (komut isimleri config'ten okunduğu için kötü niyetli config = komut yürütme).

---

## 📚 Atıflar

[ATTRIBUTIONS.md](ATTRIBUTIONS.md)

---

## 📄 Lisans

MIT — bkz. [LICENSE](LICENSE).
