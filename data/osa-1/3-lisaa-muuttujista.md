---
path: "/osa-1/3-lisaa-muuttujista"
title: "Lisää muuttujista"
hidden: false
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

Tämän osion jälkeen

- Osaat käyttää muuttujia eri yhteyksissä
- Tiedät, millaista tietoa muuttujiin voidaan tallentaa
- Ymmärrät merkkijonojen sekä kokonais- ja liukulukujen eron

</text-box>

Vastaa seuraavaan kyselyyn ennen osion aloittamista. Saat vastaamisesta yhden tehtäväpisteen.

<quiz id="6a4ceb56-be87-5007-add2-1f28fad7bdfc"></quiz>



Muuttujia tarvitaan ohjelmissa moniin tarkoituksiin. Voimme tallentaa muuttujiin mitä tahansa sellaista tietoa, jota tarvitaan ohjelmassa myöhemmin.

Muuttuja luodaan Pythonissa seuraavasti:

`muuttujan_nimi = ...`

Tässä `...` tarkoittaa arvoa, joka tallennetaan muuttujaan.

Esimerkiksi kun luemme `input`-komennolla merkkijonon käyttäjältä, sijoitamme merkkijonon muuttujaan, jotta voimme käyttää sitä myöhemmin ohjelmassa:

```python
nimi = input("Anna nimesi: ")
print("Moi, " + nimi)
```

<sample-output>

Anna nimesi: **Kummitus**
Moi, Kummitus

</sample-output>

Muuttujille voidaan antaa arvoja myös esimerkiksi näin:

```python
etunimi = "Pekka"
sukunimi = "Pythonen"

nimi = etunimi + " " + sukunimi

print(nimi)
```

<sample-output>

Pekka Pythonen

</sample-output>

Tässä tapauksessa muuttujan arvo ei tule käyttäjältä vaan se on sama ohjelman jokaisella suorituskerralla.

## Muuttujan arvon muuttaminen

Muuttujan arvo voi nimensä mukaisesti muuttua. Niin kuin edellisessä osassa todettiin, uusi arvo ylikirjoittaa vanhan arvon.

Esimerkiksi seuraavassa ohjelmassa muuttuja `sana` saa kolme eri arvoa:

```python
sana = input("Anna sana: ")
print(sana)

sana = input("Anna toinen sana: ")
print(sana)

sana = "kolmas"
print(sana)
```

<sample-output>

Anna sana: **eka**
eka
Anna toinen sana: **toka**
toka
kolmas

</sample-output>

Muuttujan sisältö siis vaihtuu jokaisen sijoituksen yhteydessä.

Muuttujan uusi arvo voi myös perustua sen vanhaan arvoon. Esimerkiksi seuraavassa muuttuja `sana` saa ensin arvoksi käyttäjän syötteen. Tämän jälkeen muuttuja saa arvoksi vanhan arvonsa, jonka perään on lisätty kolme huutomerkkiä:

```python
sana = input("Anna sana: ")
print(sana)

sana = sana + "!!!"
print(sana)
```

<sample-output>

Anna sana: **testi**
testi
testi!!!

</sample-output>

<text-box variant="hint" name="Lisää muuttujan nimen valinnasta">

* Muuttujat kannattaa nimetä niiden käyttötarkoituksen mukaan.
  Esimerkiksi jos muuttujassa on sana, nimi `sana` on parempi kuin `a`.

* Python ei rajoita muuttujien nimien pituutta, mutta eräitä muita sääntöjä muuttujien nimiin liittyy. Nimen täytyy alkaa kirjaimella ja se saa sisältää vain kirjaimia, numeroita ja alaviivoja &#95;.

* Huomaa myös, että pienet ja isot kirjaimet ovat eri merkkejä. Muuttuja `nimi` on siis eri muuttuja kuin `Nimi` tai `NIMI`.

* Pythonissa muuttujien nimet on tapana kirjoittaa pienillä kirjaimilla. Jos nimessä on useita sanoja, niiden välissä on alaviiva.

</text-box>

## Kokonaisluvut

Tähän mennessä olemme tallentaneet muuttujiin vain merkkijonoja. Usein ohjelmissa halutaan kuitenkin tallentaa myös muun tyyppistä tietoa. Tarkastellaan aluksi kokonaislukuja.

Seuraava ohjelma luo muuttujan `ika`, jonka sisältönä on kokonaisluku.

```python
ika = 24
print(ika)
```

Ohjelman tulostus on seuraava:

<sample-output>

24

</sample-output>

Kokonaisluvun ympärille ei kirjoiteta lainausmerkkejä. Itse asiassa luvun ympärille kirjoitettavat lainausmerkit tarkoittavat, että kyseessä ei ole luku vaan merkkijono (joka tosin saattaa sisältää numeroita).

