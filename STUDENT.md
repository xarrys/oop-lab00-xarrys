#Hello from Java!Hello from Java! Moje wykonanie Lab00

- Login GitHub / pseudonim: xarrys
- System i terminal (np. Windows + WSL Ubuntu): Arch Linux + zsh
- Edytor / IDE: Neovim
- Wersja Git: 2.55.0
- Wersja kompilatora C++: 16.2.1
- Wersje java i javac: 21.0.12.1
- Link do pierwszego PR (uzupełnij w zadaniu 5): https://github.com/xarrys/oop-lab00-xarrys/pull/1

## Uruchomienie lokalne
Wynik programu C++:
```text
Hello from C++!
```
Wynik programu Java:
```text
Hello from Java!
```

## Błąd i poprawka (zadanie 5)
- Krótki fragment komunikatu błędu i numer linii: `cpp/main.cpp:5:58: error: expected ‘;’ before ‘return’ cpp/main.cpp:5:58: error: expected ‘;’ before ‘return’`
- Przyczyna oraz sposób naprawy: Brakujący średnik między instrukcjami. Aby naprawić należy dodać średnik na końcu 5 linijki.
- Commit z błędem (SHA lub link): e741581ab038a71927ace488abab0f3bcd5a223e
- Czy Actions pokazały błąd, a po naprawie sukces? Tak

## Krótkie odpowiedzi
1. Co różni commit od push? Commit tylko tworzy zapis aktualnego stanu, pull synchronizuje go ze zdalnym repozytorium.
2. Dlaczego po scaleniu PR wykonuję lokalnie pull? Aby zsynchronizować stan lokalnego repozytorium ze zmianami na zdalnym repozytorium. Pozwala to uniknąć potem konfliktów przy zmianach z obu stron.
3. Co potwierdza zielony wynik naszego CI, a czego nie potwierdza? Potwierdza tylko kompilację i uruchomienie programu, nie weryfikuje jego wyjścia.

## Ewentualne problemy środowiska
Brak / opis problemu i sposób rozwiązania: Brak
