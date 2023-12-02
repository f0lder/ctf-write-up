# CTF Write-up


## [1. Cho-Choo - Medium](#1-cho-choo---medium)
## [2. Easy Fix - Easy](#2-easy-fix---easy)
## [3. Gone for Ice Cream - Medium](#3-gone-for-ice-cream---medium)
## [4. Hidden Stuff - Easy](#4-hidden-stuff---easy)
## [5. Is this the Full View - Medium](#5-is-this-the-full-view---medium)
## [6. Old School - Hard](#6-old-school---hard)
## [7. Spamming - Easy Fix](#7-spamming---easy-fix)
## [8. SSTV - Medium](#8-sstv---medium)
## [9. Strange sPi - Easy](#9-strange-spi---easy)
## [10. UCE - Hard](#10-uce---hard)
## [11. Weird Cat - Easy](#11-weird-cat---easy)

---

## 1. Cho-Choo - Medium

```
[STORYLINE]
Prietenul meu Bob este pasionat de criptografie. Zilele trecute mi-a trimis acest mesaj pe care nu il pot descifra. 
Crezi ca ma poti ajuta?
```

În acest folder avem 2 fișiere `cipher.jpg` si `message.enc`. Nu putem deschide direct fisierul `cipher.jpg` dar daca rulam comanda: `binwalk -e cipher.jpg` extragem o arhiva ascunsa in poza care contine un fisier `key.txt` care are parola. 

Ruland comanda: `sudo fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt 1F870.zip` gasim parola ca fiind: `evil_fencer`.


In fisierul `key.txt` avem:
```
Hmmmm, i think you are looking for this.


Ok, so this is your key: 100.


Good luck!
```

---