Mitä eroa muuttujan tyypeillä siis on, kun seuraava ohjelma tulostaa samat arvot?

```python
luku1 = 100
luku2 = "100"

print(luku1)
print(luku2)
```

<sample-output>

100
100

</sample-output>

Tyypeillä on merkitystä, koska
erilaiset operaatiot vaikuttavat eri tavalla erityyppisiin muuttujiin. Tarkastellaan seuraavaa esimerkkiä:

```python
luku1 = 100
luku2 = "100"

print(luku1 + luku1)
print(luku2 + luku2)
```

Ohjelman tulostus on seuraava:

<sample-output>

200
100100

</sample-output>

Kahdelle lukuarvolle `+`-operaattori siis merkitsee yhteenlaskua, merkkijonoille taas yhdistämistä peräkkäin.

## Arvojen yhdistäminen tulostettaessa

Seuraava ohjelma ei toimi, koska `"Tulos on "` ja `tulos` ovat erityypisiä:

```python
tulos = 10 * 25
# seuraava rivi tuottaa virheen
print("Tulos on " + tulos)
```

Ohjelma ei tulosta mitään, vaan antaa virheen

<sample-output>

TypeError: unsupported operand type(s) for +: 'str' and 'int'

</sample-output>

Python kertoo, ettei kahden erityyppisen arvon yhdistäminen toimi. Tässä tapauksessa arvon `"Tulos on"` tyyppi on merkkijono ja arvon `tulos` tyyppi on kokonaisluku.

Jos haluamme tulostaa yhdellä komennolla merkkijonon ja luvun, yhdistäminen onnistuu kuitenkin muuttamalla luku merkkijonoksi `str`-funktiolla. Esimerkiksi

```python
tulos = 10 * 25
print("Tulos on " + str(tulos))
```

<sample-output>

Tulos on 250

</sample-output>

Toinen mahdollisuus on käyttää pilkkua `print`-komennossa. Tällöin komento tulostaa kaikki pilkuilla erotetut arvot riippumatta niiden tyypistä:

```python
tulos = 10 * 25
print("Tulos on", tulos)
```

<sample-output>

Tulos on 250

</sample-output>

Huomaa, että tässä tapauksessa arvojen väliin ilmestyy automaattisesti yksi välilyönti tulostuksessa.

## Tulostaminen f-merkkijonojen avulla

Niin sanotut _f-merkkijonot_ tarjoavat kolmannen edellisiä joustavamman ja jopa helppokäyttöisemmän tavan tulostuksen muotoiluun.

Aiempi tekstin ja kokonaisluvun tulostava esimerkki tehtäisiin f-merkkijonojen avulla seuraavasti:

```python
tulos = 10 * 25
print(f"Tulos on {tulos}")
```

Tulostettavan merkkijonon alussa on kirjain _f_, joka tarkoittaa, että kyseessä on f-merkkijono. Merkkijonon sisälle on sijoitettu aaltosuluissa muuttuja `tulos`, jonka arvo tulee tulostuvan merkkijonon osaksi. Tulostus on täsmälleen sama kuin aiemmissa esimerkeissä eli

<sample-output>

Tulos on 250

</sample-output>

Yksittäisen f-merkkijonon sisälle on mahdollista laittaa useampiakin muuttujia. Esimerkiksi koodi

```python
nimi = "Arto"
ika = 39
kaupunki = "Espoo"
print(f"Hei {nimi}, olet {ika}-vuotias. Asuinpaikkasi on {kaupunki}.")
```

tuottaa seuraavan tuloksen:

<sample-output>

Hei Arto, olet 39-vuotias. Asuinpaikkasi on Espoo.

</sample-output>

Huomaa, että täsmälleen tämän esimerkin kaltaista tulostusta on mahdotonta saada aikaan käyttämällä `print`-komennossa pilkkua:

```python
nimi = "Arto"
ika = 39
kaupunki = "Espoo"
print("Hei", nimi, ", olet", ika, "-vuotias. Asuinpaikkasi on", kaupunki, ".")
```

Tulostus on seuraava:

<sample-output>

Hei Arto , olet 39 -vuotias. Asuinpaikkasi on Espoo .

</sample-output>

Tulostuksessa on nyt välilyönti jokaisen erillisen osan välissä ja muutamassa kohdassa se aiheuttaa ongelman.

Vaikka pilkullinen muoto `print`-komennosta on joskus kätevä, se aiheuttaa välillä harmaita hiuksia ja silloin on parempi käyttää f-merkkijonoja. Osassa 4 opimme lisää f-merkkijonojen käteviä ominaisuuksia tulosteen muotoilussa.

<in-browser-programming-exercise name="Välilyönnillä vai ilman" tmcname="osa01-10b_valilyonnilla_vai_ilman" height=400px>

