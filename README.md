# eMAG AI Review Analyzer (RPA & ReFramework)

Acest proiect este un robot software (RPA) dezvoltat în **UiPath** folosind **Robotic Enterprise Framework (ReFramework)**. Robotul extrage recenzii ale produselor de pe eMAG și utilizează Inteligența Artificială pentru a traduce recenziile în engleză, a extrage sentimentul (Pozitiv/Negativ) și a genera un rezumat, transmis ulterior pe email.

## Funcționalități Principale

- **Web Scraping:** Navighează automat pe eMAG și extrage recenziile produselor (ex: electrocasnice mici, accesorii IT).
- **Procesare Tranzacțională (ReFramework):** Folosește `TransactionItem` (Queue) pentru a procesa fiecare recenzie în parte, oferind stabilitate și mecanisme de *Retry* în caz de eroare.
- **Integrare AI (API):** Trimite textul în română către un model AI printr-un `HTTP Request` pentru traducere în limba engleză și analiză de sentiment.
- **Raportare:** Generează un raport structurat cu rezultatele analizei.
- **Confirmare mail:** Trimite raportul creat pe email ca o confirmare.

## Arhitectura Proiectului (Dispecer / Performer)

Pentru a asigura scalabilitatea, proiectul este împărțit în **doi roboți independenți**:

### 1. Dispatcher (Dispecerul)
Acest robot este responsabil de preluarea datelor brute.
- Citește produsele dintr-un fișier Excel.
- Încarcă fiecare produs în UiPath Orchestrator sub formă de elemente într-o coadă (Queue Items), pregătindu-l pentru procesare.

### 2. Performer (Procesatorul AI)
Acest robot preia datele din coadă. Este construit pe arhitectura **UiPath ReFramework**, fiind structurat în 4 etape principale:
1. **Initialization:** Citirea setărilor din `Config.xlsx` / Orchestrator și inițializarea aplicațiilor (ex: deschiderea/crearea fișierului Excel pentru raportul final).
2. **Get Transaction Data:** Preluarea următoarei recenzii de procesat (Transaction Item) direct din coada Orchestratorului.
3. **Process Transaction:** Munca efectivă pentru fiecare recenzie:
   - Extragerea textului recenziei din item-ul curent.
   - Apelul API (HTTP Request) către modelul de Inteligență Artificială.
   - Preluarea și structurarea răspunsului AI (Traducere + Sentiment + Rezumat).
4. **End Process:** Închiderea curată a aplicațiilor, salvarea raportului final structurat și trimiterea acestuia pe email, urmate de oprirea robotului.

## Tehnologii Utilizate
- **UiPath Studio** (ReFramework, Data Scraping, UI Automation)
- **API (REST)** pentru integrarea modelului AI (HTTP Requests, parsare JSON)
- **Excel** (pentru input de produse și output raport)

1. Clonează acest repository:
   ```bash
   git clone [https://github.com/numele-tau/numele-repo-ului.git](https://github.com/numele-tau/numele-repo-ului.git)
