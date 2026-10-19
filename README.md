# Payment reminders

Who paid, who is late, and the reminder already written. The bank file is matched to the invoice list. Every overdue invoice gets a reminder draft in Swedish or English, with the exact amount, fee and interest. A person approves each draft. Nothing is sent by the machine.

**Try it:** https://anas-qatanani.github.io/payment-reminders/

**Watch it (32 s):** [demo.mp4](https://anas-qatanani.github.io/payment-reminders/demo.mp4)

The firm (Kustby Ventilation AB, 12 people), its customers, invoices and payments are all made up. No client data.

## Result on 64 test invoices

| Check | Result |
|---|---|
| Wrong amounts (left to pay, fee, amount in the draft) | **0** |
| Bank payments placed right (or sent to a person when unclear) | 29 / 29 |
| Invoice status right (paid, not due, overdue, held, ...) | 64 / 64 |
| Reminder tone right (friendly, firmer, last notice) | 64 / 64 |
| Reminders drafted | 22 |
| Invoices sent to a person instead | 8 |
| Tampered drafts caught by the number check | 22 / 22 drafts, 468 / 468 bad copies |

Every expected answer was written by hand per case, not by the program.

## How it works
1. **Read.** The invoice list (a CSV export like Fortnox or Visma: invoice no, customer, email, language, amount incl. moms, due date, paid amount, paid date) and the bank's payment file. Amounts are kept exact to the öre.
2. **Match.** A valid OCR reference first. Then invoice numbers in the payment message, also one payment for two invoices. A known customer who used an old OCR is moved only when exactly one open invoice fits. Anything else goes to a person.
3. **Find late.** What is left on each invoice and how many days late it is. 1–14 days: friendly. 15–30: firmer. 31 or more: last notice. Disputed invoices, overpayments, tiny rests and customers with an unplaced payment go to a person.
4. **Draft and check.** One reminder per invoice, in the customer's language. A check proves that every number, date and reference in the draft equals the books, and that no other number is in it.

All numbers come from plain code, never from an AI model. An optional local model (Gemma 4 E4B) may reword the text. Its text is used only if the number check passes. In the test run it reworded all 22 drafts and kept every number.

## Swedish rules it follows
- **Reminder fee 60 kr** only when the customer agreed to it, at the latest when the debt arose. Lag (1981:739) om ersättning för inkassokostnader m.m., 2 § and 4 §.
- **Late payment interest** (dröjsmålsränta) = referensränta + 8 procentenheter, from the due date, on what is left each day. Räntelagen (1975:635), 3 §, 6 § and 9 §.
- **Referensränta** 2,00 % from 1 July 2026 (Riksbanken, checked 19 October 2026), so the interest is 10 % per year.

Sources: [riksdagen.se, lag 1981:739](https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/lag-1981739-om-ersattning-for-inkassokostnader_sfs-1981-739/) · [riksdagen.se, räntelag 1975:635](https://www.riksdagen.se/sv/dokument-och-lagar/dokument/svensk-forfattningssamling/rantelag-1975635_sfs-1975-635/) · [riksbank.se, referensränta](https://www.riksbank.se/sv/statistik/rantor-och-valutakurser/referensranta/). This is not legal advice.

## How it maps to a real firm
| Demo | Real setup |
|---|---|
| `invoices.csv` | Invoice export or API from the accounting program |
| `bank.csv` | The bank's payment file (for example Bankgirot) |
| Draft on the page | Draft saved in the mail program; a person presses Send |
| One laptop | One office computer; nothing in the cloud |

## How it was built
Python standard library only, no cloud, no API costs. The page runs offline in the browser and plays one real run. The source code is private. Ask on a call if you want to see it.

Gmail is a trademark of Google LLC. The video only shows where a draft would wait; it is not connected to Google.

---
Built by Anas Qatanani · [AI-Driven](https://www.linkedin.com/company/ai-driven-tech) · [Book a 15-min call](https://calendly.com/anas-aidriven/15min)
