# Klienta pieteikuma apstiprinājums

Jaunam pieteikumam serveris saglabā atsevišķu `customer_email_state`. Apstiprinājums tiek gatavots uz formā norādīto e-pastu uzreiz pēc datubāzes saglabāšanas, paralēli AI. Pārdevēja e-pasts un Telegram turpina darboties neatkarīgi.

Dzīvajā konfigurācijā 2026. gada 19. novembra gala pārbaudē izmantots sūtītājs no `lead.psybele.com`. Resend pieņēma klienta apstiprinājumu paredzētajai testa adresei `delivered@resend.dev` 0,427 s pēc pieteikuma saglabāšanas; saglabāts ziņas ID un `sent` stāvoklis. Tas apliecina atsevišķās klienta fāzes darbību ar reālu API, nevis vēstules nonākšanu cilvēka iesūtnē.

Ziņas temats ir **Pieteikums saņemts · [numurs]**. Ziņā ir neitrāls apstiprinājums un tas pats pieteikuma numurs, kuru rāda formas apstiprinājuma ekrāns. Nav iesūtītā mērķa, AI vērtējuma, piezīmju, PIN vai inbox saites. Ir teksta un telefonā lasāma HTML versija. Klienta atbildes adrese ir konfigurētais komandas saņēmējs, ja tas norādīts.

## Pieslēgšana

1. Resend sadaļā **Domains** pievieno savu domēnu vai tam atvēlētu apakšdomēnu.
2. Domēna DNS panelī pievieno tieši Resend norādītos verifikācijas ierakstus. Nepārraksti citus vietnes vai e-pasta ierakstus.
3. Sagaidi Resend statusu **Verified** un lietotnes savienojumos norādi sūtītāja adresi no šī domēna.
4. Atstāj ieslēgtu **Sūtīt klientam pieteikuma apstiprinājumu**, saglabā, tad iesniedz jaunu pieteikumu ar savu e-pastu.
5. Inbox līda detaļās pārbaudi **Klienta apstiprinājums**, e-pasta iesūtni un mēstuļu mapi.

Resend testa sūtītājs `onboarding@resend.dev` drīkst sūtīt tikai uz paša Resend konta adresi. Lietotne to nepārveido par visiem pieejamu sūtītāju: citām klientu adresēm saglabā `blocked` ar skaidru iemeslu, bet pats pieteikums un pārdevēja paziņojumi turpina darboties. Formas apstiprinājums neapgalvo, ka bloķēts e-pasts ir nosūtīts.

Ja sūtītājs vairs nav testa domēnā, saglabāti pieslēguma dati vēl neapliecina, ka domēns ir verificēts: to apliecina Resend un reāla jaunā pieteikuma pieņemšana. Resend pieņemšana savukārt vēl neapliecina nonākšanu iesūtnē vai izlasīšanu.

## Atkārtošana un vēsturiskie pieteikumi

Inbox rāda katra apstiprinājuma kļūdu, nosūtīšanas laiku un pieņemšanas stāvokli. Kļūdainu vai bloķētu jauno pieteikumu var atkārtot pēc konfigurācijas izlabošanas. Veiksmīgu apstiprinājumu neatkārto. Resend idempotences atslēga ir atdalīta no pārdevēja ziņas un stabila nemainītam saturam; servisam ir 24 stundu idempotences periods.

Vēsturiskie pieteikumi un pieteikumi ar izslēgtu klienta apstiprinājumu paliek `disabled`, arī izmantojot atkārtošanas pogu. Ieslēdzot funkciju, veciem klientiem ziņas automātiski nesūta. Vienai adresei atļauj līdz 3 apstiprinājumu mēģinājumiem stundā, apvienojot dažādu IP pieprasījumus. Limita sasniegšana neizdzēš un nenoraida saglabāto pieteikumu.

Migrācija ir `drizzle/0002_open_rage.sql`. Produkcijā to piemēro publicēšanas platforma; lokāli jau sagatavotai datubāzei piemēro tikai jauno migrāciju, nevis atkārtoti visas iepriekšējās.

Oficiālais ierobežojuma apraksts: https://resend.com/docs/knowledge-base/403-error-resend-dev-domain
