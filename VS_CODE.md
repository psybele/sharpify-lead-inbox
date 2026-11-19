# Palaišana VS Code uz MacBook

GitHub ir koda glabātuve, VS Code ir redaktors, bet publicētās vietnes hostings jau darbojas atsevišķi. Lai vērtētājs aizpildītu formu, viņam nekas nav jāinstalē.

## Pirmā lokālā palaišana

1. Instalē Node.js 24 LTS un VS Code, ja tie vēl nav datorā.
2. Klonē GitHub repozitoriju vai lejupielādē pilno pirmkoda ZIP no dzīvas vietnes `/kods` lapas un izpako to.
3. VS Code izvēlnē **File → Open Folder…** atver mapi, kurā atrodas `package.json`.
4. Augšējā izvēlnē **Terminal → New Terminal** atver termināli.
5. Secīgi izpildi:

```bash
npm ci
npm run local:setup
npm run build
npm run db:local
npm run dev
```

Ja lieto Git, projekta iegūšanai terminālī:

```bash
git clone https://github.com/psybele/sharpify-lead-inbox.git
cd sharpify-lead-inbox
unzip lead-inbox-source.zip
cd lead-inbox
```

GitHub pašlaik nodod pilnu pirmkoda ZIP. Klonēšana pati to neizpako, tāpēc vajadzīgs `unzip` solis. VS Code atver iekšējo `lead-inbox` mapi un tajā izpildi iepriekš norādītās `npm` komandas.

`local:setup` izvadē parāda jaunu lokālo PIN. Tas atšķiras no publicētā rīka PIN. `.dev.vars` paliek tikai tavā datorā, un Git to ignorē. `db:local` ir paredzēts pirmreizējai tukšas lokālās datubāzes sagatavošanai; neatkārto to datubāzei, kas jau sagatavota.

Atver terminālī parādīto adresi (parasti `http://localhost:5173`). Publiskā forma ir `/`, pārdevēja skats — `/inbox`. Lokālā vide nelasa dzīvos pieteikumus un nesatur dzīvos savienojumus. Ja vajag lokālu AI vai paziņojumu pārbaudi, atslēgas ievadi lokālās lietotnes savienojumos.

Nākamajā reizē pietiek ar:

```bash
npm run dev
```

Serveri aptur ar **Control + C** terminālī.

## Sagatavotie VS Code uzdevumi

Nospied **Command + Shift + P**, izvēlies **Tasks: Run Task** un vajadzīgo uzdevumu:

- **Lead Inbox: first local setup** — secīgi uzstāda atkarības, izveido lokālo PIN, veic build un sagatavo tukšu lokālo datubāzi; izmanto tikai pirmajā reizē.
- **Lead Inbox: dev** — palaiž vietējo serveri.
- **Lead Inbox: check**, **Lead Inbox: test**, **Lead Inbox: build** — pārbaudes un build.

## Ja GitHub repozitorijs vēl jāpublicē pašam

Pilnā pirmkoda ZIP nav Git vēstures. Atverot tā mapi VS Code:

1. Kreisajā malā atver **Source Control** un izvēlies **Initialize Repository**.
2. Pievieno failus, ieraksti commit ziņu, piemēram, `Lead Inbox submission`, un izveido commit.
3. Izvēlies **Publish to GitHub**, pieslēdz savu GitHub kontu un nosauc repozitoriju `sharpify-lead-inbox`.
4. Ja kodu paredzēts dot vērtētājiem ar publisku saiti, izvēlies publisku repozitoriju. Pirms publicēšanas pārliecinies, ka nav pievienoti `.dev.vars`, `.env`, atslēgas, PIN vai privāti pieteikumu dati.

Ja repozitorijs jau ir klonēts, nevajag veidot jaunu: pēc savām koda izmaiņām veic commit un **Sync Changes**. GitHub push pats nepublicē izmaiņas esošajā dzīvajā vietnē.

## Publiskā Vercel adrese

Publicētā adrese ir https://sharpify-lead-inbox-psybele.vercel.app. Esošā Cloudflare datubāze un ārējie savienojumi tiek izmantoti fonā. Vercel vārtejas avots ir `deploy/vercel`, kopīgais paraksta modulis — `lib/lead-proxy.mjs`. Ar `npm run package:vercel` izveido patstāvīgu Vercel publicēšanas mapi `work/vercel-deploy`. Darba secība un nepieciešamie servera vides iestatījumi aprakstīti `docs/VERCEL.md`.

## Izmaiņas un GitHub push

Ja repozitorijs ir klonēts un kods izpakots iekšējā `lead-inbox` mapē, vari tajā strādāt ar VS Code. Pirmais commit pievienos izpakotos avota failus kā parastu `lead-inbox/` mapi, un turpmāk Git redzēs katra faila izmaiņas. Repo ārējā mapē izpildi:

```bash
git add lead-inbox
git commit -m "Update Lead Inbox"
git push origin main
```

Pirms koda pievienošanas saglabā projekta `.gitignore`; tas izslēdz atslēgas, lokālo datubāzi, atkarības un build. GitHub push šobrīd automātiski nepublicē izmaiņas. Publiskā Vercel lapa izmanto Cloudflare lietotni, tādēļ saskarnes/API izmaiņas jāpublicē arī tur. Vercel vārteju pārpublicē tikai tad, ja mainījusies pati vārteja. Asistents var veikt koda labojumus un publicēšanu, kamēr pieejami šie projekta savienojumi.
