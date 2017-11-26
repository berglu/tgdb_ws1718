# Tutorium - Grundlagen Datenbanken - Blatt 8

## Vorbereitungen
* Für dieses Aufgabenblatt wird die SQL-Dump-Datei `tutorium.sql` benötigt, die sich im Verzeichnis `sql` befindet.
* Die SQL-Dump-Datei wird in SQL-Plus mittels `start <Dateipfad/zur/sql-dump-datei.sql>` in die Datenbank importiert.
* Beispiele
  * Linux `start ~/Tutorium.sql`
  * Windows `start C:\Users\max.mustermann\Desktop\Tutorium.sql`

## Datenbankmodell
![Datenbankmodell](./img/datamodler_schema.png)

### Aufgabe 1
Erstelle eine Prozedur, die das anlegen von Benutzern durch übergabe von Parametern ermöglicht.

#### Lösung
```sql
CREATE OR REPLACE PROCEDURE insert_account (surname_in IN VARCHAR2, forename_in IN VARCHAR2, email_in in VARCHAR2)
AS
v_account_id account.account_id%TYPE;
BEGIN
IF surname_in IS NULL OR forename_in IS NULL OR email_in IS NULL THEN
RAISE_APPLICATION_ERROR(-20001, 'Feld muss einen Wert enthalten');
END IF;
SELECT MAX(account_id)+1 
INTO v_account_id
FROM account;
INSERT INTO account
values(v_account_id, surname_in, forename_in, email_in, SYSDATE, SYSDATE);
END;
/
```

### Aufgabe 2
Erstelle eine Prozedur, die das erstellen von Quittungen ermöglicht.  Fange entsprechende übergebene Parameter auf `NULL` oder ` ` ab und gebe eine Meldung aus. Ergänze eventuell Parameter die `NULL` sind mit Informationen die sich durch Abfragen abrufen lassen. Berücksichtige die Fehlerbehandlung und mögliche Constraints die gebrochen werden könnten!

#### Lösung
```sql
Deine Lösung
```

