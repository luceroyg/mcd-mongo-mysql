# Practica Modelo Relacional.

La siguiente practica evalua su conocimiento de consultas con Modelo Relacional.

## Como enviar sus respuesta.

1. Debe crear este archivo en su `fork` con el mismo nombre.
2. Debe copiar el contenido.
3. Debe comenzar a resolver las consultas.
4. Debe guardar el progreso en cada paso, puede cambiar la respuesta las veces que quiera, pero debe guardar el progreso cada vez. (Muy importante).

## Guardar progreso.

Para guardar su progreso cada vez que termine un ejercicio, o cambie una consulta. Ejecuta lo siguiente en un nuevo terminal:

```bash
git add .
git commit -m "Enviando intento"
git push
```

## Ejercicios:

1. Listar la cantidad de tiendas por ciudad.

Salida:
```
+---------+------------+-------------+
| city_id | city       | store_count |
+---------+------------+-------------+
|     300 | Lethbridge |           1 |
|     576 | Woodridge  |           1 |
+---------+------------+-------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT city.city_id as city_id, city.city, count(store_id) as store_count
FROM store,address,city
WHERE store.address_id = address. address_id
AND address.city_id = city.city_id
GROUP BY city.city_id;

```

2. Listar la cantidad de películas que se hicieron por lenguaje.

Salida:
```
+-------------+----------+------------+
| language_id | language | film_count |
+-------------+----------+------------+
|           1 | English  |       1000 |
|           2 | Italian  |          0 |
|           3 | Japanese |          0 |
|           4 | Mandarin |          0 |
|           5 | French   |          0 |
|           6 | German   |          0 |
+-------------+----------+------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT language.language_id as language_id, language.name as language, count(film.film_id) as film_count
FROM language
LEFT JOIN film ON language.language_id = film.language_id
GROUP BY language_id;

```

3.  Seleccionar todos los actores que participaron mas de 35 peliculas.

Salida:

```
+----------+------------+-----------+------------+
| actor_id | first_name | last_name | film_count |
+----------+------------+-----------+------------+
|       23 | SANDRA     | KILMER    |         37 |
|       81 | SCARLETT   | DAMON     |         36 |
|      102 | WALTER     | TORN      |         41 |
|      107 | GINA       | DEGENERES |         42 |
|      181 | MATTHEW    | CARREY    |         39 |
|      198 | MARY       | KEITEL    |         40 |
+----------+------------+-----------+------------+
6 rows in set (0.01 sec)
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT a.actor_id, a.first_name, a.last_name, count(film_actor.film_id) as film_count
FROM actor a, film_actor, film
WHERE a.actor_id = film_actor.actor_id
AND film.film_id = film_actor.film_id
GROUP BY film_actor.actor_id
HAVING film_count > 35;

```

4. Mostrar el listado de los 10 de actores que mas peliculas realizó en la categoria `Comedy`.

Salida:
```
+----------+------------+-----------+-------------------+
| actor_id | first_name | last_name | comedy_film_count |
+----------+------------+-----------+-------------------+
|      196 | BELA       | WALKEN    |                 6 |
|      143 | RIVER      | DEAN      |                 5 |
|      149 | RUSSELL    | TEMPLE    |                 5 |
|       82 | WOODY      | JOLIE     |                 4 |
|      127 | KEVIN      | GARLAND   |                 4 |
|       58 | CHRISTIAN  | AKROYD    |                 4 |
|       24 | CAMERON    | STREEP    |                 4 |
|      129 | DARYL      | CRAWFORD  |                 4 |
|       83 | BEN        | WILLIS    |                 4 |
|       37 | VAL        | BOLGER    |                 4 |
+----------+------------+-----------+-------------------+
```

Respuesta:
```sql
-- Su respuesta aqui:

SELECT a.actor_id, a.first_name, a.last_name, count(film_actor.film_id) as film_count
FROM actor a, film_actor, film, film_category, category
WHERE a.actor_id = film_actor.actor_id
AND film.film_id = film_actor.film_id
AND film.film_id = film_category.film_id
AND film_category.category_id = category.category_id
AND category.name = 'Comedy'
GROUP BY film_actor.actor_id
ORDER BY film_count DESC
LIMIT 10;

```

5. Obtener la lista de actores que NO han participado en ninguna película de categoría "Comedy":

Salida:
```
+----------+------------+-------------+
| actor_id | first_name | last_name   |
+----------+------------+-------------+
|        3 | ED         | CHASE       |
|        6 | BETTE      | NICHOLSON   |
|        7 | GRACE      | MOSTEL      |
|        8 | MATTHEW    | JOHANSSON   |
|        9 | JOE        | SWANK       |
... Se corta por longitud
|      189 | CUBA       | BIRCH       |
|      190 | AUDREY     | BAILEY      |
|      195 | JAYNE      | SILVERSTONE |
|      200 | THORA      | TEMPLE      |
+----------+------------+-------------+

53 filas total
```
Respuesta:
```sql
-- Su respuesta aqui:

SELECT a.actor_id, a.first_name, a.last_name
FROM actor a, film_actor b, film c
WHERE a.actor_id = b.actor_id
AND c.film_id = b.film_id
AND NOT EXISTS(SELECT * FROM film_actor x, film_category,category
WHERE x.film_id = film_category.film_id 
AND film_category.category_id = category.category_id
AND category.name = 'Comedy'
AND x.actor_id = b.actor_id)
GROUP BY a.actor_id;
```

