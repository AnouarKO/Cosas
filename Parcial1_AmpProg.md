# Parcial 1 Ampliacio

Material de suport:

- [Parcial1_Ampliacio_visual.html](Parcial1_Ampliacio_visual.html)
- [Parcial1_Ampliacio_visual.md](Parcial1_Ampliacio_visual.md)
- [01_extensio_procedimental.sql](solucions/01_extensio_procedimental.sql)
- [02_mongodb.js](solucions/02_mongodb.js)
- [03_fragmentacio.md](solucions/03_fragmentacio.md)
- [04_xpath.md](solucions/04_xpath.md)

Aquest resum esta pensat per estudiar teoria i entendre els patrons dels exercicis. La idea no es memoritzar els PDFs sencers, sino reduir-los a:

- idea central
- avantatges i inconvenients
- comandes o sintaxi tipica
- trampes frequents

## Com estudiar-ho be

Fes 3 passades:

1. Primera passada: entendre la idea central de cada tema.
2. Segona passada: memoritzar paraules clau i diferencies.
3. Tercera passada: practicar la sintaxi tipus examen.

Ordre recomanat:

1. `Tema 1`: vistes, funcions, procediments i triggers.
2. `Tema 2`: SQL vs MongoDB.
3. `Tema 3`: fragmentacio i transparencia.
4. `Tema 4`: noSQL general, XML i XPath.

## Tema 1. Optimitzacio de BDR

### Idea central

L'extensio procedimental serveix per afegir a SQL coses de programacio estructurada:

- variables
- condicionals
- bucles
- funcions
- procediments
- triggers

L'objectiu es fer consultes reutilitzables, automatitzar tasques i controlar millor les dades.

### Peces principals

- `VIEW`: consulta guardada. Serveix per simplificar consultes i reutilitzar-les.
- `FUNCTION`: retorna un valor o una taula. En aquest temari s'utilitza sobretot com a "vista amb parametres".
- `PROCEDURE`: encapsula logica d'actualitzacio i validacio.
- `TRIGGER`: s'executa automaticament quan hi ha `INSERT`, `UPDATE` o `DELETE`.

Frase curta:

`view consulta, function retorna, procedure actua, trigger reacciona`

### Avantatges

- reutilitzacio de codi
- millor manteniment
- mes seguretat
- menys logica repetida a l'aplicacio
- millor rendiment quan la feina es fa al SGBD
- automatitzacio de validacions i camps calculats

### Inconvenients o limits

- la sintaxi no es totalment estandard entre SGBD
- les vistes tenen restriccions
- massa triggers poden complicar el manteniment
- si abuses de logica al servidor, costa mes depurar

### Sintaxi clau

#### Variables, condicionals i bucles

```sql
DECLARE @x INT;
SET @x = 10;

IF @x > 5
BEGIN
    PRINT('gran');
END
ELSE
BEGIN
    PRINT('petit');
END;
```

```sql
WHILE @x > 0
BEGIN
    SET @x = @x - 1;
END;
```

#### Vistes

```sql
CREATE VIEW v_alumnes
AS
SELECT nom, cognoms
FROM alumnes;
```

Recorda:

- una vista no accepta parametres
- si hi ha `COUNT`, `AVG` o camps calculats, cal alias
- `ORDER BY` dintre d'una vista te restriccions

#### Funcions que retornen taula

```sql
CREATE FUNCTION f_alumnes_grau (@nomGrau VARCHAR(100))
RETURNS TABLE
AS
RETURN
(
    SELECT *
    FROM alumnes
    WHERE grau = @nomGrau
);
```

#### Procediments

```sql
CREATE PROCEDURE sp_exemple (@dni CHAR(9))
AS
BEGIN
    DELETE FROM alumnes
    WHERE dni = @dni;
END;
```

Crida:

```sql
EXEC sp_exemple '12345678A';
```

#### Triggers

```sql
CREATE TRIGGER trg_exemple
ON alumnes
AFTER INSERT
AS
BEGIN
    PRINT('S''ha inserit un alumne');
END;
```

Tipus que has de distingir:

- `INSTEAD OF`: substitueix la instruccio que el dispara
- `AFTER`: s'executa despres

### Trampes frequents

- una `VIEW` no es una taula, es una consulta guardada
- una `FUNCTION` no es un `PROCEDURE`
- un `PROCEDURE` no es crida amb `SELECT`, es crida amb `EXEC`
- els triggers treballen amb les taules virtuals `inserted` i `deleted`

