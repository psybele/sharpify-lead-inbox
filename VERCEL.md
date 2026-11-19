# Vercel publiskā adrese

Publiskā forma: https://sharpify-lead-inbox-psybele.vercel.app/

Inbox: https://sharpify-lead-inbox-psybele.vercel.app/inbox

## Arhitektūra

Vercel piegādā esošā servera lapas ar ārējo pāradresācijas noteikumu, nemainot pārlūkā redzamo Vercel adresi. API pieprasījumus apstrādā Vercel Node.js 24 funkcija. D1 datubāze, sesijas, šifrētie servisu iestatījumi un fona apstrāde turpina darboties esošajā Cloudflare Worker. Tas saglabā jau pieslēgtos Groq/Telegram/Resend servisus un visus pieteikumus. Esošajam serverim jāpaliek publicētam un pieejamam.

Vārteja pieņem tikai šīs lietotnes API ceļus, sūta pieprasījumus tikai uz fiksēto serveri un ārējas novirzes neseko. Privātas API atbildes un lapas netiek kešotas. Pārsūta tikai lietotnes sesijas cookie, nevis pārējos Vercel vai pārlūka cookie.

Vercel un Worker runtime glabā vienādu slepenu `LEAD_PROXY_SECRET`. HMAC saista publisko adresi, klienta IP, laiku, metodi, ceļu, ķermeni un cookie ar konkrēto pieprasījumu; paraksts ir derīgs īsu laiku. Worker pārbauda to, pirms izmanto pārsūtīto klienta IP vai Vercel adresi. Tādēļ PIN mēģinājumu limits netiek apvienots visiem apmeklētājiem vienā Vercel servera IP, un paziņojumu saites ved uz Vercel inbox.

## Pirmkods un iepakošana

- `lib/lead-proxy.mjs` — kopīgā HMAC parakstīšana un pārbaude.
- `lib/vercel-gateway.mjs` — publiskās API vārtejas loģika.
- `deploy/vercel/` — Vercel manifesti un funkcijas ieejas punkts.
- `scripts/package-vercel.mjs` — izveido patstāvīgu publicēšanas mapi ar kopīgajiem moduļiem.
- `tests/proxy.test.mjs` — izolēti izcelsmes, paraksta, cookie un noviržu testi.

```bash
npm run package:vercel
```

Rezultāts ir `work/vercel-deploy`. Tas nesatur API atslēgas, datubāzi, PIN vai vides noslēpumus. Vercel CLI lietošanai šajā mapē piesaisti pareizo projektu un iestati servera produkcijas `LEAD_PROXY_SECRET`; identisku vērtību iestata Worker serverī un piemēro ar publicēšanu. Neizmanto `NEXT_PUBLIC_` prefiksu un neliec šo vērtību kodā.

Šis Vercel projekts ir lietotāja Hobby kontā. Nav pieslēgts maksas plāns, jauna maksas datubāze vai maksas AI alternatīva. Visu izmantoto servisu kvotas un pieejamība joprojām ir spēkā.

## Pārbaude pēc publicēšanas

Atver formu un inbox tieši Vercel adresē. Pārbaudi PIN, iesniegšanu, saglabāšanu, AI, abu paziņojumu kanālu stāvokļus un statusa saglabāšanos. Paziņojuma saitei jāved uz Vercel. Vērtētājam nav jābūt pierakstītam Vercel vai ChatGPT; privātos datus aizsargā lietotnes PIN sesija.
