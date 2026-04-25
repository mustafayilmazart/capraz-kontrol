# Atıflar & Yararlanılan Kaynaklar

## Bağımlılıklar

Sadece Node.js standart kütüphaneleri (`child_process`, `fs`, `path`) kullanılır. Hiçbir 3. parti npm paketine bağımlı değildir.

## Entegre Edilen Üçüncü Taraf CLI'lar

Bu proje aşağıdaki LLM CLI araçlarını **dış süreç olarak çağırır** (subprocess). Hiçbirinin kodu içermez.

| Araç | Sahibi | Lisans | Repo |
|---|---|---|---|
| Claude Code | Anthropic PBC | Proprietary (kullanım: API key ile) | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| Gemini CLI | Google LLC | Apache 2.0 | [github.com/google-gemini/generative-ai-cli](https://github.com/google-gemini/) |
| OpenAI Codex CLI | OpenAI | MIT | [github.com/openai/codex](https://github.com/openai/codex) |
| llxprt (Qwen runner) | Topluluk | MIT | npm: `llxprt` |

> Her LLM'in kendi ToS'una ve API kullanım kotasına tabisiniz. Bu araç sadece prompt'unuzu CLI'a iletir; hiçbir model davranışını değiştirmez.

## İlham

Bu yaklaşım kısmen şu çalışmalardan ilham aldı:

- [**aider**](https://github.com/Aider-AI/aider) (Apache 2.0) — Multi-model AI coding workflow
- [**Continue.dev**](https://github.com/continuedev/continue) (Apache 2.0) — Side-by-side LLM kıyaslama UI
- **Wisdom of crowds** literatürü — Surowiecki, James (2004). *The Wisdom of Crowds.* Doubleday.

> Hiçbir kod kopyalanmamıştır; yalnızca "birden çok kaynağı kıyasla" mental modeli ödünç alındı.

## Güvenlik Durumu (v1.0)

### ✅ v1.0'da Implement Edildi

- **Argüman array'leri** — `spawnSync` shell yerine doğrudan executable + array args ile çağrılıyor (`shell: false`). Komut enjeksiyonu yüzeyi **sıfırlandı**.
- **Per-CLI env whitelist** — `_filteredEnv(provider)` her CLI'a yalnızca kendi `*_API_KEY`'ini ve temel sistem değişkenlerini geçirir. Cross-provider key leak **engellendi**.

### 🛣️ v1.1+ Roadmap

- **Config schema validation** — `config.json` JSON Schema ile doğrulansın (kötü niyetli alan engellensin)
- **Platform autodetect** — Linux/macOS için sh, Windows için cmd otomatik (mevcut sürüm Windows-first)
- **Dosya boyutu limit** — `fs.readFileSync` öncesi size kontrolü (büyük dosyalarda OOM koruması)

## Markalar

"Claude", "Gemini", "OpenAI", "Codex", "Qwen" ilgili sahiplerinin tescilli markalarıdır. Bu projede yalnızca tanımlama amaçlı (nominative fair use) kullanılmıştır.