Din cheie, parola arhivei si numele probei putem deduce ca cipherul este [Rail Fence](https://en.wikipedia.org/wiki/Rail_fence_cipher). Incarcam mesajul [aici](https://gchq.github.io/CyberChef/), selctam **Rail Fence Cipher Decode** cu cheia **100**. <kbd>Ctrl</kbd> + <kbd>F</kbd> pe textul rezultat cu termenul `CTF` gasim flagul:

```
[...]The innkeeper came in exclaiming, âWhere art thou, strumpetCTF{r4iL_f3nC3_4Tw}? 
Of course this is some of thy work.â At this Sancho awoke, and [...]

Flag:
CTF{r4iL_f3nC3_4Tw}

```


## 2. Easy Fix - Easy

```
[STORYLINE]

In timpul procesului de backup cateva din fisierele mele au fost corupte. 
Am nevoie sa recuperez informatiile.

```

Primul pas cand avem de cautat ceva intr-o imagine, folosim comanda strings `file.rsm` pentru a lista in terminal toate stringurile din fisier. Majoritatea nu au importanta, dar daca este ceva ascuns, il vom gasi fie la inceput, in header, sau la final.  
Dupa utilizarea comenzii `strings file.rms`, observam la final un sir lung de caractere, grupate cate doua:
```
 51 31 52 47 65 7a 4e 75 59 32 39 6b 4d 57 35 6e 58 7a 52 6e 4e 47 6c 75 58 32 46 75 5a 46 38 30 5a 7a 52 70 62 6e 30 3d
```
Intuitia ne spune ca ar fi un string codat din ASCII in Hex, dar pentru a ne asigura, introducem textul pe https://www.dcode.fr/cipher-identifier , si dupa apasarea butonului Analyze, se pare ca avem intradevar de a face cu un Hex to ASCII.
Convertim sirul gasit din Hex in ASCII si obtinem urmatorul string: 

```
Q1RGezNuY29kMW5nXzRnNGluX2FuZF80ZzRpbn0=
```
La prima vedere pare a fi ceva fara semnificatie, dar pentru un ochi antrenat, ne dam seama ca este defapt un string codat in Base64. Decodam stringul obtinut din Base64 si astfel obtinem Flag-ul pe care il cautam: 

```
CTF{3ncod1ng_4g4in_and_4g4in}
```

---

## 3. Gone for Ice Cream - Medium

```
[STORYLINE]

Compania Xtech a fost tinta unui atac. 
Trebuie sa determinam impactul acestui atac asupra companiei si imaginii acesteia.

```

In folder gasim un fisierul `capture.pcapng` care este o captra de Wireshark. In Wireshark navigam la `Statistics` -> `Conversations`. Alegem `UDP` si `Follow Stream...`. La `Entire conversations`. 

Selectam conversatia `8.8.8.8:53 -> 192.168.92.135:53 (6,512 bytes)`. Putem salva continutul intrun fisier text

```

T............QW.example.com................,.ns.icann.org..noc.dns./x..T... 
......u..................Z0.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......L...........ZX.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....Sf...........YS.example.com..............L.,.ns.icann.org..noc.dns./x..T.
.. ......u.....n............Ig.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................Bs.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......g...........IG.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....@............b2.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....0d...........5n.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....l............Fu.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....=............ZC.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....H!...........Bl.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....1............eG.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....q............hh.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....b............dX.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....2............5n.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....3............N0.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......f...........aW.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................IG.example.com................,.ns.icann.org..noc.dns./x..T.
.. ......u......_...........dX.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......_...........pv.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................Ju.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................ZX.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....m............ks.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......?...........lv.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......9...........dS.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....j............IH.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....zl...........Bo.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................YX.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....S............Zl.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................IH.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....3............Jl.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....x............YW.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................No.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................ZW.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....[K...........Qg.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....|............eW.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......@...........91.example.com................,.ns.icann.org..noc.dns./x..T
... ......u......]...........ci.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................Bk.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................ZX.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................aW.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................N0.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....>`...........5h.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....f............dG.example.com................,.ns.icann.org..noc.dns./x..T
... ......u..................lv.example.com................,.ns.icann.org..noc.dns./x..T
... ......u.....	
..........bi.example.com................,.ns.icann.org..noc.dns./x..T... ......u.....El
...........B0.example.com................,.ns.icann.org..noc.dns./x..T... ......u.....*
..........cm.example.com................,.ns.icann.org..noc.dns./x..T... ......u.....Zx
...........F2.example.com................,.ns.icann.org..noc.dns./x..T... ......u......p
...........ZW.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....|............ci.example.com................,.ns.icann.org..noc.dns./x..T... ......u
......5...........xl.example.com................,.ns.icann.org..noc.dns./x..T... ......u
..................Eg.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....u............RG.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....i............Q1.example.com................,.ns.icann.org..noc.dns./x..T... ......u
......X...........ez.example.com................,.ns.icann.org..noc.dns./x..T... ......u
..................do.example.com................,.ns.icann.org..noc.dns./x..T... ......u
......a...........MV.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....7............Nf.example.com................,.ns.icann.org..noc.dns./x..T... ......u
......x...........MX.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....kF...........Nf.example.com................,.ns.icann.org..noc.dns./x..T... ......u
..................eT.example.com................,.ns.icann.org..noc.dns./x..T... ......u
......6...........B1.example.com................,.ns.icann.org..noc.dns./x..T... ......u
..................9G.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....Z............Ul.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....]............MU.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....r;...........5h.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....d,...........bF.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....6............9m.example.com................,.ns.icann.org..noc.dns./x..T... ......u
..................bG.example.com................,.ns.icann.org..noc.dns./x..T... ......u
..................E5.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....]............fQ.example.com................,.ns.icann.org..noc.dns./x..T... ......u
.....O9...........==.example.com................,.ns.icann.org..noc.dns./x..T... ......u.....
```
Cu o comanda `grep`: `grep -o -P '.{0,2}.ex' ex.txt`

Output: 
```
QW.ex
Z0.ex
ZX.ex
Ig.ex
YS.ex
Bs.ex
b2.ex
5n.ex
IG.ex
Fu.ex
ZC.ex
Bl.ex
eG.ex
hh.ex
dX.ex
N0.ex
aW.ex
5n.ex
IG.ex
pv.ex
dX.ex
Ju.ex
ZX.ex
ks.ex
IH.ex
lv.ex
dS.ex
Bo.ex
YX.ex
Zl.ex
IH.ex
Jl.ex
YW.ex
No.ex
ZW.ex
Qg.ex
eW.ex
91.ex
ci.ex
Bk.ex
ZX.ex
N0.ex
aW.ex
5h.ex
dG.ex
lv.ex
bi.ex
B0.ex
cm.ex
F2.ex
ZW.ex
xl.ex
ci.ex
Eg.ex
Q1.ex
RG.ex
ez.ex
do.ex
MV.ex
Nf.ex
MX.ex
Nf.ex
eT.ex
B1.ex
Ul.ex
9G.ex
MU.ex
5h.ex
bF.ex
9m.ex
bG.ex
E5.ex
fQ.ex
==.ex
```

In `Notepad++` inlocuim `.ex\n` cu ` ` 

```
Base64:
QWZ0ZXIgYSBsb25nIGFuZCBleGhhdXN0aW5nIGpvdXJuZXksIHlvdSBoYXZlIHJlYWNoZWQgeW91ciBkZXN0aW5hdGlvbiB0cmF2ZWxlciEgQ1RGezdoMVNfMXNfeTB1Ul9GMU5hbF9mbGE5fQ==

