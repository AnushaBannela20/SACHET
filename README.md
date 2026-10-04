# Sachet 🛡️ – Investment Message Checker

**Built for SANGYAN Hackathon (IIT BHU × SEBI × NSDL)**
**Tracks:** A – Digital Fraud & Scam Resilience · E – Misinformation & Content Literacy

> Participant Name: `<ANUSHA BANNELA>`

## 1. The problem

Retail investing in India is growing fast, especially among first-time investors in Tier-2 and Tier-3 cities. Many of them first meet "investing" through a forwarded WhatsApp or Telegram message, not through a broker. These messages follow repeatable fraud patterns:

- Guaranteed or "risk-free" returns
- Private "VIP" groups with "sure-shot" calls
- Fake trading apps installed from an APK link
- Requests for OTPs, upfront fees or "withdrawal tax"
- Fake claims of SEBI approval, "institutional accounts" or guaranteed IPO allotment

The victims are often people with limited financial or digital literacy, or elderly relatives, who are rushed into acting before they can verify anything. Existing tools are mostly English-only, require an account or an app install, or ask users to upload personal data.

## 2. Who it is for

| User                                       | Why Sachet helps |

| First-time investor, Tier-2/3 city         | Gets a plain-language reason for every warning |
| Hindi-speaking user                        | Full Hindi interface, Hindi and Hinglish pattern matching, voice input and read-aloud |
| Elderly investor or family member          | Large buttons, simple wording, "show this to a family member" step |
| Young investor following social media tips | Learns which tricks tip-sellers use |

## 3. What it does

1. The user pastes a suspicious message (or speaks it).
2. Sachet checks it against 14 known scam-pattern rules.
3. It shows a **warning level (0–100)** with a Low / Medium / High verdict.
4. Each flagged pattern comes with a short explanation of *why* it is risky.
5. It lists concrete next steps: do not pay or share OTP, verify registration on the SEBI site, call **1930**, report at cybercrime.gov.in, complain on SEBI SCORES.
6. The result can be read aloud in English or Hindi.

It always states its limits: a low score does **not** mean a message is safe.

## 4. Architecture

```
User (text / voice)
      │
      ▼
Browser Web Speech API (optional voice → text)
      │
      ▼
Rule engine: 14 weighted regex patterns (English / Hindi / Hinglish)
      │
      ▼
Risk scorer: score = 100 × (1 − e^(−Σweights / 6))
      │
      ▼
Bilingual explanation + next-step links + read-aloud (SpeechSynthesis)
```

- **Single file:** `index.html` (about 15 KB, no framework, no build step, no backend).
- **Runs 100% on the device.** There is no server, database, analytics or network call after the page loads.
- **Scoring:** each rule has a weight from 1 to 4. Weights are summed and passed through a saturating curve so that several independent red flags raise the score quickly while a single weak flag does not. Verdict thresholds: below 25 = Low, 25–59 = Medium, 60 or above = High.
- **Explainability:** every score is traceable to the exact rules that fired.

## 5. Technology and third-party components

| Component | Use | Cost |
|---|---|---|
| HTML, CSS, vanilla JavaScript | App and UI | Free |
| Web Speech API (`SpeechRecognition`) | Voice input (Chrome / Edge) | Browser built-in |
| Web Speech API (`speechSynthesis`) | Read-aloud | Browser built-in |
| GitHub Pages / Netlify | Hosting | Free |

**No external APIs, libraries, datasets, trackers or fonts are used.** Fonts fall back to system fonts, including system Devanagari fonts.

## 6. Guardrail compliance

| Hackathon rule | How Sachet complies |
|---|---|
| No stock tips or buy/sell/hold advice | It never recommends any security. Messages that *contain* tips are flagged as risky |
| No price or outcome predictions | None |
| No speculative-investment systems | It is a protection tool only |
| No promotion of brokers, products or schemes | None. Links go only to regulators and government portals |
| No monetisation, commissions or upsells | None, and no ads |
| No OTP / SMS / sensitive-data collection | Nothing is collected or sent. The UI tells users not to paste OTPs, passwords or card numbers |
| Privacy-by-design | On-device processing, no storage, no accounts |
| Communicate uncertainty | A disclaimer appears with every result |

## 7. Accessibility and Bharat-first design

- English and Hindi interface, with Hindi and Hinglish scam phrases recognised.
- Voice input and read-aloud for users who find typing or reading difficult.
- Works on low bandwidth: one small file, no images, no external requests.
- Large tap targets (44 px minimum), visible keyboard focus, light/dark theme, respects reduced-motion settings.
- Plain-language explanations written for non-experts.

## 8. Run it locally

```bash
git clone <https://github.com/AnushaBannela20/SACHET>
cd sachet
# open index.html in Chrome or Edge. No installation needed.
```

Optional local server:

```bash
python3 -m http.server 8000
# visit http://localhost:8000
```

## 9. Example

**Input**
> Dear investor, join our VIP WhatsApp group. Sure shot multibagger call, buy above ₹120 target ₹300. Guaranteed 40% profit in 15 days, SEBI registered. Only 5 slots left, act now! Download our app: trading-pro.apk

**Output (summary):** High warning level. Flags include guaranteed returns, private group push, specific buy/target tip, claimed SEBI approval, urgency, and APK install request, each with an explanation and next steps.

## 10. Limitations 

- Detection is **rule-based**, not machine learning. It can miss scams phrased in new ways and can flag harmless messages that happen to contain trigger words.
- Only English and Hindi (with Hinglish) are supported in this prototype.
- Voice input depends on browser support; Hindi voice quality varies by device.
- It has not been validated on a large labelled dataset yet.
- It cannot check whether a phone number, link or registration number is genuine. It tells users how to verify independently.

## 11. Roadmap and scalability

1. **More languages:** Tamil, Telugu, Bengali, Marathi, Gujarati, Kannada. Only the pattern and explanation text changes.
2. **Rules as open data:** Move rules into a community-maintained JSON file so regulators, investor associations and researchers can contribute.
3. **Offline PWA:** Installable on low-end Android phones with a service worker.
4. **On-device ML layer:** A small multilingual classifier (for example IndicBERT via transformers.js) to catch paraphrased scams, with the rule engine kept as the explainable baseline.
5. **Labelled test set:** 200+ real scam and safe messages to measure precision and recall.
6. **Distribution:** WhatsApp-shareable link, investor-awareness campaigns, and possible integration with regulator and depository awareness channels.
7. **Family mode:** A "send result to a trusted contact" option for elderly users.

## 12. Disclaimer

Sachet is an educational investor-protection prototype. It does not provide legal or financial advice and cannot guarantee that any message is genuine or fraudulent. Always verify through official channels: SEBI (sebi.gov.in), SEBI SCORES (scores.sebi.gov.in), the National Cyber Crime Portal (cybercrime.gov.in) or helpline 1930.

## 13. Licence

MIT. See `LICENSE`.
