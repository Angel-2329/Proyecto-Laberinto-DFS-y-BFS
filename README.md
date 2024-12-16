##  Proyecto Final Laberinto Con Grafos

###  Autores
- Mendez Garcia Angel de Jesus
- Perez Jimenez Santiago Enmanuel
---
### Descripcion
Este proyecto implementa un sistema en Java para generar y resolver laberintos utilizando algoritmos de búsqueda en profundidad (DFS) y búsqueda en amplitud (BFS). El sistema también incluye clases auxiliares para gestionar entradas y excepciones.

------------
### Caracteristicas
-  Generación de Laberintos:
		 Crea laberintos aleatorios con paredes y caminos.
		 Configura probabilidades de múltiples caminos y laberintos sin solución.
		 Generación aleatoria de laberintos con parámetros configurables.
		 Visualización del laberinto en la consola.
		 Configuración flexible del tamaño del laberinto.
- Resolución del Laberinto:
	Dos algoritmos de búsqueda de rutas para resolver laberintos:
		Búsqueda en profundidad (DFS)
		Búsqueda en amplitud (BFS)
- Validación de Entrada:
		Asegura que las dimensiones del laberinto sean números impares entre 3 y 99.
		Manejo de excepciones personalizadas y validación de entradas incorrectas.

------------
### Clases Principales
#### Clase Grafo
- Representa el laberinto como un grafo para realizar búsquedas.
- Descripcion
		Representa el laberinto como una matriz de adyacencia.
		Implementa algoritmos de búsqueda en profundidad y en amplitud.
		Encuentra rutas entre los nodos inicial y final.
- Métodos clave:
  
		AgregarNodo: Conecta nodos en la matriz de adyacencia.
```java
public void AgregarNodo(int Origen, int Destino)
{
	MatrizDeAdyacencia[Origen][Destino] = 1;``
}
```
		EncontrarCaminoPorProfundidad: Implementa DFS para encontrar un camino.
```java
public int[] EncontrarCaminoPorProfundidad(int NodoDeInicio, int NodoFinal) 
 {
	boolean[] TotalDeNodosVisitados = new boolean[NumeroTotalDeTotalDeNodos];
	int[] CaminoASeguir = new int[NumeroTotalDeTotalDeNodos];
	int[] LongitudDelCamino = {0};
	if (BusquedaPorProfundidad(NodoDeInicio, NodoFinal, TotalDeNodosVisitados, CaminoASeguir, LongitudDelCamino)) 
	{
		int[] CaminoEncontrado = new int[LongitudDelCamino[0]];
		System.arraycopy(CaminoASeguir, 0, CaminoEncontrado, 0, LongitudDelCamino[0]);
		return CaminoEncontrado;
	}
	return null;
}
```
		 EncontrarCaminoPorAmplitud: Implementa BFS para encontrar un camino.
```java
public int[] EncontrarCaminoPorAmplitud(int NodoDeInicio, int NodoFinal) 
{
	boolean[] TotalDeNodosVisitados = new boolean[NumeroTotalDeTotalDeNodos];
	int[] NodoPadre = new int[NumeroTotalDeTotalDeNodos];
	int[] NodoCola = new int[NumeroTotalDeTotalDeNodos];
	int NodoDeInicioCola = 0, NodoFinalCola = 0;     
	for (int i = 0; i < NumeroTotalDeTotalDeNodos; i++) 
	 {
		 NodoPadre[i] = -1;
	}
	NodoCola[NodoFinalCola++] = NodoDeInicio;     
	TotalDeNodosVisitados[NodoDeInicio] = true;     
	while (NodoDeInicioCola < NodoFinalCola) 
	{
		 int NodoActual = NodoCola[NodoDeInicioCola++];
		 if (NodoActual == NodoFinal) 
		 {
			int[] CaminoASeguirTemporal = new int[NumeroTotalDeTotalDeNodos];
			int LongitudDelCamino = 0;
			 for (int Nodo = NodoFinal; Nodo != -1; Nodo = NodoPadre[Nodo]) 
			 {
				 CaminoASeguirTemporal[LongitudDelCamino++] = Nodo;
			 }
			 int[] CaminoASeguir = new int[LongitudDelCamino];
			 for (int i = 0; i < LongitudDelCamino; i++) 
			 {
				 CaminoASeguir[i] = CaminoASeguirTemporal[LongitudDelCamino - i - 1];
			 }         
			 return CaminoASeguir;        
		 }   
		for (int NodoVecino = 0; NodoVecino < NumeroTotalDeTotalDeNodos; NodoVecino++)     
		{       
			if (MatrizDeAdyacencia[NodoActual][NodoVecino] == 1 && !TotalDeNodosVisitados[NodoVecino])            
			{               
				NodoCola[NodoFinalCola++] = NodoVecino;              
				TotalDeNodosVisitados[NodoVecino] = true;               
				NodoPadre[NodoVecino] = NodoActual;           
			}        
		}      
	}      
	return null;
}
```
#### Clase Laberinto
- Genera el laberinto y lo resuelve mediante DFS y BFS.
- Descripcion:
		Genera laberintos aleatorios con dimensiones personalizables.
		Métodos para imprimir el diseño del laberinto
		Implementa la resolución de laberintos mediante el recorrido de gráficos.