Text:
After a long and exhausting journey, you have reached your destination traveler! CTF{7h1S_1s_y0uR_F1Nal_fla9}
```


---

## 4. Hidden Stuff - Easy

```
[STORYLINE]

Extrageti flag-ul din fisier.
```

Pentru a extrage flagul din `newfile.jpg` extragem continutul ascuns cu `binwalk -e newfile.jpg`. 

Gasim o arhiva cu parola care se poate sparge cu `sudo fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt 16D46.zip`. Gasim parola `crazy12` si continutul arhivei, respectiv `flax.txt`:

```
AAABABAABAAABAB{AABBBABBABABBBAAABAA_BABBAABBABBAABB_AABAAABBAAABAAAABBABBABBA_BAABAAABBBAABAA_AABBAAAAAAABABBAABAA}
```

Putem observa ca avem de aface cu Bacon Cipher (Translation = `A/B`)

```
Raw text
CTFHOPEYOUENIOYTHEGAME

Final Flag

CTF{HOPE_YOU_ENIOY_THE_GAME} sau CTF{HOPE_YOU_ENJOY_THE_GAME}
```

---

## 5. Is this the Full View - Medium


```
[STORYLINE]

Laptopul personal al lui John s-a stricat, iar acesta este unul dintre fisierele pe care am reusit sa le recuperam. Dar nu se deschide corect. 🤔
```

Gasim un fisier `BMP` care nu se poate deschide. Deschizand acest fisier cu un editor hex putem vedea ca header-ul este corupt.

Comparand cu alt fisier bmp:

| file | HEX | ASCII |
|-|-|--|
|file.bmp| <mark>78 21</mark> 8A C0 A8 00 00 00 00 00 8A 00 00 00 7C 00 00 00 80 07 00 00 A0 00 00 00 01 00 20 00 03 00| <mark>x!</mark>ŠÀ¨�����Š������€�� ������
|sample.bmp|<mark>42 4D</mark> 8A C0 A8 00 00 00 00 00 8A 00 00 00 7C 00 00 00 80 07 00 00 82 05 00 00 01 00 20 00 03 00| <mark>BM</mark>ŠÀ¨�����Š��� ���€��‚��� ��

Daca modificam headerul corespunzator obtinem imaginea:

![imagine](images/ok.bmp)

Inca nu avem flagul. Putem modifica inaltimea imaginii prin modificarea octetilor

```
42 4D 8A C0 A8 00 00 00 00 00 8A 00 00 00 7C 00 
00 00 80 07 00 00 (82 05) 00 00 01 00 20 00 03 00
```

Octetii `82 05` corespund cu `1410`.

Deschidem fisierul si gasim flagul.

![imagine](images/file_final.bmp)

```
Flag: CTF{0n_t0P_Of_tH3_W0r1D}
```

---
## 6. Old School - Hard

```
[STORYLINE]

