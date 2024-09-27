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