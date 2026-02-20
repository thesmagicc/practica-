#include <iostream>
#include <vector>
#include <queue>
#include <string>

using namespace std;

// Estructura para almacenar los datos de la cita
struct Cita {
    string paciente;
    string medico;
    string fecha;
    string hora;
    string estado; // Pendiente, Atendida
};

// Variables globales (Estructuras de datos principales)
vector<Cita> listaCitas;
queue<string> colaFarmacia; // Cola (FIFO) para la entrega de medicamentos

// ================= FUNCIONES DEL SISTEMA =================

// 1. Mostrar Menú (5% - Menú continuo)
void mostrarMenu() {
    cout << "\n=========================================\n";
    cout << "   🏥 SISTEMA DE CITAS Y FARMACIA 🏥   \n";
    cout << "=========================================\n";
    cout << "1. Agendar nueva cita (Asignacion)\n";
    cout << "2. Buscar cita por paciente (Busqueda)\n";
    cout << "3. Ver todas las citas (Presentar info)\n";
    cout << "4. Cancelar/Eliminar cita (Borrado)\n";
    cout << "5. Atender paciente (Enviar a Farmacia)\n";
    cout << "6. Llamar siguiente en Farmacia (Colas)\n";
    cout << "7. Salir del sistema\n";
    cout << "=========================================\n";
    cout << "Seleccione una opcion (1-7): ";
}

// 2. Operaciones de Asignación (5% - Guardar información)
void agendarCita() {
    Cita nuevaCita;
    cout << "\n--- AGENDAR NUEVA CITA ---\n";
    cout << "Nombre del paciente: ";
    cin.ignore();
    getline(cin, nuevaCita.paciente);
    cout << "Nombre del medico: ";
    getline(cin, nuevaCita.medico);
    cout << "Fecha (DD/MM/AAAA): ";
    getline(cin, nuevaCita.fecha);
    cout << "Hora (HH:MM): ";
    getline(cin, nuevaCita.hora);
    nuevaCita.estado = "Pendiente";

    // Validacion basica de conflictos de horario
    bool conflicto = false;
    for (size_t i = 0; i < listaCitas.size(); i++) {
        if (listaCitas[i].medico == nuevaCita.medico && 
            listaCitas[i].fecha == nuevaCita.fecha && 
            listaCitas[i].hora == nuevaCita.hora) {
            conflicto = true;
            break;
        }
    }

    if (conflicto) {
        cout << "⚠️ ERROR: El medico ya tiene una cita en ese horario. Elija otro.\n";
    } else {
        listaCitas.push_back(nuevaCita);
        cout << "✅ Cita agendada con exito.\n";
    }
}

// 3. Operaciones de Búsqueda (10%)
void buscarCita() {
    if (listaCitas.empty()) {
        cout << "\nNo hay citas registradas.\n";
        return;
    }
    
    string nombreBusqueda;
    cout << "\n--- BUSCAR CITA ---\n";
    cout << "Ingrese el nombre del paciente a buscar: ";
    cin.ignore();
    getline(cin, nombreBusqueda);

    bool encontrada = false;
    for (size_t i = 0; i < listaCitas.size(); i++) {
        if (listaCitas[i].paciente == nombreBusqueda) {
            cout << "\n🔍 RESULTADO: \n";
            cout << "Paciente: " << listaCitas[i].paciente 
                 << " | Medico: " << listaCitas[i].medico 
                 << " | Fecha: " << listaCitas[i].fecha 
                 << " | Hora: " << listaCitas[i].hora 
                 << " | Estado: " << listaCitas[i].estado << "\n";
            encontrada = true;
        }
    }

    if (!encontrada) {
        cout << "❌ No se encontraron citas para el paciente indicado.\n";
    }
}

