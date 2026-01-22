<h1>ProjektSun</h1>
ProjektSun je aplikacija temeljena na Javi i JavaFX-u, dizajnirana za praćenje solarnih pretvarača i
potrošene električne energije. Pruža podatke u stvarnom vremenu o proizvodnji i potrošnji
solarne energije, omogućujući korisnicima optimizaciju potrošnje energije i automatizaciju
pametnih uređaja.

## Preduvjeti
- Java 8 ili novija verzija
- Maven za upravljanje gradnjom
- Internetska veza za dohvaćanje vremenskih prognoza i drugih vanjskih izvora podataka
- JavaFX biblioteke (uključene u projekt ili putem Maven dependency-ja)

## Značajke
- **Praćenje:** Praćenje proizvodnje solarne energije i potrošnje električne energije u stvarnom vremenu.
- **Automatizacija pametnog doma:** Automatizacija pametnih prekidača na temelju vremenske prognoze i podataka o solarnoj energiji.
- **Korisničko sučelje:** Intuitivno dizajnirano za jednostavno korištenje i praćenje.
- **Vizualizacija podataka:** Grafovi i dijagrami za vizualizaciju proizvodnje i potrošnjeenergije

<h1>Aplikacija</h1>
<h2>Početni zaslon</h2>
Podaci o električnoj energiji u stvarnom vremen

