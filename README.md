
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <cmath>
#include <iomanip>
#include <sstream>
#include <limits>

using namespace std;

// Estructura que representa a un estudiante
struct Estudiante {
	string nombre;
	int anioNacimiento;
	double GPA;
	double presupuesto;
};

// Vector global de estudiantes
vector<Estudiante> estudiantes;

// Funcion para leer numeros de forma segura
template <typename T>
T leerNumero(string mensaje) {
	T valor;
	string linea;
	
	while (true) {
		cout << mensaje;
		getline(cin, linea);
		
		stringstream ss(linea);
		char extra;
		
		if (ss >> valor && !(ss >> extra)) {
			return valor;
		}
		
		cout << "Entrada no valida. Intente nuevamente.\n";
	}
}

// Funcion para ingresar los datos de un estudiante
Estudiante ingresarDatos() {
	Estudiante e;
	
	cout << "\n--- DATOS DEL ESTUDIANTE ---\n";
	
	cout << "Nombre: ";
	getline(cin, e.nombre);
	
	e.anioNacimiento = leerNumero<int>("Anio de nacimiento: ");
	e.GPA = leerNumero<double>("Promedio GPA: ");
	e.presupuesto = leerNumero<double>("Presupuesto: ");
	
	return e;
}

// Calcular indice universidad
double indiceUniversidad(const Estudiante& e) {
	return abs(e.GPA - 3.0) * e.presupuesto;
}

// Mostrar un estudiante
void mostrarEstudiante(const Estudiante& e, int indice) {
	cout << left
		<< setw(8) << indice
		<< setw(20) << e.nombre
		<< setw(18) << e.anioNacimiento
		<< setw(12) << fixed << setprecision(2) << e.GPA
		<< setw(15) << e.presupuesto
		<< setw(15) << indiceUniversidad(e)
		<< endl;
}

// Imprimir todos los estudiantes
void imprimir() {
	cout << "\n========== LISTA DE ESTUDIANTES ==========\n";
	
	if (estudiantes.empty()) {
		cout << "El vector estudiantes esta vacio.\n";
		return;
	}
	
	cout << left
						  << setw(8) << "Indice"
						  << setw(20) << "Nombre"
						  << setw(18) << "Nacimiento"
						  << setw(12) << "GPA"
						  << setw(15) << "Presupuesto"
						  << setw(15) << "Indice Univ."
						  << endl;
	
	cout << string(88, '-') << endl;
	
	for (size_t i = 0; i < estudiantes.size(); i++) {
		mostrarEstudiante(estudiantes[i], i);
	}
	
	cout << "\nTotal de estudiantes: " << estudiantes.size() << endl;
}

// INSERTAR ESTUDIANTE
void insertar() {
	int opcion;
	
	cout << "\n========== INSERTAR ESTUDIANTE ==========\n";
	cout << "1. Insertar al final\n";
	cout << "2. Insertar en un indice especifico\n";
	
	opcion = leerNumero<int>("Seleccione una opcion: ");
	
	if (opcion != 1 && opcion != 2) {
		cout << "Opcion no valida.\n";
		return;
	}
	
	int tipo;
	
	cout << "\n1. Insertar estudiante por defecto (NN, 0, 0, 0)\n";
	cout << "2. Insertar estudiante con datos\n";
	
	tipo = leerNumero<int>("Seleccione el tipo de insercion: ");
	
	if (tipo != 1 && tipo != 2) {
		cout << "Opcion no valida.\n";
		return;
	}
	
	Estudiante nuevo;
	
	if (tipo == 1) {
		nuevo = {"NN", 0, 0.0, 0.0};
	} else {
		nuevo = ingresarDatos();
	}
	
	if (opcion == 1) {
		estudiantes.push_back(nuevo);
		cout << "\nEstudiante insertado al final correctamente.\n";
	} else {
		int indice = leerNumero<int>("Ingrese el indice donde desea insertar: ");
		
		if (indice < 0 || indice > (int)estudiantes.size()) {
			cout << "Indice fuera de rango.\n";
			return;
		}
		
		estudiantes.insert(estudiantes.begin() + indice, nuevo);
		cout << "\nEstudiante insertado correctamente en el indice "
			<< indice << ".\n";
	}
	
	imprimir();
}

