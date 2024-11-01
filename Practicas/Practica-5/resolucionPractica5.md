# _Practica 5_
<details>
<summary>Se requiere modelar un puente de un único sentido que soporta hasta 5 unidades de peso.
El peso de los vehículos depende del tipo: cada auto pesa 1 unidad, cada camioneta pesa 2
unidades y cada camión 3 unidades. Suponga que hay una cantidad innumerable de
vehículos (A autos, B camionetas y C camiones). Analice el problema y defina qué tareas,
recursos y sincronizaciones serán necesarios/convenientes para resolverlo.</summary>
<details>
<summary> Realice la solución suponiendo que todos los vehículos tienen la misma prioridad.
</summary>

``` Ada
procedure ejercicio1
	task type Camion is 
	begin
	end Camion;
	
	task type Auto is
	begin
	end Auto;
	
	task type Camioneta is
	begin
	end Camioneta;
	
	task Puente is
	begin
		// ENTRYS de task puente
		
		ENTRY salida(peso : IN integer); // Salida de un vehiculo
		
		ENTRY entradaCamion();	
		ENTRY entradaCamioneta();
		ENTRY entradaCamion(); 
		
	end;
	-------------------------------------------------------------
	//Implementacion de las tareas
	Task  body Puente is
		total : integer := 5;
	begin
		SELECT
			accept salida(peso : IN integer);
			begin
				total:= total+peso;
			end salida;
		or when(total >= 2)
			accept entradaCamioneta();
			begin
				total:=total-2;
			end entradaCamioneta;
		or when(total >= 1)
			accept entradaAuto();
			begin
				total:=total-1;
			end entradaAuto;
		or when(total >= 3)
			accept entradaCamion();
			begin
				total:= total-3;
			end entradCamion;
	end SELECT;
	end Puente;
	
	Task body Camion is
	begin
		
		//Puente.entradaCamion(3);// Especifico mi peso en el encuentro ?
		// Estaria mal especificar mi peso en el llamadao ya que las variables 
		// ya que el valor solo lo conoceria si ya se realizo el accept
		Puente.entradaCamion();
		pasoElPuente();
		Puente.salirCamion(3);// Aqui si podria especificar el peso ?
	end;
	
	Task body Camioneta is
	begin
		Puente.entradaComioneta();
		pasoElPuente();
		Puente.salirCamioneta();
	end;
	
	Task body Auto is
	begin
		Puente.entradaAuto();
		pasarElPuente();
		Puente.salirCamioneta();
	end;
	vectorAutos : array (0..A-1) of Auto;
	vectorCamiones : array (0..C-1) of Camion;
	vectorCamionetas : array (0..B-1) of Camionetas;
begin
	// Se le debe asignar su peso a cada uno de los vehiculos ?
end;
```
</details>
<details>
<summary>Modifique la solución para que tengan mayor prioridad los camiones que el resto de los 
vehículos</summary>

``` Ada	
    procedure ejercicio1
	task type Camion is 
	begin
	end Camion;
	
	task type Auto is
	begin
	end Auto;
	
	task type Camioneta is
	begin
	end Camioneta;
	
	task Puente is
	begin
		// ENTRYS de task puente
		
		ENTRY salida(peso : IN integer); // Salida de un vehiculo
		
		ENTRY entradaCamion();	
		ENTRY entradaCamioneta();
		ENTRY entradaCamion(); 
		
	end;
	-------------------------------------------------------------
	//Implementacion de las tareas
	Task  body Puente is
		total : integer := 5;
	begin
		SELECT
			accept salida(peso : IN integer);
			begin
				total:= total+peso;
			end salida;
		or when((total >= 2) and (entradaCamion'count == 0))
			accept entradaCamioneta();
			begin
				total:=total-2;
			end entradaCamioneta;
		or when((total >= 1) and (entradaCamion'count == 0))
			accept entradaAuto();
			begin
				total:=total-1;
			end entradaAuto;
		or when(total >= 3)
			accept entradaCamion();
			begin
				total:= total-3;
			end entradCamion;
	end SELECT;
	end Puente;
	
	Task body Camion is
	begin
		
		//Puente.entradaCamion(3);// Especifico mi peso en el encuentro ?
		// Estaria mal especificar mi peso en el llamadao ya que las variables 
		// ya que el valor solo lo conoceria si ya se realizo el accept
		Puente.entradaCamion();
		pasoElPuente();
		Puente.salirCamion(3);// Aqui si podria especificar el peso ?
	end;
	
	Task body Camioneta is
	begin
		Puente.entradaComioneta();
		pasoElPuente();
		Puente.salirCamioneta();
	end;
	
	Task body Auto is
	begin
		Puente.entradaAuto();
		pasarElPuente();
		Puente.salirCamioneta();
	end;
	vectorAutos : array (0..A-1) of Auto;
	vectorCamiones : array (0..C-1) of Camion;
	vectorCamionetas : array (0..B-1) of Camionetas;
begin
	// Se le debe asignar su peso a cada uno de los vehiculos ?
end;
```
</details>
</details>