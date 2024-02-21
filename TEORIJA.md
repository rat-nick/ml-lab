# Masinsko ucenje

## Osnovni pojmovi

### Uvod


Mašinsko učenje je grana veštačke inteligencije koja se bavi razvojem algoritama i tehnika koji omogućavaju računarima da nauče kako da donose odluke ili izvode zadatke na osnovu podataka, bez eksplicitnog programiranja.

### Grupisanje mašinskog učenja
Ono se deli na 3 velike grupe:

1. Nadgledano mašinsko učenje (supervised learning)
2. Nenadgledano mašinsko učenje (unsupervised learning)
3. Učenje pojačanjem (reinforcement learning)

### Nadgledano učenje

Nadgledano učenje jeste svako učenje gde su sama **ciljana promenjiva** i njena vrednost unapred poznate za svaki slučaj iz skupa podataka.
Zadatak ovih modela jeste da optimizuju svoje parametre tako da što manje greše u predviđanju vrednosti ciljane promenjive. Algoritmi nadgledanog učenja rešavaju probleme **klasifikacije** i **regresije**.

### Nenadgledano učenje

Nenadgledano učenje jeste ono učenje gde ciljana promenjiva nije unapred definisana.
Zadatak ovih modela je da traže obrasce, strukture ili grupisanja unutar podataka kako bi stekli razumevanje problema ili identifikovali skrivene karakteristike. Algoritmi nenadgledanog učenja rešavaju probleme **klasterizacije** i **redukcije dimenzionalnosti**.

## Skup podataka

Ono bez čega ne mogu funkcionisati algoritmi mašinskog učenja jesu podaci, i to u velikim količinama.
Skupovi podataka mogu da budu u različitim oblicima, ali razlikujemo dve različite grupe:

1. Struktuirani podaci (tabelarni, json format, svaki element ima istu strukturu)
2. Nestruktuirani podaci (stream-ovi bajtova, loše ili nepostojeće strukture)

### Struktuirani podaci

Sa struktuiranim podacima se najčšće susrećemo u tabelarnom obliku.
Kolone tabele predstavljaju promenjive, dok redovi predstavjaju jedan slučaj iz skupa podatka.
**Slučaj** je reprezentacija jednog primerka entiteta koji je od interesa za modelovanje poput:

- Transakcija u banci
- Merenja izvesnih bioloških markera ispitanika
- Nekretnine

#### Tipovi promenjivih

Glavna podela tipova promenjivih je sledeća:
- Kategorijske (diskretne)
  - Nominalne (bez definisanog poretka)
  - Ordinalne (sa definisanim poretkom)
- Numeričke
  - Diskretne 
  - Kontinualne

#### Preprocesiranje podataka
##### Čišćenje podataka

Podaci iz realnog sveta su često nesavršeni i nepogodni za neposrednu primenu u mašinskom učenju.
Stoga je potrebno izvršiti čišćenje.
Najčešće čišćenje podrazumeva:

- Rešavanje nepostojećih vrednosti
- Otklanjanje suvišnih promenjivih

Nepostojeće vrednosti se mogu rešiti na 3 glavna načina:

1. Uklanjanje nepostojeće vrednosti
   1. Uklanjanje cele promenjive tj. kolone
   2. Uklanjanje samo tog reda
2. Zamena nepostojećih vrednosti
   1. Koristeći mod
   2. Koristeći medijanu
   3. Koristeći srednju vrednost
3. Nepostojanje vrednosti kao vrednost

##### Preprocesiranje podataka
###### Pretvaranje tipova
Postoje algoritmi mašinskog učenja koji zahtevaju da sve promenjive budu numeričke.

Nominalne promenjive se pretvaraju u numeričke tako što je primenjuje postupak koji se zove **one-hot encoding**.
Svaka moguća vrednost nominalne promenjive postaje jedna kolona, a jedinice se nalaze u onoj koloni koja reprezentuje originalnu vrednost nominalne promenjive.

| Color | Is_Red | Is_Green | Is_Blue |
| ----- | ------ | -------- | ------- |
| Red   | 1      | 0        | 0       |
| Green | 0      | 1        | 0       |
| Blue  | 0      | 0        | 1       |

Ordinalne promenjive nemaju problem sa pretvaranjem u numeričke jer postoji prirodan poredak.

| StressLevel | SLNumeric |
| ----------- | --------- |
| Low         | 1         |
| High        | 3         |
| Medium      | 2         |

###### Normalizacija

Dešava se da različite numeričke promenjive imaju značajno različite domene.
Uzmimo za primer GodišnjiPrihod i GodineStarosti.
Godišnji prihod može varirati od 0 do nekoliko miliona, dok GodineStarosti od 0 do 100 recimo.
Kada se ove dve promenjive prebace u neki vektorski prostor (što se veoma često dešava u algoritmima mašinskog učenja), GodišnjiPrihod bi znatno više uticao na distancu, uglove medju vektorima i sl. 
Da bismo izbegli ovaj problem pribegavamo procesu normalizacije.

