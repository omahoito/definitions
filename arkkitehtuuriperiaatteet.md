# Arkkitehtuuriperiaatteet
Käytännöt kehittyvät jatkuvasti. Sääntöjä ei tule ohittaa millään tekosyyllä, vaan tarpeen tullen niihin tulee ehdottaa muutoksia. Kehitykseen osallistuvien tulee tuntea olevansa oikeutettu joko ehdottamaan muutosta tai tekemään muutos sen mukaan mihin heidän rooli organisaatiossa sallii.

Arkkitehtuuriperiaatteita käytetään pohjana suunnittelupäätöksten tekemisen kun kehitetään uusia verkkopalveluita. Arkkitehtuuriperiaatteet on laadittu mahdollistamaan hajautettu palvelukehitys avoimen lähdekoodin ympäristössä, hajautetussa pilotoinnissa ja Lean-sovelluksehityksessä.

## Modulaariset, vaihdettavat rakennuspalikat
Suosimme modulaarisuutta mahdollistaaksemme hajautetun, standardipohjaisen ja yhteentoimivan systeemilähtöisen palvelumuotoilun. Ratkaisumme hyödyntää avoimen lähdekoodin komponentteja, kirjastoja ja olemassaolevia sovelluksia. Muotoilu tulee pitää modulaarisena niin että yksittäiset toiminnallisuudet voidaan vaihtaa toisiin vastaaviin, riippuen vaikkapa teknologian elinkaarien vaiheista. Näin mahdollistetaan palveluratkaisut jotka on rakennettu muuttumaan (built to change) ennemmin kuin kestämään (built to last). http://en.wikipedia.org/wiki/Separation_of_concerns

## Modulaarinen API ensin -sovellus
Rakennamme sovellukset sovellusrajapinta edellä, sitten kehitämme käyttökokemuksen joka tästä seuraa. Tämä mahdollistaa synergisen monikanavaisen lähestymisen joka yhdistää käyttökokemuksen ja konekäyttötapaukset mahdollistaen uudelleenkäytettävyyden, minimoiden regressiot. Uudelleenkäytettävät sovellusmoduulit viestivät sovelluksen sisäisesti standardirajapintojen kautta niin palvelimella kuin päätelaitteessa. API ensin -lähestyminen mahdollistaa Lean-kehittämisen jossa voimme kehittää muuttaen yhtä asiaa kerrallaan jo sovellusversioiden välissä.

## Mikropalveluihin perustuva palveluratkaisu
Mikropalvelu on palvelu jota voi kehittää itsenäisesti riippumatta muista mikropalveluista. Se toteuttaa yhden vastuullaan olevan toiminnallisen kokonaisuuden. Palvelu tarjoaa hyvin määritellyn rajapinnan, ja palvelu voidaan helposti korvata saman rajapinnan toteuttavalla uudella palvelulla. Mikropalveluilla rakennamme korvattavissa olevia kokonaisuuksia, jolloin riskienhallinta helpottuu ja ratkaisun elinkaari pitenee. Mikropalvelut mahdollistavat standardoidun/vakioidun kehittämismallin rajapinnoille, jotka halutaan liittää rinnakkaisjärjestelmiin (API Discovery, Watchdog/recovery etc). Mikropalvelut mahdollistaa tarkan kapasiteetin resurssoinnin kun pullonkaulat on helpompi tunnistaa, skaalata ja ajaa tarkoituksenmukaisissa paikoissa sovellusarkkitehtuurin kannalta.

## Yksinkertaisuus ja minimalistinen käyttökokemus
Kun suunnitellaan toiminnallisuuksia, toteutetaan yksinkertaisin mahdollinen toiminnallisuus tai käyttöliittymä jolla käyttäjä saa ydintoiminnallisuuden hyödyt. Näin jätämme tilaa palvelun varsinaiselle sisällölle.
Valittaessa toiminnallisuuksia, keskitytään yksinkertaisimpaan ratkaisuvaihtoehtoon ratkaisun ydinhyötyjen maksimoimimiseksi. Päätökset sivukäyttötapausten toteuttamisesta tulee perustua mittaustuloksiin tai reaalimaailman käytön havainnointiin.

## Käyttäjäkeskeisyys
Pyrimme aina asettamaan kansalaisen kokeman hyödyn etusijalle sunnittelupäätöksissä.

## Lean palvelumuotoilu
Päätettäessä kehitettäviä toiminnallisuuksia, perustamme päätöksenteon jatkuvaan palautteen ja käyttöstatistiikan takaisinsyöttöön käyttäjiltä sovelluskehittäjille. Sovellukset rakennetaan pilotoimalla, selkeällä fokuksella aitojen konkreettisten mitattavien ongelmien ratkaisuun. Arvioimme käyttökokemuksellisten ratkaisujen menestyksen testaamalla sitä varsinaisia tunnistettuja  tarpeita vasten seuraamalla käyttäjien toimintaa piloteissa, ennemmin kuin luottaen arvioihin, spekulaatioihin tai ammattitestaajiin. Alustaa tullaan kehittämään pienissä iteraatioissa, ja toiminnallisuudet jalostuvat käyttäjätyytyväisyyttä jatkuvasti kohentaen.

