Cilj ovog projektnog zadatka je omgućiti Yettel korisnicima da kupe vinjete/putarine direktno preko Yettel aplikacije ili USSD koda koristeci njihov MSISDN.
API endpointi: 
  /putarine/plati: Kupovina/plaćanje putarine.
Ulazni podaci:
Registarski broj, ID deonice/puta, MSISDN(Yettelov)
Izlazni podaci:
Status transakcije (Uspešno/Neuspešno), ID transakcije, Cena transakcije, Dodatne informacije 
  /vinjete/kupi: Kupovina vinjete.
Ulazni podaci:
Registarski broj, tip vinjete (dnevna, sedmična, mesečna, godišnja), MSISDN(Yettelov)
Izlazni podaci:
Status transakcije (Uspešno/Neuspešno), ID transakcije, Cena vinjete/karte

  /vinjete/proveri: Proverava da li vozilo već ima važeću vinjetu.
Ulazni podaci: Registarski broj
Izlaz:
Status (Važeća/Istekla/Nema vinjete), Datum isteka (ako je važeća)

Planirani resursi:

Backend: 2-3 developera
Frontend: 1 developer (uglavnom za implementaciju ili za blage izmene u postojećoj Yettel aplikaciji)
QA Inženjer: 1 inženjer za testiranje
Project Manager: 1 PM za koordinaciju

Vremenski rok:
Planiranje i dizajn: 2 nedelje
Razvoj: 6 nedelja
Testiranje: 2 nedelje
Ukupno vreme: 10 nedelja


Šta bih koristion na Backendu: Node.js.
Koju bih tehnologiju izabrao za rad sa bazom: MySQL 

PSEUOD API IMPLEMENTIRAN U NODE,JS:

Funkcija proveriVinjete(registracija):
    vinjeta = pronadjiVinjetaU(bazi, registracija)

    Ako vinjeta postoji:
        sadašnjiDatum = trenutniDatum()
        datumIsteka = konvertujUDatum(vinjeta.datumIsteka)

        Ako datumIsteka > sadašnjiDatum:
            Vrati { "status": "Važeća", "datumIsteka": vinjeta.datumIsteka }
        Inače:
            Vrati { "status": "Istekla", "datumIsteka": vinjeta.datumIsteka }
    Inače:
        Vrati { "status": "Nema vinjete" }



Funkcija kupiVinjetu(registracija, tipVinjete, yettelBroj):
    cena = nadjiCenuZa(tipVinjete)

    Ako cena nije pronađena:
        Vrati { "status": "Neuspešno", "poruka": "Pogrešan tip vinjete" }

    idTransakcije = generišiIDTransakcije()

    datumIsteka = trenutniDatum()
    Ako tipVinjete je "dnevna":
        datumIsteka = dodajDane(datumIsteka, 1)
    Ako tipVinjete je "sedmična":
        datumIsteka = dodajDane(datumIsteka, 7)
    Ako tipVinjete je "mesečna":
        datumIsteka = dodajMesece(datumIsteka, 1)
    Ako tipVinjete je "godišnja":
        datumIsteka = dodajGodine(datumIsteka, 1)

    sacuvajVinjetuU(bazi, registracija, datumIsteka, idTransakcije)

    Vrati { "status": "Uspešno", "idTransakcije": idTransakcije, "cena": cena }



Funkcija platiPutarina(registracija, idDeonice, yettelBroj):
    cena = nadjiCenuZaDeonicu(idDeonice)

    Ako cena nije pronađena:
        Vrati { "status": "Neuspešno", "poruka": "Pogrešan ID deonice" }

    idTransakcije = generišiIDTransakcije()

    sacuvajTransakcijuU(bazi, registracija, idDeonice, yettelBroj, cena, idTransakcije)

    Vrati { "status": "Uspešno", "idTransakcije": idTransakcije, "cena": cena }