Dva tipa normalizacije su:
- Linearna koja koristi linearnu interpolaciju od 0 do 1 gde 0 predstavlja minimalnu vrednost a 1 maksimalnu.
- z-score koja svaku vrednost predstavlja kao odstupanje od srednje vrednosti izraženo u standardnim devijacijama

Prednosti linearne normalizacije su:
1. Jednostavnost
2. Očuvanje relativnog odstojanja

Mane su:
1. Osetljivost na ekstremne vrednost

Prednosti z-score normalizacije su:
1. Otpornost na ekstremne vrednosti
2. Očuvanje oblika raspodele

Mane su:
1. Potencijalni gubitak interpretabilnosti
2. Pretpostavlja da je raspodela normalna

## Obucavanje 

Ukoliko smo uspešno pripremili podatke, možemo pristupiti procesu obuke.
Proces obuke se sastoji od:
1. Podele podataka
2. Definicija funkcije gubitka i validacione metrike
3. Faze obuke
4. Evaluacije modela

### Podela podataka

Prvo i osnovno je da podelimo podatke na trening i testni skup.
Obično trening skup sadrži 80% podataka a testni 20%.
Trening skup je skup podataka koji se koristi za optimizaciju parametara modela.
Testni skup služi za procenu kvaliteta modela na do sada neviđenom skupu podataka i radi se nakon faze obuke.
Validacioni skup podataka se koristi za praćenje kvaliteta modela tokom faze obuke na neviđenom skupu podataka.

### Definicija funkcije gubitka i validacione metrike

Da bismo mogli da obučavamo modele mašinskog učenja, potrebno je odrediti numeričku vrednost koja predstavlja kvalitet modela i nju zovemo **funkcija gubitka** ili **ciljna funkcija**. 
Ta vrednost će zavisiti od:

1. Skupa podataka
2. Hiperparametara modela
3. Parametara modela

Kako je skup podataka na kom se model obučava nepromenjiv, ostaje to da kvalitet modela zavisi od parametara i hiperparametara. 
Hiperparametri predstavljaju vrednosti koje se tiču procesa obuke ili same arhitekture modela i ne mogu se menjati kada otpočne faza obuke.

Primeri hiperparametara:
- Maksimalan broj epoha
- Veličina mini-batch-a
- Broj slojeva u neuronskoj mreži
- Stopa učenja
- Broj suseda koji se koristi u knn algoritmu

Kako su skup podataka i hiperparametri fiksni kada počne faza obuke, jasno je da se vrednost funkcije gubitka izražava kao funkcija parametara modela.
Cilj obuke jeste da se nađu optimalne vrednosti parametara modela.
Optimalne vrednosti parametara modela su one vrednosti za koje je funkcija gubitka optimalna.

Validaciona metrika takođe može biti ista kao funkcija gubitka, ali ne nužno.

### Faza obuke

Faza obuke predstavlja proces koji se odnosi na optimizaciju parametara modela.
U zavisnosti od modela, faza obuke može biti različita.
Kod proste linearne regresije, faza obuke predstavlja rešavanje sistema linearnih jednačina.
Kod nekih drugih modela se primenjuju optimizacione metode ciljne funkcije.

Obuka se vrši na trening skupu, a praćenje procesa obuke na validacionom skupu podataka.

### Evaluacija modela

Često nije poznato unapred koji hiperparametri mogu iznedriti najbolji model niti je moguće sa sigurnošću utvrditi.
Zato se često obučava nekoliko modela sa različitim hiperparametrima i biraju se oni koji imaju najbolje testne metrike.

Ukoliko model ima značajno bolje rezultate na trening skupu nego na testnom skupu, onda kažemo da model nedovoljno dobro generalizuje i kažemo da je model preučen (overfitted).

Ukoliko model nema dovoljno dobre performanse ni na trening ni na testnom skupu onda se kaže da je model nedoučen (underfitted).

## Metrike

Metrike su numericke vrednosti koje imaju ulogu da kvantifikuju valjanost modela.

### Regresione metrike

#### MSE (Mean Squared Error)

Srednja kvadratna greška. Predstavlja srednju vrednost sume kvadrata odstupanja predikcije od vrednosti ciljne promenjive. 
Manje vrednosti ukazuju na bolji model.
Može se koristiti kao ciljna funkcija za problem regresije.

#### RMSE (Root Mean Squared Error)

Koren srednje kvadratne greške. Isto važi kao i za MSE.

#### MAE (Mean Absolute Error)

Srednja apsolutna greška. Srednja vrednost apsolutnog odstupanja predikcije od vrednosti ciljne promenjive. 
Manje vrednosti ukazuju na bolji model.
Nije preporučljivo da se koristi kao ciljna funkcija za problem regresije jer nije diferencijablina na celom svom domenu. Može se koristiti kao validacona metrika.