## Tema 2. Bases de dades documentals: SQL -> MongoDB

### Idea central

MongoDB es una base de dades documental. Guarda la informacio en documents tipus JSON/BSON dintre de col.leccions.

S'utilitza quan interessa:

- model flexible
- dades heterogenies
- creixer facilment
- consultes rapides

No substitueix sempre el model relacional. El complementa.

### Avantatges de MongoDB i les documentals

- esquema flexible
- facilitat per afegir nous camps
- escalabilitat horitzontal
- alt rendiment en consultes
- bona adaptacio a dades semiestructurades

### Inconvenients

- mes redundancia de dades
- consistencia mes feble que al model relacional
- no hi ha claus foranies com a SQL
- les consultes complexes es resolen diferent
- menys natural per a dades molt normalitzades

### Equivalencies SQL vs MongoDB

| SQL | MongoDB |
|-----|---------|
| base de dades | base de dades |
| taula | collection |
| fila | document |
| columna | field |
| clau primaria | `_id` o camp propi |
| `JOIN` | `$lookup` |
| `SELECT` | `find` / `aggregate` |
| `INSERT` | `insertOne` / `insertMany` |
| `UPDATE` | `updateOne` / `updateMany` |
| `DELETE` | `deleteOne` / `deleteMany` |

### ACT1 ex. 1: passar d'una taula SQL a MongoDB

Segons el vostre enunciat de classe:

- la base de dades s'ha de dir `Hotel`
- les col.leccions han de tenir el mateix nom que les taules SQL

Patro general:

1. Esculls la BD:

```javascript
use("Hotel");
```

2. Cada fila SQL passa a ser un document MongoDB.

3. Cada columna SQL passa a ser un camp del document.

4. Si tens moltes files, normalment fas `insertMany`.

Exemple relacional:
```sql
INSERT INTO ciutat (nom, nHabitants, port)
VALUES ('Barcelona', 2000000, 'Si');
```

Equivalent a MongoDB:

```javascript
db.ciutat.insertOne({
  nom: "Barcelona",
  nHabitants: 2000000,
  port: "Si"
});
```

Exemple de moltes files:

```javascript
db.ciutat.insertMany([
  { nom: "Barcelona", nHabitants: 2000000, port: "Si" },
  { nom: "Madrid", nHabitants: 3000000, port: "No" }
]);
```

### Com es representen les relacions

Al model de classe, el que feu normalment es:

- mantenir una col.leccio per taula
- guardar als documents els camps que "apunten" a una altra col.leccio
- quan cal unir dades, fer servir `$lookup`

Exemple:

- `reserva` te un camp `dniClient`
- `client` te el camp `dni`
- la unio es fa amb `$lookup`

```javascript
db.client.aggregate([
  {
    $lookup: {
      from: "reserva",
      localField: "dni",
      foreignField: "dniClient",
      as: "reserves"
    }
  }
]);
```

### DDL i DML a MongoDB

Segons els apunts:

- no hi ha un DDL tan fix com a SQL
- la col.leccio i els camps apareixen quan hi insereixes dades

Comandes basiques:

#### Insercio

```javascript
db.col.insertOne({ camp: "valor" });
db.col.insertMany([{ camp: 1 }, { camp: 2 }]);
```

#### Modificacio

```javascript
db.col.updateOne(
  { campCerca: valor },
  { $set: { campModificar: nouValor } }
);
```

```javascript
db.col.updateMany(
  { estat: "actiu" },
  { $inc: { comptador: 1 } }
);
```

Operadors de modificacio importants:

- `$set`
- `$inc`
- `$push`
- `$addToSet`
- `$pull`
- `$rename`
- `$unset`

#### Eliminacio

```javascript
db.col.deleteOne({ nom: "Girona" });
db.col.deleteMany({ port: "Si" });
```

### Consultes a una sola col.leccio

#### SELECT ... FROM ... WHERE

```javascript
db.ciutat.find(
  { nHabitants: { $gt: 120000 } },
  { _id: 0, nom: 1, port: 1 }
);
```

Recorda:

- el primer objecte es el filtre
- el segon objecte es la projeccio
- si no vols veure `_id`, posa `_id:0`

#### Operadors de comparacio

- `$gt`
- `$lt`
- `$gte`
- `$lte`
- `$eq`
- `$ne`
- `$in`
- `$nin`
- `$exists`

#### Operadors logics

- `$and`
- `$or`
- `$not`
- `$nor`

#### BETWEEN, IN i LIKE

`BETWEEN`:

```javascript
{ nHabitants: { $gte: 120000, $lte: 140000 } }
```

`IN`:

```javascript
{ nom: { $in: ["Barcelona", "Tarragona"] } }
```

`LIKE`:

```javascript
{ nom: { $regex: ".*ona" } }
```

#### ORDER BY i LIMIT

```javascript
db.ciutat.find({}, { _id: 0, nom: 1 })
  .sort({ nom: 1 })
  .limit(10);
```

### `distinct`

Per valors sense repetir:

```javascript
db.avio.distinct("tipus");
```

### `aggregate` i pipelines

Quan una consulta nomes filtra i mostra camps, pensa en `find`.

Quan la consulta:

- compta
- suma
- fa mitjanes
- agrupa
- uneix col.leccions

pensa en `aggregate`.

Etapes tipiques d'una pipeline:

- `$match`
- `$sort`
- `$group`
- `$project`
- `$limit`

Exemple:

```javascript
db.ciutat.aggregate([
  { $match: { port: "Si" } },
  { $group: { _id: "$port", Mitja: { $avg: "$nHabitants" } } },
  { $project: { _id: 0, Mitja: 1 } }
]);
```

### `JOIN` a MongoDB: `$lookup`

```javascript
db.ciutat.aggregate([
  {
    $lookup: {
      from: "empresa",
      localField: "nom",
      foreignField: "seuSocial",
      as: "dadesEmpresa"
    }
  },
  {
    $project: {
      _id: 0,
      nom: 1,
      "dadesEmpresa.sector": 1
    }
  }
]);
```

Parts que no pots oblidar:

- `from`
- `localField`
- `foreignField`
- `as`

### `$unwind`

Despres d'un `$lookup`, el resultat acostuma a ser un array.

Si vols tractar cada element de l'array com si fos una fila independent:

```javascript
{ $unwind: "$dadesEmpresa" }
```

### `$unionWith`

Equivalent conceptual de `UNION`:

```javascript
db.ciutat.aggregate([
  { $project: { _id: 0, nomciutat: "$nom" } },
  {
    $unionWith: {
      coll: "empresa",
      pipeline: [{ $project: { _id: 0, nomempresa: "$nom" } }]
    }
  }
]);
```

### Trampes frequents

- `aggregate` no funciona igual que `find`
- sense `_id:0`, MongoDB mostra `_id`
- si no hi ha filtre a `updateMany`, pots modificar tota la col.leccio
- `LIKE` es fa amb regex, no amb `%`
- no hi ha claus foranies enforced com a SQL

## Tema 3. Bases de dades distribuides

### Idea central

Una BDD distribuida es una sola base de dades repartida entre diversos nodes. Idealment, per a l'usuari hauria de funcionar com si no fos distribuida.

### Avantatges

- dades mes a prop d'on s'utilitzen
- millor disponibilitat
- millor fiabilitat
- paral.lelisme
- facilitat d'expansio

### Inconvenients

- mes complexitat
- consultes possiblement mes lentes per la xarxa
- problemes de consistencia i replicacio
- transaccions mes dificils

### Tipus

- `Top-Down` o homogenies: es dissenyen des de zero pensant la distribucio despres
- `Bottom-Up` o heterogenies: s'integra el que ja existeix

### Fragmentacio

#### Horitzontal

Divideix per files.

#### Vertical

Divideix per atributs.

La clau primaria ha d'apareixer a tots els fragments verticals.

#### Mixta

Combina horitzontal i vertical.

### Regles de correccio

Les tres paraules que mes poden caure:

- `completesa`
- `disjuntivitat`
- `reconstruccio`

Has de poder explicar-les sempre.

### Transparencia

#### Transparencia de fragmentacio

L'usuari consulta la taula global i no coneix els fragments.

#### Transparencia d'ubicacio

L'usuari coneix els fragments pero no sap en quin node estan.

#### Transparencia de SGBD local

L'usuari coneix fragments i nodes, pero el sistema unifica l'acces als SGBD locals.

Frase curta:

`com menys transparencia, mes feina fa l'usuari`

### Altres idees de teoria que val la pena saber

- replicacio de dades
- consistencia immediata vs consistencia diferida
- processador global i local de consultes
- protocol `2PC`

## Tema 4. noSQL general, XML i integracio

### Part A. noSQL en general

#### Idea central

`noSQL` vol dir `Not Only SQL`.

No es tracta de substituir sempre SQL, sino d'utilitzar estructures mes adequades per a dades molt grans, flexibles o distribuibles.

