# Tutorium - Grundlagen Datenbanken - Blatt 5

## Vorbereitungen
* Für dieses Aufgabenblatt wird die SQL-Dump-Datei `01_tutorium.sql` benötigt, die sich im Verzeichnis `sql` befindet.
* Die SQL-Dump-Datei wird in SQL-Plus mittels `start <Dateipfad/zur/sql-dump-datei.sql>` in die Datenbank importiert.
* Beispiele
  * Linux `start ~/Tutorium.sql`
  * Windows `start C:\Users\max.mustermann\Desktop\Tutorium.sql`

## Datenbankmodell
![Datenbankmodell](./img/datamodler_schema.png)

## Aufgaben

### Aufgabe 1
Erstelle mit Dia oder einem anderen Werkzeug eine Abbilung der Mengen, die durch `INNER JOIN`, `RIGHT JOIN`, `LEFT JOIN` und `OUTER JOIN` gemeint sind.

#### Lösung
```sql
Inner Join: Liefert Ergebnisse aus beiden Tabellen, deren Werte in beiden Tabellen vorhanden sind
Left Join:  Liefert Ergebnisse aus der linken Tabelle, deren Werte  in beiden Tabellen vorhanden sind
Right Join:  Liefert Ergebnisse aus der rechten Tabelle, deren Werte  in beiden Tabellen vorhanden sind
Outer Join: Liefer Ergebnisse aus beiden Tabellen, deren Werte in einer der Tabellen vorhanden sind
```

### Aufgabe 2
Welche Personen haben kein Fahrzeug? Löse dies einmal mit `LEFT JOIN` und `RIGHT JOIN`.

#### Lösung
```sql
#Left Join
select forename, surname
from account acc
left join acc_vehic accv on (acc.account_id=accv.account_id)
where accv.vehicle_id is null;

#Right Join
select forename, surname
from acc_vehic accv
right join account acc on (accv.account_id=acc.account_id)
where accv.vehicle_id is null;
```