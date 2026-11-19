# Lead Inbox — Sharpify

Latviešu valodā veidots praktiskā atlases uzdevuma risinājums. Autors: **Mārcis Sidorčiks**, izstrādāts ar AI palīdzību.

## Pilns pirmkods

**[Lejupielādēt pilnu projekta pirmkodu](lead-inbox-source.zip)** — arhīvā ir visa lietotne, servera API, datubāzes migrācijas, testu faili un VS Code uzdevumi. Izpakojot veidojas mape `lead-inbox`. Šajā repozitorijā pirmkods tiek nodots kā pilns ZIP arhīvs.

Arhīvs atbilst publicētās vietnes 9. versijas pirmkodam (`6fd429571f80cfb7c58b61af3f38459e9e385203`). Privātie pieteikumi, PIN, sesijas un API atslēgas arhīvā nav iekļautas.

## Dzīvā demonstrācija

- [Pārdevēja demonstrācija un reālais savienojumu statuss](https://sharpify-lead-inbox-psybele.vercel.app/demo)
- [Publiskā pieteikuma forma](https://sharpify-lead-inbox-psybele.vercel.app/)
- [PIN aizsargāts inbox](https://sharpify-lead-inbox-psybele.vercel.app/inbox)
- [Pirmkoda lejupielāde no vietnes](https://sharpify-lead-inbox-psybele.vercel.app/kods)

PIN vērtētājs saņem atsevišķi. Publiskā forma ir tikai sākums; demonstrāciju sāc pārdevēja inbox un formu atver otrā cilnē.

## Kas darbojas

| Prasība | Risinājums |
| --- | --- |
| Mobilā forma | Visi 7 uzdevumā prasītie lauki; validācija un saglabāšanas apstiprinājums. |
| Datu saglabāšana | Cloudflare D1; līds saglabājas pirms ārējo servisu apstrādes. |
| Pārdevēja inbox | Servera PIN, droša sesija, jaunākie līdi augšā, automātiska atjaunošana ik pēc 5 s. |
| Statusi | Jauns, Piezvanīts, Tikšanās norunāta, Nav aktuāls; saglabātas piezīmes. |
| AI vērtējums | Groq API: Karsts/Silts/Auksts, iemesls un sarunas sākums. |
| Paziņojumi | Telegram un papildus Resend e-pasts; atsevišķi apstrādes stāvokļi un atkārtošana. |
| Bonusi | Zvanīt ar `tel:`, numura kopēšana, līda vecums, meklēšana, filtri un prioritātes. |

2026. gada 19. novembrī publiskajā Vercel formā iesniegts atzīmēts QA pieteikums. AI vērtējums saglabāts **0,431 s**, Telegram ziņa pieņemta **0,625 s** un e-pasts pieņemts **0,654 s** pēc līda izveides serverī. Līds ir pirmais inbox; statusi un piezīmes saglabājās atsevišķās atkārtotās nolasīšanās. QA līds pēc pārbaudes atzīmēts `Nav aktuāls`.

Tā ir viena sekmīga testa pārbaude, nevis pieejamības garantija. Servisa pieņemšana nav ziņas izlasīšanas vai e-pasta iesūtnes piegādes apliecinājums. Skatīt [pilnu pārbaudes aprakstu un zināmās robežas](VERIFICATION.md).

## Atvērt VS Code un palaist uz MacBook

Vajadzīgs Node.js 24 un npm. Lejupielādē `lead-inbox-source.zip`, izpako un VS Code izvēlnē **File → Open Folder…** atver izpakoto `lead-inbox` mapi, kurā atrodas `package.json`.

Izvēlnē **Terminal → New Terminal** secīgi izpildi:

```bash
npm ci
npm run local:setup
npm run build
npm run db:local
npm run dev
```

`local:setup` parāda atsevišķu lokālo PIN. `db:local` izmanto tikai vienreiz jaunai, tukšai lokālajai datubāzei. Atver terminālī parādīto adresi. Nākamreiz pietiek ar `npm run dev`; serveri aptur ar **Control + C**.

Ja vispirms klonē šo repozitoriju, arhīvs vēl jāizpako:

```bash
git clone https://github.com/psybele/sharpify-lead-inbox.git
cd sharpify-lead-inbox
unzip lead-inbox-source.zip
cd lead-inbox
```

Pēc tam izpildi iepriekš norādītās `npm` komandas šajā `lead-inbox` mapē. VS Code uzdevumi atrodas izpakotā projekta `.vscode/tasks.json`, detalizētās instrukcijas — `docs/VS_CODE.md`. Lokālā datubāze un savienojumi ir atsevišķi no dzīvajiem.

## Pārbaudes

```bash
npm test
npm run check
npm run build
```

Pārbaudītajam risinājumam iziet 41 automātiskie testi, TypeScript un produkcijas build. Ārējie servisi automātiskajos testos ir simulēti; īsto atslēgu pārbaude ir aprakstīta atsevišķi.

## Tehnoloģijas un izvēles

React, TypeScript, Vinext, Cloudflare Workers/D1, Groq Chat Completions, Telegram Bot API un Resend. Groq modelis `openai/gpt-oss-20b` tiek darbināts Groq; tam izmanto Groq atslēgu. Nav automātiskas pārslēgšanās uz maksas OpenAI.

Publiskā adrese ir **Vercel**. Vercel piegādā lapas un Node.js API vārteju; noturīgie dati, PIN sesijas, šifrētie integrāciju iestatījumi un fona apstrāde paliek esošajā Cloudflare Worker/D1 serverī. Tas ir kopīgs risinājums, nevis pilna datubāzes migrācija uz Vercel. API vārteja izmanto HMAC parakstītu kontekstu, saglabā katra klienta PIN mēģinājumu limitu un privātas atbildes nekešo. Paziņojumu saites ved uz Vercel inbox. Esošajam serverim jāpaliek publicētam. Vercel paketi sagatavo ar `npm run package:vercel`; detaļas ir arhīva `docs/VERCEL.md`. GitHub kodu glabā atsevišķi; tā izmaiņas automātiski nemaina publicēto vietni.

Groq Free kvotai ir lietošanas limiti. Resend testa sūtītājs sūta uz Resend konta adresi; sūtīšanai uz citām adresēm vajag apstiprināt savu domēnu. Demonstrācijā izmantots kopīgs komandas PIN; plašākai lietošanai vajag individuālus lietotājus un administratora lomu.
