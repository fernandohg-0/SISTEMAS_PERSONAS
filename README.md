# Sistema de Personas

Sitio con el menú en el navegador: [https://fernandohg-0.github.io/SISTEMAS_PERSONAS/](https://fernandohg-0.github.io/SISTEMAS_PERSONAS/)

Aplicación de consola en Java que administra personas en MySQL. Permite listar, agregar, buscar, actualizar y eliminar registros desde un menú en terminal, usando **JPA** con **Hibernate** y el patrón **DAO**.

La base de datos se llama `escuela` y la tabla `persona` guarda id, nombre, edad y correo. Si MySQL está en marcha y el usuario tiene permisos, Hibernate crea la base y la tabla al iniciar.

## Qué hace

Al ejecutarse, el programa se conecta a MySQL en `localhost:3306`, abre una unidad de persistencia JPA y muestra este menú:

1. Mostrar todas las personas
2. Agregar persona
3. Buscar persona por ID
4. Actualizar persona
5. Eliminar persona
0. Salir

Cada opción llama a la capa DAO. Esa capa traduce objetos Java en operaciones de base de datos (insertar, consultar, actualizar y eliminar) mediante `EntityManager`.

## Cómo está organizado

| Capa | Clase | Función |
| --- | --- | --- |
| Entidad | `modelo/Persona.java` | Representa la tabla `persona` con anotaciones JPA (`@Entity`, `@Id`, `@Column`). |
| Contrato DAO | `dao/PersonaDAO.java` | Define las operaciones CRUD sin detallar cómo se guardan. |
| Implementación DAO | `dao/impl/PersonaDAOImpl.java` | Ejecuta el CRUD con `EntityManager` y transacciones. |
| Conexión | `conexion/JPAUtil.java` | Crea y cierra el `EntityManagerFactory` hacia MySQL. |
| Configuración | `META-INF/persistence.xml` | Declara la unidad `SistemaPersonasPU` y a Hibernate como proveedor. |
| Interfaz | `prueba/PruebaPersona.java` | Menú de consola. Es la clase principal de Maven. |

Flujo de una operación:

```text
Menú (PruebaPersona)
        |
        v
PersonaDAO  -->  PersonaDAOImpl
                        |
                        v
              EntityManager (JPA / Hibernate)
                        |
                        v
                 MySQL: escuela.persona
```

## Requisitos

- Java 17
- Maven
- MySQL en `localhost`, puerto `3306`
- Usuario y contraseña configurados en `PruebaPersona.java` (por defecto `root` / `root`)

## Cómo ejecutarlo

1. Enciende MySQL.
2. Si hace falta, cambia usuario y contraseña en `src/main/java/mx/edu/tesoem/sistemapersonas/prueba/PruebaPersona.java`.
3. Desde la carpeta del proyecto:

```bash
mvn compile exec:java
```

También se puede abrir como proyecto Maven en NetBeans y ejecutarlo desde ahí.

La URL de conexión usa `createDatabaseIfNotExist=true` y Hibernate está en modo `update`, así que la base `escuela` y la tabla `persona` se crean o se actualizan al arrancar. El script `sql/escuela.sql` es opcional y sirve para crearlas a mano (por ejemplo, desde MySQL Workbench) si el usuario no tiene permiso para crear bases.

## Tecnologías

- Java 17
- Maven
- Jakarta Persistence (JPA 3.1)
- Hibernate 6
- MySQL Connector/J

## Sitio en GitHub Pages

`index.html` es el frente del menú de la terminal: mostrar, agregar, buscar, actualizar y eliminar. GitHub Pages no puede ejecutar Java ni MySQL, así que esa página guarda las personas en el navegador. El programa original sigue siendo la consola con `mvn compile exec:java`.
