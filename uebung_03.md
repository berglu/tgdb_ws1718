# Tutorium - Grundlagen Datenbanken - Blatt 3

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
Erstelle eine `INNER JOIN` (optional `WHERE`) Abfrage um die Beziehungen zwischen den Tabellen `GAS_STATION`, `Provider`, `COUNTRY` und `ADDRESS` aufzulösen, sodass eine ähnliche Ausgabe erstellt wird wie abgebildet.

| Anbieter  | Straße            | PLZ   | Ort         | Land        | Steuer  |
| --------- | ----------------- | ----- | ----------- | ----------- | ------- |
| Shell     | Zurlaubener 1     | 54292 | Trier       | Deutschland | 0.19    |
| Esso      | Triererstraße 10  | 53937 | Hellenthal  | Deutschland | 0,19    |
| ...       | ...               | ...   | ...         | ...         | ...     |

#### Lösung
```sql
select distinct pro.provider_name "Anbieter", gas.street "Straße", adr.plz "PLZ", adr.city "Ort", cou.country_name "Land", cou.duty_amount "Steuer"
from gas_station gas
inner join provider pro on(gas.provider_id = pro.provider_id)
inner join country cou on(gas.country_id = cou.country_id )
inner join address adr on(gas.address_id = adr.address_id);
```

### Aufgabe 2
Suche alle Tankstellen raus, deren Straßenname an zweiter Stelle ein `U` haben (case-insensetive). Verändere dazu die Abfrage aus Aufgabe 1. Optional für Enthusiasten, suche mittels Regulärem Ausdruck.

#### Lösung
```sql
select distinct pro.provider_name "Anbieter", gas.street "Straße", adr.plz "PLZ", adr.city "Ort", cou.country_name "Land", cou.duty_amount "Steuer"
from gas_station gas
inner join provider pro on(gas.provider_id = pro.provider_id) 
inner join country cou on(gas.country_id = cou.country_id )
inner join address adr on(gas.address_id = adr.address_id)
where lower (gas.street) like '_u%';
```

### Aufgabe 3
Suche alle Tankstellen raus, die sich in Trier befinden.

#### Lösung
```sql
select distinct pro.provider_name "Anbieter", gas.street "Straße", adr.plz "PLZ", adr.city "Ort", cou.country_name "Land", cou.duty_amount "Steuer"
from gas_station gas
inner join provider pro on(gas.provider_id = pro.provider_id) 
inner join country cou on(gas.country_id = cou.country_id )
inner join address adr on(gas.address_id = adr.address_id)
where adr.city = 'Trier';
```

#### Aufgabe 4
Füge eine fiktive Tankstelle hinzu. Sie darf auf keine bestehenden Informationen basieren. Nutze möglichst wenige SQL-Befehle. Rufe fehlende Informationen möglichst direkt ab.

#### Lösung
```sql
--Adresse
insert into address
values ((select max(address_id)+1 from address),10100,'Bangkok');

--Country
insert into country
values ((select max(country_id)+1 from country),'Thailand',0.07);

--Provider
insert into provider
values ((select max(provider_id)+1 from provider),'LukasOil');

--Gas Station
insert into gas_station
values ((select max*(gas_station_id)+1 from gas_station),
(select max(provider_id) from provider),
(select max(country_id) from country),
(select max(address_id) from address),
'Silon Road');
```

### Aufgabe 5
Erstelle eine INNER JOIN (optional `WHERE`) Abfrage um die Beziehung zwischen den Tabellen `ACCOUNT`, `VEHICLE`, `VEHICLE_TYPE`, `GAS` und `PRODUCER` aufzulösen und zeige die Spalten `FORNAME`, `SURNAME`, `VEHICLE_TYPE_NAME`, `VERSION`, `BUILD_YEAR`, `PRODUCER_NAME` und `GAS_NAME` an. Richte SQL-Plus so ein, dass möglicht jeder Datensatz nur eine Zeile belegt.

* COLUMN <SPALTENNAME> FORMAT a<Zeichenlänge>
* COLUMN <SPALTENNAME> FORMAT <Zahlenlänge, pro Länge eine 9>
* Beispiel für eine Spalte des Datentyps `VARCHAR2`: `COLUMN SURNAME FORMAT a16`
* Beispiel für eine Spalte des Datentyps `NUMERIC`: `COLUMN SOLD_KILOMETER 9999`

