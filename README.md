# Proyecto-Laberinto-DFS-y-BFS
Proyecto Final de la materia de Estructura de Datos del grupo 3SB
//Este es el bueno tiene los 2 y funciona
import java.util.*;
class Grafo {
    private int numNodos;
    private int[][] matrizAdyacencia;  
    public Grafo(int numNodos) 
    {
        this.numNodos = numNodos;
        this.matrizAdyacencia = new int[numNodos][numNodos];
    }
    public void agregarArista(int origen, int destino) 
    {
        matrizAdyacencia[origen][destino] = 1;
    }

    public List<Integer> encontrarCaminoPorProfundidad(int inicio, int fin) 
    {
        boolean[] visitado = new boolean[numNodos];
        List<Integer> camino = new ArrayList<>();
        if (busquedaPorProfundidadRecursiva(inicio, fin, visitado, camino)) 
        {
            return camino;
        }
        return null;
    }
    private boolean busquedaPorProfundidadRecursiva(int nodoActual, int fin,boolean[] visitado, List<Integer> camino) 
    {
        visitado[nodoActual] = true;
        camino.add(nodoActual);
        if (nodoActual == fin) 
        {
            return true;
        }
        for (int vecino = 0; vecino < numNodos; vecino++) 
        {
            if (matrizAdyacencia[nodoActual][vecino] == 1 && !visitado[vecino]) {
                if (busquedaPorProfundidadRecursiva(vecino, fin, visitado, camino)) {
                    return true;
                }
            }
        }
        
        camino.remove(camino.size() - 1);
        return false;
    }

    public List<Integer> encontrarCaminoPorAmplitud(int inicio, int fin) {
        boolean[] visitado = new boolean[numNodos];
        int[] padre = new int[numNodos];
        Arrays.fill(padre, -1);

        Queue<Integer> cola = new LinkedList<>();
        cola.add(inicio);
        visitado[inicio] = true;

        while (!cola.isEmpty()) {
            int nodoActual = cola.poll();

            if (nodoActual == fin) {
                // Reconstruir el camino
                List<Integer> camino = new ArrayList<>();
                for (int nodo = fin; nodo != -1; nodo = padre[nodo]) {
                    camino.add(0, nodo);
                }
                return camino;
            }

            // Explorar vecinos
            for (int vecino = 0; vecino < numNodos; vecino++) {
                if (matrizAdyacencia[nodoActual][vecino] == 1 && !visitado[vecino]) {
                    cola.add(vecino);
                    visitado[vecino] = true;
                    padre[vecino] = nodoActual;
                }
            }
        }

        // No se encontró camino
        return null;
    }
}

class Laberinto {
    private int Largo;
    private int Ancho;
    private char Pared = '#';
    private char Camino = ' ';
    private char Inicio = 'E';
    private char Fin = 'S';
    private char Solucion = '-';
    private char[][] Mapa;
    private Random Generador;
    private double ProbabilidadDeVariosCaminos;
    private double ProbabilidadSinSolucion;
    private Grafo grafo;

    public Laberinto(int Largo, int Ancho, double ProbabilidadDeVariosCaminos, double ProbabilidadSinSolucion) {
        this.Largo = Largo;
        this.Ancho = Ancho;
        this.ProbabilidadDeVariosCaminos = ProbabilidadDeVariosCaminos;
        this.ProbabilidadSinSolucion = ProbabilidadSinSolucion;
        Mapa = new char[Largo][Ancho];
        this.Generador = new Random();
        inicializar();
    }

    private void inicializar() {
        for (int x = 0; x < Largo; x++) {
            for (int y = 0; y < Ancho; y++) {
                Mapa[x][y] = Pared;
            }
        }
    }

