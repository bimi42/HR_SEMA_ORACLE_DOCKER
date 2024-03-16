Enterprise kicsit más
## 0.
https://container-registry.oracle.com oldalon Database alatt Enterprise verzió, Jobb oldalon be kell jelentkezni, és el kell fogadni a licenszet, ezután tudjuk letölteni az imaget
## 0.1.
`docker login https://container-registry.oracle.com`, itt kéri az Oracle oldalon megadott felhasználónevet és jelszót, ezután tudjuk futtatni az adatbázist lokálisan
## 1. 
Letöltés és regisztrálás, először el kell fogadni az enterprise verzióhoz a feltételeket

## 2. 
indítás `docker run -d -p 1521:1521 -e ORACLE_PWD=oracle --name practice-db container-registry.oracle.com/database/enterprise:21.3.0.0`

Ez kicsit több időt vesz majd igénybe

## 3. 
le kell tölteni a hivatalos GITHUB repóban lévő szkripteket (https://github.com/oracle-samples/db-sample-schemas/tree/main/human_resources) és be kell másolni a következő prancsokkal a futó konténerbe  

```
docker cp <<PATH TO FILE>>hr_code.sql practice-db:/home/oracle  
docker cp <<PATH TO FILE>>hr_create.sql practice-db:/home/oracle  
docker cp <<PATH TO FILE>>hr_install.sql practice-db:/home/oracle  
docker cp <<PATH TO FILE>>hr_populate.sql practice-db:/home/oracle  
```

## 4.
miután megvan `sqlplus / as sysdba` terminálba a konténeren belül, ha TNS hibára panaszkodik, akkor `export ORACLE_SID=<<SID_NEVE>>` ami nálunk, alapból ORCLCDB 

## 5.
`ALTER SESSION SET CONTAINER=ORCLPDB1;`
`SELECT name, pdb
FROM   v$services
ORDER BY name;` -> ebből megvan a pdb neve

## 6.
`@hr_install.sql`
Itt kér pár paramétert, HR-nek adjunk meg tetszőleges jelszavat, ezek után a default értékek jók lesznek (USERS és YES)

## 7. 
Ezután pl SQL Developerben be is tudunk jelentkezni a HR "adatbázisba"
| Name:               | HR                              |
|---------------------|---------------------------------|
| Authentication Type | Default                         |
| Username            | hr                              |
| Password            | HR User jelszava, amit megadtunk|
| Connection Type     | Basic                           |
| Hostname            | localhost                       |
| Port                | 1521                            |
| SID                 | nem kell kitölteni              |
| Service name        | ORCLPDB1                        |
