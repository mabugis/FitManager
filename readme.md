# Specifikacija projekta

**Univerzitet u Sarajevu**
**Elektrotehnicki Fakultet**
**Objektno Orijentisana Analiza i Dizajn**

**izmjena: Ahmed**

---

## 1. Osnovne informacije o sistemu

**Naziv teme:** FitManager - Sistem za upravljanje teretanom

**Nastavna grupa:** _(popuniti)_

**Link na repozitorij tima:** _(popuniti)_

**Clanovi tima:**
1. _(ime i prezime, broj indeksa)_
2. _(ime i prezime, broj indeksa)_
3. _(ime i prezime, broj indeksa)_
4. _(ime i prezime, broj indeksa)_

### Namjena sistema:

FitManager je web aplikacija namijenjena upravljanju teretanom koja objedinjuje rad sa clanovima, clanarinama i grupnim treninzima. Sistem omogucava clanovima registraciju, kupovinu razlicitih tipova clanarina te rezervaciju mjesta na grupnim treninzima. Treneri putem sistema kreiraju i upravljaju rasporedom grupnih treninga. Administrator ima potpunu kontrolu nad korisnicima, prihodima i konfiguracijom teretane. Dolasci clanova se evidentiraju putem simulacije QR kod skenera na ulazu u teretanu. Sistem asinhrono obavjestava clanove o isteku clanarine putem email-a. Svi podaci se trajno cuvaju u relacionoj bazi podataka koristeci Entity Framework Core.

---

## 2. Funkcionalnosti (poslovni procesi) sistema

### 1) Registracija i upravljanje clanovima

**Vrsta funkcionalnosti:** Perzistencija podataka (CRUD operacije)

**Opis funkcionalnosti:**
Administrator i zaposlenik mogu kreirati, pregledati, azurirati i brisati profile clanova teretane. Svaki clan ima podatke: ime, prezime, email, telefon, datum rodjenja i datum registracije. Prilikom registracije, sistemu se automatski generise jedinstveni QR kod za clana. Clan moze putem svog profila pregledati historiju dolazaka i status clanarine.

---

### 2) Kupovina i upravljanje clanarinama

**Vrsta funkcionalnosti:** Usluga sistema

**Opis funkcionalnosti:**
Clan bira tip clanarine (mjesecna, kvartalna ili godisnja) i vrsi kupovinu putem sistema. Sistem evidentira placanje, aktivira clanarinu i automatski racuna datum isteka na osnovu odabranog tipa. Administrator moze pregledati sve aktivne i istekle clanarine, te rucno produziti ili deaktivirati clanarinu. Cijene clanarina konfigurise administrator.

---

### 3) Rezervacija grupnih treninga

**Vrsta funkcionalnosti:** Usluga sistema

**Opis funkcionalnosti:**
Clan pregledava raspored dostupnih grupnih treninga i rezervise mjesto na zeljenom treningu. Sistem provjerava da li clan ima aktivnu clanarinu i da li ima slobodnih mjesta na treningu (provjera kapaciteta). Ukoliko su svi uslovi ispunjeni, rezervacija se potvrdjuje. Clan moze otkazati rezervaciju najkasnije 2 sata prije pocetka treninga.

---

### 4) Automatsko obavjestavanje o isteku clanarine

**Vrsta funkcionalnosti:** Asinhrona operacija

**Opis funkcionalnosti:**
Sistem periodicno (jednom dnevno) asinhrono provjerava sve aktivne clanarine i identificira one koje isticu u narednih 7 dana. Za svaku takvu clanarinu, sistem asinhrono salje email obavjestenje clanu sa informacijom o skorom isteku i pozivom na obnovu. Operacija se izvrsava u pozadini koristeci Background Service u ASP.NET Core, bez blokiranja rada glavne aplikacije.

---

### 5) Generisanje statistike i izvjestaja

**Vrsta funkcionalnosti:** Operacija sa specificnim algoritmom obrade

**Opis funkcionalnosti:**
Administrator moze generisati izvjestaje o poslovanju teretane za odabrani vremenski period. Sistem koristi algoritme za agregaciju podataka (ukupni prihodi, broj novih clanova, stopa obnove clanarina), sortiranje (najpopularniji treninzi po broju rezervacija, najaktivniji clanovi po broju dolazaka) i filtriranje (po datumu, tipu clanarine, tipu treninga). Rezultati se prikazuju tabelarno i graficki.

---

### 6) Evidencija dolazaka putem QR koda