- Metodos Clave
  
  		GenerarLaberinto: Crea un laberinto con caminos y paredes.
```java
public char[][] GenerarLaberinto() 
   {
      int FilaActual = Generador.nextInt(Largo / 2) * 2 + 1;
      int ColumnaActual = Generador.nextInt(Ancho / 2) * 2 + 1;
      int[][] MovimientosPosibles = {{-2, 0}, {2, 0}, {0, -2}, {0, 2}};
      Mapa[FilaActual][ColumnaActual] = Camino;
      boolean[][] MapaVisitado = new boolean[Largo][Ancho];
      MapaVisitado[FilaActual][ColumnaActual] = true;
      int CeldasVisitadas = 1;
      int MaximoDeCaminosPosibles = (Largo / 2) * (Ancho / 2);
      while (CeldasVisitadas < MaximoDeCaminosPosibles) 
      {
         int[] MovimientoElegido = MovimientosPosibles[Generador.nextInt(4)];
         int NuevaFila = FilaActual + MovimientoElegido[0];
         int NuevaColumna = ColumnaActual + MovimientoElegido[1];
         if (NuevaFila > 0 && NuevaFila < Largo && NuevaColumna > 0 && NuevaColumna < Ancho) 
         {
            if (!MapaVisitado[NuevaFila][NuevaColumna]) 
            {
              Mapa[FilaActual + MovimientoElegido[0] / 2][ColumnaActual + MovimientoElegido[1] / 2] = Camino;
               Mapa[NuevaFila][NuevaColumna] = Camino;
               MapaVisitado[NuevaFila][NuevaColumna] = true;
               CeldasVisitadas++;
            }
            FilaActual = NuevaFila;
            ColumnaActual = NuevaColumna;
         }
      }
      Mapa[0][1] = Inicio;
      Mapa[Largo - 1][Ancho - 2] = Fin;
      if (Generador.nextDouble() < ProbabilidadSinSolucion) 
      {
         BloquearCaminos();
      } 
      else 
      {
         VariosCaminos();
      }
      return Mapa;
   }
```
		ResolverLaberintoDFS: Resuelve el laberinto utilizando DFS.
