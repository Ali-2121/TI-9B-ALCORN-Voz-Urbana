# Modelo Estrella para Plataforma VozUrbana

# 📦 Modelo de Data Warehouse: Plataforma de Reportes Ciudadanos en Xicotepec de Juárez

## 📘 Descripción general

Este modelo de Data Warehouse está diseñado para analizar los reportes ciudadanos generados en la plataforma digital Voz Urbana de Xicotepec, organizada por categorías como servicios públicos, vialidad, seguridad, salud y medio ambiente. Se estructura como un modelo estrella con una tabla de hechos central y múltiples dimensiones relacionadas.

#### 📦 Hechos
Conjunto de datos numéricos y medibles que representan eventos o transacciones del negocio.

**Ejemplo:** número de reportes ciudadanos, tiempo de respuesta.

---

#### 📊 Dimensiones
Conjuntos de atributos descriptivos que permiten analizar los hechos desde diferentes perspectivas.

**Ejemplo:** tiempo, ubicación, categoría del reporte.

---

#### 🧭 Jerarquías
Estructuras dentro de una dimensión que organizan los datos en niveles de detalle.

**Ejemplo:** Día → Mes → Año (jerarquía dentro de la dimensión tiempo).

---

#### 🧾 Metadatos
Información que describe la estructura, contenido y origen de los datos del Data Warehouse.

**Ejemplo:** definición de cada campo, tipo de dato, fuente y significado.

## 📥 Orígenes de Datos Externos para el Data Warehouse de Voz Urbana

### 1. INEGI – Instituto Nacional de Estadística y Geografía

**Datos útiles que se obtendrían:**
- Población total y por colonia en Xicotepec
- Nivel de marginación y rezago social por localidad
- Acceso a servicios básicos (agua, drenaje, electricidad)
- Viviendas particulares habitadas y sus características

---

### 2. Redes Sociales (Facebook, Twitter/X, TikTok)

**Datos útiles que se obtendrían:**
- Publicaciones ciudadanas relacionadas con problemáticas locales
- Frecuencia y ubicación estimada de temas reportados (seguridad, servicios, salud)
- Sentimiento general de la población respecto a temas comunitarios
- Detección temprana de eventos o crisis locales (bloqueos, fugas, accidentes)

---

### 3. Secretaría de Salud del Estado de Puebla / Jurisdicción Sanitaria No. 1

**Datos útiles que se obtendrían:**
- Incidencia de enfermedades en Xicotepec (dengue, COVID-19, infecciones respiratorias)
- Cobertura de centros de salud y hospitales locales
- Reportes de vigilancia epidemiológica municipal
- Estadísticas de campañas de salud y vacunación en la región

### A continuación se presentan las dimensiones, hechos jerarquias y metadatos del modelo de Data Warehouse

## 📦 Tabla de Hechos: `hechos_reportes`

Contiene los datos cuantificables y medibles de los reportes ciudadanos.

## 📦 Tabla de Hechos: `hechos_reportes`

| Campo              | Tipo de Dato     | Descripción                                                                 |
|--------------------|------------------|-----------------------------------------------------------------------------|
| id_reporte         | INT              | Identificador único del reporte                                             |
| id_tiempo          | INT (FK)         | Clave foránea a la dimensión tiempo                                         |
| id_ubicacion       | INT (FK)         | Clave foránea a la dimensión ubicación                                      |
| id_categoria       | INT (FK)         | Clave foránea a la dimensión categoría                                      |
| id_usuario         | INT (FK)         | Clave foránea a la dimensión usuario                                        |
| id_salud           | INT (FK)         | Clave foránea a la dimensión salud pública                                  |
| id_social          | INT (FK)         | Clave foránea a la dimensión redes sociales                                 |
| id_demografia      | INT (FK)         | Clave foránea a la dimensión demográfica (INEGI)                            |
| cantidad_reportes  | INT              | Número de reportes agrupados                                                |
| tiempo_respuesta   | INT              | Tiempo en días entre creación y resolución del reporte                      |
| estado_reporte     | VARCHAR(20)      | Estado actual del reporte (abierto, cerrado, en proceso)                    |

