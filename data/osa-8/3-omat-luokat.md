---
path: '/osa-8/3-omat-luokat'
title: 'Omat luokat'
hidden: false
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

Tämän osion jälkeen

- Tiedät, miten omia luokkia määritellään
- Osaat muodostaa itse määritellystä luokasta olion
- Osaat kirjoittaa konstruktorin
- Tiedät, mitä tarkoittaa avainsana `self`
- Tiedät, mitä ovat attribuutit ja miten niitä käytetään

</text-box>

Luokka määritellään avainsanan `class` avulla. Syntaksi on

```python
class LuokanNimi:
    # Luokan toteutus
```

Luokat nimetään yleensä _camel case_ -käytännöllä niin, että sanat kirjoitetaan yhteen ja jokainen sana alkaa isolla alkukirjaimella. Esimerkiksi seuraavat ovat tämän käytännön mukaisia luokkien nimiä:

* `Pankkitili`
* `OhjelmaApuri`
* `KirjastoTietokanta`
* `PythonKurssinArvosanat`

Yhdellä luokalla pyritään mallintamaan jokin sellainen yksittäinen kokonaisuus, jonka sisältämät tiedot liittyvät kiinteästi yhteen. Monimutkaisemmissa ratkaisuissa luokka voi sisältää toisia luokkia (esimerkiksi luokka `Kurssi` voisi sisältää luokan `Osasuoritus` mukaisia olioita).

Tarkastellaan esimerkkinä yksinkertaista luokkamäärittelyä, josta sisältö vielä puuttuu:

```python
class Pankkitili:
    pass
```

Koodissa määritellään luokka, jonka nimi on `Pankkitili`. Luokalle ei ole määritelty varsinaista sisältöä, mutta tästä huolimatta luokasta voidaan muodostaa olio.

Tarkastellaan ohjelmaa, jossa luokasta muodostetun olion sisälle on määritelty kaksi muuttujaa, `saldo` ja `omistaja`. Olion muuttujia kutsutaan _attribuuteiksi_. Attribuutista käytetään myös nimitystä _oliomuuttuja_.

Kun luokasta luodaan olio, voidaan attribuuttien arvoja käsitellä olion kautta:

```python
class Pankkitili:
    pass

pekan_tili = Pankkitili()
pekan_tili.omistaja = "Pekka Python"
pekan_tili.saldo = 5.0

print(pekan_tili.omistaja)
print(pekan_tili.saldo)
```

<sample-output>

Pekka Python
5.0

</sample-output>

Attribuutit ovat käytettävissä ainoastaan sen olion kautta, jossa ne on määritelty. Pankkitili-luokasta muodostetuilla olioilla on jokaisella omat arvonsa attribuuteille. Attribuuttien arvot haetaan olioiden kautta, esimerkiksi näin:

```python
tili = Pankkitili()
tili.saldo = 155.50

print(tili.saldo) # Viittaa tilin attribuuttiin saldo
print(saldo) # TÄSTÄ TULEE VIRHE, koska oliomuuttuja ei ole mukana!
```

## Konstruktorin lisääminen

Kuten edellisestä esimerkistä huomataan, luokasta voi luoda uuden olion kutsumalla konstruktoria, joka on muotoa `LuokanNimi()`. Yleensä olisi kuitenkin kätevä antaa attribuuteille arvot heti kun olio luodaan – nyt esimerkiksi Pankkitilin omistaja ja saldo asetetaan vasta, kun pankkitiliolio on luotu.

Attribuuttien asettamisessa ilman konstruktoria on myös se ongelma, että samasta luokasta luoduilla olioilla voi olla eri attribuutit. Seuraava ohjelmakoodi esimerkiksi antaa virheen, koska oliolle `pirjon_tili` ei ole määritelty attribuuttia `saldo`:

```python
class Pankkitili:
    pass

pekan_tili = Pankkitili()
pekan_tili.omistaja = "Pekka"
pekan_tili.saldo = 1400

pirjon_tili = Pankkitili()
pirjon_tili.omistaja = "Pirjo"

print(pekan_tili.saldo)
print(pirjon_tili.saldo) # TÄSTÄ TULEE VIRHE
```

Sen sijaan että attribuuttien arvot alustettaisiin luokan luomisen jälkeen, on huomattavasti parempi ajatus alustaa arvot samalla, kun luokasta luodaan olio.

Konstruktori kirjoitetaan luokan sisään metodina `__init__` yleensä heti luokan alkuun.

Tarkastellaan `Pankkitili`-luokkaa, johon on lisätty konstruktori:

