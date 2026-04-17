# 👻 Phantomoji

**Invisible prompt injection toolkit — encode hidden LLM instructions via Unicode Tags and emoji variation selectors.**

By [kryptokat](https://github.com/cfoudysec)

🔗 **Live tool:** [https://cfoudysec.github.io/phantomoji-tool/](https://cfoudysec.github.io/phantomoji-tool/)

## What This Does

Phantomoji lets you create text payloads that look innocent to humans but carry hidden instructions that LLMs can read. It uses two encoding techniques:

**Unicode Tag Characters (U+E0020–E007E)** — Converts ASCII text into characters that are completely invisible in any UI. Zero visual footprint. The text field looks empty, but an LLM processing it reads your hidden instructions.

**Emoji Variation Selectors (U+FE00–FE0F)** — Hides data inside normal-looking emoji. Each emoji carries one encoded ASCII character via attached variation selectors. The output looks like a row of smiley faces; the LLM reads your payload.

## Features

| Feature | Description |
|---------|-------------|
| 🏷️ **Encode Tags** | ASCII → invisible Unicode Tag characters |
| 🔍 **Decode Tags** | Detect and highlight hidden tags in pasted text |
| 😊 **Encode Emoji** | ASCII → data hidden inside emoji via variation selectors |
| 🧬 **Decode Emoji** | Extract hidden data from emoji payloads |
| ⚡ **Combine** | Merge visible text + hidden instructions into one payload |

## Use Cases

- **CTF competitions** — Craft indirect prompt injection payloads for authorized AI red-teaming challenges (Gray Swan Arena, Gandalf, etc.)
- **Security research** — Test whether LLM-integrated applications strip invisible Unicode before processing
- **Defense testing** — Verify your own AI products sanitize Unicode Tag characters and variation selectors from user input
- **Education** — Demonstrate invisible prompt injection to engineering teams and stakeholders

## How to Use

1. Visit the [live tool](https://cfoudysec.github.io/phantomoji-tool/)
2. Pick a tab based on what you need
3. Type your payload text
4. Click convert/encode
5. Copy the output to your clipboard
6. Paste into the target field

For the **Combine** tab: enter what humans should see in the first box, enter the hidden LLM instructions in the second box, choose your encoding method, and copy the combined output. One string, two messages.

## How It Works

### Unicode Tags

Unicode defines a block of "tag" characters (U+E0001–E007F) that map 1:1 to ASCII but render as invisible zero-width characters. They were originally intended for language tagging and are widely considered deprecated for that purpose — but LLMs trained on web data can interpret them as text.

```
ASCII:    H        e        l        l        o
Tags:     U+E0048  U+E0065  U+E006C  U+E006C  U+E006F
Visible:  (nothing)
```

### Emoji Variation Selectors

Variation selectors (U+FE00–FE0F) are Unicode characters that modify the presentation of the preceding character. By attaching two variation selectors to a carrier emoji, each selector encodes 4 bits (0–15), giving 8 bits (one ASCII byte) per emoji.

```
ASCII:    H (0x48)
High:     4 → U+FE04
Low:      8 → U+FE08
Payload:  😊︄︈
Visible:  😊 (just a smiley)
```

## Running Locally

No build step, no dependencies. Just open `index.html` in any browser.

```bash
git clone https://github.com/cfoudysec/phantomoji-tool.git
open phantomoji-tool/index.html
```

## Responsible Use

This tool is for **authorized security research and CTF competitions only**. Invisible prompt injection is a real attack vector with real consequences. Do not use this against systems you don't have explicit permission to test.

Legitimate uses include:
- Authorized red-team engagements
- CTF competitions (Gray Swan Arena, Gandalf, HackAPrompt, etc.)
- Testing your own AI products for Unicode sanitization
- Security research and responsible disclosure

## Defenses

If you're building AI products and want to protect against this attack:

- **Strip Unicode Tag characters** (U+E0000–E007F) from all user input before LLM processing
- **Strip variation selectors** (U+FE00–FE0F, U+E0100–E01EF) from untrusted input
- **Normalize Unicode** before processing — many invisible characters collapse under NFC/NFKC normalization
- **Log raw byte sequences** so hidden content is visible in security monitoring

## References

- [Tags (Unicode block)](https://en.wikipedia.org/wiki/Tags_(Unicode_block)) — Wikipedia
- [ASCII Smuggler](https://embracethered.com/blog/posts/2024/hiding-and-finding-text-with-unicode-tags/) — Johann Rehberger's research on invisible Unicode
- [rez0's Invisible Prompt Injection Playground](https://josephthacker.com/invisible_prompt_injection) — Joseph Thacker's tool
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — LLM01: Prompt Injection

## License

MIT — see [LICENSE](LICENSE) for details.

---

Built with assistance from [Claude](https://claude.ai) by Anthropic.