## Monikielisyys
Kaikki komponentit tulee merkitä kielikoodilla kieliversioinnin mahdollistamiseksi. Kaikki koodi ja kommentit tehdään kansainvälisellä englannilla.

## Tekninen takaisinsyöte
Toteamme karkeasti konsensuksen ja alamme työstämään toimivaa koodia välittömästi. Implementaatiomme uudelleenkäyttää standardeja, milloin käytettävissä IETF/W3C/HL7 lähteistä, ja jaamme palautteen alkuperäiselle laatijalle kun tunnistamme parhaita käytäntöjä tai käyttäjätodisteluja. Rakennamme takaisinsyöttösilmukoita käyttäjien ja kehittäjien välille sekä varmistamme standardointiyhteisöjen osallisuuden ehdotustemme ratifiointiin.

## Ketterä kehitys
Jaamme ja julkaisemme avoimen lähdekoodin ohjelmistoja suosiaksemme nopeaa innovatiivista kehittämistä sekä välttääksemme toimittaja- ja teknologialukkoja. Tämä voidaan saavuttaa rajapinta- ja standardikeskeisellä kehittämistavalla sekä pitämällä yllä erilaisia teknologiavaihtoehtoja eri palvelumoduleille. Rakennamme moduleille elinkaaria päästäksemme standardoituihin taksonomioihin ja rajapintoihin.
Uudelleenkäytettävä ja refaktoroitu
Uudelleenkäytämme dataa ja uudelleenkirjoitamme koodia. Ymmärrämme että sovelluskehitystyö on toisinaan myös aloitettava puhtaalta pöydältä taas seuraavassa iteraatiossa.

## Systeemimuotoilu
Suunnittelemme systeemisiä järjestelmiä ennemmin kuin yksittäisiä sovelluksia. Web-muotoilun kehittyessä, olemme tunnistaneet tarpeen kehittää ennemmin ajatuksella punnittuja syklisesti toimivia järjestelmiä kuin yksinkertaisten web-sivujen kokoelmia. Systeemimuotoilulähestyminen mahdollistaa useampien käyttäjätarinoiden toteutumisen pienemmällä määrällä komponentteja. http://en.wikipedia.org/wiki/Systems_design Kukin komponentti tulee yksikkötestata komponenttien uudelleenkäyttämisen mahdollistamiseksi.

## Pieni on kaunista
Luomme ennemmin pieniä kuin isoja sovellusmoduuleja https://en.wikipedia.org/wiki/Unix_philosophy ja datakokonaisuuksia http://en.wikipedia.org/wiki/Small_data. Toiminnallisuuksien ja datakokonaisuuksien tulee olla riittävän pieniä ihmisen hahmotettavaksi.

## Avoin tieto
Avoin tieto on tietoa jota kuka tahansa voi vapaasti lukea, käyttää, muokata ja jakaa mihin tahansa käyttötarkoitukseen. http://opendefinition.org/od/1.1/fi/ Avoin data vähentää transaktiokustannuksia organisaatioiden välillä ja sallii käyttäjille reilun pääsyn kultturiperintöönsä, kannustaen osallistumiseen ja datan uudelleenkäyttöön.

## Avoin lähdekoodi
Luomme sovelluksia ja ratkaisuja joita voi avoimesti ja laajasti uudelleenkäyttää, näin avaten kehittäjäyhteisölle tilaisuuden tunnistaa tuotoksillemme odottamattomia hyötyjä. https://coss.fi/avoimuus/avoin-lahdekoodi/

## Avoin rajapinta
Tullaakseen tehokkaasti sovelluskehittäjien uudelleenkäyttämäksi, data on julkaistava yksinkertaisessa ja avoimessa muodossa. Helposti käytettävien teknologioiden ja rajapintakäytäntöjen käyttö helpottaa datan tutkimista ja käyttötarkoitusten löytämistä.  http://avoinrajapinta.fi/

## Avoimet standardit
Avoimet standardit ovat vapaasti kaikkien saatavissa. Suljettujen, toimittajakohtaisten standardien käyttö nostaa osanoton kynnystä ja estää ei-kaupallisten kehittäjien osallistumisen toimintaan.

## My Data -tietoisuus
Otamme huomioon yksilön oikeudet saada vapaasti käyttöönsä itseään koskevat tiedot sekä tiedot, jotka on luotu hänen omistuksessaan olevin välinein. Kunnioitamme kaikkia oikeuksia jotka koskevat datan suojausta ja käyttäjien oikeuksia ja toissijaisesti kaupallisia, tieteellisiä ja julkishallinnon oikeuksia henkilökohtaiseen dataan. Avoimen datan tavoin  henkilökohtaisen datan (Mydata) tulee olla käyttäjän saatavilla teknisesti  mahdollisimman helposti. Se, että käyttäjät saavat hallinnoida omaa dataansa, luo luottamusta käyttäjien ja organisaatioiden välille. Käyttäjäkokemus joka mahdollistaa tietoisen datan avaamisen organisaatioille on toivottavampi lähestymistapa kuin monimutkaiset tai epämääräiset juridiset kuvaukset siitä mitä organisaatio voi käyttäjän datalla tehdä.
