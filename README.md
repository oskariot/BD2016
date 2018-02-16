# Projekt z baz danych 2016
## Instalacja
Program napisany jest w języku *Ruby* z wykorzystaniem biblioteki graficznej *shoes*.
#### Instalacja *shoes*
Aby aplikacja funkcjonowała poprawnie należy pobrać *shoes 3.3.1* ze strony http://shoesrb.com/downloads/.
#### Instalacja gemów
```
gem install pg
gem install digest
```
#### Przygotowywanie bazy danych
##### Utworzenie użytkownika
```
sudo -u postgres createuser <user>
sudo -u postgres psql postgres
postgres=# ALTER USER <user> WITH password <password>;
postgres=# \q
```
##### Utworzenie bazy danych
```
sudo -u postgres createdb <dbname> --owner <user>
```

#### Konfiguracja aplikacji
Aby skonfigurować aplikację należy zmodyfikować plik *config.rb*
```ruby
CONFIG = {
    shoes_path: <path_to_shoes>, # folder zawierający shoes, domyślnie ~/.shoes/walkabout
    user: <user>, # nazwa utworzonego użytkownika
    password: <password>, # hasło utworzonego użytkownika
    dbname: <dbname> # nazwa utworzonej bazy danych
}
``` 
## Aplikacja
#### Utworzenie schematu bazy danych
```
rake init
```
#### Populacja bazy danych przykładowymi danymi
```
rake seed
```
Polecenie ```rake setup``` jest równoważne uruchomieniu następująco ```rake init``` i ```rake seed```.
#### Uruchomienie aplikacji
```
rake run
```
lub
```
rake
```
#### Podział logiczny aplikacji
* */db/db.rb* - moduł łączący się bezpośrednio z bazą danych, udostępniający funkcje "yieldujące" połączenie: *with_connection* oraz  *with_connection_deal_errors*. Tych funkcji można potem używać następująco:
```ruby
DB.with_connection do |con|
  con.exec "..."
end
errors = DB.with_connection_deal_errors do |con|
  con.exec "..."
end
```
* */helpers* - małe funkcje pomocnicze niekomunikujące się z bazą danych.
* */images* - wszystkie grafiki używane w aplikacji.
* */modules/login.rb* - moduł logujący / wylogowujący użytkownika.
* */modules/session.rb* - definiuje klasę ```Session```, która przechowuje identyfikator obecnie zalogowanego użytkownika, typ użytkownika i jego pseudonim. Przy wylogowaniu następuje reset wszystkich parametrów.
* */modules/controllers* - kontrolery pośredniczące pomiędzy widokami, a bazą danych. Łączą się z bazą danych za pomocą klasy ```DB``` zdefiniowanej w */db/db.rb*
* */views* - widoki, czyli okna aplikacji *shoes*. Widoki używają funkcji zdefiniowanych w kontrolerach, żeby zdobyć potrzebne informacje.
* *config.rb* - konfiguracja bazy danych aplikacji.
* *shoes.rb* - konfiguruje ustawienia *shoes* i uruchamia pierwsze okno *shoes*.

#### Interfejs
Interfejs jest bardzo intuicyjny i nie wymaga dalszych instrukcji.

*oskarito*
*18.06.2016*
