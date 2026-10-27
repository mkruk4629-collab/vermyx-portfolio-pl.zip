# Notatka z laboratorium: FTP i rozpoznanie usług

> **Status:** wykonane z pomocą wskazówek; do samodzielnego powtórzenia  
> **Środowisko:** autoryzowany pokój TryHackMe  
> **Zakres:** host udostępniony przez platformę laboratoryjną

## Cel
Zrozumieć podstawowy proces rozpoznania usługi sieciowej, sprawdzenia anonimowego dostępu FTP i pobrania pliku w autoryzowanym laboratorium.

## Obserwacje
- Skan Nmap wykazał otwarte porty TCP, w tym port 8075 z usługą rozpoznaną jako FTP.
- Połączenie klientem FTP z portem 8075 powiodło się.
- Serwer zaakceptował login anonymous bez indywidualnego konta.
- Polecenie ls pokazało listę plików udostępnionych przez FTP.
- Polecenie get pobrało plik do lokalnego katalogu Kali; cat wyświetlił jego zawartość.

## Przebieg i znaczenie poleceń

| Polecenie | Cel | Interpretacja |
|---|---|---|
| nmap -Pn -sV -sC -p- IP_LABU | Sprawdzenie portów i usług | -Pn pomija wykrywanie hosta przez ping, -sV próbuje rozpoznać wersję usługi, -sC uruchamia domyślne skrypty, -p- sprawdza wszystkie porty TCP |
| ftp IP_LABU 8075 | Połączenie z FTP na niestandardowym porcie | Numer portu nie musi być domyślnym portem FTP 21 |
| anonymous | Próba anonimowego logowania | To popularny mechanizm publicznego dostępu FTP; powinien być świadomie skonfigurowany |
| ls | Lista plików | Pokazuje zasoby widoczne dla bieżącej sesji |
| get NAZWA_PLIKU | Pobranie pliku | Kopiuje plik ze zdalnej usługi do bieżącego katalogu lokalnego |
| bye | Zakończenie sesji FTP | Zamyka klienta FTP |
| cat NAZWA_PLIKU | Odczyt lokalnego pliku tekstowego | Nie łączy się z serwerem — czyta plik zapisany na komputerze |

## Ważna lekcja
Otwarty port oznacza, że usługa odpowiada na danym porcie; sam w sobie nie oznacza przejęcia systemu. W tym ćwiczeniu dostęp uzyskano do funkcji udostępniania plików, a nie automatycznie do powłoki systemowej ani konta root.

## Błąd pasywnego FTP
Klient może próbować połączyć się z dodatkowym portem danych, który jest niedostępny lub którego adres został błędnie ogłoszony przez serwer. W tej sesji przełączenie trybu pasywnego pozwoliło wyświetlić listing i pobrać pliki. To przykład, dlaczego warto czytać komunikaty i sprawdzać, czy operacja faktycznie się zakończyła.

## Perspektywa defensywna
- Wyłącz anonimowy dostęp, jeśli nie jest potrzebny.
- Ogranicz katalogi i uprawnienia konta usługi do niezbędnego minimum.
- Jeśli FTP musi pozostać dostępny, rozważ bezpieczniejszy protokół transferu i ograniczenie dostępu sieciowego.
- Monitoruj logowania anonimowe, nietypowe pobrania i ekspozycję usługi na niepotrzebnych interfejsach.

## Co należy utrwalić
- Różnicę między portem, usługą i uprawnieniami użytkownika.
- Różnicę między zdalnym get a lokalnym cat.
- Umiejętność interpretowania komunikatów FTP zamiast zakładania, że każda komenda zadziałała.
- Konieczność odtwarzania rozwiązania bez podpowiedzi, zanim uzna się temat za utrwalony.

## Bezpieczeństwo publikacji
Nie umieszczam tu wartości flag, adresów aktywnych maszyn laboratoryjnych ani sekretów. Szczegóły rozwiązania pozostają zgodne z zasadami publikacji danego pokoju.
