# Lead Inbox — īsais nodošanas apraksts

Sveiki! Nosūtu Lead Inbox praktisko uzdevumu.

- Forma: https://sharpify-lead-inbox-psybele.vercel.app/
- Pārdevēja inbox: https://sharpify-lead-inbox-psybele.vercel.app/inbox
- PIN: nosūtāms vērtētājiem atsevišķi.
- Paziņojumi: Telegram grupā “Sharpify-Līdi”; grupas uzaicinājuma saiti pievieno privātajai nodošanas ziņai.
- Kods: https://github.com/psybele/sharpify-lead-inbox
- Pilns pirmkods: https://sharpify-lead-inbox-psybele.vercel.app/lead-inbox-source.zip

Ir visi prasītie formas lauki, pastāvīga datubāze, PIN aizsargāts inbox ar četriem statusiem un AI vērtējums “Karsts / Silts / Auksts” ar iemeslu. Papildus pievienoju zvana pogu, līda vecumu, meklēšanu, filtrus, sarunas sākuma teikumu, piezīmes, mobilo sānu izvēlni un klienta e-pasta apstiprinājumu.

Izmantoju React/TypeScript, Vercel, Cloudflare Workers/D1, Groq AI, Telegram un Resend. Grūtākais bija savienot ārējos servisus tā, lai kļūmes nepazaudē pieteikumu un neveiksmīgo apstrādi var atkārtot. Gala pārbaudē Telegram pieņēma līda ziņu aptuveni sekundē; klienta apstiprinājumu Resend pieņēma 0,427 sekundēs. Tas ir viena testa rezultāts.

Izstrādes laiks: [projekta autors ieraksta faktisko stundu skaitu — uzdevumā tas ir prasīts].

Mārcis Sidorčiks

---

Šis ir publisks teksta pamats. PIN, API atslēgas un privātas grupas uzaicinājuma saite šajā failā nav iekļauti. Pilni pārbaudes nosacījumi un ierobežojumi: `docs/VERIFICATION.md`.
