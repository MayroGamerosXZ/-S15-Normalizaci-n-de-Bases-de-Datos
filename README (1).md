
# Normalización de Bases de Datos Relacionales hasta 5NF

## Objetivo
Aplicar los principios de normalización de bases de datos relacionales hasta la Quinta Forma Normal (5NF), estructurando la información de manera que los datos sean consistentes, no redundantes y fáciles de mantener.

## Contexto
Durante la clase revisamos la importancia de la normalización y las cinco primeras formas normales (1NF a 5NF). En esta tarea pondrás en práctica esos conceptos trabajando con un caso realista que requiere analizar y reorganizar datos para mejorar su estructura.
Tablas del esquema (lista general)
Persona (persona_id, nombre, apellido, calle, numero, ciudad_id, codigo_postal)
Telefonos (telefono_id, persona_id, numero)
Producto (producto_id, nombre, descripcion, precio_unitario)
Venta (venta_id, fecha, cliente_id, total)
VentaDetalle (venta_id, producto_id, cantidad) -- PK compuesta: (venta_id, producto_id)
Cliente (cliente_id, nombre, ciudad_id)
Ciudad (ciudad_id, nombre, pais_id)
Pais (pais_id, nombre)
Empleado (empleado_id, nombre, departamento_id)
Departamento (departamento_id, nombre, jefe_id)
Aula (aula_id, nombre, profesor_id)
Curso (curso_id, nombre, creditos)
CursoAula (curso_id, aula_id)

## Ejercicio práctico
Una universidad necesita registrar información sobre los cursos que imparte, los profesores que los enseñan y los estudiantes que los inscriben. Actualmente, los datos se guardan en una única tabla llamada `Registro_Cursos` con la siguiente estructura:

| ID_Registro | Curso            | Profesor       | Email_Profesor     | Estudiantes               | Emails_Estudiantes                                | Créditos | Facultad    |
|-------------|------------------|----------------|---------------------|----------------------------|---------------------------------------------------|----------|-------------|
| 1           | Bases de Datos   | Ana Torres     | ana@uni.edu         | Luis, María, Jorge         | luis@uni.edu, maria@uni.edu, jorge@uni.edu        | 4        | Ingeniería  |
| 2           | Programación Web | Carlos López   | carlos@uni.edu      | Pedro, Ana, Lucía          | pedro@uni.edu, ana@uni.edu, lucia@uni.edu         | 5        | Ingeniería  |

---

## Primera Forma Normal (1NF)
Una tabla está en 1NF si todos los atributos contienen valores atómicos. Se deben eliminar los grupos repetitivos.

Qué exige:

Atributos atómicos y valores únicos por celda.
Tablas y cambios aplicados:

Persona
Se separó la dirección compuesta: calle, numero, ciudad_id, codigo_postal.
Se eliminó cualquier columna con múltiples valores (ej. telefonos).
Telefonos
Nueva tabla para almacenar números atómicos: (telefono_id, persona_id, numero).
Producto, Venta, Cliente, Empleado, Curso
Verificación y separación de campos que contuvieran listas o valores repetidos en una celda.
Comentarios:

Resultado: cada tabla contiene atributos atómicos; las filas están identificadas por claves primarias (ej. persona_id, producto_id).

### Estructura resultante:

| ID_Registro | Curso            | Profesor       | Email_Profesor     | Estudiante | Email_Estudiante     | Créditos | Facultad    |
|-------------|------------------|----------------|---------------------|------------|-----------------------|----------|-------------|
| 1           | Bases de Datos   | Ana Torres     | ana@uni.edu         | Luis       | luis@uni.edu          | 4        | Ingeniería  |
| 1           | Bases de Datos   | Ana Torres     | ana@uni.edu         | María      | maria@uni.edu         | 4        | Ingeniería  |
| 1           | Bases de Datos   | Ana Torres     | ana@uni.edu         | Jorge      | jorge@uni.edu         | 4        | Ingeniería  |
| 2           | Programación Web | Carlos López   | carlos@uni.edu      | Pedro      | pedro@uni.edu         | 5        | Ingeniería  |
| 2           | Programación Web | Carlos López   | carlos@uni.edu      | Ana        | ana@uni.edu           | 5        | Ingeniería  |
| 2           | Programación Web | Carlos López   | carlos@uni.edu      | Lucía      | lucia@uni.edu         | 5        | Ingeniería  |


---

## Segunda Forma Normal (2NF)
Una tabla está en 2NF si está en 1NF y todos los atributos no clave dependen completamente de la clave primaria. Se eliminan dependencias parciales separando la información en múltiples tablas.

Qué exige:

Estar en 1NF.
No debe haber dependencia parcial respecto a una PK compuesta.
Tablas y cambios aplicados:

VentaDetalle (venta_id, producto_id, cantidad) — PK compuesta: (venta_id, producto_id)
Extracción de atributos dependientes sólo de producto_id:
Si existía precio_unitario o descripcion_producto en VentaDetalle, se movieron a la tabla Producto.
VentaDetalle queda con los campos propios de la relación venta-producto (ej. cantidad).
Producto
Contiene precio_unitario, descripcion y demás atributos que dependen únicamente de producto_id.
Otras PK compuestas (si existieran)
Se revisaron y se movieron atributos a tablas independientes cuando dependían solo de una parte de la PK.
Comentarios:

Evita redundancia como repetir el precio del producto por cada línea de venta.


### Tablas resultantes:

**Tabla Cursos**

| ID_Curso | Curso            | Créditos | Facultad    |
|----------|------------------|----------|-------------|
| 1        | Bases de Datos   | 4        | Ingeniería  |
| 2        | Programación Web | 5        | Ingeniería  |

**Tabla Profesores**

| ID_Profesor | Nombre         | Email           |
|-------------|----------------|------------------|
| 1           | Ana Torres     | ana@uni.edu      |
| 2           | Carlos López   | carlos@uni.edu   |

**Tabla Estudiantes**

| ID_Estudiante | Nombre | Email           |
|----------------|--------|------------------|
| 1              | Luis   | luis@uni.edu     |
| 2              | María  | maria@uni.edu    |
| 3              | Jorge  | jorge@uni.edu    |
| 4              | Pedro  | pedro@uni.edu    |
| 5              | Ana    | ana@uni.edu      |
| 6              | Lucía  | lucia@uni.edu    |

**Tabla Registro_Cursos**

| ID_Registro | ID_Curso | ID_Profesor |
|-------------|----------|-------------|
| 1           | 1        | 1           |
| 2           | 2        | 2           |

**Tabla Inscripciones**

| ID_Registro | ID_Estudiante |
|-------------|----------------|
| 1           | 1              |
| 1           | 2              |
| 1           | 3              |
| 2           | 4              |
| 2           | 5              |
| 2           | 6              |

---

## Tercera Forma Normal (3NF)
Una tabla está en 3NF si está en 2NF y no existen dependencias transitivas. La facultad depende del curso, no del registro, por lo que se separa en una tabla aparte.

Qué exige:

Estar en 2NF.
Eliminar dependencias transitivas (atributos no clave que dependen de otros atributos no clave).
Tablas y cambios aplicados:

Cliente
Antes: Cliente tenía ciudad y pais como columnas de texto.
Después: Cliente (cliente_id, nombre, ciudad_id) → referencia a Ciudad.
Ciudad
Ciudad (ciudad_id, nombre, pais_id) → referencia a Pais.
Pais
Pais (pais_id, nombre).
Empleado
Antes: Empleado podía contener departamento_nombre y departamento_jefe.
Después: Empleado (empleado_id, nombre, departamento_id) → referencia a Departamento.
Departamento
Departamento (departamento_id, nombre, jefe_id).
Persona
Validación de dependencias transitivas (ej. codigo_postal depende de ciudad_id) y normalización si correspondía.
Comentarios:

Se eliminaron dependencias transitivas moviendo datos a tablas propias (Ciudad, Pais, Departamento), dejando solo claves foráneas en las tablas principales.

**Tabla Facultad**

| ID_Facultad | Nombre      |
|-------------|-------------|
| 1           | Ingeniería  |

**Tabla Cursos (actualizada)**

| ID_Curso | Curso            | Créditos | ID_Facultad |
|----------|------------------|----------|-------------|
| 1        | Bases de Datos   | 4        | 1           |
| 2        | Programación Web | 5        | 1           |

---

## Cuarta Forma Normal (4NF)
Una tabla está en 4NF si está en 3NF y no contiene dependencias multivaluadas. Si un curso puede tener múltiples profesores y múltiples estudiantes independientemente, se deben separar en relaciones distintas.

Qué exige:

Para toda dependencia funcional X -> Y, X debe ser una clave candidata.
Tablas y cambios aplicados (ejemplos concretos del esquema):

Aula / CursoAula / Curso
Problema detectado: si existía la regla de negocio "aula determina profesor" (aula -> profesor_id) y había una tabla combinada con curso, profesor y aula, surgía una dependencia donde aula no era clave.
Solución:
Aula (aula_id, nombre, profesor_id) — ahora aula_id determina profesor_id.
CursoAula (curso_id, aula_id) — relación entre Curso y Aula.
Revisión general
Se revisaron tablas donde un determinante no era clave candidata y se descompusieron para que el determinante resultara clave en su tabla.
Comentarios:

Con BCNF se eliminaron dependencias funcionales no deseadas que causaban anomalías en inserciones/actualizaciones.

**Tablas adicionales:**

- `Curso_Profesor (ID_Curso, ID_Profesor)`
- `Curso_Estudiante (ID_Curso, ID_Estudiante)`

---

## Quinta Forma Normal (5NF)
Una tabla está en 5NF si está en 4NF y no contiene uniones que introduzcan redundancia. Se asegura que todas las uniones posibles entre tablas no generen datos redundantes o inconsistentes.

**Ejemplo de tabla adicional:**

- `Evaluaciones (ID_Curso, ID_Estudiante, ID_Evaluacion, Calificación)`

---

## Conclusión
El modelo de datos evolucionó desde una tabla no normalizada hasta un conjunto de tablas en 5NF. Cada forma normal permitió eliminar redundancias, dependencias parciales, transitivas y multivaluadas, mejorando la integridad, consistencia y mantenimiento de los datos.