Mark de la contabilitate ne-a cerut ajutorul. 
Fiul lui a vrut sa descarce un joc, dar nu poate rula fisierul. Crezi ca il poti ajuta?
```

Gasim `old_school.bin` care deschid cu un hex editor contine `NES` indicand faptul ca este un joc pentru NES.
Descarcam [Messen](https://www.mesen.ca/) si deschidem bin-ul.

![Messem pacman](https://i.imgur.com/lSSawr2.png)

La `Tools -> Cheats -> Cheats Database` putem adauga cheaturi pentru a incere in orice nivel. Adaugam pentru Ms. Pac-Man. 

In cateva nivele gasim numere in nivel:

![Ex level](https://i.imgur.com/YqG6Fyy.png)

Extragem aceste numere din toate nivele obtinem:

```
84 72 65 78 75 89 79 44 85

80 76 65 89 69 65 89 69 82

84 82 69 65 83 85 82 69

73 83 73 78

65 78 79 84 72 69 82

67 65 83 84 476 69

47 99 77 56 104 116 65 48 69
```

Convertim in ASCII obtinem:

```
THANKYO,UPLAYEAYERTREASUREISINANOTHERCAST/E/cM8htA0E
```

In joc putem observa ca High-Score-ul este inlocuit de `ON-PASTE` si scorul curent este inlocuit de `BIN`.
Ne putem da seama ca putem forma un link de pastebin.

```
Link: https://pastebin.com/cM8htA0E

Flag: CTF{1_u53d_70_b3_4n_4dv3n7ur3r_l1k3_y0u,_7h3n_1_700k_4n_4rr0w_1n_7h3_kn33}
```

---

## 7. Spamming - Easy Fix


Dupa ce analizam mail-ul trimis, observam anumite tipare precum: "Senate bill 
2416 ; Title 5 ; Section 308", cuvinte scrise cu majuscule la intamplare, si povestea este despre cum sa te imbogatesti rapid. Aceste trei tipare sunt de obicei asociate cu un mail de spam care contine un mesaj ascuns in spate. Pentru a obtine acest mesaj, accesam pagina https://www.spammimic.com/decode.shtml , si introducem mail-ul dat. Dupa ce dam click pe butonul de decode, obtinem Flag-ul ascuns: 

```
CTF{N0t_rE4LLy_4_sp4M}
```
---
## 8. SSTV - Medium

Am observat in numele challange-ului cuvantul **SSTV** care inseamna Slow-Scan TeleVision. Am instalat utilitatile necesare pentru a decoda fisierul .wav din sunet in poza.

### Pas1. Instalare utilitati

Pentru a putea construi QSSTV de la sursa, avem nevoie de urmatoarele librarii, pe care le vom instala: 
```
sudo apt-get install qt5-default libopenjp2-7-dev libpulse-dev libv4l-dev libasound2-dev libgtk-3-dev libfftw3-dev
```

Instalam si Hamlib pentru a putea rula QSSTV
```
sudo apt-get install hamlib-dev
```

Instalam pavucontrol pentru a gestiona sursele audio
```
sudo apt-get install pavucontrolbutonul
```

Instalam si QSSTV intr-un final, pentru a putea receptiona sunetul dat de proba.
```
sudo apt-get install qsstv
```

### Pas2. Configurarea aplicatiei si surselor de sunet si inregistrare
**O sa avem nevoie de 3 terminale: unul pentru a deschide QSSTV, unul pentru a deschide pavucontrol si unul pentru a da play la sunet.**
Incarcam modulul PulseAudio: `pactl load-module module-null-sink sink_name=virtual-cable`.
Pentru a verifica daca s-a incarcat modulul, deschidem cu comanda `pavucontrol` fereastra de gestionare a dispozitivelor de sunet, si verificam in tab-ul Output Devices daca avem **Null Output**.
Dupa ce am verificat ca s-a incarcat cu succes modulul, pornim QSSTV cu comanda `qsstv`. Odata deschis, mergem in meniul Options -> Confirguration si ne ducem pe tabul Sound. Aici, ne asiguram ca la Audio Interface este selectat PulseAudio si sunt selectate la Input Audio Device si Output Audio Device: pulse -- PulseAudio Sound Server.
Salvam modificarile facute si ne intoarcem la fereastra deschisa de pavucontrol. Pentru ca QSSTV sa inregistreze fisierul audio trebuie sa mergem la tabul Recording si sa ne asiguram ca la aplicatia QSSTV este setat **Monitor of Null Output**.

## Pas3. Transformam sunetul in poza

Tot ce a ramas de facut este sa decodam sunetul.
Apasam pe butonul de Receive in fereastra QSSTV, dupa care dam play din terminal cu comanda `paplay -d virtual-cable transmission.wav` la fisierul pe care vrem sa il decodam. Si asteptam sa se converteasca. Rezultatul final este urmatorul: 


![Poza Rezultat cu Flag SSTV](images/SSTV.png)

Din poza putem observa ca este scris si flag-ul pe care il cautam, si trebuie sa il transcriem manual: 
```
CTF{3asy_a5_123_c0nga}
```
---
## 9. Strange sPi - Easy

Ni se ofera o lista de IP-uri, pe care daca le analizam, se observa cu ochiul liber ca nu sunt IP-uri valide. Observam din numele probei cuvantul sPI, care este defapt cuvantul IPs scris invers.
Astfel, ne decidem sa scriem toate "IP-urile" invers. Acum pare ca am avea o lista de IP-uri valide, dar totusi, ele nu duc nicaieri. Observand ca dupa inversare, se repeta mereu ultimele bucati din IP, ne gandim sa extragem doar cifrele pana la primul punct, si obtinem astfel urmatorul sir de numere:
```
103 124 106 173 146 64 156 116 171 137 61 120 163 175
```
Observam ca avem de a face iar cu un cod ASCII, asa ca il introducem pe https://www.dcode.fr/ascii-code . Dupa decodarea sirului de caractere, observam ca daca convertim in baza OCT, obtinem flag-ul pe care il cautam: 

```
CTF{f4nNy_1Ps}
```

---
## 10.  UCE - Hard

```
[STORYLINE]

