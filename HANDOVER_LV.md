# Lead Inbox — nodošanas apraksts

Sveiki! Nosūtu izstrādāto Lead Inbox praktisko uzdevumu. Demonstrāciju iesaku sākt ar pārdevēja inbox, bet formu atvērt otrā cilnē, lai redzētu jauna pieteikuma apstrādi.

- Demonstrācija un aktuālais savienojumu statuss: https://sharpify-lead-inbox-psybele.vercel.app/demo
- Forma: https://sharpify-lead-inbox-psybele.vercel.app/
- Pārdevēja inbox: https://sharpify-lead-inbox-psybele.vercel.app/inbox
- PIN: nosūtāms vērtētājiem atsevišķi.
- Paziņojumi: Telegram grupā “Sharpify-Līdi”; grupas uzaicinājuma saiti pievieno privātajai nodošanas ziņai.
- Kods: https://github.com/psybele/sharpify-lead-inbox
- Pilns pirmkods: https://sharpify-lead-inbox-psybele.vercel.app/lead-inbox-source.zip

Formā ir visi septiņi prasītie lauki. Pieteikums vispirms saglabājas datubāzē, pēc tam saņem AI vērtējumu “Karsts / Silts / Auksts” ar vienu pamatojuma teikumu. PIN aizsargātajā inbox jaunākie līdi ir augšā, saraksts atjaunojas ik pēc piecām sekundēm, un var mainīt visus četrus prasītos statusus. Paziņojumi nonāk Telegram grupā un pārdevēja e-pastā; klients saņem atsevišķu pieteikuma apstiprinājumu.

Papildus pievienoju zvana pogu, līda vecumu, meklēšanu, filtrus, prioritāšu kārtošanu, AI sarunas sākuma jautājumu un saglabājamas piezīmes. Telefonā pieejama sānu izvēlne ar tiem pašiem statusiem un savienojumiem. Sākuma skatā ir trīs skaidri marķēti DEMO līdi ar iepriekš iegūtiem īstiem AI vērtējumiem; vecie pārbaudes ieraksti saglabāti atjaunojamā arhīvā.

Izmantoju React/TypeScript, Vercel, Cloudflare Workers/D1, Groq AI, Telegram un Resend. D1 izvēlējos noturīgai glabāšanai, savukārt Groq — AI novērtēšanai ar pieejamo bezmaksas kvotu. Grūtākais bija nodrošināt, lai ārējo servisu kļūmes nepazaudē pieteikumu un neveiksmīgo apstrādi var atkārtot. Gala pārbaudē Telegram pieņēma līda ziņu 1,046 sekundēs; klienta apstiprinājumu Resend pieņēma 0,427 sekundēs. Tie ir viena testa rezultāti, nevis piegādes garantija.

Iziet 60 automātiskie testi, TypeScript pārbaude un produkcijas build. Pilnais pirmkods, migrācijas, testi un VS Code palaišanas instrukcijas ir pieejami GitHub un ZIP arhīvā.

Izstrādes laiks: [projekta autors ieraksta faktisko stundu skaitu — uzdevumā tas ir prasīts].

Mārcis Sidorčiks

---

Šis ir publisks teksta pamats. PIN, API atslēgas un privātas grupas uzaicinājuma saite šajā failā nav iekļauti. Pilni pārbaudes nosacījumi un ierobežojumi: `docs/VERIFICATION.md`.