    public char[][] GenerarLaberinto() {
        int FilaActual = Generador.nextInt(Largo / 2) * 2 + 1;
        int ColumnaActual = Generador.nextInt(Ancho / 2) * 2 + 1;
        int[][] movimientos = {{-2, 0}, {2, 0}, {0, -2}, {0, 2}};
        Mapa[FilaActual][ColumnaActual] = Camino;
        boolean[][] MapaVisitado = new boolean[Largo][Ancho];
        MapaVisitado[FilaActual][ColumnaActual] = true;
        int celdasVisitadas = 1;
        int maximoDeCaminos = (Largo / 2) * (Ancho / 2);
        while (celdasVisitadas < maximoDeCaminos) {
            int[] movimientoElegido = movimientos[Generador.nextInt(4)];
            int NuevaFila = FilaActual + movimientoElegido[0];
            int NuevaColumna = ColumnaActual + movimientoElegido[1];
            if (NuevaFila > 0 && NuevaFila < Largo && NuevaColumna > 0 && NuevaColumna < Ancho) {
                if (!MapaVisitado[NuevaFila][NuevaColumna]) {
                    Mapa[FilaActual + movimientoElegido[0] / 2][ColumnaActual + movimientoElegido[1] / 2] = Camino;
                    Mapa[NuevaFila][NuevaColumna] = Camino;
                    MapaVisitado[NuevaFila][NuevaColumna] = true;
                    celdasVisitadas++;
                }
                FilaActual = NuevaFila;
                ColumnaActual = NuevaColumna;
            }
        }
        Mapa[0][1] = Inicio;
        Mapa[Largo - 1][Ancho - 2] = Fin;
        if (Generador.nextDouble() < ProbabilidadSinSolucion) {
            BloquearCaminos();
        } else {
            VariosCaminos();
        }
        return Mapa;
    }

    private void VariosCaminos() {
        int NumeroDeCaminosADespesjar = (int) (Largo * Ancho * ProbabilidadDeVariosCaminos);
        for (int i = 0; i < NumeroDeCaminosADespesjar; i++) {
            int Fila = Generador.nextInt(Largo);
            int Columna = Generador.nextInt(Ancho);
            if (Mapa[Fila][Columna] == Pared && Mapa[Fila][Columna] != Inicio && Mapa[Fila][Columna] != Fin) {
                Mapa[Fila][Columna] = Camino;
            }
        }
        for (int x = 0; x < Largo; x++) {
            Mapa[x][0] = Pared;
            Mapa[x][Ancho - 1] = Pared;
        }
        for (int y = 0; y < Ancho; y++) {
            Mapa[0][y] = Pared;
            Mapa[Largo - 1][y] = Pared;
        }
        Mapa[0][1] = Inicio;
        Mapa[Largo - 1][Ancho - 2] = Fin;
    }

    private void BloquearCaminos() {
        int NumeroDeCaminosBloqueados = (int) (Largo * Ancho * ProbabilidadSinSolucion);
        for (int i = 0; i < NumeroDeCaminosBloqueados; i++) {
            int Fila = Generador.nextInt(Largo);
            int Columna = Generador.nextInt(Ancho);
            if (Mapa[Fila][Columna] == Camino && Mapa[Fila][Columna] != Inicio && Mapa[Fila][Columna] != Fin) {
                Mapa[Fila][Columna] = Pared;
            }
        }
        for (int x = 0; x < Largo; x++) {
            Mapa[x][0] = Pared;
            Mapa[x][Ancho - 1] = Pared;
        }
        for (int y = 0; y < Ancho; y++) {
            Mapa[0][y] = Pared;
            Mapa[Largo - 1][y] = Pared;
        }
        Mapa[0][1] = Inicio;
        Mapa[Largo - 1][Ancho - 2] = Fin;
    }

    public void ImprimirMapaDelLaberinto() {
        for (int x = 0; x < Largo; x++) {
            for (int y = 0; y < Ancho; y++) {
                System.out.print(Mapa[x][y] + " ");
            }
            System.out.println();
        }
    }

