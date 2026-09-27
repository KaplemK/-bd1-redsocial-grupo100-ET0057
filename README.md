# Red Social Pascualina 

Base de datos relacional para la **Red Social Estudiantil Pascualina**, desarrollada como Tarea 2 de la asignatura **Bases de Datos I** — Institución Universitaria Pascual Bravo.

El proyecto transforma un modelo conceptual inicial en un **modelo relacional lógico** implementado en **PostgreSQL**, aplicando reglas estrictas de integridad referencial y normalización hasta la **Tercera Forma Normal (3FN)**.

---

##  Integrantes

| Nombre | Rol en el proyecto |
|---|---|
| Emanuel Ortiz | Normalización (1FN, 2FN, 3FN) y diagrama del modelo lógico |
| Saray Muñoz | Script SQL, creación de tablas en pgAdmin y justificación de tipos de dato |

---

##  Descripción general

La Red Social Pascualina busca soportar:

- Comunicación fluida entre estudiantes
- Intercambio de información académica
- Interacción social mediante publicaciones
- Integración a través de **grupos de estudio** y **eventos**

---

##  Estructura del repositorio

```
red-social-pascualina/
├── informe/
│   └── Red_Social_Pascualina_Informe_Completo.docx   # Informe técnico completo
├── video/
│   └── (video explicativo del proyecto)
├── sql/
│   └── script_creacion_tablas.sql                    # Script de creación de las 6 tablas
├── diagrama/
│   └── diagrama_modelo_logico.png                    # Diagrama entidad-relación
└── README.md
```

---

##  Modelo de datos

El sistema está compuesto por **6 tablas**:

| Tabla | Descripción |
|---|---|
| `usuarios` | Perfil de cada estudiante registrado |
| `publicaciones` | Contenido publicado por los estudiantes |
| `seguidores` | Relación N:M — quién sigue a quién |
| `grupos` | Grupos de estudio o proyectos |
| `miembros_grupo` | Relación N:M entre usuarios y grupos |
| `eventos` | Eventos académicos o sociales |

### Relaciones principales

- **1:N** → `usuarios` → `publicaciones` (un usuario tiene muchas publicaciones)
- **1:N** → `usuarios` → `eventos` (un usuario crea muchos eventos)
- **N:M** → `usuarios` ↔ `usuarios` vía `seguidores`
- **N:M** → `usuarios` ↔ `grupos` vía `miembros_grupo`

Todas las claves foráneas usan `ON DELETE CASCADE` para evitar registros huérfanos.

 Diagrama completo disponible en `/diagrama/diagrama_modelo_logico.png` y en el informe técnico.

---

##  Proceso de normalización

| Forma Normal | Qué resuelve | Aplicación en el proyecto |
|---|---|---|
| **1FN** | Atomicidad de los datos | `intereses` y `habilidades` se guardan como `TEXT` simple, sin listas anidadas |
| **2FN** | Dependencias parciales en claves compuestas | `seguidores` y `miembros_grupo`: sus atributos dependen de ambas claves foráneas combinadas |
| **3FN** | Dependencias transitivas | `publicaciones` no guarda `nombre_autor` ni `email_autor`; se obtienen por `JOIN` con `usuarios` |

Detalle completo del razonamiento en el informe técnico (`/informe`).

---

##  Instalación y puesta en marcha

### Requisitos previos

- PostgreSQL 14+
- pgAdmin 4 (o cualquier cliente SQL de tu preferencia)

### Pasos

1. Crear la base de datos:
   ```sql
   CREATE DATABASE red_social_pascualina;
   ```
2. Conectarte a la base de datos desde pgAdmin.
3. Ejecutar el script de creación de tablas ubicado en `/sql/script_creacion_tablas.sql`. Este crea, en orden, las tablas:
   `usuarios → seguidores → publicaciones → grupos → miembros_grupo → eventos`
4. Verificar que las 6 tablas existan:
   `Schemas → public → Tables`

### Prueba rápida

```sql
-- Insertar usuarios de prueba
INSERT INTO usuarios (nombre, email, area_estudio, intereses)
VALUES ('Camilo Pérez', 'camilo@pascualbravo.edu.co', 'Sistemas', 'Programación, Bases de Datos'),
       ('Maria Gómez', 'maria@pascualbravo.edu.co', 'Diseño', 'UI/UX, Ilustración');

-- Insertar una publicación
INSERT INTO publicaciones (id_usuario_autor, contenido, categoria)
VALUES (1, '¿Alguien sabe a qué hora es la clase de Bases de Datos?', 'Consulta');

-- Consultar
SELECT * FROM usuarios;
SELECT * FROM publicaciones;
```

---

##  Tecnologías utilizadas

- **PostgreSQL** — motor de base de datos relacional
- **pgAdmin 4** — administración y ejecución de scripts SQL
- **GitHub** — control de versiones y organización del proyecto

---

##  Documentación adicional

- Informe técnico completo (justificación, normalización, diccionario de datos, diagrama y conclusiones): `/informe/Red_Social_Pascualina_Informe_Completo.docx`
- Video explicativo del proyecto: `/video/`

---

##  Licencia

Proyecto académico desarrollado para la asignatura Bases de Datos I — Institución Universitaria Pascual Bravo, Medellín, 2026. Uso educativo.

