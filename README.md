# VitalBPX
Repozytorium z gotowymi rozwiązaniami

## Dodanie komunikatów w języku polski do vitlPBX


# Instrukcja wdrożenia polskiej paczki językowej (katalog "pl")

Niniejsza instrukcja opisuje proces instalacji przygotowanego wcześniej katalogu `pl` (zawierającego pliki `.alaw` oraz podfoldery, np. `digits`) na serwerze telekomunikacyjnym **Asterisk / VitalPBX**.

## 📋 Wymagania wstępne
* **WinSCP**: do bezpiecznego transferu plików z systemu Windows.
* **PuTTY**: do wykonywania komend administracyjnych na serwerze.
* **Katalog `pl`**: gotowy folder na dysku lokalnym z poprawnie sformatowanymi plikami `.alaw`.
* **Dostęp**: Uprawnienia użytkownika (np. `admin`) oraz hasło `root`.

---

## 🚀 Instrukcja krok po kroku

### Krok 1: Przesłanie katalogu przez WinSCP
Z powodów bezpieczeństwa systemy Linux często blokują bezpośredni zapis w katalogach systemowych dla zwykłych użytkowników. Dlatego folder wgrywamy najpierw do katalogu domowego.

1.  Uruchom **WinSCP** i połącz się z serwerem.
2.  W prawym oknie (serwer) przejdź do: `/home/<TWOJA_NAZWA_UZYTKOWNIKA>/`.
3.  Przeciągnij folder `pl` z Twojego komputera do okna WinSCP.
4.  Zaczekaj na zakończenie przesyłania wszystkich plików.

### Krok 2: Logowanie do konsoli (PuTTY)
1.  Otwórz **PuTTY**, wpisz adres IP serwera i kliknij **Open**.
2.  Zaloguj się na swojego użytkownika.
3.  Podnieś uprawnienia do poziomu administratora (root):
    ```bash
    su -
    ```
    *(Wprowadź hasło roota, gdy zostaniesz o to poproszony).*

### Krok 3: Instalacja katalogu w strukturze Asterisk
Przenosimy przygotowany folder z Twojego katalogu domowego do systemowego folderu z dźwiękami. Używamy flagi `-a`, aby zachować strukturę podfolderów (np. `digits`).

```bash
# Kopiowanie folderu pl do katalogu dźwięków systemowych
cp -a /home/<TWOJA_NAZWA_UZYTKOWNIKA>/pl /var/lib/asterisk/sounds/
```

### Krok 4: Nadanie uprawnień (Kluczowe)
Pliki po skopiowaniu należą do roota, co uniemożliwia centrali ich odtworzenie (cisza w słuchawce). Należy rekurencyjnie nadać własność użytkownikowi asterisk.

```bash
chown -R asterisk:asterisk /var/lib/asterisk/sounds/pl
```

### Krok 5: Porządki (Opcjonalne)
Po udanej instalacji możesz usunąć kopię roboczą folderu ze swojego katalogu domowego:

```bash
rm -rf /home/TWOJA_NAZWA_UZYTKOWNIKA/pl
```

### 🛠 Diagnostyka i weryfikacja
Sprawdzenie zawartości folderu głównego (czy pliki należą do 'asterisk'):

```bash
ls -l /var/lib/asterisk/sounds/pl
```

Sprawdzenie zawartości folderu z cyframi:

```bash
ls -l /var/lib/asterisk/sounds/pl/digits
```

Przeładowanie rdzenia Asteriska (wymuszenie odświeżenia konfiguracji):

```bash 
asterisk -rx "core reload"
```
