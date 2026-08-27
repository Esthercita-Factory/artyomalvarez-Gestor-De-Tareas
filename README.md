Gestor de Tareas — Proyecto de práctica en C#

Proyecto de consola para practicar CRUD, POO, manejo de excepciones, LINQ, asincronismo y arquitectura en capas. Sin frameworks web todavía — solo consola + persistencia en archivo.

🎯 Temas que cubre (y solo estos)
CRUD (Crear, Leer, Actualizar, Eliminar)
Programación Orientada a Objetos
Manejo de excepciones
LINQ
Asincronismo (async/await, Task)
Arquitectura en capas
🏗️ Arquitectura

El proyecto se divide en 4 capas (4 proyectos dentro de la misma solución):

GestorTareas/
├── GestorTareas.Models/         → Las clases del problema (qué ES una tarea)
├── GestorTareas.Repositories/   → Acceso a datos (DÓNDE viven las tareas)
├── GestorTareas.Services/       → Lógica de negocio (qué se PUEDE HACER con las tareas)
└── GestorTareas.UI/             → El menú de consola (cómo el usuario INTERACTÚA)

Regla de dependencia: UI depende de Services, Services depende de Repositories, y Repositories depende de Models. Nunca al revés. Models no depende de nada.

GestorTareas.Models

Contiene las clases que representan el problema, sin lógica de negocio ni de acceso a datos.

Tarea — la entidad principal
Prioridad — enum (Baja, Media, Alta)
Excepciones propias (ej: TareaNoEncontradaException)
GestorTareas.Repositories

Contiene el acceso a los datos: cómo se guardan y se leen las tareas, sin decidir cuándo ni por qué se llaman.

Una interfaz (ej: ITareaRepository) con las operaciones CRUD contra los datos
Una implementación en memoria (List<Tarea>) para las primeras fases
Más adelante, una implementación que persiste a un archivo JSON de forma asíncrona
GestorTareas.Services

Contiene la lógica de negocio: las reglas, validaciones y consultas sobre las tareas. Usa un repositorio, nunca accede a los datos directamente.

Validaciones (ej: título no puede estar vacío) antes de delegar al repositorio
Métodos que usan LINQ para filtrar/ordenar/agrupar tareas
Orquesta las llamadas asíncronas al repositorio
GestorTareas.UI

El punto de entrada (Program.cs) y el menú de consola. Llama a Services, nunca implementa lógica de negocio ni de acceso a datos directamente acá.

🗓️ Plan por fases
Fase 1 — POO: los modelos
 Clase Tarea (Id, Titulo, Descripcion, Completada, FechaCreacion, Prioridad)
 Enum Prioridad
 Mini objetivo: instanciar una Tarea en Main y mostrarla por consola
Fase 2 — Arquitectura en capas + CRUD en memoria
 Separar en los 4 proyectos (Models, Repositories, Services, UI)
 ITareaRepository + implementación en memoria (List<Tarea>) en Repositories
 TareaService en Services con Crear/Leer/Actualizar/Eliminar, usando el repositorio
 Menú de consola en UI que use el servicio
 Mini objetivo: crear y listar tareas en memoria (sin guardar a disco todavía)
Fase 3 — Excepciones + LINQ
 Excepción propia TareaNoEncontradaException
 Validaciones (ej: título vacío) con try/catch en UI
 LINQ en Services: filtrar completadas/pendientes, ordenar por prioridad o fecha, contar por prioridad
 Mini objetivo: opción de menú "ver resumen" con estadísticas vía LINQ
Fase 4 — Asincronismo + persistencia
 El repositorio pasa a guardar/cargar como JSON con File.WriteAllTextAsync / File.ReadAllTextAsync
 Los métodos del repositorio son async Task, Services y UI los esperan con await
 Mini objetivo: cerrar y reabrir el programa, y que las tareas sigan ahí
Fase 5 — Pruebas
 Probar el flujo completo: crear, editar, eliminar, filtrar, cerrar, reabrir
✅ Cómo correr el proyecto
bash
git clone <url-de-este-repo>
cd GestorTareas
dotnet restore
dotnet run --project GestorTareas.UI
🤝 Cómo contribuir

Cada fase es una rama (feature/fase-1-models, feature/fase-2-capas, etc.) que se mergea a main cuando esa fase compila y cumple su mini objetivo. Commits en formato Conventional Commits (feat:, fix:, chore:).

Este README define el problema y la estructura — la implementación de cada archivo se resuelve en equipo, no está pre-armada acá.
