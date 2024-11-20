# Practica de repaso Memoria Distribuida
## Ejercicio 1
1) En una oficina existen 100 empleados que envían documentos para imprimir en 5 impresoras
compartidas. Los pedidos de impresión son procesados por orden de llegada y se asignan a la primera
impresora que se encuentre libre:

a) Implemente un programa que permita resolver el problema anterior usando PMA.

``` java
    process Empleado[id:0..N-1]{
        Text doc;
        while(true){
            generarDocumento(doc);
            send pedidoDeImpresion(doc);
        }
    }
    process Impresora[id:1..5]{
        Text doc;
        while(true){
            receive pedidoDeImpresion(doc);
            imprimirDocumento(doc);
        }
    }
```
b) Resuelva el mismo problema anterior pero ahora usando PMS

``` java
    process Empleado[id:0..P-1]{
        Text doc;
        while(true){
            generarDocumento(doc);
            Admin!pedido(doc);// El Empleado solo envia los documentos a imprimir 
        }
    }
    process Impresora[id:1..5]{
        while(true){
            Admin!pedido(id);
            Admin?documentoAImprimir(doc);
            imprimirDocumento(doc);
        }
    }
    // Admin se encarga de guardar los documentos en orden de llegada
    process Admin{
        Cola c;
        Text doc;
        int idImpresora;
        while(true){
            do Empleado[*]?pedido(doc)->
                                push(c,doc);
            □ not empty(c);Impresora[*]?pedido(idImpresora)->
                                Impresora[idImpresora]!documentoAImprimir(pop(c,doc));

        }
    }

```
## Ejercicio 2
2) Resolver el siguiente problema con PMS. En la estación de trenes hay una terminal de SUBE que
debe ser usada por P personas de acuerdo con el orden de llegada. Cuando la persona accede a la
terminal, la usa y luego se retira para dejar al siguiente. Nota: cada Persona usa sólo una vez la
terminal.
``` java
    process persona[id:0..P-1]{
        Estacion!solicitarUso(id);
        Estacion?usar();
        //Persona usando la estacion
        Estacion!finUso();

    }

    process Estacion{
        Cola c;
        boolean libre=true;
        for(int i=0; i < P; i++){
            do Persona[*]?solicitarUso(idPersona)->
                        if(libre){
                            Persona[idPersona]!usar();
                            libre=false;
                        }else{
                            push(c,idPersona);
                        }
            □ Persona[*]?finUSo()->
                        if(empty(c)){
                            libre=true;
                        }else{
                            pop(c,idPersona)
                            Persona[idPersona]!usar();
                        }
        }
    }

```
## Ejercicio 3

3) Resolver el siguiente problema con PMA. En un negocio de cobros digitales hay P personas que
deben pasar por la única caja de cobros para realizar el pago de sus boletas. Las personas son
atendidas de acuerdo con el orden de llegada, teniendo prioridad aquellos que deben pagar menos
de 5 boletas de los que pagan más. Adicionalmente, las personas embarazadas tienen prioridad sobre
los dos casos anteriores. Las personas entregan sus boletas al cajero y el dinero de pago; el cajero les
devuelve el vuelto y los recibos de pago.
``` java
```