```python
class Pankkitili:

    # Konstruktori
    def __init__(self, saldo: float, omistaja: str):
        self.saldo = saldo
        self.omistaja = omistaja
```

Konstruktorin nimi on aina `__init__`. Huomaa, että nimessä sanan `init` molemmilla puolilla on _kaksi alaviivaa_.

Konstruktorin ensimmäinen parametri on nimeltään `self`. Tämä viittaa olioon, jota käsitellään. Asetuslause

`self.saldo = saldo`

asettaa parametrina annetun saldon luotavan olion saldoksi. On tärkeä huomata, että tässä yhteydessä muuttuja `self.saldo` on eri muuttuja kuin muuttuja `saldo`:

* Muuttuja `self.saldo` viittaa olion attribuuttiin. Jokaisella Pankkitili-luokan oliolla on oma saldonsa.

* Muuttuja `saldo` on konstruktorimetodin `__init__` parametri, jolle annetaan arvo, kun metodia kutsutaan (eli kun halutaan luoda uusi olio luokasta).

Nyt kun konstruktorille on määritelty parametrit, voidaan attribuuttien arvot antaa oliota luotaessa:

```python
class Pankkitili:

    # Konstruktori
    def __init__(self, saldo: float, omistaja: str):
        self.saldo = saldo
        self.omistaja = omistaja

# Parametrille self ei anneta arvoa, vaan Python antaa sen
# automaattisesti
pekan_tili = Pankkitili(100, "Pekka Python")
pirjon_tili = Pankkitili(20000, "Pirjo Pythonen")

print(pekan_tili.saldo)
print(pirjon_tili.saldo)
```

<sample-output>

100
20000

</sample-output>

Esimerkistä huomataan, että olioiden luominen helpottuu, kun arvot voidaan antaa heti oliota muodostaessa. Samalla tämä varmistaa, että arvojen antaminen ei unohdu, ja ohjaa käyttäjää antamaan arvot attribuuteille.

Attribuuttien arvoja voi edelleen muuttaa myöhemmin ohjelmassa, vaikka alkuarvo olisikin annettu konstruktorissa:

```python
class Pankkitili:

    # Konstruktori
    def __init__(self, saldo: float, omistaja: str):
        self.saldo = saldo
        self.omistaja = omistaja

pekan_tili = Pankkitili(100, "Pekka Python")
print(pekan_tili.saldo)

# Saldoksi 1500
pekan_tili.saldo = 1500
print(pekan_tili.saldo)

# Lisätään saldoon 2000
pekan_tili.saldo += 2000
print(pekan_tili.saldo)
```

<sample-output>

100
1500
3500

</sample-output>

Tarkastellaan vielä toista esimerkkiä luokasta ja olioista. Kirjoitetaan luokka, joka mallintaa yhtä lottokierrosta:

```python
from datetime import date

class LottoKierros:

    def __init__(self, viikko: int, pvm: date, numerot: list):
        self.viikko = viikko
        self.pvm = pvm
        self.numerot = numerot


# Luodaan uusi lottokierros
kierros1 = LottoKierros(1, date(2021, 1, 2), [1,4,8,12,13,14,33])

# Tulostetaan tiedot
print(kierros1.viikko)
print(kierros1.pvm)

for numero in kierros1.numerot:
    print(numero)
```

<sample-output>

1
2021-01-02
1
4
8
12
13
14
33

</sample-output>

Attribuutit voivat olla siis minkä tahansa tyyppisiä – esimerkiksi edellisessä esimerkissä jokaiseen olioon tallennetaan lista ja päivämäärä.


<programming-exercise name='Kirja' tmcname='osa08-06_kirja'>

Tee luokka `Kirja`, jolla on attribuutteina muuttujat `nimi`, `kirjoittaja`, `genre` ja `kirjoitusvuosi` sekä konstruktori, joka alustaa muuttujat.

Luokkaa käytetään seuraavasti:

```python
python = Kirja("Fluent Python", "Luciano Ramalho", "ohjelmointi", 2015)
everest = Kirja("Huipulta huipulle", "Carina Räihä", "elämänkerta", 2010)

print(f"{python.kirjoittaja}: {python.nimi} ({python.kirjoitusvuosi})")
print(f"Kirjan {everest.nimi} genre on {everest.genre}")
```

<sample-output>

Luciano Ramalho: Fluent Python (2015)
Kirjan Huipulta huipulle genre on elämänkerta

</sample-output>

</programming-exercise>

<programming-exercise name='Kirjoita luokat' tmcname='osa08-07_kirjoita_luokat'>

Kirjoita alla pyydetyt luokat. Jokaisen luokan alle on kuvattu attribuuttien nimet ja tyypit.

