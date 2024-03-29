# Splošna dokumentacija za celoten projekt

## Naslov projekta: Primerjalnik cen izdelkov

## Člani skupine:
- Erazem Pušnik (ep7525@student.uni-lj.si)
- Rok Miklavčič (rm9039@student.uni-lj.si)
- Aljaž Šmaljcelj (as9721@student.uni-lj.si)

## Opis projekta

Projekt bo predstavljal primerjalnik cen izdelkov v različnih trgovinah.
Uporabnik bo lahko za določen izdelek primerjal njegovo ceno v vseh trgovinah.
Če se uporabnik prijavi v aplikacijo, si lahko izbere priljubljene izdelke in vidi tudi zgodovino cene posameznega izdelka v vsaki trgovini.

Prijavljeni uporabnik z vlogo *Administrator* lahko ureja, kateri izdelki so prikazani v aplikaciji in ročno proži proces pridobivanja cen izdelkov.
Hkrati lahko tudi upravlja z priljubljeni izdelki uporabnika.

## Uporabljeno ogrodje in razvojno okolje

Pri razvoju projekta bomo uporabili razvojno okolje [IntelliJ IDEA](https://www.jetbrains.com/idea/), za katerega bomo uporabili študentsko licenco za *Ultimate* verzijo.
Uporabili bomo tudi [WebStorm](https://www.jetbrains.com/webstorm/) za razvoj čelnega dela aplikacije ter [DataGrip](https://www.jetbrains.com/datagrip/) za pregled podatkovne baze.

Uporabljena ogrodja:
- čelni del (*frontend*): [Angular](https://angular.io)
- zaledni del (*backend*): [Spring Boot](https://spring.io/projects/spring-boot)
- trajna hramba podatkov: [MySQL](https://www.mysql.com)

## Shema arhitekture in interakcij med mikrostoritvami

![Shema arhitekture](/general/src/shema_arhitekture.png)

## Podatkovni model 

![Podatkovni_model](/general/src/podatkovni_model.png)

## Seznam funkcionlnosti posamezne mikrostoritve

- Ažuriranje podatkov: vmesni člen med čelnim delom in zalednim delom aplikacije. 
Njegova naloga je ustrezno klicati preostale mikrostoritve in izvajati ustrezne poizvedbe na podatkovno bazo
- Avtentikacija: upravljanje s registracijo in prijavo uporabnika v sistem
- Izračun cene izdelkov: izračun cene skupine izbranih izdelkov (ob podanem seznamu izdelkov izračuna skupno ceno, če bi vse izdelke kupili v eni trgovini)
- Pridobivanje cen izdelkov: priskrbitev cen vseh izdelkov v aplikaciji v vseh trgovinah.
  Zbrane rezultate nato zapiše v bazo za namen prikaza na čelnem delu aplikacije
- Priljubljeni izdelki: urejanje priljubljenih izdelkov določenega uporabnika. 
Na zahtevo pripravi tudi zgodovino cene posameznega izdelka v vseh trgovinah
- Administracija: upravljanje z uporabnikimi računi, ročno proženje pridobivanje cene izdelkov, upravljanje z priljubljenimi izdelki in izdelki, ki so prikazani v aplikaciji 

## Kompleksnejši zahtevek z najmanj tremi mikrostoritvami

Uporabnik zahteva izračun cene košarice, sestavljene iz njegovih priljubljenih izdelkov.
Kliče se mikrostoritev za serviranje podatkov, ki pokliče mikrostoritev za priljubljene izdelke in izračun cene.

## Okviren seznam primerov uporabe aplikacije

V času načrtovanja aplikacije smo identificirali naslednje primere uporabe (*use-case*):

- neavtenticirani uporabnik vidi v tabeli prikazane vse izdelke, ki so v primerjalniku v obliki tabele.
  S klikom na vrstico v tabeli se uporabniku odprejo podrobnosti o izdelku, kjer je prikazana cena izdelka v različnih trgovinah,
- neavtenticirani uporabnik lahko dodatno filtrira nad tabelo z izdelki.
  Lahko direktno uporabi filter nad vsemi stolpci (*full-text search*) ali pa s klikom na tri pikice uporabi filter le nad določenim stolpcem,
- v drugem zavihku "Sestavi košarico" neavtencirani uporabnik lahko izbere izdelke, ki jih želi dodati v svojo nakupovalno košarico.
  Sistem nato izračuna ceno izbranih izdelkov v različnih trgovinah.
- v tretjem zavihku "Priljubljeni izdelki" so prikazani izdelki, ki jih je avtenticirani uporabnik že označil kot priljubljene.
  Ti izdelki so prikazani v obliki tabele in klik na določen izdelek prikaže zgodovino cen specifičnega izdelka v različnih trgovinah.
- v četrtem zavihku "Administracija" avtenticirani uporabnik z vlogo *Administrator* upravlja z izdelki, ki so vnešeni v primerjalnik.
  Lahko doda nov izdelek, ročno proži proces pridobitve cen vseh izdelkov, dodeljuje priljubljene izdelke uporabnikom itd.

## Razvoj projekta

Za lokalni razvoj projekta je potrebno namestiti [MySQL](https://dev.mysql.com/downloads/installer/).
Ob namestitvi je potrebno namestiti tudi `MySQL Workbench` program.

Znotraj `MySQL Workbench` programa dodamo novo MySql konekcijo s klikom na gumb `+`.
- Za lokalni razvoj je pomembno nastaviti port na `3306`, saj je v vseh podprojektih naveden ta port.
- Kot admin userja definiramo `admin/123456`

### Azure

V primeru, da želimo preizkusiti delovanje naše aplikacije na produkcijskem okolju, se prijavimo na Azure portal (credentialsi na voljo na Discordu).
Za bazo potrebujemo `primerjalnik-cen-server`, za deploy pa `primerjalnikCenCluster`.

#### Podatkovna baza

- izberemo storitev `primerjalnik-cen-server`
- kliknemo na gumb `Start`, označen na sliki

![zagon_baze](/general/src/DB.png)

- počakamo, da se vzpostavi podatkovna baza
- baza je dostopna na produkcijkem JDBC URL-ju, ki ga pridobimo:
  - na desni strani izberemo `Connect`
  - razširimo meni `Connect from your app`
  - skopiramo url v `application.properties` na ustrezno mesto
  - namesto placeholderja `{your_database}` uporabimo `primerjalnik-cen`

**Po končanem pregledu ne pozabi ugasniti baze, saj se delovanje računa na minuto, da ne izgubimo vseh kreditov!!! (gumb `Stop` poleg gumba `Start`)**

#### Cluster

- izberemo storitev `primerjalnik-cen-cluster`
- kliknemo gumb `Start` 
- počakamo, da se cluster zažene
- če želimo pregledati, ali pod teče:
  - kliknemo `Workloads` na desni strani
  - izberemo ustrezen workload (poimenovan po nazivu mikrostoritve)
  - pregledamo status tega workload-a (mora biti zelen - `Running`)
- če se želimo povezati na ta pod:
  - odpremo terminal v Azure-u

![azure_CLI](/general/src/CLI.png)

  - počakamo, da se terminal vzpostavi
  - vnesemo ukaz `kubectl get service {service-name} --watch`, kjer `{service-name}` zamenjamo z imenom mikrostoritve (tako je poimenovan nas pod)
  - počakamo, da se v stolpcu `EXTERNAL-IP` pojavi IP naslov
  - v Browser vnesemo pridobljen IP, zraven dodamo še `:{PORT}`, kjer `{PORT}` nadomestimo s številko porta v stolpcu poleg eksternega API-ja
