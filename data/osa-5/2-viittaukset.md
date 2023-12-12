---
path: '/osa-5/2-viittaukset'
title: 'Viittaukset'
hidden: false
---

<text-box variant='learningObjectives' name='Oppimistavoitteet'>

Tämän osion jälkeen

- Tiedät, mitä tarkoitetaan viittaustyyppisellä muuttujalla
- Tiedät, että samaan olioon voi olla useampia viittauksia
- Osaat käyttää listoja funktioiden parametreina
- Tiedät, mitä tarkoitetaan funktion sivuvaikutuksella

</text-box>

Olemme tähän asti ajatelleet, että muuttuja on eräänlainen "laatikko", joka sisältää muuttujan arvon. Teknisesti ottaen tämä ei pidä paikkaansa Pythonissa: muuttujat eivät sisällä arvoa vaan ne _viittaavat_ arvona olevaan _olioon_, kuten lukuun, merkkijonoon tai listaan.

Käytännössä tämä tarkoittaa, että muuttujaan _ei tallenneta_ arvoa, vaan tieto siitä paikasta, mistä muuttujan arvo löytyy.

Viittausta voidaan kuvata nuolena muuttujasta sen varsinaiseen arvoon:

<img src="5_2_1.png">

Viittaus siis kertoo, mistä varsinainen arvo löytyy. Funktio `id` kertoo, mihin muuttuja viittaa:

```python
a = [1, 2, 3]
print(id(a))
b = "Tämäkin on viittaus"
print(id(b))
```

<sample-output>

4538357072
4537788912

</sample-output>

Viittaus eli muuttujan id on kokonaisluku, jonka voi ajatella olevan muuttujan arvon sijainnin osoite tietokoneen muistissa. Huomaa, että jos suoritat yllä olevan koodin omalla koneellasi, tulos on luultavasti erilainen, koska muuttujilla on eri viitteet.

