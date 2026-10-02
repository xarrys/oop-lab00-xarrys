# Laboratorium 00 — Git, GitHub i środowisko pracy
**Programowanie obiektowe · 2026/2027**

## Po co ta praca?
Przygotujesz środowisko do kolejnych laboratoriów. Przećwiczysz pobranie repozytorium, lokalną zmianę kodu, commit, push, pracę w gałęzi i pull request. Uruchomisz gotowe programy C++ i Java oraz przeczytasz wynik automatycznej kompilacji.

Nie implementujesz jeszcze klas ani testów. Nie musisz znać OOP. Gotowe programy służą do sprawdzenia narzędzi.

**Forma:** indywidualna. **Ocena:** zaliczono / nie zaliczono. **Czas:** około 90 minut przy zainstalowanych narzędziach; instalację wykonaj przed zajęciami, jeśli to możliwe. Termin i sposób przekazania linku określa prowadzący.

Po ukończeniu potrafisz:
- odróżnić Git od GitHub oraz repozytorium lokalne od zdalnego;
- wykonać clone, status, diff, add, commit, push i pull;
- utworzyć gałąź i scalić jej zmiany przez pull request;
- uruchomić program C++ i Java;
- znaleźć błąd w logu GitHub Actions;
- wyjaśnić, dlaczego commit nie wysyła automatycznie zmian na GitHub.

## 1. Przygotowanie narzędzi
Potrzebujesz konta GitHub, Git, **JDK 17 lub nowszego** (nie tylko JRE), kompilatora **g++ obsługującego C++17** i edytora lub IDE.

Zalecana konfiguracja Windows: WSL + Ubuntu + edytor obsługujący WSL. Jeżeli masz już działające JDK i g++ w Windows, możesz użyć PowerShell. Nie instaluj drugiego środowiska bez potrzeby. Samo Visual Studio z kompilatorem MSVC nie zapewnia polecenia g++; w takim przypadku uzgodnij konfigurację z prowadzącym.

### Ubuntu / WSL Ubuntu
Jeżeli narzędzi brakuje:
```bash
sudo apt update
sudo apt install git g++ openjdk-17-jdk
```