### Klasifikacione metrike

#### Matrica konfuzije


Matrica konfuzije je alat koji se koristi u evaluaciji performansi klasifikacionih modela u mašinskom učenju

| Actual/Predicted | Positive | Negative |
| ---------------- | -------- | -------- |
| Positive         | TP       | FN       |
| Negative         | FP       | TN       |

-TP - True Positive - Broj tačnih pozitivnih predikcija
-FP - False Positive - Broj netacnih pozitivnih predikcija
-TN - True Negative - Broj netacnih negativnih predikcija
-FN - False Negative - Broj tačnih negativnih predikcija

#### Tačnost (Accuracy)

$$accuracy = \frac{TP + TN}{TP+TN+FP+FN}$$

Udeo tačnih predikcija u ukupnom broju predikcija.

#### Preciznost (Precision, Positive Predictive Value - PPV)

$$precision = \frac{TP}{TP+FP}$$

Udeo tačnih predikcija u ukupnom broju pozitivnih predikcija.

#### Senzitivnost (Sensitivity, Recall, True Positive Rate - TPR)

$$recall = \frac{TP}{TP+FN}$$

Udeo tačno prepoznatih stvarnih pozitivnih slučajeva.

#### Specifičnost (Specificity, True Negative Rate - TNR)

$$specificity = \frac{TN}{TN+FP}$$

Udeo tačno prepoznatih stvarnih negativnih slučajeva.

#### F1 Score

$$F1 = 2 \times\frac{precision \times recall}{precision + recall}$$

Harmonijska sredina preciznosti i senzitivnosti.

Za sve ove vrednosti važi što veće to bolje. Sve vrednosti su u rasponu od 0 do 1.

## Optimizacione metode

Onda kada imamo ciljnu funkciju takvu da je analitičko traženje optimuma ili previše računski skupo ili potpuno neizvodljivo, tada pribegavamo optimizacionim metodama za nalaženje optimuma.

### Gradijentni spust

Gradijentni spust je optimizaciona metoda koja koristi gradijent funkcije za njenu optimizaciju. 
Ideja je da trenutno stanje modela predstavimo kao tačku u n-dimenzionom prostoru (n je broj parametara modela, vrednost svakog parametra je jedna komponenta), a valjanost modela kao funkciju u nad tim prostorom. Gradijent predstavlja vektor u čijem pravcu i smeru funkcija najviše raste.
Trenutnu minimizaciju funkcije postižemo tako što pomerimo parametre modela u suprotnom smeru od gradijenta za neki realni umnožak $\eta ( 0 < \eta < 1)$.
Ponavljajuci ovaj postupak postepeno nalazimo parametre modela sa sve boljim performansama. 
Gradijentni spust ne garantuje nalazenje globalnog optimuma.
Gradijent je n-dimenzioni vektor čija n-ta komponenta predstavlja parcijalni izvod n-tog parametra u odnosu na ciljnu funkciju.

Postoje tri varijante gradijentnog spusta:

1. Paketni gradijentni spust (Mini-Batch Gradient Descent - MBGD)
2. Gradijentni spust (Gradient Descent - GD)
3. Stohastički gradijentni sipust (Stochastic Gradient Descent - SGD)

MBGD vrši optimizaciju na podskupu skupa podataka koji zovemo paket i najčešće je broj slučajeva u paketu stepen dvojke. Gradijent se računa na datom podskupu. Najčešće je najbolji od sva tri jer dovoljan broj puta u jednoj epohi primeni gradijente na model, a svaki gradijent liči dovoljno na gradijent na celom skupu zbog [centralne granične teoreme](https://en.wikipedia.org/wiki/Central_limit_theorem).

GD vrši optimizaciju na celom skupu podataka, tj. veličina paketa je ista kao veličina trening skupa. Dobar je na skupovima podataka koji su dovoljno mali. Sporije napreduje od MBGD.

SGD vrši optimizaciju na jednom slučaju, tj. veličina paketa je 1.
Nije dobar jer je moguće da različiti slučajevi proizvedu gradijente koji se na kraju potru pa stoga učenje teče najsporije.

#### Early stopping

Kada se koriste optimizacione metode za obučavanje modela mašinskog učenja, ne zna se tačno posle koliko vremena će proces obuke stagnirati i ući u lokalni minimum. 
Kako bismo izbegli nepotrebno trošenje resursa poput energije i procesorskih ciklusa, primenjuje se tehnika zvana rano zaustavljanje.
Definišu se tolerancija i strpljenje. Tolerancija predstavlja minimalno poboljšanje validacione metrike koje se smatra značajnim. Strpljenje predstavlja broj epoha koji mora da prodje bez značajnog poboljšanja
da bi se proces obuke završio ranije nego predviđeno.

