
Iată explicațiile.

1. Aceasta este o apelare normală a metodei obiectului.

2. La fel, parantezele nu schimbă ordinea operațiilor aici, punctul este oricum primul.

3. Aici avem un apel mai complex `(expresie)()`. Apelul funcționează ca și cum ar fi fost împărțit în două linii:

    ```js no-beautify
    f = obj.go; // calculează expresia
    f();        // apelează ce avem
    ```

    Aici `f()` este executat ca funcție, fără `this`.

4. Același lucru similar cu `(3)`, la stânga parantezelor `()` avem o expresie.

Pentru a explica comportamentul apelurilor `(3)` și `(4)` trebuie să ne reamintim că accesorii de proprietăți (punct sau paranteze pătrate) returnează o valoare de Tip Referință.

Orice operație pe aceasta, cu excepția unui apel de metodă (precum alocarea `=` sau `||`) o transformă într-o valoare obișnuită, care nu poartă informațiile ce permit setarea variabilei `this`.