**Vrsta funkcionalnosti:** Koristenje vanjskog uredjaja

**Opis funkcionalnosti:**
Na ulazu u teretanu, clan skenira svoj QR kod putem simuliranog QR citaca (web kamera ili rucni unos koda). Sistem validira QR kod, provjerava da li clan ima aktivnu clanarinu i evidentira dolazak sa tacnim vremenom. Ukoliko clanarina nije aktivna, sistem odbija ulaz i prikazuje odgovarajucu poruku. Simulacija vanjskog uredjaja se realizuje putem web kamere ili textualnog unosa generisanog koda.

---

### 7) Upravljanje rasporedom grupnih treninga

**Vrsta funkcionalnosti:** Perzistencija podataka (CRUD operacije)

**Opis funkcionalnosti:**
Trener kreira, pregledava, azurira i brise grupne treninge u sistemu. Svaki trening ima naziv, opis, maksimalni kapacitet, datum i vrijeme odrzavanja, trajanje i dodijeljenog trenera. Sistem onemogucava kreiranje treninga koji se vremenski preklapaju za istog trenera. Administrator moze pregledati rasporedom svih trenera.

---

### 8) Preporuka plana treninga na osnovu ciljeva clana

**Vrsta funkcionalnosti:** Operacija sa specificnim algoritmom obrade

**Opis funkcionalnosti:**
Clan bira svoj fitness cilj (mrsavljenje, jacanje misica, poboljsanje izdrzljivosti) i unosi osnovne parametre (tezina, visina, godine). Algoritam na osnovu unesenih podataka i historije posjeta generise personalizirani sedmicni plan treninga. Plan ukljucuje preporucene tipove grupnih treninga i broj posjeta sedmicno. Algoritam koristi formule za izracun BMI i preporucene intenzitete treninga.

---

## 3. Akteri sistema

### a) Clan teretane

**Vrsta aktera:** Korisnik sistema

**Funkcionalnosti u kojima akter ucestvuje:**

| Funkcionalnost sistema | Nacin ucesca |
|---|---|
| 1) Registracija i upravljanje clanovima | Pregled i azuriranje vlastitog profila |
| 2) Kupovina i upravljanje clanarinama | Pokretanje (kupovina clanarine) |
| 3) Rezervacija grupnih treninga | Pokretanje (rezervacija/otkazivanje) |
| 6) Evidencija dolazaka putem QR koda | Pokretanje (skeniranje QR koda) |
| 8) Preporuka plana treninga | Pokretanje (unos ciljeva, pregled plana) |

---

### b) Trener

**Vrsta aktera:** Zaposlenik sistema

**Funkcionalnosti u kojima akter ucestvuje:**

| Funkcionalnost sistema | Nacin ucesca |
|---|---|
| 7) Upravljanje rasporedom grupnih treninga | Pokretanje (CRUD treninga) |
| 3) Rezervacija grupnih treninga | Pregled (lista rezervacija za svoj trening) |
| 8) Preporuka plana treninga | Pregled i azuriranje (prilagodjavanje planova) |

---

### c) Administrator

**Vrsta aktera:** Administrator

**Funkcionalnosti u kojima akter ucestvuje:**

| Funkcionalnost sistema | Nacin ucesca |
|---|---|
| 1) Registracija i upravljanje clanovima | Pokretanje (puni CRUD nad clanovima) |
| 2) Kupovina i upravljanje clanarinama | Upravljanje (konfiguracija cijena, pregled) |
| 4) Automatsko obavjestavanje o isteku | Konfiguracija (podesavanje parametara) |
| 5) Generisanje statistike i izvjestaja | Pokretanje (generisanje izvjestaja) |
| 7) Upravljanje rasporedom grupnih treninga | Pregled (nadzor svih rasporeda) |

---

## 4. Funkcionalni zahtjevi

| ID | Zahtjev |
|---|---|
| FR-01 | Sistem mora omoguciti registraciju novih clanova sa svim potrebnim podacima |
| FR-02 | Sistem mora podrzavati tri tipa clanarina: mjesecna, kvartalna, godisnja |
| FR-03 | Sistem mora automatski racunati datum isteka clanarine |
| FR-04 | Sistem mora provjeravati kapacitet treninga prilikom rezervacije |
| FR-05 | Sistem mora asinhrono slati email obavjestenja o isteku clanarine |
| FR-06 | Sistem mora generisati QR kod za svakog registrovanog clana |
| FR-07 | Sistem mora evidentirati svaki dolazak clana sa tacnim vremenom |
| FR-08 | Sistem mora generisati statistiku prihoda, posjeta i popularnosti treninga |
| FR-09 | Sistem mora onemoguciti vremensko preklapanje treninga za istog trenera |
| FR-10 | Sistem mora implementirati kontrolu pristupa za tri razlicite uloge |
| FR-11 | Sistem mora omoguciti otkazivanje rezervacije najkasnije 2 sata prije pocetka |
| FR-12 | Sistem mora generisati personalizirani plan treninga na osnovu ciljeva clana |