```java
public void ResolverLaberintoDFS() 
   {
      int TotalDeNodos = Largo * Ancho;
      Grafo = new Grafo(TotalDeNodos);
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            if (Mapa[x][y] != Pared) 
            {
               int NodoActual = x * Ancho + y;
               if (x > 0 && Mapa[x - 1][y] != Pared) 
               {
                  int NodoDeArriba = (x - 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeArriba);
               }
               if (x < Largo - 1 && Mapa[x + 1][y] != Pared) 
               {
                  int NodoDeAbajo = (x + 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeAbajo);
               }
               if (y > 0 && Mapa[x][y - 1] != Pared) 
               {
                  int NodoDeLaIzquierda = x * Ancho + (y - 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaIzquierda);
               }
               if (y < Ancho - 1 && Mapa[x][y + 1] != Pared) 
               {
                  int NodoDeLaDerecha = x * Ancho + (y + 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaDerecha);
               }
            }
         }
      }
      int NodoDeInicio = 0 * Ancho + 1;
      int NodoFinal = (Largo - 1) * Ancho + (Ancho - 2);
      int [] CaminoASeguir = Grafo.EncontrarCaminoPorProfundidad(NodoDeInicio, NodoFinal);
      if (CaminoASeguir != null) 
      {
         for (int Nodo : CaminoASeguir) 
         {
            int x = Nodo / Ancho;
            int y = Nodo % Ancho;
            if (Mapa[x][y] != Inicio && Mapa[x][y] != Fin) 
            {
               Mapa[x][y] = Solucion;
            }
         }
         System.out.println("\nCamino encontrado y marcado en el laberinto (DFS):\n");
         ImprimirMapaDelLaberinto();
      } 
      else 
      {
   System.out.println("No se encontro un CaminoASeguir desde el NodoDeInicio hasta el NodoFinal (DFS).\n");
      }
   }
```
		ResolverLaberintoBFS: Resuelve el laberinto utilizando BFS.
```java
public void ResolverLaberintoBFS() 
   {
      int TotalDeNodos = Largo * Ancho;
      Grafo = new Grafo(TotalDeNodos);
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            if (Mapa[x][y] != Pared) 
            {
               int NodoActual = x * Ancho + y;
               if (x > 0 && Mapa[x - 1][y] != Pared) 
               {
                  int NodoDeArriba = (x - 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeArriba);
               }
               if (x < Largo - 1 && Mapa[x + 1][y] != Pared) 
               {
                  int NodoDeAbajo = (x + 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeAbajo);
               }
               if (y > 0 && Mapa[x][y - 1] != Pared) 
               {
                  int NodoDeLaIzquierda = x * Ancho + (y - 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaIzquierda);
               }
               if (y < Ancho - 1 && Mapa[x][y + 1] != Pared) 
               {
                  int NodoDeLaDerecha = x * Ancho + (y + 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaDerecha);
               }
            }
         }
      }
      int NodoDeInicio = 0 * Ancho + 1;
      int NodoFinal = (Largo - 1) * Ancho + (Ancho - 2);
      int[] CaminoASeguir = Grafo.EncontrarCaminoPorAmplitud(NodoDeInicio, NodoFinal);
      if (CaminoASeguir != null) 
      {
         for (int Nodo : CaminoASeguir) 
         {
            int x = Nodo / Ancho;
            int y = Nodo % Ancho;
            if (Mapa[x][y] != Inicio && Mapa[x][y] != Fin) 
            {
               Mapa[x][y] = Solucion;
            }
         }
         System.out.println("\nCamino encontrado y marcado en el laberinto (BFS):\n");
         ImprimirMapaDelLaberinto();
      } 
      else 
      {
System.out.println("No se encontro un CaminoASeguir desde el NodoDeInicio hasta el NodoFinal (BFS).\n");
      }
   }
```
		ImprimirMapaDelLaberinto: Muestra el laberinto en consola.
```java
public void ImprimirMapaDelLaberinto() 
   {
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            System.out.print(Mapa[x][y] + " ");
         }
         System.out.println();
      }
   }
```
#### Clase Lectura
- Gestiona la entrada del usuario, validando números enteros impares.
- Maneja la validación de la entrada del usuario
- Garantiza una entrada de tamaño de laberinto válida

#### Clase Excepcion
- Define excepciones personalizadas para manejar errores de entrada.
- Proporciona manejo de excepciones personalizado para la validación de entrada

----

### Parámetros de generación del laberinto

- Dimensiones del laberinto: Número impar definido por el usuario
- Probabilidad de caminos múltiples: 0,55
- Probabilidad de no solución: 0,1

----

### Como ejecutarlo

- Compilar los archivos Java
- Ejecutar el archivo Proyecto Final Laberinto 3SB
- Introduzca un número impar entre 3 y 99 para las dimensiones del laberinto
  