#### Caracteristiques

- model de dades flexible
- escalabilitat horitzontal
- alt rendiment
- alta disponibilitat
- consistencia eventual

#### Avantatges

- millor adaptacio a dades no estructurades o semiestructurades
- facil creixement afegint nodes
- consultes rapides
- bona tolerancia a errades

#### Inconvenients

- redundancia
- consistencia menys estricta
- model menys natural per a certes aplicacions transaccionals
- pot complicar el manteniment si el model es descontrola

#### Tipus de BBDD noSQL

- `clau-valor`
- `documentals`
- `grafs`
- `orientades a objectes`

### Part B. XML

#### Per a que serveix

XML serveix per integrar dades entre sistemes heterogenis. Es molt util per exportar dades relacionals a una estructura jerarquica portable.

#### Estructura basica

```xml
<?xml version="1.0" encoding="UTF-8"?>
<hotel>
  <usuari dni="11111111A">
    <client nom="Anna" />
  </usuari>
</hotel>
```

Concepte clau:

- hi ha un node arrel
- hi ha nodes pare i fills
- els atributs descriuen el node
- la informacio es jerarquica

#### Regles basiques de XML

- tot document ha de tenir node arrel
- tots els nodes s'han de tancar
- les etiquetes han d'estar ben niuades
- XML diferencia majuscules i minuscules
- els atributs han de tenir valor
- els valors van entre cometes

#### Caracters especials

- `<` -> `&lt;`
- `>` -> `&gt;`
- `"` -> `&quot;`
- `'` -> `&apos;`
- `&` -> `&amp;`

### ACT2 ex. 1: passar de SQL a XML

Segons el vostre enunciat:

- node arrel: `hotel`
- fills de l'arrel: `usuari`
- dintre de `usuari`: `client`
- dintre de `client`: `reserva`
- dintre de `reserva`: `habitacio`
- la informacio ha d'anar com a atributs, no com a subnodes de text

Per tant, la millor eina conceptual del temari es `FOR XML PATH`, perque es la que et deixa controlar:

- el nom dels nodes
- quins valors seran atributs
- l'arrel final

### `FOR XML RAW`, `AUTO`, `PATH`

#### `FOR XML RAW`

Cada fila del resultat es transforma en un node `<row ... />`.

Exemple:

```sql
SELECT avio.id_avio, aeroport.ciutat
FROM avio
INNER JOIN aeroport ON avio.aeroport = aeroport.nom
FOR XML RAW;
```

Tipus de sortida:

```xml
<row id_avio="AV1" ciutat="BCN" />
```

Quan t'interessa:

- sortida rapida
- exportacio simple
- no cal controlar la jerarquia

#### `FOR XML AUTO`

La jerarquia depen de l'ordre de les taules al `FROM` i al `JOIN`.

```sql
SELECT avio.id_avio, aeroport.ciutat
FROM avio
INNER JOIN aeroport ON avio.aeroport = aeroport.nom
FOR XML AUTO;
```

Tipus de sortida:

```xml
<avio id_avio="AV1">
  <aeroport ciutat="BCN" />
</avio>
```

Quan t'interessa:

- vols una jerarquia automatica
- no necessites gaire control sobre els noms

#### `FOR XML PATH`

Es la mes important per examen i per activitats.

Permet decidir:

- el nom del node
- quins camps seran atributs
- quins seran elements

Exemple:

```sql
SELECT avio.id_avio AS '@codi',
       aeroport.ciutat AS 'Situacio'
FROM avio
INNER JOIN aeroport ON avio.aeroport = aeroport.nom
FOR XML PATH('Flota');
```

Tipus de sortida:

```xml
<Flota codi="AV1">
  <Situacio>BCN</Situacio>
</Flota>
```

Regla clau:

- si l'alias comenca per `@`, genera atribut
- si no comenca per `@`, genera node/element

### Opcions addicionals

#### `ELEMENTS`

Converteix els camps en elements en lloc d'atributs:

```sql
SELECT avio.id_avio, aeroport.ciutat
FROM avio
INNER JOIN aeroport ON avio.aeroport = aeroport.nom
FOR XML RAW, ELEMENTS;
```

#### `ROOT('nom')`

Afegeix el node arrel:

```sql
SELECT avio.id_avio, aeroport.ciutat
FROM avio
INNER JOIN aeroport ON avio.aeroport = aeroport.nom
FOR XML AUTO, ROOT('avions');
```

### Com construir XML jerarquic com el de l'ACT2