Kuten jo [edellisen osan](/osa-5/1-lisaa-listoja#sisakkaisia-listoja-kayttavan-koodin-visualisointi) esimerkistä näimme, Python Tutorin visualisaattori näyttää viitteet "nuolina" varsinaiseen sisältöön. Visualisaattori kuitenkin "huijaa" merkkijonojen tapauksessa ja näyttää ne ikään kuin merkkijonon sisältö olisi tallennettu muuttujan sisälle:

<img src="5_2_1a.png">

Näin ei kuitenkaan ole todellisuudessa, vaan merkkijonotkin käsitellään Pythonin sisäisissä rakenteissa samaan tapaan kuin listat.

Monet Pythonin sisäänrakennetut tyypit, kuten `str`, ovat _muuttumattomia_. Tämä tarkoittaa, että olion arvo ei voi koskaan muuttua. Sen sijaan arvo voidaan korvata uudella arvolla:

<img src="5_2_2.png">

Pythonissa on myös tyyppejä, jotka ovat muuttuvia. Esimerkiksi listan sisältö voi muuttua ilman, että tarvitsee luoda kokonaan uusi lista:

<img src="5_2_3.png">

Hieman yllättäen myös lukuja ja totuusarvoja edustavat perustietotyypit `int`, `float` ja `bool` ovat muuttumattomia. Tarkastellaan esimerkkinä seuraavaa koodia:

```python
luku = 1
luku = 2
luku += 10
```

Vaikka vaikuttaa siltä, että koodi muuttaa lukua, teknisesti ottaen ei näin ole, vaan jokainen komento luo uuden luvun.

Seuraavan ohjelman tulostus on mielenkiintoinen:

```python
luku = 1
print(id(luku))
luku += 10
print(id(luku))
a = 1
print(id(a))
```

<sample-output>

4535856912
4535856944
4535856912

</sample-output>

Aluksi muuttuja `luku` viittaa paikkaan 4535856912, ja kun muuttujan arvo muuttuu, se alkaa viitata paikkaan 4535856944. Kun muuttujaan `a` sijoitetaan arvo 1, se alkaa viitata samaan paikkaan kuin mihin `luku` viittasi, kun sen arvo oli 1.

Vaikuttaakin siltä, että Python on tallentanut luvun 1 paikkaan 4535856912, ja aina kun jonkin muuttujan arvona on 1, muuttuja _viittaa_ tuohon paikkaan "tietokoneen muistissa".

Vaikka perustietotyypit `int`, `float` ja `bool` ovat viittauksia, ohjelmoijan ei oikeastaan tarvitse välittää asiasta.

## Useampi viittaus samaan listaan

Tarkastellaan esimerkkinä listamuuttujan arvon kopiointia:

```python
a = [1, 2, 3]
b = a
b[0] = 10
```

Sijoitus `b = a` kopioi muuttujan `a` arvon muuttujaan `b`. On tärkeä kuitenkin huomata, että muuttujan arvona _ei ole lista_ vaan _viittaus listaan_.

Sijoitus `b = a` siis kopioi viittauksen, minkä seurauksena kopioinnin jälkeen samaan listaan on kaksi viittausta:

<img src="5_2_4.png">

Listaa voidaan käsitellä kumman tahansa viittauksen avulla:

```python
lista = [1, 2, 3, 4]
lista2 = lista

lista[0] = 10
lista2[1] = 20

print(lista)
print(lista2)
```

<sample-output>

[10, 20, 3, 4]
[10, 20, 3, 4]

</sample-output>

Mikäli samaan listaan on useampia viittauksia, sitä voidaan käsitellä minkä tahansa viittauksen kautta samalla tavalla. Toisaalta yhden viittauksen kautta tehtävä muutos heijastuu myös muihin viittauksiin.

Visualisaattori näyttää jälleen selkeästi mitä ohjelmassa tapahtuu:

<img src="5_2_4a.png">

## Listan kopiointi

Jos haluamme tehdä listasta erillisen kopion, voimme luoda uuden listan ja lisätä siihen jokaisen aluperäisen listan alkion:

```python
lista = [1, 2, 3, 3, 5]

kopio = []
for alkio in lista:
    kopio.append(alkio)

kopio[0] = 10
kopio.append(6)
print("lista", lista)
print("kopio", kopio)
```

<sample-output>

lista [1, 2, 3, 3, 5]
kopio [10, 2, 3, 3, 5, 6]

</sample-output>

Visualisaattorilla tarkastellen kopiointi näyttää seuraavalta:

<img src="5_2_4b.png">

Muuttuja `kopio` siis viittaa nyt eri listaan kuin muuttuja `lista`.

Helpompi tapa listan kopioimiseen on hyödyntää `[]`-operaattoria, johon tutustuimme aiemmin kurssilla. Merkintä `[:]` tarkoittaa, että listalta valitaan kaikki alkiot, ja tämän sivuvaikutuksena syntyy kopio listasta:

```python
lista = [1,2,3,4]
kopio = lista[:]

lista[0] = 10
kopio[1] = 20

print(lista)
print(kopio)
```

<sample-output>

[10, 2, 3, 4]
[1, 20, 3, 4]

</sample-output>

## Lista funktion parametrina

Kun lista välitetään parametrina funktiolle, välitetään viittaus listaan. Tämä tarkoittaa, että funktio voi muuttaa parametrinaan saamaansa listaa.

Esimerkiksi seuraava funktio lisää uuden alkion parametrinaan saamaansa listaan:

```python
def lisaa_alkio(lista: list):
    uusi_alkio = 10
    lista.append(uusi_alkio)

lista = [1,2,3]
print(lista)
lisaa_alkio(lista)
print(lista)
```

<sample-output>
[1, 2, 3]
[1, 2, 3, 10]
</sample-output>

Huomaa, että funktio `lisaa_alkio` ei palauta mitään, vaan muuttaa parametrinaan saamaansa listaa.

Visualisaattori havainnollistaa tilanteen seuraavasti:

<img src="5_2_4c.png">

_Global frame_ tarkoittaa pääohjelman muuttujia ja sinisellä oleva laatikko *lisaa_alkio* taas funktion parametreja ja muuttujia. Kuten visualisaatio havainnollistaa, funktio viittaa samaan listaan mihin pääohjelmakin viittaa, eli funktiossa listalle tehtävät muutokset näkyvät pääohjelmaan.

Toinen tapa olisi luoda uusi lista ja palauttaa se:

```python
def lisaa_alkio(lista: list) -> list:
    uusi_alkio = 10
    kopio = lista[:]
    kopio.append(uusi_alkio)
    return kopio

luvut = [1, 2, 3]
luvut2 = lisaa_alkio(luvut)

print("Alkuperäinen lista:", luvut)
print("Uusi lista:", luvut2)
```

<sample-output>

Alkuperäinen lista: [1, 2, 3]
Uusi lista: [1, 2, 3, 10]

</sample-output>

Jos et ole 100% varma mitä koodissa tapahtuu, käy sen toiminta läpi visualisaattorilla!

## Parametrina olevan listan muokkaaminen

Seuraavassa on yritys tehdä funktio, joka kasvattaa parametrina saamansa listan jokaista alkiota kymmenellä:

```python
def kasvata_kaikkia(lista: list):
    uusilista = []
    for alkio in lista:
        uusilista.append(alkio + 10)
    lista = uusilista

luvut = [1, 2, 3]
print("alussa ",luvut)
kasvata_kaikkia(luvut)
print("funktion jälkeen", luvut)
```

<sample-output>

alussa: [1, 2, 3]
funktion jälkeen: [1, 2, 3]

</sample-output>


Jostain syystä funktio ei kuitenkaan näytä toimivan. Mistä on kyse?

Funktiolle on välitetty parametrina _viite_ muutettavaan listaan. Sijoitus `lista = uusilista` saa aikaan sen, että parametriin talletettu viite muuttaa arvoaan funktion sisällä eli se alkaa viitata funktion sisällä luotuun uuteen listaan. Sijoitus ei kuitenkaan vaikuta funktion ulkopuolelle, siellä viitataan edelleen alkuperäiseen listaan.

Seuraava kuvasarja havainnollistaa, mihin eri muuttujat viittaavat ohjelman suorituksen aikana:

<img src="5_2_6.png" width="400">

Funktion sisällä muutettu lista siis "kadotetaan" kun funktiosta palataan, ja muuttuja `luvut` viittaa koko ajan alkuperäiseen listaan.

Visualisaattori on tässäkin tapauksessa ystävä: se näyttää selkeästi, miten funktio ei koske alkuperäiseen listaan ollenkaan vaan luo uuden listan, johon muutokset tehdään:

<img src="5_2_4d.png">

Yksi tapa korjata ongelma on kopioida uuden listan kaikki alkiot takaisin vanhaan listaan:

```python
def kasvata_kaikkia(lista: list):
    uusilista = []
    for alkio in lista:
        uusilista.append(alkio + 10)

    # kopioidaan vanhaan listaan uuden listan arvot
    for i in range(len(lista)):
        lista[i] = uusilista[i]
```

Pythonissa on olemassa myös ovela tapa sijoittaa monta alkiota kerrallaan listaan:

```python
>>> lista = [1, 2, 3, 4]
>>> lista[1:3] = [10, 20]
>>> lista
[1, 10, 20, 4]
```

Esimerkissä siis sijoitetaan "osalistaan" eli listan kohtiin 1 ja 2 taulukollinen alkioita.

Osalistaksi voidaan myös valita koko lista:

```python
>>> lista = [1, 2, 3, 4]
>>> lista[:] = [100, 99, 98, 97]
>>> lista
[100, 99, 98, 97]
```

Eli näin tulee korvatuksi koko vanhan listan sisältö. Siispä eräs toimiva versio funktiosta näyttää seuraavalta:

```python
def kasvata_kaikkia(lista: list):
    uusilista = []
    for alkio in lista:
        uusilista.append(alkio + 10)

    lista[:] = uusilista
```

...tai ilman listan kopiointia yksinkertaisesti sijoittamalla uudet arvot heti vanhaan listaan:

```python
def kasvata_kaikkia(lista: list):
    for i in range(len(lista)):
        lista[i] += 10

```


<programming-exercise name='Alkiot tuplana' tmcname='osa05-06a_alkiot_tuplana'>

Tee funktio `tuplaa_alkiot(luvut: list)`, joka saa parametrikseen lukuja sisältävän listan.

Funktio palauttaa uuden listan, jossa alkuperäisen listan alkiot on kerrottu kahdella. Funktio _ei_ saa muuttaa alkuperäistä listaa.

Esimerkki funktion kutsumisesta:

```python
if __name__ == "__main__":
    luvut = [2, 4, 5, 3, 11, -4]
    tuplaluvut = tuplaa_alkiot(luvut)
    print("alkuperäinen:", luvut)
    print("tuplattu:", tuplaluvut)
```
<sample-output>

alkuperäinen: [2, 4, 5, 3, 11, -4]
tuplattu: [4, 8, 10, 6, 22, -8]

</sample-output>

</programming-exercise>


<programming-exercise name='Poista pienin' tmcname='osa05-06b_poista_pienin'>

Tee funktio `poista_pienin(luvut: list)`, joka saa parametrikseen lukuja sisältävän listan.

Funktio etsii ja poistaa listasta pienimmän alkion. Voit olettaa, että pienin alkio esiintyy listassa vain kerran.

Funktio ei siis palauta mitään, vaan muokkaa parametrinaan saamaansa listaa!

Esimerkki funktion kutsumisesta:

```python
if __name__ == "__main__":
    luvut = [2, 4, 6, 1, 3, 5]
    poista_pienin(luvut)
    print(luvut)
```
<sample-output>

[2, 4, 6, 3, 5]

</sample-output>

</programming-exercise>


<programming-exercise name='Sudoku: ruudukon tulostus ja luvun lisäys' tmcname='osa05-07_sudoku_osa5'>

Tässä tehtävässä toteutetaan vielä kaksi funktiota sudokua varten: `tulosta` ja `lisays`.

Funktio `tulosta` saa parametriksi sudokuruudukkoa esittävän kaksiulotteisen listan ja tulostaa sen alla olevan esimerkkitulostuksen mukaisessa muodossa.

Funktio `lisays(sudoku: list, rivi_nro: int, sarake_nro: int, luku:int)` saa parametriksi sudokuruudukkoa esittävän kaksiulotteisen listan, rivi- ja sarakenumerot sekä luvun väliltä 1–9. Funktio lisää luvun parametrien ilmoittamaan kohtaan sudokuruudukkoa.

```python
sudoku  = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0]
]

tulosta(sudoku)
lisays(sudoku, 0, 0, 2)
lisays(sudoku, 1, 2, 7)
lisays(sudoku, 5, 7, 3)
print()
print("Kolme numeroa lisätty:")
print()
tulosta(sudoku)
```

<sample-output>

<pre>
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

Kolme numeroa lisätty:

2 _ _  _ _ _  _ _ _
_ _ 7  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ 3 _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

</pre>

</sample-output>

**Vihje**

Saatat tässä tehtävässä hyötyä siitä, että `print`-komentoa on mahdollista käyttää myös siten, että se ei aiheuta rivinvaihtoa:

```python
print("merkkejä ", end="")
print("ilman välejä", end="")
```

<sample-output>

merkkejä ilman välejä

</sample-output>

Joskus taas tarvitaan pelkkää rivinvaihtoa, ja se onnistuu seuraavasti:

```python
print()
```

</programming-exercise>

<programming-exercise name='Sudoku: luvun lisäys ruudukon kopioon' tmcname='osa05-08_sudoku_osa6'>

Viimeisessä sudokua käsittelevässä tehtävässä toteutetaan hieman erilainen versio funktiosta, jonka avulla sudokuruudukkoon lisätään uusia lukuja.

Funktio `kopioi_ja_lisaa(sudoku: list, rivi_nro: int, sarake_nro: int, luku:int)` saa parametreikseen sudokuruudukkoa esittävän kaksiulotteisen listan, rivinumeron, sarakenumeron sekä luvun väliltä 1–9. Funktio _palauttaa_ parametrina saadusta sudokuruudukosta _kopion_, johon on lisätty parametrina saatu luku parametrina saatuun sijaintiin sijoitettuna. Funktio _ei saa muuttaa_ parametrina annettua sudokuruudukkoa.

Seuraavassa on edellisen tehtävän funktiota `tulosta` hyödyntävä käyttöesimerkki:

```python
sudoku  = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0]
]

kopio = kopioi_ja_lisaa(sudoku, 0, 0, 2)
print("Alkuperäinen:")
tulosta(sudoku)
print()
print("Kopio:")
tulosta(kopio)
```

<sample-output>

<pre>
Alkuperäinen:
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

Kopio:
2 _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _
_ _ _  _ _ _  _ _ _

</pre>

</sample-output>

**Vihje** tässä tehtävässä pitää olla tarkkana mitä kaikkea tulee kopioida, ja mihin lisäys lopulta kohdistuu. Kuten yleensäkin, [visualisaattori](http://www.pythontutor.com/visualize.html#mode=edit) auttaa myös nyt. Sudokuruudukon koon takia näkymä tosin on hieman normaalia sekavampi.

</programming-exercise>

<programming-exercise name='Ristinolla' tmcname='osa05-09_ristinolla'>

Ristinollaa pelataan 3 x 3 -kokoisella ruudukolla, johon pelaajat merkitsevät vuorotellen ristin tai nollan. Pelin voittaa se pelaaja, joka saa ensimmäisenä kolme merkkiä pystyyn, vaakaan tai kulmittain. Peli päättyy tasapeliin, jos kumpikaan pelaaja ei saa kolmen sarjaa.

Kirjoita funktio `pelaa_siirto(lauta: list, x: int, y: int, nappula: str)`, jossa sijoitetaan annettu pelinappula annettuihin koordinaatteihin pelilaudalla. Koordinaattien arvot ovat väliltä 0..2.

**Huomaa** että tässä tehtävässä parametrit ovat eri päin kuin sudokussa, ensin annetaan saraketta kuvaava `x` ja sen jälkeen riviä kuvaava `y`.

Pelilauta koostuu merkkijonoista seuraavasti:

* `""`: tyhjä ruutu
* `"X"`: pelaajan 1 merkki
* `"O"`: pelaajan 2 merkki

Funktio palauttaa arvon `True`, jos nappula saatiin sijoitettua laudalle (eli jos paikka oli tyhjä), ja arvon `False`, jos paikka oli varattu TAI jos koordinaatin arvo oli liian pieni tai suuri (eli ei väliltä 0..2).

Esimerkiksi:

```python
lauta = [["", "", ""], ["", "", ""], ["", "", ""]]
print(pelaa_siirto(lauta, 2, 0, "X"))
print(lauta)
```

<sample-output>

True
[['', '', 'X'], ['', '', ''], ['', '', '']]

</sample-output>

</programming-exercise>

<programming-exercise name='Matriisin kääntö' tmcname='osa05-10_matriisin_kaanto'>

Kirjoita funktio `transponoi(matriisi: list)`, joka saa parametrikseen kaksiulotteisen kokonaislukuja sisältävän taulukon eli matriisin. Funktio _transponoi_ matriisin eli muuntaa rivit sarakkeiksi ja päinvastoin.

Voit olettaa, että matriisissa on yhtä monta riviä kuin sarakettakin (eli matriisi on _neliömatriisi_).

Esimerkiksi matriisista

```python
1 2 3
4 5 6
7 8 9
```

tulisi transponoinnin jälkeen tällainen:

```python
1 4 7
2 5 8
3 6 9
```

Funktio ei palauta mitään, vaan muokkaa parametrinaan saamaansa matriisia.

</programming-exercise>

## Funktioiden sivuvaikutukset

Koska funktio saa parametrinaan viittauksen listaan, se voi muuttaa tätä listaa. Jos funktion varsinaisena tarkoituksena ei ole muuttaa listaa, muutokset voivat aiheuttaa ongelmia toisaalla ohjelmassa.

Tarkastellaan esimerkkinä funktiota, jonka tarkoituksena on etsiä listan toiseksi pienin alkio:

```python
def toiseksi_pienin(lista: list) -> int:
    # järjestetyn listan toiseksi pienin alkio on kohdassa 1
    lista.sort()
    return lista[1]

luvut = [1, 4, 2, 5, 3, 6, 4, 7]
print(toiseksi_pienin(luvut))
print(luvut)
```

<sample-output>
2
[1, 2, 3, 4, 4, 5, 6, 7]
</sample-output>

Funktio kyllä etsii ja löytää toiseksi pienimmän alkion, mutta sen lisäksi se muuttaa listan alkioiden järjestyksen. Jos järjestyksellä on merkitystä muualla ohjelmassa, funktion kutsuminen voi aiheuttaa virheitä. Esimerkin kaltaista muutosta viittauksena saatuun olioon kutsutaan funktion _sivuvaikutukseksi_.

Voimme toteuttaa funktion ilman sivuvaikutuksia näin:

```python
def toiseksi_pienin(lista: list) -> int:
    kopio = sorted(lista)
    return kopio[1]

luvut = [1, 4, 2, 5, 3, 6, 4, 7]
print(toiseksi_pienin(luvut))
print(luvut)
```

<sample-output>

2
[1, 4, 2, 5, 3, 6, 4, 7]

</sample-output>

Koska funktio `sorted` palauttaa uuden järjestetyn listan, toiseksi pienimmän alkion etsiminen ei enää sotke listan alkuperäistä järjestystä.

Usein pidetään hyvänä asiana, että funktiot eivät aiheuta sivuvaikutuksia, sillä sivuvaikutukset voivat hankaloittaa ohjelmien toimivuuden varmistamista.

Sivuvaikutuksettomia funktioita kutsutaan myös _puhtaiksi funktioiksi_ ja erityisesti funktionaalista ohjelmointityyliä käytettäessä funktiot pyritään rakentamaan näin. Palaamme aiheeseen tarkemmin _Ohjelmoinnin jatkokurssilla_.

<quiz id="7384687a-fe87-53d4-be99-05a98f5e467e"></quiz>
