# Tutorium - Grundlagen Datenbanken - Blatt 2

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
Schaue dir das Datenbankmodell an. Wofür steht hinter dem Datentyp `NUMBER` die Zahlen in den runden Klammern?
Nehme dir die Oracle [Dokumentation](https://docs.oracle.com/cd/B28359_01/server.111/b28318/datatype.htm#CNCPT012) zu Hilfe.

#### Lösung
Sind hinter 'NUMBER' Zahlen in Klammern angegeben, steht die erste Zahl für die Gesamtzahl der Stellen und die zweite Zahl für die Anzahl der Nachkommastellen

### Aufgabe 2
Was bedeuten die durchgezogenen Linien, die zwischen einigen Tabellen abgebildet sind?

#### Lösung
Die durchgezogenen Linie bedeutet, totale Partizipation(Jede Instanz einer Klasse muß mit einer Instanz der zweiten
Klasse in Beziehung stehen).Diese Attribute dürfen nicht 'null' sein.

### Aufgabe 3
Was bedeutet die gestrichelte Linie, die zwischen der Tabelle `ACC_VEHIC` und `GAS_STATION` abgebildet ist?

#### Lösung
Die Bestrichelte Linie bedeutet Partielle Partizipation (eine Instanz einer Klasse kann in Beziehung zu einer
Instanz der zweiten Klasse stehen). 'DEFAULT_GAS_STATION' kann auch 'null' sein.

### Aufgabe 3
Die folgende Abbildung beschreibt eine Beziehung zwischen Tabellen. Sie wird auch `n` zu `m` Beziehung genannt. Beschreibe kurz die Bedeutung dieser Beziehung.
Nehme dir diesen [Artikel](https://glossar.hs-augsburg.de/Beziehungstypen) zu Hilfe.

![n-to-m-relationship](./img/n-to-m-relationship.png)

In diesem Fall wird dargestellt, dass eine Person mehrere (n) Hobbys haben kann, jedoch jedem Hobby eine genaue ID zugeordnet sein muss.

### Aufgabe 4
Was bedeutet der Buchstabe `P` und `F` neben den Attributen von Tabellen?

#### Lösung
P steht für Primärschlüssel und F für Fremdschlüssel

### Aufgabe 5
Importiere die SQL-Dump-Datei in dein eigenes Schema. Wie lautet dazu der Befehl um dem import zu starten?

#### Lösung
```sql
start tutorium.sql
```

### Aufgabe 6
Gebe alle Datensätze der Tabelle `ACCOUNT` aus.

#### Lösung
```sql
select * from account;
```

### Aufgabe 7
Modifiziere Aufgabe 6 so, dass nur die Spalte `ACCOUNT_ID` ausgegeben wird.

#### Lösung
```sql
 select ACCOUNT_ID from ACCOUNT;
```

### Aufgabe 8
Gebe alle Spalten der Tabelle `VEHICLE` aus.

#### Lösung
```sql
select COLUMN_NAME from USER_TAB_COLUMNS where TABLE_NAME = 'VEHICLE';
```

### Aufgabe 9
Kombiniere Aufgabe 7 und 8 so, dass nur Personen (`ACCOUNT`) angezeigt werden, die ein Auto (`VEHICLE`) besitzen.

#### Lösung
```sql
SELECT DISTINCT account.* 
FROM ACCOUNT account, ACC_VEHIC vehicle 
WHERE account.ACCOUNT_ID = vehicle.ACCOUNT_ID;
```

### Aufgabe 10
Modifizierde die Aufgabe 9 so, dass nur die Person mit der `ACCOUNT_ID` = `7` angezeigt wird.

#### Lösung
```sql
SELECT DISTINCT account.* 
FROM ACCOUNT account, ACC_VEHIC vehicle 
WHERE account.ACCOUNT_ID = vehicle.ACCOUNT_ID
AND vehicle.ACCOUNT_ID=7;
```

### Aufgabe 11
Erstelle für dich einen neuen Benutzer.
> Achtung, nutze für die Spalten `C_DATE` und `U_DATE` vorerst die Syntax `SYSDATE` - [Dokumentation](https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions172.htm)

#### Lösung
```sql
insert into ACCOUNT
values ((SELECT MAX(ACCOUNT_ID) + 1 from ACCOUNT),'Berg','Lukas','berglu@hochschule-trier.de',sysdate,sysdate);

```

### Aufgabe 12
Erstelle für deinen neuen Benutzer ein neues Auto. Dieses Auto dient als Vorlage für die nächten Aufgaben.

#### Lösung
insert into VEHICLE
values ((SELECT MAX(VEHICLE_ID) + 1 from VEHICLE),
1,
6,
'355 S Spider Scaglietti',
1,
365,
'08.02.1957',
2,
sysdate,
sysdate);
-- Es handelt sich dabei um ein Ferrari 355 S Spider Scaglietti aus dem Jahre 1957
Deine Lösung
```

### Aufgabe 13
Verknüpfe das aus Aufgabe 12 erstellte neue Auto mit deinem neuen Benutzer aus Aufgabe 11 in der Tabelle `ACC_VEHIC` und erstelle den ersten Rechnungsbeleg.

#### Lösung
insert into ACC_VEHIC
values ((SELECT MAX(ACC_VEHIC_ID) + 1 from ACC_VEHIC),
10,
14,
'355 S Spider',
'Ferrari',
null,
null,
2000000,
20,
null,
'08.02.2011',
null,
sysdate,
sysdate);
Deine Lösung
```

### Aufgabe 14
Ändere den Vorname `SURNAME` des Datensatzes mit der ID `7` in der Tabelle `ACCOUNT` auf `Zimmermann`.

#### Lösung
update ACCOUNT set SURNAME='Zimmermann' where ACCOUNT_ID='7';
Deine Lösung
```

### Aufgabe 15
Speichere alle Änderungen deiner offenen Transaktion. Wie lautet der SQL-Befehl dazu?

#### Lösung
```sql
commit;
```


