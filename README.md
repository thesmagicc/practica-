# 🏥 Sistema de Citas y Farmacia

> Sistema de gestión hospitalaria desarrollado en **C++**, diseñado para administrar citas médicas y el flujo de atención en farmacia de forma eficiente.

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Características](#-características)
- [Estructuras de Datos](#-estructuras-de-datos)
- [Funcionalidades del Menú](#-funcionalidades-del-menú)
- [Requisitos](#-requisitos)
- [Instalación y Uso](#-instalación-y-uso)
- [Flujo del Sistema](#-flujo-del-sistema)
- [Estructura del Proyecto](#-estructura-del-proyecto)

---

## 📖 Descripción

Este sistema simula la gestión de citas en una clínica u hospital. Permite agendar, buscar, visualizar y cancelar citas médicas, así como gestionar la cola de atención en farmacia mediante una estructura **FIFO (First In, First Out)**.

---

## ✨ Características

| Característica | Descripción |
|---|---|
| ✅ Agendamiento de citas | Registra paciente, médico, fecha y hora |
| 🔍 Búsqueda de citas | Localiza citas por nombre de paciente |
| 📋 Visualización completa | Muestra toda la agenda del sistema |
| 🗑️ Cancelación de citas | Elimina registros del sistema |
| 🏥 Atención de pacientes | Finaliza consultas y deriva a farmacia |
| 💊 Cola de farmacia | Gestiona el orden de entrega de medicamentos |
| ⚠️ Validación de horarios | Detecta y previene conflictos de agenda |

---

## 🗂️ Estructuras de Datos

### `struct Cita`
Almacena toda la información de una cita médica:

```cpp
struct Cita {
    string paciente;  // Nombre del paciente
    string medico;    // Nombre del médico asignado
    string fecha;     // Formato: DD/MM/AAAA
    string hora;      // Formato: HH:MM
    string estado;    // "Pendiente" o "Atendida"
};
```

### Contenedores Globales

| Variable | Tipo | Propósito |
|---|---|---|
| `listaCitas` | `vector<Cita>` | Almacena todas las citas registradas |
| `colaFarmacia` | `queue<string>` | Gestiona el turno de pacientes en farmacia (FIFO) |

---

## 🖥️ Funcionalidades del Menú

```
=========================================
   🏥 SISTEMA DE CITAS Y FARMACIA 🏥   
=========================================
1. Agendar nueva cita         → Asignación
2. Buscar cita por paciente   → Búsqueda
3. Ver todas las citas        → Presentar info
4. Cancelar/Eliminar cita     → Borrado
5. Atender paciente           → Enviar a Farmacia
6. Llamar siguiente en Farmacia → Colas
7. Salir del sistema
=========================================
```

### Descripción de cada opción

**1️⃣ Agendar nueva cita**
Solicita los datos del paciente y verifica que no exista un conflicto de horario con el mismo médico antes de guardar.

**2️⃣ Buscar cita por paciente**
Realiza una búsqueda lineal en el vector por nombre de paciente y muestra todos sus registros.

**3️⃣ Ver todas las citas**
Lista la agenda completa con numeración, e indica cuántos pacientes están esperando en farmacia.

**4️⃣ Cancelar / Eliminar cita**
Busca y elimina la primera cita encontrada para el nombre indicado usando iteradores del vector.

**5️⃣ Atender paciente**
Cambia el estado de la cita a `"Atendida"` y, si el paciente requiere medicamentos, lo agrega a la cola de farmacia.

**6️⃣ Llamar siguiente en Farmacia**
Muestra al primer paciente de la cola (FIFO) y lo retira con `pop()` tras confirmar la entrega.

---

## ⚙️ Requisitos

- Compilador C++ con soporte para **C++11 o superior**
- Sistema operativo: Windows, Linux o macOS
- Librerías estándar: `iostream`, `vector`, `queue`, `string`

---

## 🚀 Instalación y Uso

### 1. Clonar o descargar el archivo
```bash
# Descarga el archivo sistema_citas_farmacia.cpp
```

### 2. Compilar
```bash
g++ sistema_citas_farmacia.cpp -o sistema
```

### 3. Ejecutar
```bash
# Linux / macOS
./sistema

# Windows
sistema.exe
```

---

## 🔄 Flujo del Sistema

```
  [Inicio]
     │
     ▼
┌─────────────┐     ┌──────────────────┐
│  Agendar    │────▶│  Vector de Citas  │
│   Cita      │     │  (listaCitas)     │
└─────────────┘     └──────────────────┘
                            │
                    ┌───────▼──────────┐
                    │  Atender Cita    │
                    │ (estado: Atendida│
                    └───────┬──────────┘
                            │ ¿Requiere
                            │ medicamentos?
                    ┌───────▼──────────┐
                    │  Cola Farmacia   │
                    │  (FIFO - queue)  │
                    └───────┬──────────┘
                            │
                    ┌───────▼──────────┐
                    │  Entrega de      │
                    │  Medicamentos    │
                    └──────────────────┘
```

---

## 📁 Estructura del Proyecto

```
📦 sistema-citas-farmacia
 ┣ 📄 sistema_citas_farmacia.cpp   ← Código fuente principal
 ┗ 📄 README.md                    ← Documentación del proyecto
```

---

## 👨‍💻 Conceptos Aplicados

- **Estructuras (`struct`)** para modelar datos
- **Vectores (`vector`)** para almacenamiento dinámico con inserción y eliminación
- **Colas (`queue`)** para gestión FIFO de farmacia
- **Bucle principal (`while`)** para menú continuo
- **Validación de entrada** para opciones no válidas
- **Iteradores** para recorrido y eliminación segura en vectores

---

*Desarrollado en C++ como proyecto de estructuras de datos y gestión hospitalaria.*
