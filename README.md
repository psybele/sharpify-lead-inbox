# Lead Inbox — Sharpify

Praktiskā atlases uzdevuma risinājums latviešu valodā. Autors: **Mārcis Sidorčiks**, izstrādāts ar AI palīdzību.

## Dzīvā versija

- [Publiskā pieteikuma forma](https://sharpify-lead-inbox-psybele.vercel.app/)
- [PIN aizsargāts pārdevēja inbox](https://sharpify-lead-inbox-psybele.vercel.app/inbox)
- [Demonstrācija un aktuālais savienojumu statuss](https://sharpify-lead-inbox-psybele.vercel.app/demo)
- [Īss nodošanas apraksts](HANDOVER_LV.md)

PIN un Telegram grupas uzaicinājuma saiti projekta īpašnieks nosūta vērtētājiem atsevišķi. Tos un API atslēgas publiskajā repozitorijā neglabā.

## Pilns aktuālais pirmkods

**[Lejupielādēt lead-inbox-source.zip](lead-inbox-source.zip)** — pilna lietotne, API, datubāzes migrācijas, testi, VS Code uzdevumi un dokumentācija. Izpakojot veidojas mape `lead-inbox`. Šajā repozitorijā pirmkods tiek nodots ZIP formā; klonēšana arhīvu automātiski neizpako.

Arhīvs un šī dokumentācija ir aktualizēti 2026. gada 19. novembrī pēc demonstrācijas inbox sakopšanas, atjaunojama arhīva, mobilās izvēlnes, Telegram savienojumu un klienta apstiprinājuma pārbaudēm. Pārbaudes kontrolsumma: [SHA256SUMS.txt](SHA256SUMS.txt). [Tā pati lejupielāde no dzīvās vietnes](https://sharpify-lead-inbox-psybele.vercel.app/lead-inbox-source.zip).

## Kas darbojas

| Prasība | Īstenojums |
| --- | --- |
| Publiskā forma | Visi 7 prasītie lauki, validācija un saglabāšanas apstiprinājums; pielāgota telefonam. |
| Datubāze | Cloudflare D1; pieteikums saglabājas pirms AI un ārējiem paziņojumiem. |
| Inbox | Servera PIN, HttpOnly sesija, jaunākie līdi augšā, atjaunošanās ik pēc 5 s. |
| Statusi | Jauns, Piezvanīts, Tikšanās norunāta, Nav aktuāls; saglabātas sarunas piezīmes. |
| AI | Groq: Karsts / Silts / Auksts, iemesls un sarunas sākuma jautājums. |
| Ārējie paziņojumi | Telegram grupā un atsevišķi pārdevēja e-pastā ar Resend. |
| Klienta apstiprinājums | Atsevišķa Resend ziņa uz formas e-pastu; dzīvajā konfigurācijā sūtītājs no `lead.psybele.com`. |
| Mobilā navigācija | Poga “Izvēlne” atver to pašu sānu paneli ar statusiem, skaitītājiem un savienojumiem. |
| Demonstrācija un arhīvs | Trīs skaidri marķēti DEMO piemēri ar īstiem AI vērtējumiem; agrākie pārbaudes ieraksti saglabāti atjaunojamā arhīvā. |
| Bonusi | Zvanīt, numura kopēšana, līda vecums, meklēšana, filtri, prioritāšu kārtošana un AI sarunas sākums. |

Savienojumu testa pogas ir pieejamas uzreiz un automātiski saglabā tikai savu sadaļu. Telegram grupas izvēle saglabājas uzreiz. Grupas pārveidošanas par supergrupu gadījumā sūtītājs apstrādā Telegram norādīto jauno ID; jau pieņemtu ziņu neatkārto. Discord sadaļa ir noņemta.

## Demonstrācijas dati

Aktīvajā inbox ir trīs skaidri marķēti DEMO piemēri: Karsts, Silts un Auksts. Izmantoti izdomāti kontakti un īsti iepriekš iegūti Groq vērtējumi; sākotnējie mērķi, AI atbildes un laiki nav pārrakstīti. Divpadsmit vecie pārbaudes ieraksti saglabāti PIN aizsargātā arhīvā. Tos var atjaunot no detaļu skata. Jauni pieteikumi nonāk aktīvajā inbox.

## Pārbaude

Gala QA ar izdomātiem uzņēmuma datiem pabeidza visas četras fāzes: AI, Telegram, pārdevēja e-pastu un klienta apstiprinājumu. Telegram pieņēma līda ziņu **1,046 s**, klienta apstiprinājumu Resend pieņēma **0,427 s** pēc saglabāšanas. Klienta pārbaudē izmantota Resend paredzētā testa adrese. QA līds pēc tam atzīmēts “Nav aktuāls” un pārvietots atjaunojamā arhīvā.

Tas ir viena testa novērojums; servisa pieņemšana neapliecina ziņas izlasīšanu vai nonākšanu cilvēka e-pasta iesūtnē. [Pilni pārbaudes nosacījumi un zināmās robežas](VERIFICATION.md).

Iziet **60 automātiskie testi**, TypeScript pārbaude un produkcijas build. Ārējie servisi automātiskajos testos ir simulēti; reālie API testi aprakstīti atsevišķi. Pārlūkā pārbaudīti gan datora, gan telefona platuma skati, statusu izvēle, arhivēšana, atjaunošana un mobilās izvēlnes aizvēršana.

## Tehnoloģijas un izvēles

React, TypeScript, Vinext, Vercel, Cloudflare Workers/D1, Groq Chat Completions, Telegram Bot API un Resend. Modeli `openai/gpt-oss-20b` darbina Groq, izmantojot Groq atslēgu un pieejamo Free kvotu. Uz maksas OpenAI kļūmes gadījumā nepārslēdzas.

Vercel ir publiskā adrese un API vārteja. Dati, PIN sesijas, šifrētie integrāciju iestatījumi un fona apstrāde paliek esošajā Cloudflare Worker/D1 serverī. Tam jāpaliek publicētam. Vārteja izmanto HMAC parakstītu pieprasījuma kontekstu un saglabā katra klienta PIN mēģinājumu limitu. GitHub atjauninājums pats nepārpublicē dzīvo lietotni.

Demonstrācijā ir kopīgs komandas PIN; plašākai lietošanai vajadzīgi individuāli konti un administratora loma. Groq un Resend kontu kvotas un ārējo servisu pieejamība var ietekmēt piegādi. Kļūmes ir redzamas inbox; pieteikumi paliek saglabāti.

## Palaist MacBook / VS Code

Vajadzīgs Node.js 24 un npm. Izpako ZIP un VS Code atver `lead-inbox` mapi, kur atrodas `package.json`. Terminālī izpildi:

```bash
npm ci
npm run local:setup
npm run build
npm run db:local
npm run dev
```

`local:setup` parāda atsevišķu lokālo PIN. `db:local` izpildi tikai vienreiz jaunai, tukšai lokālajai datubāzei. Nākamreiz pietiek ar `npm run dev`. Lokālā datubāze un savienojumi ir atsevišķi no dzīvajiem.

Klonējot šo repozitoriju, papildus vajadzīga izpakošana:

```bash
git clone https://github.com/psybele/sharpify-lead-inbox.git
cd sharpify-lead-inbox
unzip lead-inbox-source.zip
cd lead-inbox
```

Pēc tam izpildi iepriekšējās npm komandas. Savus labojumus var pievienot ārējam repozitorijam kā parastu `lead-inbox` apakšmapi; saglabā projekta `.gitignore` un nepievieno atslēgas, `.dev.vars` vai lokālo datubāzi. Sīkāk — arhīva `docs/VS_CODE.md`, `docs/VERCEL.md` un `docs/CUSTOMER_CONFIRMATION.md`.

```bash
npm test
npm run check
npm run build
```