Per a un arbre del tipus:

`hotel -> usuari -> client -> reserva -> habitacio`

la idea practica es:

1. consulta exterior per `usuari`
2. subconsulta XML per `client`
3. dintre, una altra per `reserva`
4. dintre, una altra per `habitacio`
5. al final, `ROOT('hotel')`

Esquema orientatiu:

```sql
SELECT
    u.dni AS '@dni',
    u.nom AS '@nom',
    (
        SELECT
            c.telefon AS '@telefon',
            (
                SELECT
                    r.numReserva AS '@numReserva',
                    r.estat AS '@estat',
                    (
                        SELECT
                            h.codi AS '@codi',
                            h.tipus AS '@tipus'
                        FOR XML PATH('habitacio'), TYPE
                    )
                FOR XML PATH('reserva'), TYPE
            )
        FOR XML PATH('client'), TYPE
    )
FROM usuari u
FOR XML PATH('usuari'), ROOT('hotel');
```

Nota important:

- als apunts la versio mes habitual que surt es `1.0` amb `UTF-8`
- al vostre `ACT2` l'enunciat demana explicitament `XML 2.0` i `UTF-8`

Si et pregunten per l'activitat, respon com diu l'enunciat. Si et pregunten teoria general, als apunts la versio tipica es `1.0`.

### XPath

#### Idea central

XPath serveix per navegar per l'arbre XML i seleccionar nodes o atributs.

#### Expressio basica

- `//node` -> busca nodes
- `@atribut` -> selecciona atributs
- `[condicio]` -> filtra
- `..` -> puja al node pare

#### Funcions i patrons importants

- `count(...)`
- `sum(...)`
- `avg(...)`
- `max(...)`
- `min(...)`
- `distinct-values(...)`
- `contains(...)`

Exemples:

```xpath
//habitacio/@codi
```

```xpath
count(//reserva)
```

```xpath
distinct-values(//habitacio[@tipus="doble"]/@codi)
```

```xpath
//reserva[@estat="realitzada"]/@numReserva
```

```xpath
avg(//reserva[@estat="realitzada"]/@preuTotal)
```

### Com pensar una consulta XPath

Patro mental:

1. situa't al node on hi ha la condicio
2. aplica la condicio
3. selecciona els atributs que vols
4. si cal, puja o baixa per l'arbre

Exemple:

Si vols "els identificadors dels avions aterrats a aeroports de Paris", pots:

1. trobar la ciutat `PAR`
2. pujar fins a `avio`
3. seleccionar `@id_avio`

### Trampes frequents

- XML es jerarquic; SQL no
- `distinct-values()` sovint cal per evitar repeticions
- `@att = (v1, v2, v3)` fa el paper d'un `IN`
- `contains()` fa el paper d'un `LIKE`
- el resultat pot repetir-se si l'XML ja porta dades replicades per jerarquia

## Preguntes teoriques probables

Si controles aquestes, vas be:

- Quina diferencia hi ha entre `view`, `function`, `procedure` i `trigger`?
- Quins avantatges i inconvenients te una BD documental?
- Com es passa d'una taula SQL a una col.leccio MongoDB?
- Quan faries servir `find` i quan `aggregate`?
- Quina funcio fa `$lookup`? I `$unwind`?
- Que es `FOR XML RAW`, `AUTO` i `PATH`?
- Per que `PATH` es el mes util per construir XML personalitzat?
- Quines son les regles basiques de XML?
- Quines son les diferencies entre fragmentacio horitzontal i vertical?
- Quines son les 3 regles de correccio de la fragmentacio?
- Quina diferencia hi ha entre transparencia de fragmentacio i d'ubicacio?

## Mini-xuleta final

- `VIEW` = consulta guardada
- `FUNCTION` = retorna valor o taula
- `PROCEDURE` = fa accions
- `TRIGGER` = reacciona automaticament
- SQL fila -> Mongo document
- SQL taula -> Mongo collection
- `find` = consulta normal
- `aggregate` = agrupacions, calculs i unions
- `RAW` = `<row .../>`
- `AUTO` = jerarquia automatica
- `PATH` = control total
- `@alias` a `PATH` = atribut XML
- `distinct-values()` = sense repetits
- fragmentacio: `completesa`, `disjuntivitat`, `reconstruccio`

## Nota sobre les solucions

A l'exercici 11 de l'extensio procedimental he interpretat que es refereix al cas d'actualitzar l'aeroport d'un avio, per no duplicar exactament el mateix trigger de l'exercici 9.
