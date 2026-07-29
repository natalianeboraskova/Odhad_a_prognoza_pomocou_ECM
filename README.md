# Odhad a prognóza pomocou ECM

Pre krajinu Taliansko sme si stiahli údaje potrebné na odhad vybraného modelu s korekčným členom (ECM). Otestovali sme stacionaritu a určili rád integrovanosti všetkých použitých premenných. Následne sme odhadli kointegračnú rovnicu a otestovali kointegráciu medzi premennými. Po jej potvrdení sme vytvorili model ECM. Na základe testovania autokorelácie v ECM sme sa pokúsili odstrániť všetky nedokonalosti modelu, prezentovali sme finálny odhad a s ním sme odhadli prognózu
 
Projekt obsahuje: 
* Teoretický popis modelu - vysvetlenie, popis a presné zdroje získania použitých údajov a ich dátový prehľad - vizualizácia, popis výkyvov a charakteristiky 
* Odhadnutý predpokladaný vstupný model so všetkými testami - vysvetlenie postupu analýzy a zásahov do modelu a finálny model s jeho odhadom a testami 
* Pomocou finálneho modelu ECM vypočítaná prognóza na najbližšie 4 obdobia
* V README sa nenachádzajú  číselné výstupy testov vzhľadom na ich zabratie priestoru, tie sa budú nachádzať v súbore:
  [Pozrieť QMD súbor](./ekonometriaprojekt.qmd)