Kirjoita jokaiselle luokalle myös konstruktori, jossa attribuutit annetaan siinä järjestyksessä kuin ne on kuvauksessa annettu.

1. Luokka Muistilista
- attribuutti otsikko (merkkijono)
- attribuutti merkinnat (lista)

2. Luokka Asiakas
- attribuutti tunniste (merkkijono)
- attribuutti saldo (desimaaliluku)
- attribuutti alennusprosentti (kokonaisluku)

3. Luokka Kaapeli
- attribuutti malli (merkkijono)
- attribuutti pituus (desimaaliluku)
- attribuutti maksiminopeus (kokonaisluku)
- attribuutti kaksisuuntainen (totuusarvo)

</programming-exercise>

## Omien luokkien olioiden käyttö

Omasta luokasta muodostetut oliot käyttäytyvät esimerkiksi funktioiden parametrina ja paluuarvona samalla tavalla kuin muutkin oliot. Voisimme esimerkiksi tehdä pari apufunktiota tilien käsittelyyn:

```python
# funktio luo uuden tiliolion ja palauttaa sen
def avaa_tili(nimi: str):
    uusi_tili =  Pankkitili(0, nimi)
    return uusi_tili

# funktio lisää parametrina saamansa rahasumman parametrina olevalle tilille
def laita_rahaa_tilille(tili: Pankkitili, summa: int):
    tili.saldo += summa

pekan_tili = avaa_tili("Pekka Python")
print(pekan_tili.saldo)

laita_rahaa_tilille(pekan_tili, 500)

print(pekan_tili.saldo)
```

<sample-output>

0
500

</sample-output>

<programming-exercise name='Muodosta lemmikki' tmcname='osa08-07b_muodosta_lemmikki'>

Määrittele luokka `Lemmikki`. Luokalla on konstruktori, jossa annetaan arvot attribuuteille `nimi`, `laji` ja `syntymavuosi` tässä järjestyksessä.

Kirjoita sitten luokan ulkopuolelle funktio `uusi_lemmikki(nimi: str, laji: str, syntymavuosi: int)`, joka luo ja palauttaa uuden `Lemmikki`-tyyppisen (eli `Lemmikki`-luokkaa vastaavan) olion.

Esimerkki funktion kutsumisesta:

```python
musti = uusi_lemmikki("Musti", "koira", 2017)
print(musti.nimi)
print(musti.laji)
print(musti.syntymavuosi)
```

<sample-output>

Musti
koira
2017

</sample-output>

</programming-exercise>

<programming-exercise name='Vanhempi kirja' tmcname='osa08-08_vanhempi_kirja'>

Tee funktio `vanhempi_kirja(kirja1: Kirja, kirja2: Kirja)`, joka saa parametriksi kaksi `Kirja`-oliota. Funktio kertoo, kumpi kirjoista on vanhempi.

Funktiota käytetään seuraavasti:

```python
python = Kirja("Fluent Python", "Luciano Ramalho", "ohjelmointi", 2015)
everest = Kirja("Huipulta huipulle", "Carina Räihä", "elämänkerta", 2010)
norma = Kirja("Norma", "Sofi Oksanen", "rikos", 2015)

vanhempi_kirja(python, everest)
vanhempi_kirja(python, norma)
```

<sample-output>

Huipulta huipulle on vanhempi, se kirjoitettiin 2010
Fluent Python ja Norma kirjoitettiin 2015

</sample-output>

</programming-exercise>

<programming-exercise name='Genren kirjat' tmcname='osa08-09_genren_kirjat'>

Tee funktio `genren_kirjat(kirjat: list, genre: str)`, joka saa parametriksi listan `Kirja`-olioita sekä genren kertovan merkkijonon.

Funktio _palauttaa_ uuden listan, jolle se laittaa parametrina olevista kirjoista ne, joilla on haluttu genre.

Funktiota käytetään seuraavasti:

```python
python = Kirja("Fluent Python", "Luciano Ramalho", "ohjelmointi", 2015)
everest = Kirja("Huipulta huipulle", "Carina Räihä", "elämänkerta", 2010)
norma = Kirja("Norma", "Sofi Oksanen", "rikos", 2015)

kirjat = [python, everest, norma, Kirja("Lumiukko", "Jo Nesbø", "rikos", 2007)]

print("rikoskirjoja ovat")
for kirja in genren_kirjat(kirjat, "rikos"):
    print(f"{kirja.kirjoittaja}: {kirja.nimi}")
```

<sample-output>

rikoskirjoja ovat
Sofi Oksanen: Norma
Jo Nesbø: Lumiukko

</sample-output>

</programming-exercise>
