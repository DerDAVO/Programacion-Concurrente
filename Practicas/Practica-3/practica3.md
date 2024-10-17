# Practica 3 - Monitores
> [!NOTE]
> Los signal(cola) en caso de que no haya un proceso dormindo no surten efecto
<details>
  <summary>1. Se dispone de un puente por el cual puede pasar un solo auto a la vez. Un auto pide permiso para pasar por el puente, cruza por el mismo y luego sigue su camino.</summary>

  ``` java
    Monitor  Puente 
    cond cola;  
    int cant= 0; 
 
    Procedure entrarPuente () 
         while ( cant > 0) wait (cola); 
         cant = cant + 1;    
    end; 
 
    Procedure salirPuente () 
        cant = cant – 1; 
        signal(cola); 
    end; 
End Monitor;  
 
Process Auto [a:1..M] 
   Puente. entrarPuente (a); 
   “el auto cruza el puente” 
   Puente. salirPuente(a); 
End Process; 

  ```
<details>
  <summary>A) ¿El código funciona correctamente?
Justifique su respuesta</summary>

  Este es el contenido que se oculta hasta que haces clic. Puedes agregar texto, imágenes o incluso código aquí.

</details>
<details>
  <summary>B) ¿Se podría simplificar el programa? ¿Sin
monitor? ¿Menos procedimientos? ¿Sin
variable condition? En caso afirmativo,
rescriba el código</summary>

  Este es el contenido que se oculta hasta que haces clic. Puedes agregar texto, imágenes o incluso código aquí.

</details>
<details>
  <summary>C)¿La solución original respeta el orden de
llegada de los vehículos? Si rescribió el código
en el punto b), ¿esa solución respeta el orden
de llegada?</summary>

  Este es el contenido que se oculta hasta que haces clic. Puedes agregar texto, imágenes o incluso código aquí.

</details>
</details>

<details>
    <summary>2. Existen N procesos que deben leer información de una base de datos, la cual es administrada por un motor que admite una cantidad limitada de consultas simultáneas.</summary>
    <details>
    <summary>a) Analice el problema y defina qué procesos, recursos y monitores serán necesarios/convenientes,  además  de  las  posibles  sincronizaciones  requeridas  para resolver el problema.</summary>
    </details>
    <details>
    <summary> b) Implemente el acceso a la base por parte de los procesos, sabiendo que el motor de base de datos puede atender a lo sumo 5 consultas de lectura simultáneas.
    </summary>


``` java
    Monitor MotorBDD{
	int lugares = 0;
	cond cola;
	
	procedure pasar(){
		if(lugares == 5 ){ // si no hay lugar en la BDD me duermo 
			wait(cola);
		}
		// si hay lugar
		lugares++;
	}
	procedure salir(){
			signal(cola);
			// podria otro proceso entrar antes del que desperte ?
			lugares--;	
	}
}
```
``` java
  Process proceso[id:0..N-1]{
	Monitor.pasar();
	leerBaseDeDatos();
	Monitor.salir();

}
```
  </details>
</details>
<details>
  <summary>3.  Existen N personas que deben fotocopiar un documento. La fotocopiadora sólo puede ser 
usada  por  una  persona  a  la  vez.  Analice  el  problema  y  defina  qué  procesos,  recursos  y 
monitores serán necesarios/convenientes, además de las posibles sincronizaciones requeridas 
para resolver el problema. Luego, resuelva considerando las siguientes situaciones:</summary>
<details><summary>a) Implemente  una  solución  suponiendo  no  importa el  orden  de  uso.  Existe  una  función Fotocopiar() que simula el uso de la fotocopiadora.</summary>

``` java
Monitor impresora{
	Procedure Fotocopiar(doc:in Documento,aux:out Fotocopia ){
		aux = fotocopiar(doc);
	}
}
process persona[id:0..N-1]{
	Documento doc;
	Impresora.Fotocopiar(doc);
}
 
```
</details>
<details><summary>b) Modifique la solución de (a) para el caso en que se deba respetar el orden de llegada.</summary>

``` java
Monitor Impresora{
	cond cola;
	boolean libre=true;
	int cant=0;
	
	Procedure pasar(){
		if( !libre ){
			cant++;
			wait(cola);
		}
		libre=false;
	}
	
	Procedure Fotocopiar(doc:in Documento,aux:out Fotocopia ){
		aux = fotocopiar(doc);
	}
	
	procedure salir(){
		if(cant > 0){
			cant--;
			signal(cola);
		}
		libre=true;
	}
	
}


process Persona[id:0..N-1]{
	Fotocopia copia;
	Documento doc;
	
	Impresora.pasar();
	Impresora.Fotocopiar(doc,copia); //persona usando la fotocopiaodora
	Impresora.salir();
}
 
```
</details>
<details><summary>c) Modifique la solución de (b) para el caso en que se deba dar prioridad de acuerdo con la 
edad de cada persona (cuando la fotocopiadora está libre la debe usar la persona de mayor 
edad entre las que estén esperando para usarla).</summary>

``` java
Monitor Impresora{
	cond espera[N];
	boolean libre=true;
	int cant=0;
	colaOrdenada fila;
	int proximo;
	Procedure pasar(id,edad : in int){
		if( !libre ){
			cant++;
			insertar(fila,edad,id); // las personas son agregadas por edad
			wait(espera[id]);
		}
		libre=false;
	}
	
	Procedure Fotocopiar(doc:in Documento,aux:out Fotocopia ){
		aux = fotocopiar(doc);
	}
	
	procedure salir(){
		if(cant > 0){
			cant--;
			sacar(fila,proximo); 
			signal(espera[proximo]);
		}
		libre=true;
	}
	
}

	
process Persona[id:0..N-1]{
	Fotocopia copia;
	Documento doc;
	
	Impresora.pasar();
	Impresora.Fotocopiar(doc,copia); //persona usando la fotocopiaodora
	Impresora.salir();
}
 
```
</details>
<details><summary>d) Modifique la solución de (a) para el caso en que se deba respetar estrictamente el orden dado por el identificador del proceso (la persona X no puede usar la fotocopiadora hasta que no haya terminado de usarla la persona X-1).</summary>
</details>
<details><summary>e) Modifique la solución de (b) para el caso en que además haya un Empleado que le indica a cada persona cuando debe usar la fotocopiadora.</summary>
</details>
<details><summary>f) Modificar la solución (e) para el caso en que sean 10 fotocopiadoras. El empleado le indica a la persona cuál fotocopiadora usar y cuándo hacerlo.</summary>
</details>
</details>

  
