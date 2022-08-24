# Tasques d'itinerari 1

## Exercici 1: A partir dels documents adjunts (estructura i dades), crea una base de dades amb MySQL. Mostra les característiques principals de l'esquema creat i explica les diferentes taules i variables que hi ha:

A continuació mostro les taules resultants:

### taula tb_genre:

La clau primària d'aquesta taula és genre_id
Les variables que hi ha són: genre_id que es codi del gènere, genre_name que és el nom del gènere, segons la taula ha de tenir màxim 40 caràcters i no pot ser null, created_by_user, l'usuari de creació, ha de tenir un màxim de 10 caràcters i tampoc pot ser null. Created_date, la data de creació, updated_date, la data d'actualització.

![image](https://user-images.githubusercontent.com/109982515/186440593-77f612ab-569a-4376-9f7d-6213cfeacb0d.png)

### taula tb_movie

la clau primària d'aquesta taula és movie_id també té una clau forànea genre_id referenciada a la taula tb_genre.
Les variables que trobem són:
movie_id, codi de la pel-lícula, movie_title: títol de la pel-lícula, no pot tenir més de 100 caràcters i no pot ser null, movie_date, data de la creació, movie_format, no pot tenir més de 50 caràcters, movie_genre,id el tipus de gènere codificat segons el codi de la taula tb_genre, created_by_user, usuari de creació, no pot tenir més de 10 caràcters i no pot ser null.
Created_date data de creació del registre i updated_date: data d'actualització del registre.

![image](https://user-images.githubusercontent.com/109982515/186440623-013dc28d-b1f9-4fbf-9222-683d6484ab48.png)

### taula_movie_person

Aquesta taula té una clau primària composta per movie_id, person_id, role_id, aquests codis a la vegada son claus forànies de les seguents taules:
movie_id referencia a la taula tb_movie, person_id referencia a tb_person, role_id referencia a la taula tb_role
Les variables que componen la taula són:

movie_id, integer no pot ser nul, person_id, integer, no pot ser null, role_id, no pot ser null, movie_award_ind valor char, no pot ser null i només pot tenir un caràcter. Created_by user, usuari de creació, pot tenir un màxim de 10 caràcters, created_date, data de creació del registre, updated_date, data d'actualizació del registre.

![image](https://user-images.githubusercontent.com/109982515/186440677-76b7dd7d-ac6c-44ec-b46e-8efea6331b5f.png)

### taula tb_person

Aquesta taula té una clau primària, person_id i una clau forània person_parent_id que referencia la clau primària de la mateixa taula, perque hi ha persones que son filles d'altres persones que apareixen a la base de dades.
Les variables son:
person_id, integer, no pot ser null, person_name, el nom no pot tenir més de 100 caràcters ni pot ser null, person_country, nacionalitat de la persona, el caràcter no pot tenir més de 40 caràcters, person_dob, data de naixement, no pot ser null, person_dod, data de mort, pot ser null, person_parent id, valor integer, created_by_user, usuari de creació, no pot ser null i la default és 'OS_SGAD', created_date, data de creació del registre, updated_date, data d'actualització del registre.

![image](https://user-images.githubusercontent.com/109982515/186440782-b75a198d-849c-45f5-b57c-0008a689f126.png)
![image](https://user-images.githubusercontent.com/109982515/186440811-14ee4ca1-27dd-4e83-b8be-02b8368bb8da.png)

### taula tb_role

la clau primària d'aquesta taula és role_id.
Les variables són:
role_id valor integer que no pot ser null, role_name, nom del rol, no pot contenir més de 60 caràcters i no pot ser null, created_by_user, usuari de creació, no pot tenir més de 10 caràcters, created_date, usuari de creació, updated_date, data d'actualització.

![image](https://user-images.githubusercontent.com/109982515/186440873-9f507702-7b0d-4d2e-b984-26e861120c2f.png)

## Exercici 2: Has d'obtenir el nom, el país i la data de naixement d'aquelles persones per les quals no consti una data de mort i ordenar les dades de la persona més vella a la persona més jove.

SELECT person_name,person_country,person_dob

FROM tb_person

WHERE person_dod IS NULL

ORDER BY person_dob ASC;

![image](https://user-images.githubusercontent.com/109982515/186436136-e0ea090b-8887-4987-be65-cb705dc382e8.png)
![image](https://user-images.githubusercontent.com/109982515/186436183-ad45f880-0b35-4ac6-a4b0-bab9c2f5dfbd.png)

## Exercici 3: Has d'obtenir el nom del gènere i el nombre total de pel-lícules d'aquest gènere i ordenar-ho per ordre descendent de nombre total de pel-lícules.

SELECT genre_name, COUNT(movie_genre_id) AS PELICULAS 

FROM tb_movie

LEFT JOIN tb_genre

ON tb_movie.movie_genre_id = tb_genre.genre_id

GROUP BY movie_genre_id

ORDER BY PELICULAS DESC;

![image](https://user-images.githubusercontent.com/109982515/186437250-ee5ab004-75bf-4c69-8deb-dd932156b523.png)

## Exercici 4: Has d'obtenir, per a cada persona, el seu nom i el nombre màxim de rols diferents que ha tingut en una mateixa pel-lícula.

SELECT tb_person.person_name, tb_movie.movie_title,  COUNT(tb_role.role_id) AS ROLES

FROM tb_movie_person

JOIN tb_person

ON tb_person.person_id = tb_movie_person.person_id

JOIN tb_role

ON tb_role.role_id = tb_movie_person.role_id

JOIN tb_movie

ON tb_movie.movie_id = tb_movie_person.movie_id

GROUP BY tb_person.person_id,tb_movie.movie_id;

![image](https://user-images.githubusercontent.com/109982515/186437660-aa64f05e-b127-4943-a4cd-12b08b76e853.png)
![image](https://user-images.githubusercontent.com/109982515/186437681-0c8f7898-366e-40e7-921b-aa4b736a2e86.png)

## Posteriorment, mostra únicament aquelles persones que hagin assumit més d'un rol en una mateixa pel-lícula

SELECT tb_person.person_name, tb_movie.movie_title, COUNT(tb_role.role_id) AS ROLES 

FROM tb_movie_person 

JOIN tb_person

ON tb_person.person_id = tb_movie_person.person_id

JOIN tb_role

ON tb_role.role_id = tb_movie_person.role_id

JOIN tb_movie

ON tb_movie.movie_id = tb_movie_person.movie_id

GROUP BY tb_person.person_id,tb_movie.movie_id

HAVING COUNT(tb_role.role_id) > 1;

![image](https://user-images.githubusercontent.com/109982515/186438488-740380ba-4ffe-44c1-9c69-ef3e0188a54d.png)

## Exercici 5: Has de crear un nou gènere anomenat "Documental" el qual tingui com a identificador el nombre 69.

INSERT INTO tb_genre (genre_id, genre_name)

VALUES (69,'Documental');

Com es veu la taula resultant:

![image](https://user-images.githubusercontent.com/109982515/186438741-3acd9bd5-0c1c-475a-b1f5-c8b8d45a0d0e.png)

## Exercici 6: Elimina la pel-lícula "La Gran Familia Española" de la base de dades.

Abans de poder eliminar-la he de canviar la configuració de la clau forànea, si intento canviar-ho sense canviar-la surt un missatge d'error

![image](https://user-images.githubusercontent.com/109982515/186440181-42cd7a81-3b23-4de1-817e-80ecd8781fe3.png)

un cop fet ja puc eliminar amb el seguent codi:

DELETE FROM tb_movie WHERE tb_movie.movie_id = 11;

i el resultat:

![image](https://user-images.githubusercontent.com/109982515/186440338-c0b07d62-1264-4cd2-b2b0-edacbf0167e2.png)

## Exercici 7: Canvia el gènere de la pel-lícula "Ocho apellidos catalanes" perquè consti com a comèdia i no com a romàntica.

Com s'observa abans de fer el canvi:

![image](https://user-images.githubusercontent.com/109982515/186439219-e6370215-841c-4508-9886-583b17dd720b.png)

UPDATE tb_movie

SET movie_genre_id = 3

WHERE movie_id = 9;

Com s'observa un cop fet el canvi:

![image](https://user-images.githubusercontent.com/109982515/186439311-0f91cdf3-614b-46d0-9ebf-cc56e708ec91.png)




