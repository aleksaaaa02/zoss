# zoss

# KT1

Inspiracija za temu potiče od diplomskog rada, razvoja multiplejer video-igre napisane u programskom jeziku rust.

## Domen 

Zamišljeni softverski sistem predstavlja **masivnu onlajn multiplayer video-igru (MMORPG)** koja omogućava velikom broju igrača da u realnom vremenu učestvuju u zajedničkom virtuelnom svetu. Sistem obezbeđuje upravljanje korisničkim nalozima, likovima, igranjem u realnom vremenu, socijalnim interakcijama i virtuelnom ekonomijom. Neki primeri ovakvih video igara su World of Warcraft, Black Desert Online i mnoge druge.

Igra je dostupna putem namenski razvijenog klijenta i oslanja se na distribuiranu serversku infrastrukturu radi obezbeđivanja skalabilnosti, niske latencije i visoke dostupnosti.

## Učesnici

- **Igrač** – krajnji korisnik sistema koji kreira nalog, upravlja likovima i učestvuje u igri.
- **Administratori** – korisnici sa povišenim privilegijama za moderaciju i održavanje sistema/sveta.

## Poslovni procesi
Na visokom nivou, sistem podržava sledeće poslovne procese:
- Registraciju i autentifikaciju korisnika
- Upravljanje korisničkim nalozima i likovima
- Izvršavanje igranja u realnom vremenu
- Razmenu virtuelnih dobara između igrača
- Socijalne interakcije (chat, guilds)
- Administraciju i nadzor sistema

# Arhitektura

- **Klijentska aplikacija** – aplikacija na strani korisnika koja renderuje igru i komunicira sa serverskom infrastrukturom. 
- **Autoritativan server za igru** – serverska komponenta zadužena za izvršavanje logike igre i sinhronizaciju stanja sveta. 
- **Server za autentifikaciju** – servis za autentifikaciju i autorizaciju korisnika. 
- **Spoljni servisi** – npr. payment provider ili anti-cheat sistemi (kernel-level anti-cheat sistemi i slični).

# Potencijalni osetljivi resursi

- Korisnički kredencijali
- Sesijski tokeni
- Podaci o likovima i inventaru
- Virtuelna valuta i predmeti  
- Chat poruke i komunikacija između igrača
- Lični podaci korisnika (email, IP adresa)
- Logovi servera za igru

## 2. Arhitektura sistema

Sistem je projektovan kao aplikacija zasnovana na server-klijent mrežnoj arhitetkuri sa jasno razdvojenim komponentama za autentifikaciju, logiku igre, komunikaciju i skladištenje podataka. Arhitektura je delimično **event-driven**, gde se određene akcije u sistemu (npr. borba, trgovina) emituju kao događaji.

Sistem je dizajniran da podrži veliki broj istovremenih korisnika uz minimalnu latenciju.

- **Server za igru (C++ / Rust)** -
Glavna serverska komponenta odgovorna za logiku igre, simulaciju sveta i obradu real-time zahteva. Izabrana tehnologija omogućava visok stepen performansi i kontrolu nad memorijom. Server za igru je autoritativan i diktira klijentima šta da prikazuju kranjim korisnicima.

- **UDP protokol**
Koristi se za komunikaciju između klijenta i game servera kako bi se postigla niska latencija neophodna za real-time igranje.

- **Authentication Service (Go / C++ / Rust)** -
Odgovoran za upravljanje korisničkim nalozima, autentifikaciju i izdavanje sesijskih tokena. Navedeni jezici omogućavaju jednostavnu implementaciju konkurentnih servisa. Nekada se koristi kao i katalog server koji omogućava uvid i odabir servera za igru.

- **Redis** -
In-memory skladište koje se koristi za čuvanje sesija, privremenog stanja igre i matchmaking podataka.

- **MongoDB (ili neka druga NoSQL baza)** -
NoSQL baza podataka koja se koristi za skladištenje profila igrača, likova i inventara, pogodna za fleksibilne i hijerarhijske strukture podataka.

- **Klijentska aplikacija (Godot / Unreal / Unity)** -
Klijentska aplikacija koju krajnji korisnici koriste kako bi komunicirali sa serverom za igru. U ovakvim sistemim često se nazivaju i "dumb clients" jer ne simuliraju svet igre već prikazuju stanje sveta koje diktira autoritativan server za igru.

## 3. Grupe slučajeva korišćenja

- Upravljanje korisničkim nalozima - Funkcionalnosti vezane za registraciju, prijavu, autentifikaciju i upravljanje korisničkim podacima.
- Upravljanje likovima - Kreiranje, brisanje i modifikacija likova, kao i čuvanje njihovog napretka.
- Real-time igranje - Izvršavanje borbi, kretanja, interakcija sa okruženjem i sinhronizacija stanja između igrača.
- Virtuelna ekonomija - Trgovina predmetima, razmena virtuelne valute i upravljanje inventarom.
- Socijalne funkcionalnosti - Chat, formiranje grupa, kao i interakcija između igrača.
- Administracija sistema - Moderacija sadržaja, nadzor sistema i upravljanje korisničkim privilegijama.

