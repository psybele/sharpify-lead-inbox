# Sharpify Lead Inbox — prasības un pārbaude

Izstrādāja Mārcis Sidorčiks ar AI palīdzību. Dzīvā pārbaude veikta 2026. gada 19. novembrī. Zemāk norādītie 0,813/1,225 sekunžu mērījumi veikti esošajā serverī pirms Vercel vārtejas pievienošanas; tie nav atsevišķs Vercel ātruma mērījums.

## Publiskās saites

- [Pārdevēja demonstrācija un aktuālais servera statuss](https://sharpify-lead-inbox-psybele.vercel.app/demo)
- [Pieteikuma forma](https://sharpify-lead-inbox-psybele.vercel.app/)
- [PIN aizsargāts inbox](https://sharpify-lead-inbox-psybele.vercel.app/inbox)
- [Pirmkoda lejupielāde](https://sharpify-lead-inbox-psybele.vercel.app/kods)

PIN vērtētājs saņem atsevišķi. API atslēgas, sesijas un pieteikumu datubāze pirmkodā nav iekļautas.

## Obligātās prasības

| Prasība | Īstenojums | Pārbaude |
| --- | --- | --- |
| Publiska forma ar 7 laukiem | Vārds, telefons, e-pasts, uzņēmums, nozare, mēneša apgrozījums, mērķis; servera un pārlūka validācija. | Publiskajā pārlūkā aizpildīta un iesniegta forma; saņemts apstiprinājums. Iepriekš pārbaudīts 390 px izkārtojums. |
| Dati saglabājas | Cloudflare D1; dati tiek saglabāti pirms AI un paziņojumu izsaukumiem. | Testa ieraksts un tā izmaiņas atkārtoti nolasītas ar atsevišķiem servera pieprasījumiem. |
| PIN inbox un 4 statusi | Servera autorizācija, HttpOnly sesija; jaunākie līdi augšā, automātiska atjaunošanās ik pēc 5 s. | Jaunais testa līds ir pirmais; pārbaudītas visas statusu pārejas un piezīmju saglabāšana. |
| Ārējs paziņojums mazāk nekā minūtē | Telegram Bot API; papildus neatkarīgs Resend e-pasts. | Telegram pieņēma testa līda ziņu 1,225 s pēc līda izveides. |
| Īsts AI vērtējums | Groq API, `openai/gpt-oss-20b`, stingra JSON shēma; Karsts/Silts/Auksts un iemesls. | Groq atgrieztais `Karsts`, iemesls un sarunas sākums saglabāti 0,813 s pēc līda izveides. |

Ātruma dati ir viena sekmīga testa mērījums serverī, nevis garantēts maksimālais laiks. Inbox pārlūkā datus nolasa ik pēc 5 s. Telegram laiks norāda API pieņemšanu, nevis ziņas izlasīšanu.

## Papildu funkcijas

- Zvana poga ar normalizētu `tel:` adresi un numura kopēšanu.
- Līda vecums minūtēs, meklēšana, statusu un prioritātes filtri, kārtošana.
- AI sarunas sākuma jautājums, kopēšana un pārdevēja piezīmes.
- Katras ārējās apstrādes fāzes stāvoklis un atkārtošana no inbox.
- Telegram un e-pasts var darboties vienlaikus, nepārtraucot otru kanālu viena kļūmes dēļ.

Zvana URI un mobilais panelis ir pārbaudīti. Attālinātā pārlūka protokola ierobežojums neļauj apliecināt īstā tālruņa zvana lietotnes atvēršanu; to pārbauda fiziskā telefonā. Ja lietotnes iebūvētais pārlūks neatver zvanu, atver saiti parastajā pārlūkā vai nokopē numuru.

## Testa pierādījumi

Publiski iesniegtā QA līda ID: `deeff6d9-8b5a-4666-b6f4-dc158a6e7f1c`. Nosaukums: `QA nodošana 19.11`; izmantoti izdomāti kontakti. Pēc pārbaudes statuss ir `Nav aktuāls` un pievienota piezīme nesazināties.

E-pasta iestatījumi tika saglabāti jau pēc šā testa līda iesniegšanas. Pēc tam ar to pašu līdu pārbaudīta e-pasta atkārtošana: Resend atgrieza pieņemtas ziņas ID. AI un Telegram netika sūtīti atkārtoti. Šis e-pasta tests nav sākotnējā iesnieguma ātruma mērījums. E-pasts izmanto Resend testa sūtītāju, tādēļ saņēmējs ir ierobežots ar Resend konta adresi; sūtīšanai uz citām adresēm jāapstiprina savs domēns.

## Automātiskā pārbaude

Pārbaudītajam risinājumam iziet 47 testi, TypeScript pārbaude un produkcijas build. Komandas:

```bash
npm test
npm run check
npm run build
```

Testi ietver autorizāciju, PIN minēšanas limitu, saglabāšanu, statusus un piezīmes, strukturētu AI atbildi, šifrētas atslēgas, atsevišķu paziņojumu kļūmes un atkārtošanu. Daļa darbojas īstajā Workers izpildvidē ar simulētiem ārējiem servisiem. Tie ir nodalīti no iepriekš aprakstītās īsto Groq/Telegram/Resend savienojumu pārbaudes.

## Zināmās robežas

- Kopīgs demonstrācijas PIN dod piekļuvi arī savienojumiem; plašākai komandai vajag individuālus kontus un administratora lomu.
- Groq Free kontam ir kvotas; lietotne nepārslēdzas uz maksas OpenAI un nepieslēdz maksas plānu.
- Ārējo servisu darbība un e-pasta piegāde nav garantēta. Kļūmes ir redzamas, līds paliek saglabāts un apstrādi var atkārtot.
- Fonā izmanto Worker `waitUntil`, bez atsevišķas pastāvīgas darbu rindas. Pārtrauktu `pending` apstrādi atjauno inbox nolasīšana; kļūmi atkārto no detaļām.
- Dzīvie privātie pieteikumi un atslēgas netiek pārnesti uz GitHub vai lokālo vidi.

## Vercel vārteja

Publiskā adrese ir Vercel, bet noturīgie dati un ārējā apstrāde ir esošajā Cloudflare serverī. API vārteja saglabā HttpOnly sesijas cookie, bloķē citas izcelsmes rakstīšanas pieprasījumus, neļauj patvaļīgu pārsūtīšanas galamērķi, ierobežo saturu un nekešo privātas atbildes. Ar HMAC parakstītais konteksts ir piesaistīts konkrētam pieprasījumam; Worker ignorētam vai viltotam parakstam neuzticas. Papildu testi pārbauda atsevišķus PIN limitus klientiem un Vercel adresi paziņojumos. Parakstīta pierakstīšanās papildus pārbaudīta īstajā Workers izpildvidē.

## Pilnā Vercel pārbaude pēc publicēšanas

Jauns QA līds iesniegts publiskajā Vercel formā: `737bedf5-c282-4f3f-8305-a2bca911320e` (`QA Vercel nodošana`). AI `Karsts` saglabāts 431 ms pēc izveides serverī, Telegram pieņēma ziņu 625 ms un Resend e-pastu 654 ms pēc izveides. Servera inbox atgrieza šo līdu kā jaunāko; pēc atsevišķām izmaiņām atkārtoti nolasīti visi 4 statusi un saglabāta piezīme. Pēc pārbaudes QA līds atzīmēts `Nav aktuāls`. Jaunā Vercel adrese atveras bez Vercel/ChatGPT konta; privātais inbox prasa lietotnes PIN. Servisu pieņemšanas laiki nav pierādījums ziņas izlasīšanai vai e-pasta nonākšanai iesūtnē.

## Klienta apstiprinājuma papildinājums — versija 10

Pievienota atsevišķa klienta apstiprinājuma fāze, kas sākas pirms AI pabeigšanas un saglabā savu kļūdu, laiku un Resend ziņas ID. Apstiprinājumā nav AI vērtējuma, komandas piezīmju, PIN vai inbox saites. Migrācija vecajiem līdiem atstāj fāzi izslēgtu. Veiksmīgu sūtījumu neatkārto; ir 3 mēģinājumu stundā limits vienai adresei.

47 automātiskie testi, TypeScript un build iziet. Faktiskā Workers izpildvides pārbaude aptver atsevišķus klienta un pārdevēja sūtījumus ar simulētu Resend. Pašreizējais `resend.dev` sūtītājs ir piemērots tikai paša Resend konta adresei; sūtīšanai uz citu klienta adresi jāpabeidz domēna verifikācija. Vispārējā klientu piegāde nav apliecināta.