// ELIMINAR ESTUDIANTE
void eliminar() {
	cout << "\n========== ELIMINAR ESTUDIANTE ==========\n";
	
	if (estudiantes.empty()) {
		cout << "No se puede eliminar. El vector esta vacio.\n";
		return;
	}
	
	cout << "1. Eliminar el ultimo estudiante\n";
	cout << "2. Eliminar por indice\n";
	
	int opcion = leerNumero<int>("Seleccione una opcion: ");
	
	if (opcion == 1) {
		estudiantes.pop_back();
		cout << "\nUltimo estudiante eliminado correctamente.\n";
	}
	else if (opcion == 2) {
		int indice = leerNumero<int>("Ingrese el indice a eliminar: ");
		
		if (indice < 0 || indice >= (int)estudiantes.size()) {
			cout << "Indice fuera de rango.\n";
			return;
		}
		
		estudiantes.erase(estudiantes.begin() + indice);
		cout << "\nEstudiante eliminado correctamente.\n";
	}
	else {
		cout << "Opcion no valida.\n";
		return;
	}
	
	imprimir();
}

// ACTUALIZAR ESTUDIANTE
void actualizar() {
	cout << "\n========== ACTUALIZAR ESTUDIANTE ==========\n";
	
	if (estudiantes.empty()) {
		cout << "No hay estudiantes para actualizar.\n";
		return;
	}
	
	imprimir();
	
	int indice = leerNumero<int>(
								 "\nIngrese el indice del estudiante que desea actualizar: "
								 );
	
	if (indice < 0 || indice >= (int)estudiantes.size()) {
		cout << "Indice fuera de rango.\n";
		return;
	}
	
	cout << "\nIngrese los nuevos datos:\n";
	
	estudiantes[indice] = ingresarDatos();
	
	cout << "\nEstudiante actualizado correctamente.\n";
	
	imprimir();
}

// ORDENAR ESTUDIANTES
void ordenar() {
	cout << "\n========== ORDENAR ESTUDIANTES ==========\n";
	
	if (estudiantes.empty()) {
		cout << "No hay estudiantes para ordenar.\n";
		return;
	}
	
	cout << "1. Por anio de nacimiento\n";
	cout << "2. Por promedio GPA\n";
	cout << "3. Por presupuesto\n";
	cout << "4. Por indice universidad\n";
	
	int opcion = leerNumero<int>("Seleccione el criterio de ordenamiento: ");
	
	switch (opcion) {
		
	case 1:
		sort(estudiantes.begin(), estudiantes.end(),
			 [](const Estudiante& a, const Estudiante& b) {
				 return a.anioNacimiento < b.anioNacimiento;
			 });
		
		cout << "\nOrdenado por anio de nacimiento.\n";
		break;
		
	case 2:
		sort(estudiantes.begin(), estudiantes.end(),
			 [](const Estudiante& a, const Estudiante& b) {
				 return a.GPA < b.GPA;
			 });
		
		cout << "\nOrdenado por GPA.\n";
		break;
		
	case 3:
		sort(estudiantes.begin(), estudiantes.end(),
			 [](const Estudiante& a, const Estudiante& b) {
				 return a.presupuesto < b.presupuesto;
			 });
		
		cout << "\nOrdenado por presupuesto.\n";
		break;
		
	case 4:
		sort(estudiantes.begin(), estudiantes.end(),
			 [](const Estudiante& a, const Estudiante& b) {
				 return indiceUniversidad(a) < indiceUniversidad(b);
			 });
		
		cout << "\nOrdenado por indice universidad.\n";
		break;
		
	default:
		cout << "Opcion no valida.\n";
		return;
	}
	
	imprimir();
}

// MENU PRINCIPAL
int main() {
	
	int opcion;
	
	do {
		cout << "\n";
		cout << "============================================\n";
		cout << "       SISTEMA DE GESTION DE ESTUDIANTES\n";
		cout << "============================================\n";
		
		cout << "1. Insertar estudiante\n";
		cout << "2. Eliminar estudiante\n";
		cout << "3. Actualizar estudiante\n";
		cout << "4. Ordenar estudiantes\n";
		cout << "5. Imprimir estudiantes\n";
		cout << "0. Salir del programa\n";
		
		cout << "============================================\n";
		
		opcion = leerNumero<int>("Seleccione una opcion: ");
		
		switch (opcion) {
			
		case 1:
			insertar();
			break;
			
		case 2:
			eliminar();
			break;
			
		case 3:
			actualizar();
			break;
			
		case 4:
			ordenar();
			break;
			
		case 5:
			imprimir();
			break;
			
		case 0:
			cout << "\nSaliendo del programa...\n";
			cout << "Gracias por utilizar el sistema.\n";
			break;
			
		default:
			cout << "\nOpcion no valida. Intente nuevamente.\n";
		}
		
	} while (opcion != 0);
	
	return 0;
}