---

## 📊 Dimensiones

### 🗓️ Dimensión: `dim_tiempo`

| Campo        | Tipo de Dato     | Descripción                     |
|--------------|------------------|---------------------------------|
| id_tiempo    | INT (PK)         | Clave primaria                  |
| fecha        | DATE             | Fecha completa del evento       |
| día          | INT              | Día del mes                     |
| mes          | INT              | Mes del año                     |
| trimestre    | INT              | Trimestre del año               |
| año          | INT              | Año calendario                  |

---

### 🌍 Dimensión: `dim_ubicacion`

| Campo         | Tipo de Dato     | Descripción                                       |
|---------------|------------------|---------------------------------------------------|
| id_ubicacion  | INT (PK)         | Clave primaria                                    |
| colonia       | VARCHAR(100)     | Nombre de la colonia                              |
| localidad     | VARCHAR(100)     | Localidad a la que pertenece                      |
| municipio     | VARCHAR(100)     | Nombre del municipio (Xicotepec de Juárez)        |
| tipo_zona     | VARCHAR(10)      | Urbana / Rural                                    |
| coordenadas   | POINT o TEXT     | Coordenadas geográficas (latitud, longitud)       |

---

### 📂 Dimensión: `dim_categoria`

| Campo         | Tipo de Dato     | Descripción                                     |
|---------------|------------------|-------------------------------------------------|
| id_categoria  | INT (PK)         | Clave primaria                                  |
| categoria     | VARCHAR(50)      | Categoría general (Ej. Seguridad)               |
| subcategoria  | VARCHAR(100)     | Subcategoría específica (Ej. Robo, Vandalismo)  |

---

### 👤 Dimensión: `dim_usuario`

| Campo          | Tipo de Dato     | Descripción                              |
|----------------|------------------|------------------------------------------|
| id_usuario     | INT (PK)         | Clave primaria                           |
| edad_rango     | VARCHAR(20)      | Rango de edad (Ej. 18–25)                |
| sexo           | VARCHAR(10)      | Masculino / Femenino                     |
| rol_usuario    | VARCHAR(20)      | Ciudadano / Administrador                |

---

### 🏥 Dimensión: `dim_salud_publica`

| Campo                  | Tipo de Dato     | Descripción                                            |
|------------------------|------------------|--------------------------------------------------------|
| id_salud               | INT (PK)         | Clave primaria                                         |
| tipo_enfermedad        | VARCHAR(100)     | Tipo de enfermedad reportada                          |
| incidencia_municipal   | INT              | Número de casos en Xicotepec                          |
| semana_epidemiológica  | VARCHAR(20)      | Código de semana epidemiológica (Ej. 2025-33)         |

---

### 🌐 Dimensión: `dim_redes_sociales`

| Campo                  | Tipo de Dato     | Descripción                                              |
|------------------------|------------------|----------------------------------------------------------|
| id_social              | INT (PK)         | Clave primaria                                           |
| red_social             | VARCHAR(20)      | Nombre de la red social (Facebook, Twitter, etc.)        |
| sentimiento            | VARCHAR(10)      | Positivo, Negativo, Neutral                              |
| volumen_publicaciones  | INT              | Número de publicaciones relacionadas                    |
| tema_principal         | VARCHAR(100)     | Tema dominante (Ej. Seguridad, Servicios, Salud, etc.)   |

---

### 🧑‍🤝‍🧑 Dimensión: `dim_demografia`

| Campo               | Tipo de Dato     | Descripción                                                |
|---------------------|------------------|------------------------------------------------------------|
| id_demografia       | INT (PK)         | Clave primaria                                             |
| nivel_marginacion   | VARCHAR(20)      | Muy alto, Alto, Medio, Bajo, Muy bajo                     |
| nivel_educativo     | DECIMAL(3,1)     | Promedio de escolaridad por colonia/localidad              |
| servicios_basicos   | VARCHAR(100)     | Porcentaje con acceso a agua, luz y drenaje (ej. "85%")    |
| tipo_vivienda       | VARCHAR(50)      | Propia, Rentada, Prestada       