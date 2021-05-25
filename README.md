Tanja Samardžić, gostujuće predavanje, ETF, Beograd, 25. 05. 2021.

# (Mikro)tokenozacija teksta 

Tekst je jedna od tri glavne vrste podataka:

<a href="https://youtu.be/ewokFOSxabs"><img src="figures/input-types.png" alt="data types" width="370"/>
<img src="figures/data-represent.png" alt="data-represent" width="210"/></a>

Izvor: [Khan Academuy](https://www.khanacademy.org/computing/computers-and-internet)

---
### 1. Problem segmentacije teksta 

U računarskoj obradi i analizi tekst se posmatra kao __niska simbola__ _T_ gde svaki simbol pripada skupu _V_ koji zovemo vokabular ili alfabet bez obzira na to kako se elementi definišu. Ako uzmemo da se alfabet sastoji od reči, onda je tekst niska reči; ako su elementi slova, onda je tekst niska slova itd.  

**Važno:** alfabet ``!=`` skup slova!


<img src="figures/Text_basics.png" alt="data types" width="600"/>


Definisanje alfabeta se često uzima kao nešto trivijalno; podrazumeva se da je alfabet skup __reči__ i da se tekst jednostavno deli na reči. Pod pojmom __token__ podrazumeva svako pojavljivanje svake reči u datom teksu. Segmentacija teksta na tokene se naziva __tokenizacija__ i može se uporediti sa segmentacijom slike na piksele ili sa segmentacijom zvuka na kratke okvire (engl. _frames_). Tokeni, dakle, služe kao jedinice obrade teksta na sličan način kao pikseli u slučaju slike i kratki okviri u u obradi zvuka. 


Za razliku od slike i zvuka, segmentaciji teksta se obično pristupa sa manje opreza. Podrazumevanje da su reči jasno razgraničeni tokeni je dosta problematična zabluda na koju lingvisti poprilično bezuspešno upozoravaju već duže vreme. Ovaj klip na engleskom objašnjava ukratko u čemu je problem. 
<br>
<br>

<a href="https://youtu.be/m8niIHChc1Y"><img src="figures/word-video.png" alt="word video" width="570"/></a>

<br>
<br>

Mi ćemo se ovde malo zadržati na jednom primeru iz srpskog:  


> Kod računara se sve može posmatrati kao "rekla-kazala" dokaz. 


Od koliko reči se sastoji ova rečenica? 


| 1 | 2      | 3 | 4   | 5          | 6        | 7            | 8            | 9          | 10  | 11   | 12 | 13  | 14 | 
|:--|:-------|:--|:----|:-----------|:---------|:-------------|:-------------|:-----------|:----|:-----|:---|:----|:---|
|Kod|računara|se |sve  |može        |posmatrati|kao           |"rekla-kazala"|dokaz.      |     |      |    |     |    |
|Kod|računara|se |sve  |može        |posmatrati|kao           |"             |rekla-kazala|"    |dokaz |.   |     |    |
|Kod|računara|se |sve  |može        |posmatrati|kao           |"             |rekla       |-    |kazala|"   |dokaz|.   |
|Kod|računara|sve|može |posmatratise|kao       |"             |rekla|-|kazala|"           |dokaz|.     |    |     |    |
|Kod|računara|sve|može |posmatratise|kao       |"rekla-kazala"|dokaz.        |            |     |      |    |     |    |


Važne pouke:

- Svi odgovori su na neki način tačni
- Od toga kako definišemo alfabet zavisi
  - dužina teksta 
  - frekvencija tokena u tekstu 

---

### 2. Problem retkih reči i mikrotokenizacija


<img src="figures/ex_eq.jpeg" alt="data types" width="330"/><img src="figures/ex_zipf.jpeg" alt="data-represent" width="330"/>

Frekvencija pojavljivanja nekog simbola u tekstu je, zapravo, srazmerna dužini tog simbola, koju obično izražavamo brojem slova. U našem (konstruisanom) primeru u grafikonu gore simbol `the` je najkraći i njegova verovatnoća je najveća. S druge strane, imamo, na primer, simbol `heaven` koji je znatno duži i ređi (manja verovatnoća). 

Ako tekst posmatramo kao kôd u smislu __teorije informacija__, onda odnos između dužine simbola i njegove verovatnoće proističe iz __komunikativne efikasnosti jezika__. Ovaj odnos je pokazao __Zipf__ još davne 1949![1] Da pojednostavimo, zbog komunikativne efikasnosti jezika, kratke reči ćemo viđati često u bilo kom tekstu i moći ćemo da procenimo njihovu verovatnoću, dok će duge reči biti retke. __Retko pojavljivanje reči__ predstavlja problem za obradu jezika ne samo zato što je teško proceniti njihovu verovatnoću, već i zato što su izvor nepoznatih simbola za trenirane modele. Koliko god da je veliki set podataka za treniranje modela, uvek će veliki broj reči ostati van njega i time biti nepoznat modelu. 

Problem retkih reči je dodatno pojačan time što je razlika u frekvenciji reči uopšte ogromna: samo nekoliko najfrekventnijih elemenata skupa _V_ zauzima veliki deo (i do polovine) teksta, dok otprilike polovina skupa _V_ ima frekvenciju 1. Ovaj odnos između frekvencijskog ranga i broja simbola koji se nalaze u njemu je poznat kao __Zipfovo pravilo__ (formlisano još 1935!). Njegovo poreklo nije još sasvim ispitano, ali verovatno ima veze sa raznim drugim prirodnim i družtvenim distribucijama. Drugim rečima, učestalost simbola u jeziku verovatno proističe iz učestalosti događaja u realnosti izvan jezika.  


Da se vratimo na problem tokenizacije, lingvisti su uočili da dužina reči ne potiče samo od komunikativne efikasnosti, već zavisi i od strukturnog tipa jezika: jezici koji imaju razvijenu __morfologiju__ u principu imaju duže reči. Tu je interesantno pogledati distribuciju medijalne dužine reči u različitim jezicima, koji je raspon vrednosti i oblik distribucije. Sledeći grafikon prikazuje procenu dužine reči u 81 jeziku na tekstu Wikipedie. 


<img src="figures/mwl_wiki.png" alt="mwl" width="380"/>


Šta mislite, koja je medijalna dužina reči u srpskom?

Kao logično rešenje za problem retkih reči se nameće redefinisanje skupa _V_ time što se uobičajene reči rastave na manje segmente, koji zatim postaju članovi skupa _V_. Ovaj proces nazivamo __mikrotokenizacija__ (eng. subword tokenization). Ovo rešenje, međutim, otvara ogromno polje mogućnosti, jer u principu bilo koja podniska reči može da bude mikrotoken. Tu ulazi u igru i morfologija: duge reči se često sastoje od regularnih podniski od kojih su neke široko produktivne. Primer imamo već u našem opisu teksta kao niske simbola na početku: podniska `ed` se pojavljuje u dve reči i uopšte je nešto što može da bude sastavni deo skoro bilo kog glagola u engleskom. To je, dakle, sjajan kandidat za novog člana skupa _V_. Svi savremeni modeli za obradu jezika funkcionišu sa mikrotokenima, ali je njihova identifikacija i dalje predmet istraživanja. 


---

### 3. Algoritmi za mikrotokenizaciju

Problema rastavljanja reči na manje segmente se već dugo istražuje u obradi jezika, ali više kao izuzetno izazovan teorijski problem, nego iz praktičnih razloga. Tu glavnu reč vode istraživači iz Finske, pošto finski ima izuzetno razvijenu morfologiju koja predstavlja problema za obradu jezika. Oni su prvi razvili sistem za raščlanjivanje reči koji je ušao u široku upotrebu: [Morfessor](https://morfessor.readthedocs.io/en/latest/). Osim toga, oni već tradicionalno organizuju međunarodno takmičenje iz raščlanjivanja reči pod imenom [Morpho Challenge](http://morpho.aalto.fi/events/morphochallenge/).  

Pre nekog vremena (2016), Sennrich i njegov tim su došli do mnogo jednostavnijeg rešenja: [Byte Pair Encoding (BPE)](https://github.com/rsennrich/subword-nmt). To je generalna tehnika za kompresiju podataka koja, kad se primeni na tekst, daje slične rezultate kao morfološka analiza. Ova tehnika je brzo stekla ogromnu popularnost u treniranju jezičkih modela za mašinsko prevođenje i uopšte.

Morfessor i BPE uzimamo ovde kao predstavnike dva glavna pristupa problemu. Osim njih, još dva algoritma su vrlo popularna: [Unigram](https://arxiv.org/pdf/1804.10959.pdf) (sličan pristup kao Morfessor) i [WordPiece](https://static.googleusercontent.com/media/research.google.com/ja//pubs/archive/37842.pdf) (sličan pristup kao BPE). 

Kako rade Morfessor i BPE:

<br>


<img src="figures/BPE_Morf.jpg" alt="data-represent" width="920"/>

<br>
<br>
.....................................................................................................................................................................................................................
<br>
<br>

<a href="https://tube.switch.ch/videos/kk6E3wHDXv"><img src="figures/BPE_0-5.png" alt="BPE steps" width="500"/></a>

<br>
<br>

U raščlanjivanju reči se ponekad ide sve do pojedinačnih slova. Interesantno je da nijedan pristup ne garantuje dobru segmentaciju teksta. Što su tokeni duži, to je skup _V_ veći a time je veći i broj retkih simbola. Što su tokeni kraći, to je tekst kao niska duži, što nosi druge probleme. Na primer, dužina niske dovodi do problema sa gubljenjem gradijenta kod LSTM mreža, dok kod Transformer mreža dovodi do kombinatorne eksplozije. Cilj je, dakle, naći optimalnu tačku između slova i reči, što još nikom nije pošlo za rukom.  

---
### 4. Mikrotokenizacija u diskretnoj i kontinualnoj reprezentacija teksta

Mikrotekinazija je postala sastavni deo obrade jezika tek otkad se prešlo na rad sa neuronskim mrežama. Bilo je pokušaja i ranije da se regularnosti unutar reči iskoriste za skraćivanje tokena, ali su rezultati često bili iznenađujuće slabi. Vilar i njegov tim 2007 postavljaju pitanje [prevođenja na nivou karaktera](https://www.aclweb.org/anthology/W07-0705/). Ispostavlja se da je segmentacija na karaktere donekle korisna za vrlo bliske jezike. Zapravo, individualni karakteri su, ipak, prekratki, tako da se kao jedinice uzimaju podniske od dva karaktera (engl. character bigrams). 

U radu iz 2009, [de Gispert i tim](https://www.aclweb.org/anthology/N09-2019/) uspevaju da poboljšaju statističko mašinsko prevođenje mikrotokenizacijom, ali i ovde su se prvobitna očekivanja ispostavila kao naivna. Naime, za uspeh je bilo potrebno uzeti u obzir više mogućih analiza iste reči, pa odabrati u toku dekodiranja jednu od njih. Inače, ako se segmentacija reči uzme kao data, to umanjuje konačni skor. 

Konačno, sa prelaskom na vektorsku (kontinualnu) reprezentaciju teksta, nedge od 2013, mikrotokenizacija je počela da daje dobre rezultate na svim nivoima i postala je neizostavni deo jezičke obrade. Nije i dalje sasvim ispitano zašto mikrotokenizacija ima više smisla u kontinualnoj reprezentaciji, ali se odgovor verovatno krije u činjenici da je tok informacija fluidniji u kontinualnoj reprezentaciji i da granice između tokena nisu toliko striktne kao u ranijim diskretnim modelima. 

Nedavno je Google izbacio novi model [CANINE](https://arxiv.org/abs/2103.06874) za koji tvrde da rešava pitanje tokenizacije tako što gradi celokupnu reprezentaciju teksta sam počevši od karaktera. Iako rezultati njihovih eksperimenata pokazuju poboljšanje u odnosu na druge metode, dosta pitanja ostaje otvoreno. 

---
### 5. Kako do optimalne dužina mikrotokena

Ideja mog tima kako da se poboljša mikrotokenizacija za sve jezike zasniva se na [Menzerat-Altmanovom pravilu](https://en.wikipedia.org/wiki/Menzerath%27s_law), koje je, za raazliku od Zipfovog pravila (verovatno i zbog njega!), ostalo dosta nepoznato široj istraživačkoj zajednici iako datira još iz 1954. 

Suština ovog pravila je da se duže reči segmentiraju na kraće segmente ili, prema Menzeratovoj opštoj formulaciji, što je celina veća to su njeni delovi manji. Gerlah[2] je pokazao kako ovo pravilo funkcioniše u nemačkom:


|Dužina reči | Frekvencija | Prosečna dužina segmenta |
|:-------------|:--------------|---------------------------|
|1 | 2391 | 4.53 |
|2 | 6343 | 3.25 |  
|3 | 4989 | 2.93 |  
|4 | 1159 | 2.78 |  
|5 | 112 | 2.65 |  
|6 | 13 | 2.58 |  

<br>

U jednoj još neobjavljenoj studiji, moj tim je pokazao da je moguće navesti i BPE i Morfessor da daju bolje segmente putem podešavanja postojećih parametara prema Menzerat-Altmanoviom pravilu. Kod BPE, u pitanju je broj iteracija dok se kod Morfessora može podesiti ponderiranje prethodne verovatnoće, koja daje prednost kraćim segmentima. Iako su naša inicijalna podešavanja dosta gruba, kod Morfessora dovode do značajnog poboljšanja, dok je kod BPE rezultat za sada dosta skroman.  

Odnos između dužine reči i prosečne dužine segmenta se može opisati dosta jednostavnom funkcijom koja se u principu može integrisati u algoritme za mikrotokenizaciju. Kako tačno to izvesti, ostaje za dalje razmišljanje.

---
\[1\]: George Kingsley Zipf (1949), Human behavior and the principle of least effort, Addison-Wesley Press

\[2\]: Reiner Gerlach (1982), Zur Überprüfung des menz-erathschen gesetzes im bereich der morphologie.Glottometrika, 4(2):95–102.


