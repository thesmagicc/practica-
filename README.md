# 🏥 MediSystem  
### Sistema de Gestión de Citas Médicas y Farmacia  

**Backend 100% en C++17**  
**Servidor HTTP con Winsock2**  
**Persistencia en archivos `.db`**  

---

## 📌 Información General

**Práctica:** Práctica 1 — Estructuras de Datos  
**Estudiante:** Sebastián Sánchez Gómez  
**Fecha:** 20 de Febrero de 2026  
**Tecnología:** C++17 | Winsock2 | HTML/CSS/JS  

---

# 📖 1. Diseño de la Solución

## 1.1 Descripción General

**MediSystem** es una aplicación web cuyo backend está desarrollado completamente en C++ sin usar frameworks externos.

El sistema implementa:

- Servidor HTTP desde cero con Winsock2  
- API REST propia  
- Base de datos persistente con archivos `.db`  
- SPA (Single Page Application) embebida en C++  

El sistema gestiona:

- Citas médicas
- Atención de pacientes
- Cola FIFO de farmacia

---

## 1.2 Arquitectura del Sistema

El proyecto está dividido en 4 capas:

| Capa | Archivo | Responsabilidad |
|------|---------|----------------|
| Modelos | `models.h` | Define estructuras `Cita` y `Stats` |
| Persistencia | `database.h` | Motor CRUD y manejo de archivos |
| Servidor | `main.cpp` | Servidor HTTP + Router REST |
| Presentación | `frontend.h` | SPA HTML/CSS/JS embebida |

---

## 🗂 Modelos de Datos

### 📌 `struct Cita`

| Campo | Tipo | Descripción |
|--------|------|------------|
| id | int | Identificador autoincrementado |
| paciente | string | Nombre del paciente |
| medico | string | Médico asignado |
| especialidad | string | Rama médica |
| fecha | string | YYYY-MM-DD |
| hora | string | HH:MM |
| estado | string | Pendiente / Atendida / Cancelada |
| notas | string | Observaciones |

---

## 💾 Persistencia en Archivos

### Archivos utilizados

```
citas.db
farmacia.db
```

### Formato `citas.db`

```
id|paciente|medico|especialidad|fecha|hora|estado|notas
```

Ejemplo:

```
1|Juan Perez|Dr. Garcia|Cardiologia|2026-02-20|09:00|Pendiente|Dolor en el pecho
```

### Formato `farmacia.db`

```
Juan Perez
Carlos Ruiz
```

---

## 🧠 Estructuras de Datos Utilizadas

### 📌 Vector dinámico

```cpp
vector<Cita> listaCitas;
```

Operaciones:

- `push_back()` → O(1) amortizado
- Búsqueda lineal → O(n)
- `erase()` → O(n)
- Acceso por índice → O(1)

---

### 📌 Cola FIFO (Farmacia)

```cpp
vector<string> colaFarmacia;
```

- `pushFarmacia()` → Encola al final
- `popFarmacia()` → Retira el primero

FIFO = First In, First Out

---

# 🌐 Servidor HTTP

- Implementado con `winsock2.h`
- Puerto: **8080**
- Procesamiento secuencial
- Respuestas en formato JSON
- SPA servida desde C++

---

## 🔀 Pseudocódigo del Servidor

```
INICIAR servidor
  Inicializar Winsock2
  Crear socket puerto 8080
  Cargar base de datos

  WHILE true:
    Aceptar conexión
    Recibir request
    Parsear request
    Manejar ruta
    Enviar respuesta
    Cerrar conexión
FIN
```

---

# 🔌 API REST

| Método | Ruta | Descripción | Código |
|--------|------|------------|--------|
| GET | / | Sirve SPA | 200 |
| GET | /api/stats | Estadísticas | 200 |
| GET | /api/citas | Listar citas | 200 |
| GET | /api/citas?search=X | Buscar | 200 |
| POST | /api/citas | Crear cita | 201 / 409 |
| DELETE | /api/citas/:id | Eliminar | 200 |
| POST | /api/citas/:id/cancelar | Cancelar | 200 |
| POST | /api/atender | Atender paciente | 200 |
| GET | /api/farmacia | Ver cola | 200 |
| POST | /api/farmacia/siguiente | Siguiente | 200 |

---

# ⚙️ 2. Implementación en C++

## 📚 Librerías utilizadas

| Librería | Uso |
|----------|-----|
| `<iostream>` | Logs |
| `<vector>` | Estructura principal |
| `<string>` | Manejo de texto |
| `<sstream>` | Parseo HTTP |
| `<map>` | Parámetros URL |
| `<fstream>` | Persistencia |
| `<algorithm>` | Búsquedas |
| `<cctype>` | Minúsculas |
| `<winsock2.h>` | Sockets |
| `<ws2tcpip.h>` | Funciones auxiliares |

---

# 🛠 Cómo Compilar y Ejecutar

## Requisitos

- Windows
- MinGW 64-bit
- VS Code (opcional)

---

## Compilar

```bash
g++ -std=c++17 -O2 -o servidor.exe main.cpp -lws2_32
```

⚠ La bandera `-lws2_32` es obligatoria en Windows.

---

## Ejecutar

```bash
.\servidor.exe
```

Abrir en navegador:

```
http://localhost:8080
```

---

# 📌 Funcionalidades

- Dashboard con estadísticas
- Agendar nueva cita
- Buscar paciente (tiempo real)
- Ver agenda con filtros
- Cancelar y eliminar citas
- Atender paciente
- Cola FIFO de farmacia
- Historial completo

---

# 📚 Glosario

| Término | Definición |
|----------|------------|
| API REST | Interfaz basada en HTTP |
| Winsock2 | API de sockets en Windows |
| FIFO | First In, First Out |
| SPA | Single Page Application |
| JSON | Formato de intercambio de datos |
| CRUD | Create, Read, Update, Delete |
| HTTP | Protocolo web |
| Puerto | Número lógico de servicio |
| Vector | Contenedor dinámico en C++ |
| Socket | Punto de comunicación TCP/IP |

---

# 👨‍💻 Autor

Sebastián Sánchez Gómez  
Práctica 1 — Estructuras de Datos en C++  
20 de Febrero de 2026