// 4. Presentar Información (5% - Informe de datos)
void verTodasLasCitas() {
    cout << "\n--- AGENDA COMPLETA ---\n";
    if (listaCitas.empty()) {
        cout << "No hay citas en el sistema.\n";
    } else {
        for (size_t i = 0; i < listaCitas.size(); i++) {
            cout << i + 1 << ". Paciente: " << listaCitas[i].paciente 
                 << " | Medico: " << listaCitas[i].medico 
                 << " | Horario: " << listaCitas[i].fecha << " " << listaCitas[i].hora 
                 << " | Estado: " << listaCitas[i].estado << "\n";
        }
    }
    cout << "\n📊 Pacientes esperando en Farmacia: " << colaFarmacia.size() << "\n";
}

// 5. Eliminar un registro (5%)
void cancelarCita() {
    if (listaCitas.empty()) {
        cout << "\nNo hay citas para cancelar.\n";
        return;
    }

    string nombreEliminar;
    cout << "\n--- CANCELAR / ELIMINAR CITA ---\n";
    cout << "Ingrese el nombre del paciente cuya cita desea cancelar: ";
    cin.ignore();
    getline(cin, nombreEliminar);

    bool eliminada = false;
    for (auto it = listaCitas.begin(); it != listaCitas.end(); ++it) {
        if (it->paciente == nombreEliminar) {
            listaCitas.erase(it); // Elimina el registro del vector
            cout << "🗑️ La cita de " << nombreEliminar << " ha sido eliminada del sistema.\n";
            eliminada = true;
            break; // Salimos para no romper el iterador
        }
    }

    if (!eliminada) {
        cout << "❌ No se encontro la cita para eliminar.\n";
    }
}

// 6. Gestión de Colas (10% - Enviar a Farmacia)
void atenderPaciente() {
    string nombreAtender;
    cout << "\n--- FINALIZAR CONSULTA ---\n";
    cout << "Ingrese el nombre del paciente atendido: ";
    cin.ignore();
    getline(cin, nombreAtender);

    bool encontrado = false;
    for (size_t i = 0; i < listaCitas.size(); i++) {
        if (listaCitas[i].paciente == nombreAtender && listaCitas[i].estado == "Pendiente") {
            listaCitas[i].estado = "Atendida";
            encontrado = true;
            
            char requiereMeds;
            cout << "¿El paciente requiere medicamentos? (s/n): ";
            cin >> requiereMeds;
            
            if (requiereMeds == 's' || requiereMeds == 'S') {
                colaFarmacia.push(listaCitas[i].paciente); // Encolar (Push)
                cout << "📝 Paciente enviado a la lista de espera de la Farmacia.\n";
            } else {
                cout << "✅ Consulta finalizada.\n";
            }
            break;
        }
    }
    
    if (!encontrado) {
        cout << "❌ Cita no encontrada o ya fue atendida.\n";
    }
}

// Gestión de Colas (10% - Atender en Farmacia)
void llamarSiguienteFarmacia() {
    cout << "\n--- MODULO DE FARMACIA ---\n";
    if (colaFarmacia.empty()) {
        cout << "No hay pacientes en espera de medicamentos.\n";
    } else {
        // Muestra al primero en la fila y luego lo saca de la cola (FIFO)
        cout << "📢 Llamando a: " << colaFarmacia.front() << " para entrega de medicamentos.\n";
        colaFarmacia.pop(); // Desencolar (Pop)
        cout << "✅ Medicamentos entregados.\n";
    }
}

// ================= BUCLE PRINCIPAL =================
int main() {
    int opcion = 0;
    
    // El sistema corre hasta que el usuario decida salir
    while (opcion != 7) {
        mostrarMenu();
        if (!(cin >> opcion)) { // Validacion en caso de que el usuario ingrese una letra
            cin.clear();
            cin.ignore(10000, '\n');
            opcion = 0;
        }

        switch (opcion) {
            case 1: agendarCita(); break;
            case 2: buscarCita(); break;
            case 3: verTodasLasCitas(); break;
            case 4: cancelarCita(); break;
            case 5: atenderPaciente(); break;
            case 6: llamarSiguienteFarmacia(); break;
            case 7: cout << "Saliendo del sistema... ¡Exito en tu entrega!\n"; break;
            default: cout << "⚠️ Opcion no valida. Intente de nuevo.\n"; break;
        }
    }
    
    return 0;
}
