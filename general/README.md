# Splošna dokumentacija za celoten projekt

## Naslov projekta: Primerjalnik cen izdelkov

## Člani skupine:
- Erazem Pušnik (**TODO email**)
- Rok Miklavčič (**TODO email**)
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