#### Lösung
```sql

--Abfrage
 select acc.forename "Vorname", acc.surname "Nachname", vet.vehicle_type_name "Fahrzeugtyp", veh.version "Modell", veh.build_year "Baujahr", pro.producer_name "Hersteller", gas.gas_name "Kraftstoff"
 from acc_vehic acv
 inner join account acc on(acv.account_id=acc.account_id)
 inner join vehicle veh on (acv.vehicle_id=veh.vehicle_id)
 inner join vehicle_type vet on(veh.vehicle_type_id=vet.vehicle_type_id)
 inner join producer pro on(veh.producer_id=pro.producer_id)
 inner join gas gas on(veh.default_gas_id=gas.gas_id);


-- Zeilen
column forename format a15;
column surname format a15;
column vehicle_type_name format a15;
column version format a15;
column build_year format a15;
column producer_name format a15;
column gas_name format a15;
```

### Aufgabe 6
Welche Fahrzeuge wurden noch keinem Benutzer zugewiesen? Gebe über das Fahrzeug Informationen über den Typ, den Hersteller, das Modell, Baujahr und den Kraftstoff aus.

#### Lösung
```sql
select vet.vehicle_type_name "Typ",pro.producer_name "Hersteller" ,veh.version "Version", veh.build_year "Baujahr" , gas.gas_name "Kraftstoff"
FROM vehicle veh
inner join vehicle_type vet on(veh.vehicle_type_id=vet.vehicle_type_id) 
inner join producer pro on(veh.producer_id=pro.producer_id)
inner join gas gas on (veh.default_gas_id = gas.gas_id)
where vehicle_id not in (select account_id from acc_vehic);
```

### Aufgabe 7
Verknüpfe eines der Autos aus Aufgabe 6 mit deinem Benutzernamen. Verwende dazu möglichst wenige SQL-Statements.

#### Lösung
```sql
insert into acc_vehic
values(
(select max(acc_vehic_id)+1 from acc_vehic),
(select account_id from account where email ='berglu@hochschule-trier.de'),
14,
null,
null,
null,
null,
null,
null,
sysdate,
sysdate,
null,
sysdate,
sysdate);
```

### Aufgabe 8
An welcher Tankstelle wurde noch nie getankt? Gebe zu den Tankstellen die Informationen über den Standort der Tankstellen aus.

#### Lösung
```sql
select  gas.street "Straße", adr.plz "PLZ", adr.city "Stadt", cou.country_name "Land"
from gas_station gas
inner join address adr on (gas.address_id=adr.address_id)
inner join country cou on (gas.country_id=cou.country_id)
where gas.gas_station_id not in ( select gas_station_id from receipt); 
```

### Aufgabe 9
Liste alle Benutzer (Vorname und Nachname) mit Fahrzeug (Hersteller, Modell, Alias) auf, die noch nie einen Beleg hinzugefügt haben.

#### Lösung
```sql
select acc.surname "Nachname", acc.forename "Vorname", pro.producer_name "Hersteller", veh.version "Modell", acv.alias "Alias"
from acc_vehic acv
inner join account acc on (acv.account_id=acc.account_id)
inner join vehicle veh on (acv.vehicle_id=veh.vehicle_id)
inner join producer pro on (veh.producer_id=pro.producer_id)
where acc.account_id not in (select account_id from receipt);
```

### Aufgabe 10
Liste alle Benutzer auf, die mit einem Fahrzeug schonmal im Außland tanken waren.

#### Lösung
```sql
select acc.surname "Nachname", acc.forename "Vorname"
from receipt rec
inner join account acc on (rec.account_id=acc.account_id)
where gas_station_id in (select gas_station_id from gas_station where country_id!=1); 

```

### Aufgabe 11
Wie viele Benutzer haben einen LKW registriert?

#### Lösung
```sql
select count(distinct account_id) as "Anzahl LKW Benutzer"
from acc_vehic acv
inner join vehicle veh on (acv.vehicle_id=veh.vehicle_id)
inner join vehicle_type vet on (veh.vehicle_type_id=vet.vehicle_type_id) 
Where vet.vehicle_type_name = 'LKW';
```

### Aufgabe 12
Wie viele Benutzer haben einen PKW und einen LKW registriert?

#### Lösung
```sql
select count(distinct account_id) as "Anzahl LKW & PKW Benutzer"
from acc_vehic acv
inner join vehicle veh on (acv.vehicle_id=veh.vehicle_id)
inner join vehicle_type vet on (veh.vehicle_type_id=vet.vehicle_type_id) 
Where vet.vehicle_type_name in ('LKW','PKW');
-- UND Abfrage fehlt
```

### Aufgabe 13
Führe den Patch `02_patch.sql`, der sich im Verzeichnis `sql` befindet, in deiner Datenbank aus. Wie lautet der Befehlt zum import?

#### Lösung
```sql
Deine Lösung
```

### Aufgabe 14
Aktualisiere den Steuersatz aller Belege auf den Steuersatz des Landes, indem die Kunden getankt haben.

#### Lösung
```sql
Deine Lösung
```