![Captura de pantalla 2024-12-15 221015](https://github.com/user-attachments/assets/57c3f5ab-3cce-491f-bd7c-f510ff171972)

- Salida
  
![Captura de pantalla 2024-12-15 222024](https://github.com/user-attachments/assets/3db5ce4c-fcaa-4088-9117-a5d42c8ce4e0)

- Solucion por BFS y DFS
  
![Captura de pantalla 2024-12-15 222124](https://github.com/user-attachments/assets/6d53fbf9-fe29-42f1-be15-6823e59c0874)

------------

### Representación del laberinto

-  Muro : #
- Camino : " "
- Punto de entrada : E
- Punto de salida : S
- Ruta de la solución : "-"


------------

### Algoritmos de búsqueda de rutas
#####  Búsqueda en profundidad (DFS)

		 Explora lo más lejos posible a lo largo de cada rama antes de retroceder.
		 Puede que no encuentre el camino más corto

##### Búsqueda en amplitud (BFS)

		Explora todos los nodos vecinos en la profundidad actual antes de pasar a los nodos en el siguiente nivel de profundidad.
		Garantiza la ruta más corta en un gráfico no ponderado

------------


### Requisitos

		Kit de desarrollo de Java (JDK) 8 o superior
		Interfaz de línea de comandos o IDE con soporte para Java
		

------------

### Codigo Completo

```java
import java.util.*;
class Grafo 
{
   private int NumeroTotalDeTotalDeNodos;
   private int[][] MatrizDeAdyacencia;  
   public Grafo(int NumeroTotalDeTotalDeNodos) 
   {
      this.NumeroTotalDeTotalDeNodos = NumeroTotalDeTotalDeNodos;
      this.MatrizDeAdyacencia = new int[NumeroTotalDeTotalDeNodos][NumeroTotalDeTotalDeNodos];
   }
   public void AgregarNodo(int Origen, int Destino) 
   {
      MatrizDeAdyacencia[Origen][Destino] = 1;
   }
   public int[] EncontrarCaminoPorProfundidad(int NodoDeInicio, int NodoFinal) 
   {
      boolean[] TotalDeNodosVisitados = new boolean[NumeroTotalDeTotalDeNodos];
      int[] CaminoASeguir = new int[NumeroTotalDeTotalDeNodos];
      int[] LongitudDelCamino = {0};
      if (BusquedaPorProfundidad(NodoDeInicio, NodoFinal, TotalDeNodosVisitados, CaminoASeguir, LongitudDelCamino)) 
      {
         int[] CaminoEncontrado = new int[LongitudDelCamino[0]];
         System.arraycopy(CaminoASeguir, 0, CaminoEncontrado, 0, LongitudDelCamino[0]);
         return CaminoEncontrado;
      }
      return null;
   }
   private boolean BusquedaPorProfundidad(int NodoActual, int NodoFinal, boolean[] TotalDeNodosVisitados, int[] CaminoASeguir, int[] LongitudDelCamino) 
   {
      TotalDeNodosVisitados[NodoActual] = true;
      CaminoASeguir[LongitudDelCamino[0]++] = NodoActual;
      if (NodoActual == NodoFinal) 
      {
         return true;
      }
      for (int NodoVecino = 0; NodoVecino < NumeroTotalDeTotalDeNodos; NodoVecino++) 
      {
         if (MatrizDeAdyacencia[NodoActual][NodoVecino] == 1 && !TotalDeNodosVisitados[NodoVecino]) 
         {
            if (BusquedaPorProfundidad(NodoVecino, NodoFinal, TotalDeNodosVisitados, CaminoASeguir, LongitudDelCamino)) 
            {
               return true;
            }
         }
      }
      LongitudDelCamino[0]--;
      TotalDeNodosVisitados[NodoActual] = false;
      return false;
   }
   public int[] EncontrarCaminoPorAmplitud(int NodoDeInicio, int NodoFinal) 
   {
      boolean[] TotalDeNodosVisitados = new boolean[NumeroTotalDeTotalDeNodos];
      int[] NodoPadre = new int[NumeroTotalDeTotalDeNodos];
      int[] NodoCola = new int[NumeroTotalDeTotalDeNodos];
      int NodoDeInicioCola = 0, NodoFinalCola = 0;
      for (int i = 0; i < NumeroTotalDeTotalDeNodos; i++) 
      {
         NodoPadre[i] = -1;
      }
      NodoCola[NodoFinalCola++] = NodoDeInicio;
      TotalDeNodosVisitados[NodoDeInicio] = true;
      while (NodoDeInicioCola < NodoFinalCola) 
      {
         int NodoActual = NodoCola[NodoDeInicioCola++];
         if (NodoActual == NodoFinal) 
         {
            int[] CaminoASeguirTemporal = new int[NumeroTotalDeTotalDeNodos];
            int LongitudDelCamino = 0;
            for (int Nodo = NodoFinal; Nodo != -1; Nodo = NodoPadre[Nodo]) 
            {
               CaminoASeguirTemporal[LongitudDelCamino++] = Nodo;
            }
            int[] CaminoASeguir = new int[LongitudDelCamino];
            for (int i = 0; i < LongitudDelCamino; i++) 
            {
               CaminoASeguir[i] = CaminoASeguirTemporal[LongitudDelCamino - i - 1];
            }
            return CaminoASeguir;
         }
         for (int NodoVecino = 0; NodoVecino < NumeroTotalDeTotalDeNodos; NodoVecino++) 
         {
            if (MatrizDeAdyacencia[NodoActual][NodoVecino] == 1 && !TotalDeNodosVisitados[NodoVecino]) 
            {
               NodoCola[NodoFinalCola++] = NodoVecino;
               TotalDeNodosVisitados[NodoVecino] = true;
               NodoPadre[NodoVecino] = NodoActual;
            }
         }
      }
      return null;
   }
}
class Laberinto 
{
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
   private Grafo Grafo;
   public Laberinto(int Largo, int Ancho, double ProbabilidadDeVariosCaminos, double ProbabilidadSinSolucion) 
   {
      this.Largo = Largo;
      this.Ancho = Ancho;
      this.ProbabilidadDeVariosCaminos = ProbabilidadDeVariosCaminos;
      this.ProbabilidadSinSolucion = ProbabilidadSinSolucion;
      Mapa = new char[Largo][Ancho];
      this.Generador = new Random();
      InicializarLaberinto();
   }
   private void InicializarLaberinto() 
   {
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            Mapa[x][y] = Pared;
         }
      }
   }
   public char[][] GenerarLaberinto() 
   {
      int FilaActual = Generador.nextInt(Largo / 2) * 2 + 1;
      int ColumnaActual = Generador.nextInt(Ancho / 2) * 2 + 1;
      int[][] MovimientosPosibles = {{-2, 0}, {2, 0}, {0, -2}, {0, 2}};
      Mapa[FilaActual][ColumnaActual] = Camino;
      boolean[][] MapaVisitado = new boolean[Largo][Ancho];
      MapaVisitado[FilaActual][ColumnaActual] = true;
      int CeldasVisitadas = 1;
      int MaximoDeCaminosPosibles = (Largo / 2) * (Ancho / 2);
      while (CeldasVisitadas < MaximoDeCaminosPosibles) 
      {
         int[] MovimientoElegido = MovimientosPosibles[Generador.nextInt(4)];
         int NuevaFila = FilaActual + MovimientoElegido[0];
         int NuevaColumna = ColumnaActual + MovimientoElegido[1];
         if (NuevaFila > 0 && NuevaFila < Largo && NuevaColumna > 0 && NuevaColumna < Ancho) 
         {
            if (!MapaVisitado[NuevaFila][NuevaColumna]) 
            {
               Mapa[FilaActual + MovimientoElegido[0] / 2][ColumnaActual + MovimientoElegido[1] / 2] = Camino;
               Mapa[NuevaFila][NuevaColumna] = Camino;
               MapaVisitado[NuevaFila][NuevaColumna] = true;
               CeldasVisitadas++;
            }
            FilaActual = NuevaFila;
            ColumnaActual = NuevaColumna;
         }
      }
      Mapa[0][1] = Inicio;
      Mapa[Largo - 1][Ancho - 2] = Fin;
      if (Generador.nextDouble() < ProbabilidadSinSolucion) 
      {
         BloquearCaminos();
      } 
      else 
      {
         VariosCaminos();
      }
      return Mapa;
   }
   private void VariosCaminos() 
   {
      int NumeroDeCaminosADespesjar = (int) (Largo * Ancho * ProbabilidadDeVariosCaminos);
      for (int i = 0; i < NumeroDeCaminosADespesjar; i++) 
      {
         int Fila = Generador.nextInt(Largo);
         int Columna = Generador.nextInt(Ancho);
         if (Mapa[Fila][Columna] == Pared && Mapa[Fila][Columna] != Inicio && Mapa[Fila][Columna] != Fin) 
         {
            Mapa[Fila][Columna] = Camino;
         }
      }
      for (int x = 0; x < Largo; x++) 
      {
         Mapa[x][0] = Pared;
         Mapa[x][Ancho - 1] = Pared;
      }
      for (int y = 0; y < Ancho; y++) 
      {
         Mapa[0][y] = Pared;
         Mapa[Largo - 1][y] = Pared;
      }
      Mapa[0][1] = Inicio;
      Mapa[Largo - 1][Ancho - 2] = Fin;
   }
   private void BloquearCaminos() 
   {
      int NumeroDeCaminosBloqueados = (int) (Largo * Ancho * ProbabilidadSinSolucion);
      for (int i = 0; i < NumeroDeCaminosBloqueados; i++) 
      {
         int Fila = Generador.nextInt(Largo);
         int Columna = Generador.nextInt(Ancho);
         if (Mapa[Fila][Columna] == Camino && Mapa[Fila][Columna] != Inicio && Mapa[Fila][Columna] != Fin) 
         {
            Mapa[Fila][Columna] = Pared;
         }
      }
      for (int x = 0; x < Largo; x++) 
      {
            Mapa[x][0] = Pared;
            Mapa[x][Ancho - 1] = Pared;
      }
      for (int y = 0; y < Ancho; y++) 
      {
            Mapa[0][y] = Pared;
            Mapa[Largo - 1][y] = Pared;
      }
      Mapa[0][1] = Inicio;
      Mapa[Largo - 1][Ancho - 2] = Fin;
   }
   public void ImprimirMapaDelLaberinto() 
   {
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            System.out.print(Mapa[x][y] + " ");
         }
         System.out.println();
      }
   }
   public void ResolverLaberintoDFS() 
   {
      int TotalDeNodos = Largo * Ancho;
      Grafo = new Grafo(TotalDeNodos);
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            if (Mapa[x][y] != Pared) 
            {
               int NodoActual = x * Ancho + y;
               if (x > 0 && Mapa[x - 1][y] != Pared) 
               {
                  int NodoDeArriba = (x - 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeArriba);
               }
               if (x < Largo - 1 && Mapa[x + 1][y] != Pared) 
               {
                  int NodoDeAbajo = (x + 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeAbajo);
               }
               if (y > 0 && Mapa[x][y - 1] != Pared) 
               {
                  int NodoDeLaIzquierda = x * Ancho + (y - 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaIzquierda);
               }
               if (y < Ancho - 1 && Mapa[x][y + 1] != Pared) 
               {
                  int NodoDeLaDerecha = x * Ancho + (y + 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaDerecha);
               }
            }
         }
      }
      int NodoDeInicio = 0 * Ancho + 1;
      int NodoFinal = (Largo - 1) * Ancho + (Ancho - 2);
      int [] CaminoASeguir = Grafo.EncontrarCaminoPorProfundidad(NodoDeInicio, NodoFinal);
      if (CaminoASeguir != null) 
      {
         for (int Nodo : CaminoASeguir) 
         {
            int x = Nodo / Ancho;
            int y = Nodo % Ancho;
            if (Mapa[x][y] != Inicio && Mapa[x][y] != Fin) 
            {
               Mapa[x][y] = Solucion;
            }
         }
         System.out.println("\nCamino encontrado y marcado en el laberinto (DFS):\n");
         ImprimirMapaDelLaberinto();
      } 
      else 
      {
         System.out.println("No se encontro un CaminoASeguir desde el NodoDeInicio hasta el NodoFinal (DFS).\n");
      }
   }
   public void ResolverLaberintoBFS() 
   {
      int TotalDeNodos = Largo * Ancho;
      Grafo = new Grafo(TotalDeNodos);
      for (int x = 0; x < Largo; x++) 
      {
         for (int y = 0; y < Ancho; y++) 
         {
            if (Mapa[x][y] != Pared) 
            {
               int NodoActual = x * Ancho + y;
               if (x > 0 && Mapa[x - 1][y] != Pared) 
               {
                  int NodoDeArriba = (x - 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeArriba);
               }
               if (x < Largo - 1 && Mapa[x + 1][y] != Pared) 
               {
                  int NodoDeAbajo = (x + 1) * Ancho + y;
                  Grafo.AgregarNodo(NodoActual, NodoDeAbajo);
               }
               if (y > 0 && Mapa[x][y - 1] != Pared) 
               {
                  int NodoDeLaIzquierda = x * Ancho + (y - 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaIzquierda);
               }
               if (y < Ancho - 1 && Mapa[x][y + 1] != Pared) 
               {
                  int NodoDeLaDerecha = x * Ancho + (y + 1);
                  Grafo.AgregarNodo(NodoActual, NodoDeLaDerecha);
               }
            }
         }
      }
      int NodoDeInicio = 0 * Ancho + 1;
      int NodoFinal = (Largo - 1) * Ancho + (Ancho - 2);
      int[] CaminoASeguir = Grafo.EncontrarCaminoPorAmplitud(NodoDeInicio, NodoFinal);
      if (CaminoASeguir != null) 
      {
         for (int Nodo : CaminoASeguir) 
         {
            int x = Nodo / Ancho;
            int y = Nodo % Ancho;
            if (Mapa[x][y] != Inicio && Mapa[x][y] != Fin) 
            {
               Mapa[x][y] = Solucion;
            }
         }
         System.out.println("\nCamino encontrado y marcado en el laberinto (BFS):\n");
         ImprimirMapaDelLaberinto();
      } 
      else 
      {
         System.out.println("No se encontro un CaminoASeguir desde el NodoDeInicio hasta el NodoFinal (BFS).\n");
      }
   }
   public static void main(String[] args) throws Exception
   {
      Scanner leer = new Scanner(System.in);
      Lectura Lector = new Lectura();
      System.out.println("Ingresa Un Numero Impar Para El Largo Y Ancho Del Laberinto");
      int respuesta = Lector.NumeroEntero(leer);
      while (respuesta > 100 || respuesta <= 1 || respuesta % 2 == 0) 
      {
         System.out.println("\nPor Favor Ingresa Un Numero Entero Impar Entre 3 Y 99");
         respuesta = Lector.NumeroEntero(leer);
      }
      Laberinto LaberintoCreado = new Laberinto(respuesta, respuesta, 0.55, 0.1);
      System.out.println("\nEl laberinto Generado es:");
      char[][] LaberintoGenerado = LaberintoCreado.GenerarLaberinto();
      LaberintoCreado.ImprimirMapaDelLaberinto();
      System.out.println("\nResolviendo el laberinto por DFS:");
      LaberintoCreado.ResolverLaberintoDFS();
      System.out.println("\nResolviendo el laberinto por BFS:");
      LaberintoCreado.ResolverLaberintoBFS();
   }
}
class Excepciones extends Exception 
{
    Excepciones ()
    {
        super ();
    }
    public Excepciones(String mensaje) 
    {
        super(mensaje);
    }
}
class Lectura 
{
   public Lectura ()
   {
   }
   public int NumeroEntero(Scanner leer) throws Exception 
    {
        int num = -23;
        try
        {
            System.out.println ("Escribe Un Numero Entero Impar");
            num = leer.nextInt();
            leer.nextLine();
            if ( num < 0)
            {
                throw new Excepciones ();
            }
        } 
        catch(Excepciones e)
        {
            System.out.println ("Intenta Nuevamente, Escribe Un Numero Entero Impar Que No Sea Menor O Igual A 0");
            System.err.print(e);
        } 
        catch (InputMismatchException e) 
        {
            System.out.println ("Por Favor Escribe Un Numero Entero, Ningun Otro Formato Sera Aceptado");
            leer.next();
            System.err.print(e);
        }
        return num;  
    }   
}
```

