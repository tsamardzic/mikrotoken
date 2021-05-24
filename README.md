Tanja Samardžić, gostujuće predavanje, ETF, Beograd, 25. 05. 2021.

# (Mikro)tokenozacija teksta 

Tekst je jedna od tri glavne vrste podataka:

<a href="https://youtu.be/ewokFOSxabs"><img src="input-types.png" alt="data types" width="370"/>
<img src="data-represent.png" alt="data-represent" width="210"/></a>

Izvor: [Khan Academuy](https://www.khanacademy.org/computing/computers-and-internet)

---
### 1. Problem segmentacije teksta 

U računarskoj obradi i analizi tekst se posmatra kao niska simbola _T_ gde svaki simbol pripada skupu _V_ koji zovemo vokabular ili alfabet bez obzira na to kako se elementi definišu. Ako uzmemo da se alfabet sastoji od reči, onda je tekst niska reči. Ako su elementi slova, onda je tekst niska slova itd.  

**Važno:** alfabet ``!=`` skup slova!


<img src="Text_basics.png" alt="data types" width="600"/>


Definisanje alfabeta se često uzima kao nešto trivijalno, nešto što se može zdravorazumski rešiti segmentacijom teksta na __tokene__, gde se pod pojmom _token_ podrazumeva svako pojavljivanje svakog simbola u datom teksu. Segmentacija teksta na tokene se naziva _tokenizacija_ i može se uporediti sa segmentacijom slike na piksele ili sa segmentacijom zvuka na kratke okvire (engl. _frames_). Tokeni, dakle, služe kao jedinice obrade teksta na sličan način kao pikseli u slučaju slike i kratki okviri u u obradi zvuka. Za razliku od slike i zvuka, segmentaciji teksta se obično pristupa sa manje opreza. Reči se obično smatraju tokenima, tj. jedinicama inicijalne obrade koje je lako identifikovati. To je, međutim, dosta problematična zabluda na koju lingvisti dosta bezuspešno upozoravaju već duže vreme. Ovaj klip na engleskom objašnjava ukratko u čemu je problem. 
<br>
<br>


<iframe width="560" height="315" src="https://www.youtube.com/embed/m8niIHChc1Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br>
<br>

Mi ćemo se ovde malo zadržati na jednom primeru na srpskom:  


> Kod računara se sve može posmatrati kao "rekla-kazala" dokaz. 


Od koliko reči se sastoji ova rečenica? 


| 1 | 2      | 3 | 4   | 5          | 6        | 7            | 8            | 9          | 10  | 11   | 12 | 13  | 14 | 
|:--|:-------|:--|:----|:-----------|:---------|:-------------|:-------------|:-----------|:----|:-----|:---|:----|:---|
|Kod|računara|se |sve  |može        |posmatrati|kao           |"rekla-kazala"|dokaz.      |     |      |    |     |    |
|Kod|računara|se |sve  |može        |posmatrati|kao           |"             |rekla-kazala|"    |dokaz |.   |     |    |
|Kod|računara|se |sve  |može        |posmatrati|kao           |"             |rekla       |-    |kazala|"   |dokaz|.   |
|Kod|računara|sve|može |posmatratise|kao       |"             |rekla|-|kazala|"           |dokaz|.     |    |     |    |
|Kod|računara|sve|može |posmatratise|kao       |"rekla-kazala"|dokaz.        |            |     |      |    |     |    |


<a href="https://tube.switch.ch/videos/kk6E3wHDXv"><img src="BPE_0-5.png" alt="BPE steps" width="500"/></a>

### 2. Problem retkih reči 

### 3. Mikrotokenizacija i diskretna reprezentacija teksta

### 4. Mikrotokenizacija i kontinualna reprezentacija teksta

### 5. Dužina mikrotokena




