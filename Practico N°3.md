# Arquitectura de Base de Datos: Práctico N°3

### A través de este texto, vamos a definir y crear un DER. El texto es el siguiente: 
Me contrataron para construir un sistema de base de datos para la biblioteca de mi escuela técnica, la biblioteca consta de una x cantidad de libros ordenados por tema y autor, también se realizan las búsquedas por editorial, la biblioteca quiere que la base de datos muestre como resultado los libros que están deteriorados, no devueltos y no encontrados, también quiere saber que socios están debiendo cuotas societarias y libros prestados, por último, la biblioteca quiere saber cuantos libros tiene en su totalidad tanto en el catálogo como los libros que se encuentran prestados.

---
### 1. Diagrama Entidad-Relación (DER)
![Alt](https://github.com/mvvillanueva1706-lgtm/Practico-N3/blob/main/Biblioteca.drawio.png)

---
### 2. Normalización de la Tabla

1. Primera Forma Normal (1FN): Se eliminan los datos que no son atomicos. Cada columna contiene un solo valor y existe una clave primaria definida para cada registro.

2. Segunda Forma Normal (2FN): Se eliminan las dependencias parciales. Todos los atributos que no son clave dependen totalmente de la clave primaria completa, por eso las relaciones N:M se dividen en las tablas `autorLibro` y `detallePrestamo`.

3. Tercera Forma Normal (3FN): Se eliminan las dependencias transitivas. Los temas, editoriales y autores se extraen a sus propias tablas independientes (`Tema`, `Editorial`, `Autor`) evitando redundancia al repetirse en varios libros. 

---
### 3. Insertar datos en las tablas

  <!-- Tabla Tema) -->
<table>
  <tr>
    <th colspan="3" align="center">Tema</th>
  </tr>
  <tr>
    <td><b>id_tema</b></td>
    <td><b>nombreTema</b></td>
  </tr>
  <tr>
    <td>1</td>
    <td>Programación e Informática</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Electrónica y Circuitos</td>
  </tr>
  <tr>
    <td>3</td>
    <td>Mecánica Técnica</td>
  </tr>
</table>

  <!-- Tabla Editorial -->
<table>
  <tr>
    <th colspan="3" align="center">Editorial</th>
  </tr>
  <tr>
    <td><b>id_editorial</b></td>
    <td><b>nombreEditorial</b></td>
  </tr>
  <tr>
    <td>1</td>
    <td>Alfaomega</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Pearson</td>
  </tr>
  <tr>
    <td>3</td>
    <td>McGraw-Hill</td>
  </tr>
  <tr>
    <td>4</td>
    <td>Marcombo</td>
  </tr>
</table>

<!-- TABLA 1 -->
<table style="display: inline-table; margin-right: 20px;">
  <tr><th colspan="2" align="center">Tabla Uno</th></tr>
  <tr><td>Columna 1</td><td>Columna 2</td></tr>
  <tr><td>Dato A</td><td>Dato B</td></tr>
</table>

<!-- TABLA 2 -->
<table style="display: inline-table;">
  <tr><th colspan="2" align="center">Tabla Dos</th></tr>
  <tr><td>Columna 3</td><td>Columna 4</td></tr>
  <tr><td>Dato C</td><td>Dato D</td></tr>
</table>

### Tabla: Autor

| **id_autor** | **nombreAutor** | 
| 1 | Robert C. Martin | 
| 2 | Robert L. Boylestad | 
| 3 | Albert P. Malvino | 
| 4 | Andrew S. Tanenbaum | 
| 5 | Shigley / Budynas | 

### Tabla: Libro

| **id_libro** | **Titulo** | **estadoLibro** | **id_tema** | **id_editorial** | 
| 1 | Clean Code: Manual de desarrollo ágil de software | Disponible | 1 | 1 | 
| 2 | Teoría de Circuitos y Dispositivos Electrónicos | Deteriorado | 2 | 2 | 
| 3 | Diseño en Ingeniería Mecánica de Shigley | Deteriorado | 3 | 3 | 
| 4 | Sistemas Operativos Modernos | Deteriorado | 1 | 2 | 
| 5 | Principios de Electrónica | No encontrado | 2 | 3 | 
| 6 | Redes de Computadoras | No encontrado | 1 | 2 | 
| 7 | El Programador Pragmático | Disponible | 1 | 1 | 
| 8 | Dispositivos Semiconductores: Principios y Modelos | Disponible | 2 | 1 | 
| 9 | Principios de Electricidad y Electrónica | Disponible | 2 | 4 | 
| 10 | Manual de Mecánica Industrial | Disponible | 3 | 4 | 

### Tabla: libroAutor

| **id_libro** | **id_autor** | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 5 | 
| 4 | 4 | 
| 5 | 3 | 
| 6 | 4 | 
| 7 | 1 | 
| 8 | 2 | 
| 9 | 3 | 
| 10 | 5 | 

### Tabla: Socio

| **id_socio** | **nombreSocio** | **Cuota** | 
| 1 | Juan Pérez | Al día | 
| 2 | María Gómez | Debiendo | 
| 3 | Carlos López | Al día | 
| 4 | Ana Martínez | Debiendo | 
| 5 | Lucas Rodríguez | Al día | 
| 6 | Sofía Fernández | Debiendo | 
| 7 | Diego Torres | Al día | 
| 8 | Laura Díaz | Al día | 
| 9 | Gabriel Romero | Debiendo | 
| 10 | Valentina Silva | Al día | 

### Tabla: Prestamo

| **id_prestamo** | **estadoPrestamo** | **id_socio** | 
| 1 | No devuelto | 2 | 
| 2 | Devuelto | 1 | 
| 3 | No devuelto | 4 | 
| 4 | Devuelto | 3 | 
| 5 | No devuelto | 6 | 

### Tabla: detallePrestamo

| **id_detalle** | **id_libro** | **id_prestamo** | 
| 1 | 1 | 1 | 
| 2 | 7 | 2 | 
| 3 | 8 | 3 | 
| 4 | 9 | 4 | 
| 5 | 10 | 5 | 

## 4. Consultas SQL con JOIN

### a) Libros deteriorados y no encontrados

```
SELECT id_libro, Titulo, estadoLibro 
FROM Libro 
WHERE estadoLibro IN ('Deteriorado', 'No encontrado');

```

### b) Libros prestados que no han sido devueltos

```
SELECT L.id_libro, L.Titulo, P.id_prestamo, S.nombreSocio, P.estadoPrestamo
FROM Libro L
JOIN detallePrestamo DP ON L.id_libro = DP.id_libro
JOIN Prestamo P ON DP.id_prestamo = P.id_prestamo
JOIN Socio S ON P.id_socio = S.id_socio
WHERE P.estadoPrestamo = 'No devuelto';

```

### c) Socios que deben cuotas societarias o libros prestados

```
SELECT DISTINCT S.id_socio, S.nombreSocio, S.Cuota, P.estadoPrestamo
FROM Socio S
LEFT JOIN Prestamo P ON S.id_socio = P.id_socio
WHERE S.Cuota = 'Debiendo' OR P.estadoPrestamo = 'No devuelto';

```

### d) Cantidad total de libros en catálogo y prestados

```
SELECT 
    (SELECT COUNT(*) FROM Libro) AS total_catalogo,
    (SELECT COUNT(DISTINCT id_libro) FROM detallePrestamo DP 
     JOIN Prestamo P ON DP.id_prestamo = P.id_prestamo 
     WHERE P.estadoPrestamo = 'No devuelto') AS total_prestados;

```