### Windows bez WSL
Zainstaluj [Git for Windows](https://git-scm.com/downloads/win), [Temurin JDK](https://adoptium.net/temurin/releases/) oraz g++ np. przez [MSYS2](https://www.msys2.org/). Skorzystaj z instrukcji tych narzędzi dotyczących PATH. Po instalacji otwórz nowy terminal.

### macOS
Zainstaluj Git, JDK 17+ i kompilator C++17. Systemowe polecenie `g++` może wywoływać Apple Clang — dla tej prostej pracy jest to wystarczające. Narzędzia kompilacji można zainstalować przez `xcode-select --install`; JDK zainstaluj osobno.

### Sprawdzenie — w terminalu
```bash
git --version
g++ --version
java -version
javac -version
```
Każde polecenie powinno wypisać wersję. Jeżeli `java` działa, ale `javac` nie, sprawdź instalację JDK i PATH. Używaj tego samego środowiska do klonowania i kompilowania (np. wszystkiego w WSL).

Skonfiguruj autora commitów, jeśli jeszcze tego nie zrobiłeś/aś:
```bash
git config --global user.name "Twoja nazwa autora"
git config --global user.email "TWÓJ_EMAIL_DO_COMMITÓW"
```
Zastąp przykładowe wartości własnymi. Możesz użyć adresu noreply dostępnego w ustawieniach GitHub → Emails. Jest to identyfikacja autora, a nie logowanie do GitHub.

## 2. Krótki słownik
| Pojęcie | Znaczenie |
|---|---|
| Git | lokalny system kontroli wersji |
| GitHub | serwis przechowujący repozytoria i narzędzia współpracy |
| Repository | pliki projektu wraz z historią |
| Clone | pobranie repozytorium z historią na komputer |
| Working tree | pliki, które aktualnie edytujesz |
| Stage / git add | wybór zmian do następnego commita |
| Commit | zapis wybranych zmian w lokalnej historii |
| Push | wysłanie commitów do repozytorium zdalnego |
| Pull | pobranie i włączenie zmian ze zdalnego repozytorium |
| Branch | osobna linia pracy |
| Pull request (PR) | propozycja włączenia zmian z gałęzi do innej gałęzi |
| Merge | scalenie zmian |
| GitHub Actions | automatyczne wykonywanie zadań, tutaj kompilacji |

`commit` nie oznacza `push`. `pull request` nie jest poleceniem `git pull`.

## Zadanie 1 — Własne repozytorium zadania
**Pracuj wyłącznie w swoim repozytorium zadania, nie w szablonie prowadzącego.**

Utwórz własną kopię bezpośrednio na GitHub:
1. Otwórz link do repozytorium szablonowego podany przez prowadzącego.
2. Wybierz **Use this template → Create a new repository**.
3. Jako **Owner** wybierz swoje osobiste konto GitHub.
4. Nazwij repozytorium `oop-lab00-TWOJ_LOGIN`.
5. Ustaw widoczność wskazaną przez prowadzącego. Nie zaznaczaj **Include all branches**; wystarczy domyślna gałąź.
6. Kliknij **Create repository** i otwórz utworzoną kopię.

To samodzielne zadanie oparte na GitHub template. Wszystkie ćwiczenia, gałęzie i PR wykonujesz w swojej kopii.

Nie używaj Download ZIP jako sposobu pracy nad zadaniem: potrzebujemy lokalnej historii Git i powiązania ze zdalnym repozytorium. W tej pracy użyj kopii z szablonu, a nie forka.

Skopiuj HTTPS URL z **Code**. W terminalu:
```bash
git clone <HTTPS_URL_TWOJEGO_REPOZYTORIUM>
cd <NAZWA_UTWORZONEGO_KATALOGU>
git remote -v
git status
```
Wstaw rzeczywisty URL i nazwę katalogu bez nawiasów `< >`. `origin` ma wskazywać na Twoje repozytorium zadania. Domyślna gałąź w tej instrukcji to `main`; jeśli prowadzący używa innej, dostosuj polecenia.

Otwórz ten katalog w IDE. Nie twórz nowego projektu poza nim. Pliki źródłowe są w `cpp/` i `java/`.

Przy dostępie do prywatnego repozytorium lub przy pierwszym push może pojawić się logowanie. Użyj przeglądarkowego logowania menedżera poświadczeń lub innej metody wskazanej przez prowadzącego. Hasło konta nie służy do uwierzytelniania Git przez HTTPS. Nie wpisuj tokenów do plików projektu ani do URL zapisywanego w repozytorium.

## Zadanie 2 — Gałąź i lokalne uruchomienie
Utwórz gałąź roboczą:
```bash
git switch -c lab00-setup
```

### Linux / WSL / macOS — z katalogu głównego repozytorium
```bash
mkdir -p build/cpp build/java
g++ -std=c++17 -Wall -Wextra -Wpedantic cpp/main.cpp -o build/cpp/hello
./build/cpp/hello
javac -encoding UTF-8 -d build/java java/Main.java
java -cp build/java Main
```

### Windows PowerShell — z katalogu głównego repozytorium
```powershell
New-Item -ItemType Directory -Force -Path build/cpp, build/java
g++ -std=c++17 -Wall -Wextra -Wpedantic cpp/main.cpp -o build/cpp/hello.exe
./build/cpp/hello.exe
javac -encoding UTF-8 -d build/java java/Main.java
java -cp build/java Main
```

Oczekiwane wyniki:
```text
Hello from C++!
Hello from Java!
```

`build/` przechowuje pliki wynikowe. `.gitignore` sprawia, że nie są dodawane do historii. Po uruchomieniu sprawdź `git status`: pliki binarne i `.class` nie powinny być proponowane do commita.

## Zadanie 3 — Zmiana, diff, commit i push
1. Zmień tekst w obu programach, np. `Hello from C++!` na `Hello from C++! Author: student123` i analogicznie w Java. Użyj loginu lub pseudonimu; nie musisz wpisywać danych osobowych.
2. Skompiluj i uruchom ponownie oba programy. Sprawdź zmieniony wynik.
3. Uzupełnij `STUDENT.md`: login, środowisko, wersje narzędzi i krótkie odpowiedzi. Nie potrzebujesz osobnego sprawozdania.
4. Zobacz różnice i wybierz pliki do zapisu:
   ```bash
   git status
   git diff
   git add cpp/main.cpp java/Main.java STUDENT.md
   git diff --staged
   git commit -m "Personalize programs and document local setup"
   git push -u origin lab00-setup
   ```
5. Otwórz swoją gałąź na GitHub i sprawdź, czy pliki są zmienione.

Git zapisuje pliki, nie sam wynik uruchomienia. Zmiana widoczna lokalnie nie jest jeszcze widoczna dla prowadzącego przed push.

## Zadanie 4 — Pull request i automatyczna kontrola 
Na GitHub otwórz **Pull requests → New pull request** (lub **Compare & pull request**).

Ustaw **base: main**, **compare: lab00-setup**, w obrębie TWOJEGO repozytorium. Nie otwieraj PR do szablonu prowadzącego.

Tytuł: `Lab00: przygotowanie środowiska`. W opisie napisz:
- które pliki zmieniłeś/aś;
- czy oba programy działają lokalnie;
- czy napotkałeś/aś problem i jak go rozwiązałeś/aś.

Sprawdź zakładkę **Files changed**. Następnie otwórz **Actions → Lab00 build** i odpowiednie uruchomienie lub sekcję checks w PR. Sprawdź oba zadania: C++ i Java. Zielony wynik oznacza, że kod skompilował się i uruchomił w środowisku CI. Nie potwierdza wykonania wszystkich zadań ani konfiguracji Twojego komputera.

Po udanej kontroli scal PR przyciskiem **Merge pull request**, o ile prowadzący nie wymaga wcześniejszego przeglądu. Następnie lokalnie:
```bash
git switch main
git pull --ff-only origin main
```
Sprawdź, że w lokalnej gałęzi `main` są Twoje zmiany. Zachowaj link do scalonego PR w `STUDENT.md` w następnym zadaniu.

## Zadanie 5 — Rozpoznanie i poprawienie błędu 
Z aktualnej gałęzi `main` utwórz nową gałąź:
```bash
git switch -c lab00-debug
```

1. W `cpp/main.cpp` usuń średnik na końcu instrukcji wypisującej tekst.
2. Spróbuj skompilować program. Przeczytaj komunikat; nie uruchamiaj starego pliku wykonywalnego jako dowodu poprawności nowego kodu.
3. Zapisz celowo błędną wersję na tej gałęzi:
   ```bash
   git add cpp/main.cpp
   git commit -m "Exercise: introduce a compilation error"
   git push -u origin lab00-debug
   ```
4. W **Actions** otwórz uruchomienie dla tego commita, zadanie C++ i krok kompilacji. Znajdź błąd oraz numer linii. Czerwony wynik jest w tej części oczekiwany. Jeśli popełnienie błędu weryfikujesz wyłącznie lokalnie z powodu wyłączonych Actions, odnotuj to w `STUDENT.md`.
5. Przywróć średnik, skompiluj i uruchom program lokalnie.
6. W `STUDENT.md` wpisz komunikat błędu, sposób naprawy oraz link do pierwszego PR. Opisz, co potwierdza CI, a czego nie potwierdza.
7. Zapisz poprawkę:
   ```bash
   git add cpp/main.cpp STUDENT.md
   git commit -m "Fix compilation error and complete Lab00 notes"
   git push
   ```
8. Otwórz drugi PR: **lab00-debug → main**. Poczekaj na zielony wynik dla poprawionego commita i scal PR zgodnie z zasadą z zadania 4. Następnie:
   ```bash
   git switch main
   git pull --ff-only origin main
   git status
   git log --oneline -5
   ```

Nie scalaj błędnego kodu do `main`. Historia gałęzi pozwala zobaczyć zarówno błąd, jak i poprawkę; wcześniejszy czerwony wynik nie blokuje zaliczenia, jeśli finalny kod działa.

## Zadanie 6 — Zaliczenie
Przekaż prowadzącemu link do swojego repozytorium w sposób podany na zajęciach. Jeśli repozytorium jest prywatne, zapewnij prowadzącemu dostęp: w swoim repozytorium otwórz **Settings → Collaborators**, wybierz **Add people** i zaproś jego dokładny login GitHub podany na zajęciach. Prowadzący musi zaakceptować zaproszenie. Dla publicznego repozytorium do odczytu wystarczy link.

### Lista kontrolna
- [ ] Repozytorium zadania jest moje i zostało sklonowane lokalnie.
- [ ] Oba programy uruchomiłem/am na swoim komputerze i zmieniłem/am ich komunikaty.
- [ ] STUDENT.md jest uzupełniony, a wyniki kompilacji nie trafiły do Git.
- [ ] Pierwszy PR pokazuje moje zmiany i został scalony.
- [ ] Potrafię wskazać błędny commit i commit z poprawką.
- [ ] Drugi PR został scalony, a finalny kod działa.
- [ ] Finalne uruchomienie Actions na main jest zielone, jeśli Actions są dostępne.
- [ ] Lokalny main jest zsynchronizowany po merge.

Na krótkiej obronie pokaż uruchomienie programów i odpowiedz na dwa pytania:
1. Co różni commit od push?
2. Dlaczego po scaleniu PR na GitHub wykonujemy lokalnie pull?
3. Co różni gałąź od oddzielnego repozytorium?
4. Co sprawdza nasz workflow, a czego nie sprawdza?

Zaliczenie wymaga działających programów, zmian w zdalnym repozytorium, historii ćwiczeń, uzupełnionego STUDENT.md i wyjaśnienia podstawowych operacji. Nie ma punktów za szybkość ani za znajomość wszystkich poleceń z pamięci.

## Typowe problemy
| Problem | Co sprawdzić |
|---|---|
| `git`, `g++` lub `javac` nie znaleziono | instalacja, PATH i ponowne otwarcie terminala |
| `Permission denied` / brak dostępu | czy origin wskazuje Twoje repozytorium i czy jesteś zalogowany/a na właściwe konto |
| `Author identity unknown` | user.name i user.email w git config |
| `nothing to commit` | czy zapisano plik i czy pracujesz we właściwym katalogu |
| push odrzucony po zmianie przez przeglądarkę | nie używaj force push; sprawdź status i uzgodnij z prowadzącym bezpieczne pobranie zmian |
| po merge lokalnie wciąż stary kod | przełącz na main i wykonaj pull |
| czerwone Actions | przeczytaj pierwszy konkretny błąd w nieudanym kroku |
| brak przycisku merge lub Actions | uprawnienia/polityka organizacji; poproś prowadzącego o sprawdzenie |

## Materiały i dodatkowy trening
- [GitHub Starter Course — oficjalny materiał wprowadzający](https://github.com/classroom-resources/github-starter-course)
- [GitHub Skills — Introduction to GitHub](https://github.com/skills/introduction-to-github)
- [Git — podstawy](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)
- [Klonowanie repozytorium](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)

Ta laboratoryjna instrukcja jest samodzielnym zadaniem dydaktycznym, a nie oficjalnym kursem GitHub. Łączy zakres wprowadzenia z praktyką podobną do GitHub Skills i uruchomieniem środowiska do OOP. Nie zawiera ani nie uruchamia automatycznego bota kursu Skills. Nie musisz tworzyć drugiego repozytorium. Oficjalne Skills możesz przejść dodatkowo, korzystając z instrukcji na jego stronie.

Możesz korzystać z dokumentacji i AI, ale musisz samodzielnie wykonać operacje i umieć je wyjaśnić. Nie oddajesz osobnej deklaracji użycia AI.