[Otvoriť HTML - treba ho stiahnuť](https://htmlpreview.github.io/?https://github.com/natalianeboraskova/Odhad_a_prognoza_pomocou_ECM/blob/main/ekonometriaprojekt.html)

# Vzťah medzi HDP a tvorbou investícií - Taliansko
Model s korekčným členom (Error Correction Model, ECM) je ekonometrický nástroj využívaný na modelovanie dlhodobých rovnovážnych vzťahov medzi nestacionárnymi časovými radmi a krátkodobej dynamiky odchýlok od tejto rovnováhy. Teoretickým základom ECM je koncept kointegrácie, ktorý zaviedli Engle a Granger (1987).

V našej analýze predpokladáme, že hrubá tvorba fixného kapitálu (investície) v Taliansku závisí od celkovej ekonomickej aktivity vyjadrenej prostredníctvom hrubého domáceho produktu (HDP):
$$
INVEST=f(HDP)
$$

**HDP** – predstavuje celkový výstup ekonomiky a hlavný determinant investičnej aktivity. Podľa teórie akcelerátora rastúci domáci produkt vyvoláva potrebu rozširovania výrobných kapacít, čo vedie k dodatočným investíciám. Zároveň vyššie HDP indikuje lepšiu ekonomickú kondíciu krajiny a vyššiu kúpyschopnosť, čo motivuje firmy k rozvoju → **očakávam kladný vzťah.**

**Investície (Hrubá tvorba fixného kapitálu)** -Investície v modeli vystupujú ako závislá premenná, ktorú sa snažíme vysvetliť pomocou vývoja HDP. V kontexte talianskej ekonomiky investície predstavujú kľúčový motor rastu, avšak sú vysoko senzitívne na hospodárske šoky a zmeny v očakávaniach podnikateľov. Očakávame, že investície budú vykazovať vyššiu volatilitu než samotné HDP, čo je typický znak tejto zložky agregátneho dopytu. Dlhodobý vzťah s HDP predpokladáme na základe teórie, že stabilný rast ekonomiky spätne stimuluje investičné výdavky do technológií a infraštruktúry.

**Mechanizmus korekcie chýb (ECT)** – v modeli ECM predstavuje kľúčový prvok, ktorý prepája krátkodobé zmeny s dlhodobou rovnováhou. Ak v jednom období investície vzrastú nad úroveň zodpovedajúcu dlhodobému vzťahu s HDP, v nasledujúcom období ich mechanizmus korekcie chýb "stiahne" späť k rovnováhe. Koeficient pri tomto člene musí byť záporný a štatisticky významný, čo potvrdzuje schopnosť ekonomiky eliminovať nerovnováhy.

## Údaje – popis a zdroje

Analýza využíva štvrťročné dáta pre Taliansko za obdobie **2005Q1 – 2025Q4**. Všetky premenné boli získané z databázy Eurostatu. Dáta boli stiahnuté vo formáte .xlsx a následne importované do prostredia R pre ďalšie spracovanie. Obdobie bolo zvolené tak, aby zachytilo dlhodobý vývoj investičnej aktivity vrátane významných hospodárskych cyklov.

|  |  |  |
|------------------------|------------------------|------------------------|
| **Premenná** | **Popis** | **Zdroj** |
| **HDP** | Hrubý domáci produkt (reálny index, sezónne očistený) | Eurostat |
| **invest** | Hrubá tvorba fixného kapitálu (reálny index, sezónne očistená) | Eurostat |

Dáta reprezentujú objemové indexy, čo umožňuje eliminovať vplyv inflácie a sledovať reálny rast oboch ukazovateľov. Pred samotnou analýzou boli premenné transformované na objekty časových radov (`ts`) s kvartálnou frekvenciou.


## Dátový prehľad
<img width="826" height="510" alt="image" src="https://github.com/user-attachments/assets/27a10e8f-104b-4c50-9f54-070a9e578c80" />

**HDP (hdp)** vykazuje počas sledovaného obdobia rastúci trend, ktorý bol však prerušený dvoma kľúčovými udalosťami. Prvým je globálna finančná kríza (2008–2009) a následná dlhová kríza v eurozóne, kedy talianska ekonomika zaznamenala stagnáciu a mierny pokles. Druhým, oveľa výraznejším výkyvom, je rok 2020 a pandémia COVID-19, kedy došlo k bezprecedentnému prepadu aktivity, po ktorom však nasledovalo relatívne rýchle oživenie.

**Investície (invest)** sa vyvíjajú v úzkej korelácii s HDP, avšak s oveľa vyššou mierou volatility, čo potvrdzuje ekonomickú teóriu o citlivosti investičnej aktivity na šoky. Najväčší prepad je badateľný počas roku 2020. Na rozdiel od HDP, investície vykazujú ku koncu sledovaného obdobia (2024-2025) výraznejšie zrýchlenie, čo môže odrážať implementáciu národného plánu obnovy a odolnosti v Taliansku.

## Analýza stacionarity – ADF testy

### HDP

<img width="773" height="581" alt="image" src="https://github.com/user-attachments/assets/c6573921-eb35-4c4b-ba5c-b6c68379ac8d" />
<img width="774" height="515" alt="image" src="https://github.com/user-attachments/assets/f48737fb-2ef5-4a5b-a30d-5a7998335538" />

Začíname modelom s trendom v tvare:

$$
 \Delta HDP_t = \alpha_0 + \alpha_1 t + \delta HDP_{t-1} + u_i
$$

Výsledkom je model, v ktorom $\phi_3 = 2.521$ je menšie ako $\phi_{3crit} = 6.49$. Nulovú hypotézu $H_0: \delta = \alpha_1 = 0$ nemôžeme na 5 % hladine významnosti zamietnuť a model s trendom nie je vhodný na testovanie.

Pokračujeme modelom s driftom (bez členov riešiacich autokoreláciu podľa BIC) v tvare:

$$
\Delta HDP_t = \alpha_0 + \delta HDP\_{t-1} + u_i
$$

Výsledkom je model, v ktorom $\phi_1 = 1.8358$ je menšie ako $\phi_{1crit} = 4.71$. Nulovú hypotézu $H_0: \delta = \alpha_0 = 0$ nemôžeme na 5 % hladine významnosti zamietnuť a model s driftom nie je vhodný na testovanie.

Keďže ani model s trendom, ani model s driftom neboli vhodné, prechádzame k najjednoduchšiemu modelu bez deterministických zložiek:

$$
\Delta HDP_t = \delta HDP\_{t-1} + u_i\
$$

Hodnota testovacej štatistiky $\tau_1 = 0.389$ nie je menšia ako kritická hodnota $\tau_{crit} = -1.95$. Prijímame nulovú hypotézu $H_0: \delta = 0$, čo znamená, že časový rad HDP je **nestacionárny** a správa sa ako náhodná prechádzka. Na zistenie rádu integrácie pokračujeme testovaním prvých diferencií.

Model prvej diferencie s trendom má tvar:

$$
\Delta^2 HDP_t = \alpha_0 + \alpha_1 t + \delta \Delta HDP\_{t-1} + u_i
$$

Z výsledku vyplýva, že $\phi_3 = 49.138$ je výrazne väčšie ako $\phi_{3crit} = 6.49$. Model s trendom je teda vhodný. Hodnota testovacej štatistiky $\tau_3 = -9.9131$ je menšia ako kritická hodnota $\tau_{crit} = -3.45$, preto zamietame nulovú hypotézu o nestacionarite.

**Záver:** Časový rad prvých diferencií HDP je stacionárny. Premenná HDP je teda integrovaná rádu jeden, $HDP \sim I(1)$.

### Investície (invest)
<img width="688" height="467" alt="image" src="https://github.com/user-attachments/assets/694c3077-a0f2-4e39-b6e7-00418b6f386d" />
<img width="669" height="403" alt="image" src="https://github.com/user-attachments/assets/1fa29e60-6324-44ce-a458-314595faf473" />

Na zistenie rádu integrácie premennej investícií použijeme rozšírený Dickeyho-Fullerov test (ADF). Postupujeme od najkomplexnejšieho modelu k najjednoduchšiemu.

Začíname modelom s trendom v tvare:

$$
\Delta INVEST_t = \alpha_0 + \alpha_1 t + \delta INVEST\_{t-1} + u_i\
$$

Výsledkom je model, v ktorom $\phi_3 = 1.9273$ je výrazne menšie ako kritická hodnota $\phi_{3crit} = 6.49$. Nulovú hypotézu $H_0: \delta = \alpha_1 = 0$ nemôžeme na 5 % hladine významnosti zamietnuť a model s trendom nie je vhodný na testovanie.

Pokračujeme modelom s driftom v tvare:

$$
\Delta INVEST_t = \alpha_0 + \delta INVEST\_{t-1} + u_i
$$

Výsledkom je model, v ktorom $\phi_1 = 0.2588$ je menšie ako $\phi_{1crit} = 4.71$. Nulovú hypotézu $H_0: \delta = \alpha_0 = 0$ nemôžeme na 5 % hladine významnosti zamietnuť a model s driftom nie je vhodný na testovanie stacionarity.

Prechádzame k modelu bez deterministických zložiek (none):

$$
\Delta INVEST_t = \delta INVEST\_{t-1} + u_i
$$

Hodnota testovacej štatistiky $\tau_1 = 0.4709$ nie je menšia ako kritická hodnota $\tau_{crit} = -1.95$. Prijímame nulovú hypotézu $H_0: \delta = 0$, čo znamená, že časový rad investícií je **nestacionárny** (obsahuje jednotkový koreň). Na určenie rádu integrácie testujeme prvú diferenciu radu.

Model prvej diferencie s trendom má tvar:

$$
\Delta^2 INVEST_t = \alpha_0 + \alpha_1 t + \delta \Delta INVEST_{t-1} + u_i
$$

Z výsledku vyplýva, že $\phi_3 = 44.2995$ je väčšie ako $\phi_{3crit} = 6.49$. Model s trendom je teda vhodný. Hodnota testovacej štatistiky $\tau_3 = -9.4119$ je výrazne menšia ako kritická hodnota $\tau_{crit} = -3.45$, čo nám umožňuje zamietnuť nulovú hypotézu o nestacionarite.

**Záver:** Časový rad prvých diferencií investícií je stacionárny. Preto konštatujeme, že premenná investícií je integrovaná rádu jeden, teda $invest \sim I(1)$.

Podmienky na tvorbu kointegračnej funkcie sú splnené.

## Kointegračná analýza a kointegračný model

Druhým krokom je odhad dlhodobého vzťahu medzi premennými metódou najmenších štvorcov (OLS). Táto rovnica predstavuje predpokladaný dlhodobý rovnovážny vzťah, ku ktorému premenné v čase smerujú.

#### Odhad dlhodobej rovnice

Odhadujeme vzťah, kde HDP závisí od investícií:

$$
HDP_t = \beta_0 + \beta_1 INVEST_t + \epsilon_t
$$

Z výsledkov OLS odhadu vyplýva, že investície majú štatisticky významný vplyv na HDP. Koeficient pri investíciách je $0.2327$, čo interpretujeme tak, že pri raste investícií o jednu jednotku (indexu) vzrastie HDP dlhodobo o $0.23$ jednotky. Model vysvetľuje približne $79\ \%$ variability HDP ($R^2 = 0.7913$).

#### Skúmanie stacionarity rezíduí (Engle-Grangerov test)

Aby sme overili, či ide o skutočnú kointegráciu a nie o zdanlivú regresiu, musíme otestovať stacionaritu rezíduí $\epsilon_t$ z tohto modelu pomocou ADF testu bez konštanty a trendu (typ "none").

Výsledná hodnota testovacej štatistiky je $\tau_1 = -3.5016$. Pri kointegrácii musíme použiť špeciálne kritické hodnoty podľa Engla a Grangera. Pre model s jednou vysvetľujúcou premennou je kritická hodnota na $5\ \%$ hladine významnosti približne $-3.39$ (prípadne $-3.37$ podľa MacKinnona).

**Záver:** Keďže vypočítaná hodnota $-3.5016$ je v absolútnej hodnote väčšia ako kritická hodnota ($|-3.50| > |-3.39|$), nulovú hypotézu o nestacionarite rezíduí zamietame. Rezíduá sú stacionárne, čo potvrdzuje existenciu **kointegračného vzťahu** (dlhodobej rovnováhy) medzi HDP a investíciami.

## Model korekcie chýb (ECM)
<img width="688" height="356" alt="image" src="https://github.com/user-attachments/assets/824ec4e6-1ebc-407a-a732-562a3d373004" />
                                                         
Keďže sme v predchádzajúcom kroku potvrdili existenciu kointegrácie, môžeme pristúpiť k odhadu modelu korekcie chýb. Tento model nám umožňuje analyzovať krátkodobú dynamiku HDP a zároveň sledovať, akou rýchlosťou sa systém vracia k dlhodobej rovnováhe po nečakanom šoku.

### Špecifikácia modelu

Odhadujeme rovnicu v prvých diferenciách, kde vystupuje oneskorený korekčný člen ($e_{t-1}$):

$$
\Delta HDP_t = \alpha + \beta_1 \Delta INVEST_t + \lambda e\_{t-1} + u_t
$$

### Interpretácia výsledkov

Na základe výstupu z programu R môžeme zapísať odhadnutú rovnicu (konštantu vynechávame pre jej nevýznamnosť):

$$
\Delta HDP_t = 0.4269 \cdot \Delta INVEST_t - 0.1387 \cdot e\_{t-1}
$$

1.  **Krátkodobá dynamika (**$\beta_1$): Koeficient pri zmene investícií je $0.4269$ a je vysoko štatisticky významný. Znamená to, že ak v danom štvrťroku vzrastú investície o jednu jednotku, HDP v tom istom období vzrastie v priemere o $0.43$ jednotky.

2.  **Rýchlosť návratu do rovnováhy (**$\lambda$): Koeficient pri korekčnom člene ($e_{t-1}$) je $-0.1387$ a je štatisticky významný na $5\ \%$ hladine významnosti (p-hodnota $0.0189$).

    -   Záporné znamienko je kľúčové – potvrdzuje, že model je stabilný a vracia sa k rovnováhe.

    -   Hodnota hovorí, že približne $13.87\ \%$ z nerovnováhy z predchádzajúceho obdobia sa skorigovalo v aktuálnom štvrťroku.

3.  **Kvalita modelu:** Model vysvetľuje približne $75.93\ \%$ variability zmien HDP, čo je na model v diferenciách veľmi vysoká hodnota. F-štatistika ($p < 2.2 \times 10^{-16}$) potvrdzuje celkovú významnosť modelu.

### Diagnostika: Test autokorelácie

Pre overenie spoľahlivosti sme použili Ljung-Boxov test na rezíduá modelu ECM. Pri pohľade na p-hodnoty (v tvojich výsledkoch sú všetky výrazne nad $0.05$) nezamietame nulovú hypotézu $H_0$. Rezíduá nevykazujú autokoreláciu, čo znamená, že model je korektne špecifikovaný a získané koeficienty sú nestranné.

## Prognóza vývoja HDP na rok 2026

Posledným krokom analýzy je vytvorenie krátkodobej predpovede (prognózy) vývoja HDP na nasledujúce štyri štvrťroky (rok 2026). Keďže pre exogénnu premennú (investície) nemáme k dispozícii externé predpovede, v modeli predpokladáme, že investície porastú svojím priemerným historickým tempom.

<img width="648" height="377" alt="image" src="https://github.com/user-attachments/assets/aa0d4ec4-0e31-4cc4-a029-dd33401d54df" />

### Metodika výpočtu

Prognóza je realizovaná dynamicky (rekurzívne). V každom kroku model vypočíta očakávanú zmenu HDP ($\Delta HDP$) na základe:

1.  Priemerného prírastku investícií.

2.  Korekčného člena ($e_{t-1}$), ktorý v každom kroku znižuje odchýlku od dlhodobej rovnováhy.

Následne sú tieto zmeny (diferencie) pripočítané k poslednej známej hodnote indexu HDP z konca roka 2025.

### Výsledky prognózy

V tabuľke nižšie sú uvedené predpovedané hodnoty indexu HDP pre jednotlivé štvrťroky roka 2026:

|                              |                                              |
|-----------------------------|-------------------------------------------|
| **Obdobie (rok a štvrťrok)** | **Predpovedaná hodnota indexu (2015 = 100)** |
| 2026 Q1                      | 112.46                                       |
| 2026 Q2                      | 112.46                                       |
| 2026 Q3                      | 112.48                                       |
| 2026 Q4                      | 112.50                                       |

### Interpretácia a vizualizácia

Model predikuje stabilný vývoj s jemne rastúcim trendom v druhej polovici roka. Hodnota **112.50** v poslednom štvrťroku 2026 naznačuje, že HDP Nórska bude o $12.50\ \%$ vyššie v porovnaní so základným rokom 2015.



















