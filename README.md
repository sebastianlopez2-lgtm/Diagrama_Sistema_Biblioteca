# Diagrama-sistema-biblioteca
Un diagrama de clases UML constituye el modelo estructural de un sistema informático. Describe de forma rigurosa la anatomía del software al detallar sus clases, atributos, operaciones, multiplicidades y tipos de asociación, articulando la transición entre el análisis abstracto y la arquitectura de código en la programación orientada a objetos.


## Relaciones implementadas

- **Herencia:** `Usuario` → `UsuarioConPrestamos`, `Bibliotecario` | `Libro` → `LibroDigital`, `LibroFisico`
- **Composición:** `Usuario` *-- `Direccion`, `Prestamo` *-- `Libro`
- **Agregación:** `Autor` o-- `Libro`, `UsuarioConPrestamos` o-- `Prestamo`
- **Asociación:** `Prestamo` --> `Bibliotecario`
