# Tutorium - Grundlagen Datenbanken - Blatt 4

## Vorbereitungen
* Für dieses Aufgabenblatt wird die SQL-Dump-Datei `tutorium.sql` benötigt, die sich im Verzeichnis `sql` befindet.
* Die SQL-Dump-Datei wird in SQL-Plus mittels `start <Dateipfad/zur/sql-dump-datei.sql>` in die Datenbank importiert.
* Beispiele
  * Linux `start ~/Tutorium.sql`
  * Windows `start C:\Users\max.mustermann\Desktop\Tutorium.sql`

## Datenbankmodell
![Datenbankmodell](./img/datamodler_schema.png)

## Aufgaben

### Aufgabe 1
Um genauere Informationen und Prognosen mit Data Mining Werkzeugen zu schöpfen, ist es notwendig mehr Informationen über die registrierten Benutzer zu sammeln und zu speichern. Die in Zukunft gesammelten Informationen sollen in neuen Tabellen des bestehenden Datenbankmodells gespeichert werden. Dazu soll jedem Benutzer einen Erst- und Zweitwohnsitz zugeordnet werden. Jeder Wohnsitz besitzt eine eigene Adresse. Integriere in das bestehende Datenbankmodell Tabellen die den genauen Erst- und Zweitwohnsitz abbilden können. Beachte dazu die Normalisierungsformen bis 3NF - [Dokumentation](https://de.wikipedia.org/wiki/Normalisierung_(Datenbank)). Wie lautet deine SQL-Syntax um deine Erweiterung des Datenbankmodells zu implementieren?

#### Lösung
```sql
create table domicile(
dom_id number(4) not null,
street varchar(50) not null,
plz number(5)not null,
city varchar(30)not null);

create table dom_account(
account_id number(4)not null,
first_dom_id number(4)not null,
seconds_dom_id number(4));
```

### Aufgabe 2
Als App Entwickler/in für Android und iOS möchtest du dich nicht darauf verlassen, dass die Adresse exakt richtig ist und überlegst in dem Datenbankmodell noch zwei zusätzliche Attribute (X und Y Koordinate) zur genauen GPS Lokalisierung einer Tankstelle aufzunehmen. Wie lautet deine SQL-Syntax um das Datenbankmodell auf die zwei Attribute zu erweitern?

#### Lösung
```sql
alter table gas_station 
add (x_coor number(5,15),y_coor number(5,15));

```

### Aufgabe 3
Welche Kunden haben im Jahr 2017 mehr als den Durchschnitt getank?

#### Lösung
```sql
Deine Lösung
```

### Aufgabe 4
Ermittle, warum du INSERT-Rechte auf die Tabelle `SCOTT.EMP` und UPDATE-Rechte auf die Tabelle `SCOTT.DEPT` besitzt. Beantworte dazu schrittweise die Aufgaben von 4.1 bis 4.4.

#### Aufgabe 4.1
Wurden die Tabellen-Rechte direkt an dich bzw. an `PUBLIC` vergeben?

##### Lösung
```sql
select *
from ALL_TAB_PRIVS
where TABLE_SCHEMA=’SCOTT’
and TABLE_NAME=’DEPT’;

Update-Rechte von Scott an Public vergeben
```

#### Aufgabe 4.2
Welche Rollen besitzt du direkt?

##### Lösung
```sql
select * from user_role_privs;

BW_Student & FH_Trier
```

#### Aufgabe 4.3
Welche Rollen haben die Rollen?

##### Lösung
```sql
 select * from role_role_privs;
 
 FH_Trier hat WI-Student als Rolle
 BW_Student hat Student als Rolle
```

#### Aufgabe 4.4
Haben die Rollen Rechte an `SCOTT.EMP` oder `SCOTT.DEPT`?

##### Lösung
```sql
select *
from ALL_TAB_PRIVS
where TABLE_SCHEMA=’SCOTT’
and TABLE_NAME=’DEPT’;

Public hat Update-Rechte an Scott.Ddept, es wurden keine Rechte für Rollen vergeben
```

### Aufgabe 5
Es soll für jede Tankstelle der Umsatz einzelner Jahre aufgelistet werden auf Basis der Daten, die Benutzer durch ihre Quittungen eingegeben haben. Sortiere erst nach Jahr und anschließend nach der Tankstelle. Beispiel:

| Jahr  | Anbieter  | Straße            | PLZ   | Stadt | Land          | Umsatz    |
| ----- | --------- | ----------------- | ----- | ----- | --------------| --------- |
| 2017  | Esso      | Triererstraße 15  | 54292 | Trier | Deutschland   | 54784.14  |
| 2017  | Shell     | Zurmainerstraße 1 | 54292 | Trier | Deutschland   | 67874.78  |
| 2016  | Esso      | Triererstraße 15  | 54292 | Trier | Deutschland   | 57412.66  |
| 2016  | Shell     | Zurmainerstraße 1 | 54292 | Trier | Deutschland   | 72478.42  |

#### Lösung
```sql
Deine Lösung
```


