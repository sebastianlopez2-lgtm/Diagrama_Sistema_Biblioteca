# Diagrama-Sistema-Biblioteca
# Sistema de Biblioteca — Diagrama de clases (Actividad 1)
Un diagrama de clases UML constituye el modelo estructural de un sistema informático. Describe de forma rigurosa la anatomía del software al detallar sus clases, atributos, operaciones, multiplicidades y tipos de asociación, articulando la transición entre el análisis abstracto y la arquitectura de código en la programación orientada a objetos.

## Relaciones implementadas
- **Herencia:** `Usuario` → `UsuarioConPrestamos`, `Bibliotecario` | `Libro` → `LibroDigital`, `LibroFisico`
- **Composición:** `Usuario` *-- `Direccion`, `Prestamo` *-- `Libro`
- **Agregación:** `Autor` o-- `Libro`, `UsuarioConPrestamos` o-- `Prestamo`
- **Asociación:** `Prestamo` --> `Bibliotecario`

# Sistema de Biblioteca — Implementación en Java (Actividad 2)

Implementación en Java del diagrama de clases UML diseñado en la Actividad 1
(ver `diagrama.puml` / `Diadrama.PNG` en el repositorio
[Diagrama_Sistema_Biblioteca](https://github.com/mariaalvarez82-wq/Diagrama_Sistema_Biblioteca)),
aplicando los principios de la Programación Orientada a Objetos y buenas
prácticas de diseño de software.

## Descripción del sistema

El sistema modela el préstamo de libros de una biblioteca: usuarios que
solicitan préstamos, bibliotecarios que los registran, autores que escriben
libros (físicos o digitales), y el préstamo en sí, que vincula a todos ellos.

## Estructura del proyecto

```
biblioteca-sistema/
├── README.md
└── src/
    └── com/
        └── biblioteca/
            ├── modelo/
            │   ├── Direccion.java
            │   ├── Usuario.java              (abstracta)
            │   ├── UsuarioConPrestamos.java  (extends Usuario)
            │   ├── Bibliotecario.java        (extends Usuario)
            │   ├── Autor.java
            │   ├── Prestable.java            (interfaz)
            │   ├── Libro.java                (abstracta, implements Prestable)
            │   ├── LibroDigital.java         (extends Libro)
            │   ├── LibroFisico.java          (extends Libro)
            │   └── Prestamo.java
            └── app/
                └── Main.java                  (clase de prueba)
```

## Cómo compilar y ejecutar

```bash
cd biblioteca-sistema
javac -encoding UTF-8 -d bin $(find src -name "*.java")
java -cp bin com.biblioteca.app.Main
```

## Traducción del diagrama de clases a código

| Elemento UML | Implementación en Java |
|---|---|
| Herencia `Usuario <|-- UsuarioConPrestamos` | `class UsuarioConPrestamos extends Usuario` |
| Herencia `Usuario <|-- Bibliotecario` | `class Bibliotecario extends Usuario` |
| Herencia `Libro <|-- LibroDigital` | `class LibroDigital extends Libro` |
| Herencia `Libro <|-- LibroFisico` | `class LibroFisico extends Libro` |
| Composición `Usuario "1" *-- "1" Direccion` | atributo `private Direccion direccion` en `Usuario`, inicializado por constructor |
| Composición `Prestamo "1" *-- "1" Libro` | atributo `private Libro libro` en `Prestamo` |
| Agregación `Autor "1" o-- "0..*" Libro` | `private List<Libro> libros` en `Autor`, con `agregarLibro(...)` |
| Agregación `UsuarioConPrestamos "1" o-- "0..*" Prestamo` | `private List<Prestamo> prestamos`, con `agregarPrestamo(...)` |
| Asociación `Prestamo "0..*" --> "1" Bibliotecario` | atributo `private Bibliotecario bibliotecario` en `Prestamo` |

## Pilares de la POO aplicados

- **Abstracción**: `Usuario` y `Libro` son clases `abstract` que solo
  definen lo común a sus especializaciones; nunca se instancian directamente.
- **Encapsulamiento**: todos los atributos son `private`/`protected` y se
  exponen mediante getters/setters; el estado `disponible` de `Libro` solo
  cambia a través de los métodos de negocio `prestar()`/`devolver()`.
- **Herencia**: `UsuarioConPrestamos`/`Bibliotecario` heredan de `Usuario`;
  `LibroDigital`/`LibroFisico` heredan de `Libro`.
- **Polimorfismo**: una `List<Libro>` puede contener objetos `LibroDigital`
  y `LibroFisico` a la vez; al invocar `obtenerInstruccionesAcceso()` sobre
  cada elemento, cada uno ejecuta su propia versión sobrescrita (ver `Main`).
  - **Sobrescritura (`@Override`)**: `obtenerRolDescripcion()` en
    `UsuarioConPrestamos`/`Bibliotecario`; `obtenerInstruccionesAcceso()` en
    `LibroDigital`/`LibroFisico`.
  - **Sobrecarga**: `Autor.agregarLibro(Libro)` / `agregarLibro(List<Libro>)`;
    constructor `Prestamo(fecha, libro, bibliotecario)` / `Prestamo(fechaPrestamo, fechaDevolucion, libro, bibliotecario)`.

## Principios SOLID aplicados

1. **SRP (Single Responsibility Principle)** — `Direccion`, `Autor` y
   `Prestamo` tienen una única responsabilidad cada una (representar una
   dirección, los datos de un autor, y la lógica de un préstamo,
   respectivamente); ninguna conoce detalles internos de las otras.
2. **OCP (Open/Closed Principle)** — `Usuario` y `Libro` están abiertas a
   extensión (se pueden agregar nuevos tipos, como `UsuarioVIP` o
   `LibroAudio`) sin modificar el código ya existente que trabaja con las
   referencias `Usuario`/`Libro`.
3. **LSP (Liskov Substitution Principle)** — en cualquier lugar donde se
   espera un `Libro`, se puede usar un `LibroDigital` o un `LibroFisico`
   sin romper el comportamiento del programa (ver el recorrido polimórfico
   en `Main.catalogo`); lo mismo aplica para `Usuario` con sus subclases.
4. **ISP (Interface Segregation Principle)** — la interfaz `Prestable` es
   pequeña y específica (`prestar`, `devolver`, `isDisponible`), en vez de
   forzar a `Libro` a implementar métodos que no necesita.