Ne poti ajuta sa testam acest program? Flagul trebuie sa fie pe aici pe undeva...
```

Gasim fisierul `Ultimate_Clicker_Experience_Demo.exe` rulam aplicatia:

![clicker](https://i.imgur.com/pXI1RVK.png)

Putem incerca cu Cheat Engine sa cautam flag-ul. Atasam Cheat Engine la Procesul Clicker-ului si cautam o valoare oarecare. 

In Seactiea `Found` dam `Click Dreapta -> Browse this memory region`

![Mem Viewer](https://i.imgur.com/87LffF1.png)

Cautam `CTF{` si gasim flagul:

```
CTF{f4sT_4s_L16htN1n6}
```

---
## 11. Weird Cat - Easy

Ni se ofera un cod QR, care nu este complet. Utilizand o aplicatie de editare foto, umplem cele doua patrate goale pentru a repara codul QR. Eu am utilizat aplicatia Paint pentru  a selecta patratul bun, copia si lipi peste celelalte doua. Utilizand telefonul, am scanat codul QR si ma dus la urmatoarea pagina: https://qrco.de/beZ5cI . 
Observam imaginea care scrie pe ea: "nothing to see here", si dupa ce o descarcam si utilizam comanda `strings` pe ea, observam la final textul: TOLD.YOU.THERE.IS.NOTHING.TO.SEE . Bineinteles ca asta nu este adevarat, asa ca ne decidem sa descarcam toate pozele de pe site. 
Dupa ce descarcam si celelalte doua poze: una .gif si una .png, folosim comanda `strings` pe poza .gif si comanda `zsteg` pe poza .png. 
La poza .gif observam la final prima jumatate din Flag: `CTF{C4T5_` , iar la poza .png, observam textul: `LOOKING.FOR.SOMETHING?.PLAY.WITH.LAST.BITS`. Insa multumita comenzii `zsteg`, ni se ofera direct si cealalta jumatate din Flag: `D0m1N4nC3}`.
Astfel, unind cele doua jumatati, obtinem in final Flag-ul complet:

```
CTF{C4T5_D0m1N4nC3}
```


---

### Realizat de echipa `The CodeFathers`
Membri:
- Ursan Bogdan-Gabriel
- Uricaru Vasile-Cristian
- Chirila Octavian-Ionut
