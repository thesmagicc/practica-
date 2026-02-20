# 🏥 Sistema de Agenda Médica y Farmacia

> Documento de **Diseño e Implementación** — Aplicación interactiva en C++ para la gestión de citas médicas y el flujo de atención en farmacia.

---

## 📌 Información del Estudiante

| Campo | Detalle |
|---|---|
| 👤 **Nombre** | Sebastián Sánchez Gómez |
| 📅 **Fecha** | 20 de Febrero de 2026 |
| 📝 **Práctica** | Práctica 1 |

---

## 📋 Tabla de Contenidos

- [Descripción General](#-descripción-general)
- [Diseño de la Solución](#1️⃣-diseño-de-la-solución-30)
- [Diseño de la Interfaz Gráfica](#2️⃣-diseño-de-la-interfaz-gráfica-gui)
- [Instrucciones de Implementación en C++](#3️⃣-instrucciones-de-implementación-en-c-40)
- [Requisitos y Uso](#️-requisitos-y-uso)
- [Estructura del Proyecto](#-estructura-del-proyecto)

---

## 📖 Descripción General

El sistema propuesto es una aplicación interactiva en **C++** diseñada para gestionar las citas de un centro médico y el flujo de pacientes hacia la farmacia. Se ejecuta dentro de un ciclo continuo que presenta un menú interactivo y gestiona peticiones hasta que el usuario elige salir.

---

## 1️⃣ Diseño de la Solución (30%)

### 🧩 Descripción en Lenguaje Natural

La solución se estructura en los siguientes componentes principales:

#### 📦 Estructuras de Datos
La información de las citas *(paciente, médico, fecha, hora y estado)* se encapsula en un registro `struct`. Para el almacenamiento dinámico se utilizan dos estructuras:

| Estructura | Tipo | Uso |
|---|---|---|
| `listaCitas` | `vector<Cita>` | Almacena todas las citas activas, permite búsquedas y modificaciones |
| `colaFarmacia` | `queue<string>` | Gestiona el despacho de medicamentos en orden FIFO |

> 💡 **FIFO:** *First In, First Out* — el primero en llegar es el primero en ser atendido.

---

#### 🔄 Menú Principal
El programa se ejecuta dentro de un **ciclo continuo** que presenta un menú interactivo. El sistema corre ininterrumpidamente gestionando peticiones hasta que el usuario elige salir.

---

#### ✅ Operaciones de Asignación y Prevención de Conflictos
Al agendar una cita, un algoritmo valida el historial existente:
- Si el médico **ya tiene** una reserva en esa fecha y hora, el sistema **rechaza** la solicitud para evitar choques de horario.
- Si el horario está **libre**, guarda la cita con estado `"Pendiente"`.

---

#### 🔍 Búsqueda y Presentación
- Permite **buscar citas** por el nombre del paciente.
- Genera un **informe completo** que imprime todos los datos almacenados en el sistema.

---

#### 🗑️ Eliminación de Registros
Permite buscar a un paciente y **borrar permanentemente** su registro de la memoria, liberando ese espacio en la agenda.

---

#### 💊 Gestión de Turnos — Farmacia
1. Al finalizar una consulta, el estado de la cita cambia a `"Atendida"`.
2. Si el paciente requiere medicinas, es **insertado al final** de la cola de farmacia.
3. Al "Llamar al siguiente", el sistema **extrae al primer paciente** de la fila y actualiza la lista automáticamente.

---

### 📐 Diseño Lógico — Pseudocódigo

```plaintext
INICIO DEL SISTEMA
    ESTRUCTURA Cita: paciente, medico, fecha, hora, estado
    LISTA_DINAMICA lista_citas
    COLA cola_farmacia

    MIENTRAS (ejecutando == VERDADERO) HACER:
        IMPRIMIR Menú de Opciones
        LEER opcion_usuario

        SEGUN opcion_usuario:

            CASO 1:  // Asignación
                LEER datos_nueva_cita
                SI horario_esta_libre(datos_nueva_cita):
                    AGREGAR a lista_citas
                SINO:
                    IMPRIMIR "Conflicto de horario."

            CASO 2 y 3:  // Búsqueda y Presentación
                LEER nombre_buscar
                IMPRIMIR datos coincidentes de lista_citas

            CASO 4:  // Eliminación
                LEER nombre_eliminar
                ELIMINAR registro de lista_citas

            CASO 5:  // Enviar a Farmacia
                CAMBIAR estado A "Atendida"
                ENCOLAR paciente EN cola_farmacia

            CASO 6:  // Atender en Farmacia
                SI HAY PACIENTES EN cola_farmacia:
                    DESENCOLAR primer paciente y llamar

            CASO 7:  // Salir
                ejecutando = FALSO

        FIN SEGUN
    FIN MIENTRAS
FIN DEL SISTEMA
```

---

## 2️⃣ Diseño de la Interfaz Gráfica (GUI)

Para una futura escalabilidad visual, la aplicación se proyecta con una **ventana principal dividida**:

```
┌─────────────────────────────────────────────────────────┐
│                  🏥 SISTEMA MÉDICO                      │
├──────────────────┬──────────────────────────────────────┤
│                  │                                      │
│  📌 NAVEGACIÓN   │       🖥️ ÁREA DE TRABAJO             │
│                  │                                      │
│  [ Nueva Cita  ] │   ┌──────────────────────────────┐  │
│  [ Agenda      ] │   │  Formulario / Tabla / Cola   │  │
│  [ Farmacia    ] │   └──────────────────────────────┘  │
│                  │                                      │
└──────────────────┴──────────────────────────────────────┘
```

### 🧱 Componentes de la GUI

| Componente | Descripción |
|---|---|
| 📋 **Panel de Navegación** | Botones para cada módulo: *Nueva Cita*, *Agenda Completa*, *Farmacia* |
| 📝 **Formularios** | Campos de texto y menús desplegables para agendar citas, previniendo errores de formato |
| 📊 **Tablas de Datos** | *Data Grids* para visualizar la agenda con barra de búsqueda en tiempo real |
| ⚠️ **Alertas (Pop-ups)** | Cuadros de diálogo para advertir conflictos de horario o confirmar eliminación de registros |
| 💊 **Panel de Farmacia** | Lista visual de la cola de pacientes con botón destacado **"Llamar Siguiente"** que actualiza la vista al instante |

---

## 3️⃣ Instrucciones de Implementación en C++ (40%)

### 📚 Librerías Requeridas

```cpp
#include <iostream>   // Entradas y salidas estándar
#include <string>     // Manejo de cadenas de texto
#include <vector>     // Lista dinámica de citas
#include <queue>      // Cola FIFO para farmacia
```

---

### 🗂️ Estructuras Globales

```cpp
struct Cita {
    string paciente;
    string medico;
    string fecha;
    string hora;
    string estado;   // "Pendiente" o "Atendida"
};

vector<Cita> listaCitas;      // Lista dinámica de citas
queue<string> colaFarmacia;   // Cola de farmacia
```

---

### ⚙️ Guía de Implementación por Módulo

#### 🔁 Bucle Principal — 5%
Implementar un `while(true)` en el `main()` que imprima el menú con `cout`. Leer la opción con `cin` y usar un `switch` para dirigir el flujo del programa.

```cpp
while (opcion != 7) {
    mostrarMenu();
    cin >> opcion;
    switch (opcion) { /* casos */ }
}
```

---

#### 📥 Asignación — 5%
Crear un objeto `Cita` temporal. Antes del `push_back()`, usar un ciclo `for` para iterar sobre `listaCitas`. Si `medico`, `fecha` y `hora` coinciden con un registro existente, **bloquear la asignación**.

```cpp
for (size_t i = 0; i < listaCitas.size(); i++) {
    if (listaCitas[i].medico == nueva.medico &&
        listaCitas[i].fecha  == nueva.fecha  &&
        listaCitas[i].hora   == nueva.hora) {
        conflicto = true;
    }
}
if (!conflicto) listaCitas.push_back(nuevaCita);
```

---

#### 🔍 Búsqueda — 10% | 📋 Presentación — 5%
Usar un ciclo `for` para iterar el vector completo:
- **Búsqueda:** imprimir solo si `listaCitas[i].paciente == entrada`.
- **Presentación:** imprimir **todos** los índices sin filtro.

```cpp
for (size_t i = 0; i < listaCitas.size(); i++) {
    if (listaCitas[i].paciente == nombreBusqueda) {
        // Imprimir datos del paciente
    }
}
```

---

#### 🗑️ Eliminación — 5%
Recorrer el vector buscando coincidencias. Al encontrar al paciente, ejecutar `erase()` para liberar la memoria.

```cpp
for (auto it = listaCitas.begin(); it != listaCitas.end(); ++it) {
    if (it->paciente == nombreEliminar) {
        listaCitas.erase(it);
        break;
    }
}
```

---

#### 💊 Gestión de Colas — 10%
Al finalizar la atención médica, **encolar** al paciente. En el módulo de farmacia, validar que la cola no esté vacía, mostrar al primero y retirarlo.

```cpp
// Encolar al paciente
colaFarmacia.push(nombrePaciente);

// Atender en farmacia
if (!colaFarmacia.empty()) {
    cout << colaFarmacia.front();  // Ver primero
    colaFarmacia.pop();            // Retirar de la cola
}
```

---

### 📊 Resumen de Ponderación

| Módulo | Operación | Porcentaje |
|---|---|---|
| Menú continuo | `while` + `switch` | 5% |
| Agendamiento | `push_back` + validación | 5% |
| Búsqueda | Recorrido por nombre | 10% |
| Presentación | Listado completo | 5% |
| Eliminación | `erase` del vector | 5% |
| Cola Farmacia | `push`, `front`, `pop` | 10% |
| **Diseño de solución** | Pseudocódigo + descripción | **30%** |
| **Implementación C++** | Código funcional | **40%** |

---

## ⚙️ Requisitos y Uso

### Requisitos
- Compilador **C++11** o superior (`g++`)
- Sistema operativo: Windows, Linux o macOS

### Compilar y Ejecutar

```bash
# Compilar
g++ sistema_citas_farmacia.cpp -o sistema

# Ejecutar (Linux / macOS)
./sistema

# Ejecutar (Windows)
sistema.exe
```

---

## 📁 Estructura del Proyecto

```
📦 practica-1-sistema-medico
 ┣ 📄 sistema_citas_farmacia.cpp   ← Código fuente principal
 ┗ 📄 README.md                    ← Documento de diseño e implementación
```

---

*Práctica 1 — Estructuras de Datos y Gestión de Sistemas en C++*