    public void ResolverLaberintoDFS() {
        // Crear grafo usando matriz de adyacencia
        int nodos = Largo * Ancho;
        grafo = new Grafo(nodos);

        // Construir el grafo
        for (int x = 0; x < Largo; x++) {
            for (int y = 0; y < Ancho; y++) {
                if (Mapa[x][y] != Pared) {
                    int nodoActual = x * Ancho + y;

                    // Verificar las cuatro direcciones
                    if (x > 0 && Mapa[x - 1][y] != Pared) {
                        int nodoArriba = (x - 1) * Ancho + y;
                        grafo.agregarArista(nodoActual, nodoArriba);
                    }
                    if (x < Largo - 1 && Mapa[x + 1][y] != Pared) {
                        int nodoAbajo = (x + 1) * Ancho + y;
                        grafo.agregarArista(nodoActual, nodoAbajo);
                    }
                    if (y > 0 && Mapa[x][y - 1] != Pared) {
                        int nodoIzquierda = x * Ancho + (y - 1);
                        grafo.agregarArista(nodoActual, nodoIzquierda);
                    }
                    if (y < Ancho - 1 && Mapa[x][y + 1] != Pared) {
                        int nodoDerecha = x * Ancho + (y + 1);
                        grafo.agregarArista(nodoActual, nodoDerecha);
                    }
                }
            }
        }

        // Encontrar inicio y fin
        int inicio = 0 * Ancho + 1; // Nodo del inicio ('E')
        int fin = (Largo - 1) * Ancho + (Ancho - 2); // Nodo del final ('S')

        // Encontrar camino por profundidad
        List<Integer> camino = grafo.encontrarCaminoPorProfundidad(inicio, fin);

        if (camino != null) {
            for (int nodo : camino) {
                int x = nodo / Ancho;
                int y = nodo % Ancho;
                if (Mapa[x][y] != Inicio && Mapa[x][y] != Fin) {
                    Mapa[x][y] = Solucion;
                }
            }
            System.out.println("\nCamino encontrado y marcado en el laberinto (DFS):\n");
            ImprimirMapaDelLaberinto();
        } else {
            System.out.println("No se encontro un camino desde el inicio hasta el fin (DFS).\n");
        }
    }

    public void ResolverLaberintoBFS() {
        // Crear grafo usando matriz de adyacencia
        int nodos = Largo * Ancho;
        grafo = new Grafo(nodos);

        // Construir el grafo
        for (int x = 0; x < Largo; x++) {
            for (int y = 0; y < Ancho; y++) {
                if (Mapa[x][y] != Pared) {
                    int nodoActual = x * Ancho + y;

                    // Verificar las cuatro direcciones
                    if (x > 0 && Mapa[x - 1][y] != Pared) {
                        int nodoArriba = (x - 1) * Ancho + y;
                        grafo.agregarArista(nodoActual, nodoArriba);
                    }
                    if (x < Largo - 1 && Mapa[x + 1][y] != Pared) {
                        int nodoAbajo = (x + 1) * Ancho + y;
                        grafo.agregarArista(nodoActual, nodoAbajo);
                    }
                    if (y > 0 && Mapa[x][y - 1] != Pared) {
                        int nodoIzquierda = x * Ancho + (y - 1);
                        grafo.agregarArista(nodoActual, nodoIzquierda);
                    }
                    if (y < Ancho - 1 && Mapa[x][y + 1] != Pared) {
                        int nodoDerecha = x * Ancho + (y + 1);
                        grafo.agregarArista(nodoActual, nodoDerecha);
                    }
                }
            }
        }

        // Encontrar inicio y fin
        int inicio = 0 * Ancho + 1; // Nodo del inicio ('E')
        int fin = (Largo - 1) * Ancho + (Ancho - 2); // Nodo del final ('S')

        // Resolver usando amplitud (BFS)
        List<Integer> camino = grafo.encontrarCaminoPorAmplitud(inicio, fin);

        if (camino != null) {
            for (int nodo : camino) {
                int x = nodo / Ancho;
                int y = nodo % Ancho;
                if (Mapa[x][y] != Inicio && Mapa[x][y] != Fin) {
                    Mapa[x][y] = Solucion;
                }
            }
            System.out.println("\nCamino encontrado y marcado en el laberinto (BFS):\n");
            ImprimirMapaDelLaberinto();
        } else {
            System.out.println("No se encontro un camino desde el inicio hasta el fin (BFS).\n");
        }
    }

    public static void main(String[] args) {
        Laberinto LaberintoCreado = new Laberinto(15, 15, 0.45, 0.3);
        char[][] LaberintoGenerado = LaberintoCreado.GenerarLaberinto();
        LaberintoCreado.ImprimirMapaDelLaberinto();

        System.out.println("\nResolviendo el laberinto por DFS:");
        LaberintoCreado.ResolverLaberintoDFS();

        System.out.println("\nResolviendo el laberinto por BFS:");
        LaberintoCreado.ResolverLaberintoBFS();
    }
}
