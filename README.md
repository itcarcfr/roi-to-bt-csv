# Convertor ROI → CSV Banca Transilvania

Aplicație web simplă, fără server, care transformă un fișier `.ROI` (format de ordine de plată) într-un fișier CSV în formatul oficial acceptat de Banca Transilvania pentru încărcarea plăților multiple.

## Cum se folosește

1. Deschide pagina (link GitHub Pages, mai jos).
2. Trage fișierul `.ROI` în zona marcată, sau apasă și alege-l.
3. Verifică rândurile generate în tabel:
   - rândurile marcate **roșu** au un cont sursă la altă bancă decât BT — de regulă nu trebuie incluse în acest fișier;
   - rândurile marcate **portocaliu** au un cod de bancă beneficiar necunoscut — completează manual codul BIC în câmpul care apare sub tabel (se ține minte în acest browser pentru viitor).
   - orice celulă din tabel poate fi corectată manual (click direct pe ea).
4. Apasă **Descarcă CSV**.
5. Încarcă fișierul CSV descărcat direct în BT Go / Internet Banking BT.

## Important

- **Nimic nu este trimis pe internet.** Tot procesul (citire fișier, transformare, generare CSV) se face local, în browserul tău. Fișierul CSV se descarcă direct pe calculator.
- Aplicația **nu** se conectează la Banca Transilvania și nu poate încărca automat plățile — încărcarea finală în BT se face manual, de tine.
- **Suma (Amount)** este calculată direct din cifrele din fișierul ROI, presupunând RON întregi (fără bani/subunități), de exemplu `7130` → `7130.00`. Dacă vreo sumă din ROI conține de fapt bani (ex. ultimele două cifre sunt subunități), corectează manual valoarea din tabel înainte de a descărca CSV-ul.
- Fișierul CSV generat respectă formatul oficial cu 11 coloane: `OrderNumber, SourceAccountNumber, TargetAccountNumber, BeneficiaryName, BeneficiaryBankBIC, BeneficiaryFiscalCode, Amount, PaymentRef1, PaymentRef2, ValueDate, Urgent`.
- Virgula (`,`) și punct-și-virgula (`;`) sunt eliminate automat din câmpurile text (nume beneficiar, detalii plată), pentru că parserul de import al BT nu suportă aceste caractere în interiorul unui câmp (nu suportă câmpuri CSV între ghilimele).

## Dezvoltare locală

Fișierul `index.html` e complet independent (HTML + CSS + JS inline, fără build). Poate fi deschis direct în browser sau servit cu orice server static, de exemplu scriptul `serve.ps1` inclus (PowerShell, pornește un server pe `http://localhost:8085/`).

## Extinderea listei de bănci (BIC)

Codurile de bancă din ROI (ex. `BTRL`, `TREZ`, `RNCB`) sunt mapate la BIC complet într-un tabel din `index.html` (`DEFAULT_BIC_MAP`). Când apare un cod necunoscut, poate fi completat direct din interfață — se salvează automat în browser. Pentru a-l face permanent pentru toată lumea, adaugă-l și în `DEFAULT_BIC_MAP` din `index.html` și publică din nou pagina.
