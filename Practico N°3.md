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

```sql
INSERT INTO Socio (id_socio, nombreSocio, Cuota) VALUES 
(1, 'Juan Pérez', 'Al día'),
(2, 'María Gómez', 'Debiendo'),
(3, 'Carlos López', 'Al día'),
(4, 'Ana Martínez', 'Debiendo'),
(5, 'Lucas Rodríguez', 'Al día'),
(6, 'Sofía Fernández', 'Debiendo'),
(7, 'Diego Torres', 'Al día'),
(8, 'Laura Díaz', 'Al día'),
(9, 'Gabriel Romero', 'Al día'),
(10, 'Valentina Silva', 'Al día');

INSERT INTO Libro (id_libro, Titulo, estadoLibro) VALUES 
(1, 'Clean Code: Manual de desarrollo ágil de software', 'Disponible'),
(2, 'Teoría de Circuitos y Dispositivos Electrónicos', 'Deteriorado'),
(3, 'Diseño en Ingeniería Mecánica de Shigley', 'Deteriorado'),
(4, 'Sistemas Operativos Modernos', 'Deteriorado'),
(5, 'Principios de Electrónica', 'No encontrado'),
(6, 'Redes de Computadoras', 'No encontrado'),
(7, 'El Programador Pragmático', 'Disponible'),
(8, 'Dispositivos Semiconductores: Principios y Modelos', 'Disponible'),
(9, 'Principios de Electricidad y Electrónica', 'Disponible'),
(10, 'Manual de Mecánica Industrial', 'Disponible');

INSERT INTO Prestamo (id_prestamo, estadoPrestamo, id_socio) VALUES 
(1, 'No devuelto', 2),
(2, 'Devuelto', 1),
(3, 'No devuelto', 4),
(4, 'Devuelto', 3),
(5, 'No devuelto', 6);
```

---
### 4. Consultas SQL con JOIN

1. Libros deteriorados y no encontrados

```sql
SELECT id_libro, Titulo, estadoLibro FROM Libro 
WHERE estadoLibro IN ('Deteriorado', 'No encontrado');
```

2. Libros prestados que no han sido devueltos

```sql
SELECT Libro.id_libro, Libro.Titulo, Prestamo.id_prestamo, Socio.nombreSocio, Prestamo.estadoPrestamo FROM Libro
INNER JOIN detallePrestamo ON Libro.id_libro = detallePrestamo.id_libro
INNER JOIN Prestamo ON detallePrestamo.id_prestamo = Prestamo.id_prestamo
INNER JOIN Socio ON Prestamo.id_socio = Socio.id_socio
WHERE Prestamo.estadoPrestamo = 'No devuelto';
```

3. Socios que deben cuotas societarias o libros prestados

```sql
SELECT Socio.id_socio, Socio.nombreSocio, Socio.Cuota, Prestamo.estadoPrestamo FROM Socio
LEFT JOIN Prestamo ON Socio.id_socio = Prestamo.id_socio
WHERE Socio.Cuota = 'Debiendo' OR Prestamo.estadoPrestamo = 'No devuelto';
```
