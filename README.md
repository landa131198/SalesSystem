# SalesSystem
Proyecto Final POO
package main;

import auth.AuthManager;
import productos.GestorProductos;
import clientes.GestorClientes;
import ventas.GestorVentas;
import pedidos.GestorPedidos;
import reportes.ReporteManager;

import java.util.Scanner;

/**
 * Clase principal del sistema de ventas.
 * Aplica el concepto de modularidad y encapsulamiento al delegar funciones específicas a cada clase.
 */
public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // Inicializa productos si es la primera vez (encapsulamiento de la lógica en el gestor)
        GestorProductos.inicializarProductos();

        // Autenticación de usuario: uso de la clase AuthManager, ejemplo de abstracción
        while (!AuthManager.autenticar()) {}

        // Menú principal, ejemplo de control de flujo y uso de clases gestoras especializadas
        while (true) {
            System.out.println("\n=== SISTEMA DE VENTAS ===");
            System.out.println("1. Registrar Venta");
            System.out.println("2. Registrar Cliente");
            System.out.println("3. Generar Reporte de Ventas");
            System.out.println("4. Salir (fin de jornada)");
            System.out.print("Opción: ");
            int op = sc.nextInt();
            sc.nextLine();

            switch (op) {
                case 1:
                    // Venta en hilo: ejemplo de concurrencia y encapsulamiento
                    Thread t = new Thread(() -> GestorVentas.registrarVenta());
                    t.start();
                    try { t.join(); } catch (InterruptedException e) {}
                    break;
                case 2:
                    GestorClientes.registrarCliente();
                    break;
                case 3:
                    ReporteManager.generarReporte();
                    break;
                case 4:
                    // Simulación de pedidos automáticos si falta stock
                    GestorPedidos.revisarStockYPedir();
                    System.out.println("¡Jornada terminada!");
                    return;
                default:
                    System.out.println("Opción inválida.");
            }
        }
    }
}