Saat seuraavan koodinpätkän työnhakijoille suunnatun sovelluksen parissa työskentelevältä tuttavaltasi:

```python
nimi = "Teppo Testaaja"
ika = 20
taito1 = "python"
taso1 = "aloittelija"
taito2 = "java"
taso2 = "veteraani"
taito3 = "ohjelmointi"
taso3 = "puoliammattilainen"
ala = 2000
yla = 3000

print("nimeni on ", nimi, " , olen ", ika, "-vuotias")
print("taitoihini kuuluvat")
print("- ", taito1, " (", taso1, ")")
print("- ", taito2, " (", taso2, ")")
print("- ", taito3, " (", taso3, " )")
print("haen työtä, josta maksetaan palkkaa", ala, "-", yla, "euroa kuussa")
```

Koodin pitäisi tuottaa _täsmälleen_ seuraavanlainen tulostus:

<sample-output>

<pre>
nimeni on Teppo Testaaja, olen 20-vuotias

taitoihini kuuluvat
 - python (aloittelija)
 - java (veteraani)
 - ohjelmointi (puoliammattilainen)

haen työtä, josta maksetaan palkkaa 2000-3000 euroa kuussa
</pre>

</sample-output>

Koodi toimii melkein oikein, mutta ei kuitenkaan ihan. Tässä tehtävässä on todella tarkat testit, jotka vaativat, että tulostus on välilyönnilleen oikein.

Korjaa siis koodi siten, että tulostus näyttää oikealta. Huomaa, että erityisesti `print`-komennon muoto, jossa tulostettavat osat eritellään pilkulla, voi tuottaa yllätyksiä, sillä se lisää osien väliin välilyönnin.

Helpoiten saat muutettua koodin toimivaksi käyttämällä tulostukseen f-merkkijonoja.

Vihje: saat tulostettua tyhjän rivin komennolla `print` tai lisäämällä tulostettavaan merkkijonoon merkinnän `\n`.

Muista olla tarkkana tulostusten muodon suhteen jatkossakin kurssin tehtävissä. Osassa tehtävissä testit vaativat täsmälleen esimerkkitulostusten mukaisen muotoilun.

</in-browser-programming-exercise>

## Liukuluvut

`Liukuluku` on ohjelmoinnissa esiintyvä termi, joka tarkoittaa käytännössä desimaalilukua. Liukulukuja voidaan käyttää melko samalla tavalla kuin kokonaislukuja. Huomaa, että desimaalierottimena käytetään pistettä kuten englannissa yleensä.

Esimerkiksi seuraava ohjelma laskee kolmen liukuluvun keskiarvon:

```python
luku1 = 2.5
luku2 = -1.25
luku3 = 3.62

keskiarvo = (luku1 + luku2 + luku3) / 3
print(f"Keskiarvo: {keskiarvo}")
```

<sample-output>

Keskiarvo: 1.6233333333333333

</sample-output>

<in-browser-programming-exercise name="Laskutoimitukset" tmcname="osa01-11_laskutoimitukset">

Ohjelman tehtäväpohjassa on määritelty kaksi kokonaislukumuuttujaa `x` ja `y`:

```python
x = 27
y = 15
```

Täydennä ohjelma siten, että sen tulostus on seuraava:

<sample-output>

27 + 15 = 42
27 - 15 = 12
27 * 15 = 405
27 / 15 = 1.8

</sample-output>

Ohjelman tulee toimia siinäkin tapauksessa, että muuttujien arvoa vaihdetaan. Eli jos ensimmäiset rivit muuttuvat muotoon

```python
x = 4
y = 9
```

niin tulostus on seuraava:

<sample-output>

4 + 9 = 13
4 - 9 = -5
4 * 9 = 36
4 / 9 = 0.4444444444444444

</sample-output>

</in-browser-programming-exercise>

<in-browser-programming-exercise name="Korjaa ohjelma: Tulostukset samalle riville" tmcname="osa01-12_korjaa_ohjelma_tulostukset_samalle_riville">

Jos `print`-komennolle annetaan lisäparametri `end = ""`, komento ei tulosta rivinvaihtoa merkkijonon jälkeen.

Esimerkiksi:

```python
print("Moi ", end="")
print("kaikki!")
```

<sample-output>

Moi kaikki!

</sample-output>

Korjaa ohjelma niin, että koko lasku tuloksineen tulostetaan yhdelle riville muuttamatta kuitenkaan `print`-komentojen määrää:

```python

print(5)
print(" + ")
print(8)
print(" - ")
print(4)
print(" = ")
print(5 + 8 - 4)
```

</in-browser-programming-exercise>

Kertauskysely tämän osion asioihin liittyen:

<quiz id="7322e73e-d6c5-5490-af97-ec69c45e720b"></quiz>