![MainPage](https://github.com/user-attachments/assets/b5e5450f-8215-4567-9473-96c290271158)

Komunikacija između računala i invertera omogućena je putem USB kabela. USB kabel uspostavlja fizičku vezu između računala i invertera. Ovaj protokol omogućuje računalu da prima podatke o proizvodnji solarne energije i potrošnji, te da šalje naredbe inverteru prema potrebi.
Na gornjoj slici prikazane su sve ključne informacije o solarnoj stanici. U donjem lijevom kutu nalazi se grafički prikaz koji vizualizira podatke, dok se u donjem desnom kutu nalazi popis svih aktivnih pametnih prekidača.

<h2>Smart devices prozor</h2>
U ovom prozoru ispisana je lista pametnih prekidača. Na svaki prekidač spojeno je trošilo sa određenom potrošnjom i saki prekidač ima maksimalnu snagu koju može prenijeti.
Elementi jednog trošila u listi su:

- Točka - vizualni indikator koji pokazuje je li uređaj online, tj. ima li aktivnu internetsku vezu
- Ime uređaja
- Button Status koji predstavlja je li uređaj upaljen ili ne
- Button Activation koji stavlja prekidač u red za automatsko paljenje i gašenje
- Priority - prioritet kod automatskog paljenja/gašenja
- Power - vrijednost snage trošila (ako prekidač ima mogućnost mjerenja potrošnje, vrijednost se mijenja u stvarnom vremenu)

Za svaki prekidač moguće je prilagoditi maksimalno opterećenje i snagu trošila koja je na njega spojena. Također, prekidači mogu biti i dualni, što znači da mogu upravljati s dva odvojena trošila. U tom slučaju potrebno je specificirati ime i snagu svakog pojedinačnog izlaza (trošila) povezanog s prekidačem.

![SmartDevice edit](https://github.com/user-attachments/assets/e3580673-1021-4eed-a6ff-de55a54e8589)

Automatska kontrola potrošnje energije

Sustav automatske kontrole potrošnje energije funkcionira na temelju dinamičkog upravljanja prekidačima koji su povezani s različitim uređajima. Nakon što se neki prekidač aktivira, on se stavlja u listu "Ready". Ova lista se automatski sortira prema prioritetu, što znači da se prekidači s većim prioritetom nalaze na vrhu liste.
Kada sustav detektira da je dostupno dovoljno energije, prvi prekidač s vrha liste "Ready" se automatski uključuje i premješta u listu "Active", koja sadrži sve trenutno uključene uređaje. Na taj način sustav osigurava da su prioritetni uređaji prvi uključeni kada je dostupna energija.
Ako u nekom trenutku više nema dovoljno energije za održavanje svih aktivnih uređaja, zadnji prekidač iz liste "Active" se isključuje, čime se oslobađa energija za druge uređaje s većim prioritetom.
Ako je prvi prekidač u listi "Active" uključen, ali nema dovoljno energije za njegov rad, sustav ga isključuje i provjerava mogu li se ostali prekidači iz liste "Ready" uključiti s raspoloživom energijom. U tom slučaju, prekidači s manjim energetskim zahtjevima mogu biti aktivirani, dok se prekidač s najvećim prioritetom isključuje.

<h2>Settings</h2>

![settings](https://github.com/user-attachments/assets/a3d2d97a-7454-47dd-8b0b-89c4c8b0f0c0)

Rates (Brzine)
- U ovom dijelu se mogu prilagoditi intervali za različite funkcije aplikacije. To uključuje intervale skeniranja uređaja, učestalost spremanja podataka tijekom dana i noći, te intervale ažuriranja i resetiranja grafa
  
Smart Devices (Pametni uređaji)
- Ovdje se mogu podesiti parametri vezani uz pametne uređaje, poput maksimalne struje i snage koju kombinacija svih uređaja mogu koristiti. Također možete postaviti vrijeme u danu kada se izvršava automatsko uključivanje i isključivanje uređaja.

---

<h1> Proširenja i ciljevi razvoja </h1>

Sustav već nudi značajke poput praćenja energije u stvarnom vremenu, vizualizacije podataka te automatizacije pametnih prekidača, no cilj daljnjeg razvoja je proširiti funkcionalnosti kako bi se riješili ključni problemi vezani uz rad hibridnog solarnog invertera i optimizirala potrošnja energije.

## Trenutni problemi

### 1. Gubitak viška energije:

 - Hibridni solarni inverter koristi baterije za spremanje energije. Kada su baterije pune, višak proizvedene energije se gubi

### 2. Gašenje solarnih panela:

  - Ako se upali trošilo koje uzrokuje prekoračenje maksimalne proizvodnje solarnih panela, inverter prebacuje sustav na mrežnu energiju. Solarni paneli se tada gase, a sav protok energije dolazi iz mreže. Paneli se ponovno aktiviraju tek kada potrošnja energije padne na vrlo malu vrijednost, što dovodi do neefikasnosti.

### 3. Skokovi u potrošnji:

  - Nagli skokovi u potrošnji, poput uključivanja trošila velike snage (npr. grijač za toplu vodu), uzrokuju prebacivanje na mrežnu energiju. Sustav nema mogućnost postepenog povećanja potrošnje kako bi izbjegao takve situacije.

### 4. Ciklička potrošnja uređaja: 

  - Uređaji poput perilice rublja koriste energiju u velikim količinama samo u određenim ciklusima.

<h1>Ciljevi razvoja</h1>

### 1. Integracija povijesnih podataka i vremenske prognoze:

 - Dodavanje analize povijesnih podataka o proizvodnji energije i vremenske prognoze kako bi se bolje predvidjela maksimalna proizvodnja energije. To bi omogućilo sustavu da uključi uređaje kada postoji višak energije.

### 2. Postepeno povećanje potrošnje:

  - Implementacija postupnog povećanja potrošnje slabijim uređajima prije uključivanja uređaja s velikom potrošnjom. Na taj način smanjili bi se skokovi u potrošnji koji uzrokuju prebacivanje na mrežnu energiju.
  
### 3. Optimizacija cikličke potrošnje:

  - Razvoj sustava za prepoznavanje radnih ciklusa uređaja (npr. perilice rublja) kako bi se druga trošila uključivala između ciklusa kada je potrošnja manja. Ovo bi povećalo ukupnu efikasnost korištenja energije.

### 4. Inteligentno upravljanje:

  - Korištenjem prioriteta, povijesnih podataka i vremenske prognoze, sustav bi dinamički odlučivao koji uređaji se mogu uključiti ili isključiti kako bi se spriječilo prebacivanje na mrežu i osigurala maksimalna efikasnost solarnog sustava.