## 5. Nefunkcionalni zahtjevi

| ID | Zahtjev |
|---|---|
| NFR-01 | Sistem mora biti razvijen u ASP.NET Core MVC arhitekturi |
| NFR-02 | Podaci moraju biti perzistirani u SQL Server bazi podataka koristeci EF Core |
| NFR-03 | Korisnicki interfejs mora biti responzivan (Bootstrap) |
| NFR-04 | Sistem mora podrzavati istovremeni rad vise korisnika |
| NFR-05 | Lozinke korisnika moraju biti hashirane (ASP.NET Identity) |
| NFR-06 | Sistem mora imati vrijeme odziva manje od 3 sekunde za standardne operacije |
| NFR-07 | Aplikacija mora biti deployabilna na cloud platformi (Azure/Railway) |

---

## 6. Specifikacija algoritama

### Algoritam 1: Generisanje statistike

```
ULAZ: datumOd, datumDo, tipIzvjestaja
1. Dohvati sve relevantne podatke iz baze za zadani period
2. AKO tipIzvjestaja == "prihodi":
     - Grupiraj placanja po mjesecima
     - Izracunaj sumu za svaki mjesec
     - Sortiraj po datumu rastuce
3. AKO tipIzvjestaja == "posjete":
     - Grupiraj dolaske po danima sedmice
     - Izracunaj prosjek dnevnih posjeta
     - Sortiraj po broju posjeta opadajuce
4. AKO tipIzvjestaja == "treninzi":
     - Grupiraj rezervacije po treningu
     - Izracunaj popunjenost (rezervacije/kapacitet * 100)
     - Sortiraj po popularnosti opadajuce
     - Filtriraj top 10 najpopularnijih
IZLAZ: lista rezultata sa agregiranim podacima
```

### Algoritam 2: Preporuka plana treninga

```
ULAZ: cilj, tezina, visina, godine, historijaDolazaka
1. Izracunaj BMI = tezina / (visina/100)^2
2. Odredi kategoriju (pothranjen, normalan, prekomjeran, gojazan)
3. Na osnovu cilja i BMI kategorije, odaberi sablon treninga:
     - Mrsavljenje + prekomjeran: 4-5 kardio, 2 snaga
     - Jacanje + normalan: 4 snaga, 1 kardio, 1 fleksibilnost
     - Izdrzljivost: 3 kardio, 2 kombinovano, 1 snaga
4. Filtriraj dostupne grupne treninge po tipu iz sablona
5. Sortiraj po vremenu odrzavanja
6. Prilagodi intenzitet na osnovu historije dolazaka:
     - < 10 dolazaka: pocetnicki nivo
     - 10-50 dolazaka: srednji nivo
     - > 50 dolazaka: napredni nivo
IZLAZ: sedmicni plan sa konkretnim treninzima i vremenima
```

---

## 7. Tehnoloski stek

- **Backend:** ASP.NET Core MVC (.NET 8)
- **Baza podataka:** SQL Server + Entity Framework Core
- **Frontend:** Razor Views + Bootstrap 5
- **Autentifikacija:** ASP.NET Core Identity
- **Asinhrona obrada:** Background Services (IHostedService)
- **QR kod:** QRCoder biblioteka (generisanje) + web kamera/simulacija (citanje)
- **Email:** SMTP klijent (MailKit ili SendGrid)
- **Deployment:** Azure App Service ili Railway

---

## 8. Raspodjela zadataka po clanovima tima (prijedlog)

| Clan | Zaduzenja |
|---|---|
| Clan 1 | Registracija/upravljanje clanovima + Kontrola pristupa (Identity) |
| Clan 2 | Clanarine (kupovina, upravljanje) + Asinhrono obavjestavanje (email) |
| Clan 3 | Grupni treninzi (raspored, rezervacije) + QR kod evidencija dolazaka |
| Clan 4 | Statistika/izvjestaji + Preporuka plana treninga (algoritmi) |
