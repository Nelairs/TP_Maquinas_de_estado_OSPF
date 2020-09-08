# TP_Maquinas_de_estado_OSPF
Trabajo practico Informatica II de Santiago Etchenique. 
Tema: Open Short Path First (OSPF).

## Open Short Path First

-   Es ampliamente utilizado y respaldado, esto quiere decir que se encuentra en la mayoria de los routers.
-   Interior Gateway Protocol **IGP**, pensado para un sistema autonomo
-   Utiliza Link-state protocol **LSP**

![](Imagenes_TP/Screenshot%20from%202020-07-24%2019-01-43.png "Conexionado entre routers con OSPF")

El OSPF aprende de cada router y subnet en toda la red, teniendo como resultado que cada router tenga la misma informacion que los demas.

![](/Imagenes_TP/Screenshot%20from2020-07-24%2019-01-50.png "Busqueda de rutas")

Los routers obtienen esta informacion mediante el metodo Link-state Advertisment **LSA** siendo estos **LSA** los que contienen la informacion del router, subnet y otra informacion de la red.

![](/Imagenes_TP/Webp.net-gifmaker.gif "Flood de LSA's")

Una vez que se saturan los LSA cada OSPF guarda la informacion en una Link-state DataBase **LSDB** teniendo como resultado que todos los routers tengan la misma informcacion en su **LSDB**

![](/Imagenes_TP/Screenshot%20from%202020-07-24%2019-02-38.png "LSDB de un router")

##  Los principales pasos son:

-   **Generar vecinos**:    Dos routers con OSPF que esten conectados entre si se ponen de acuerdo para formar una relacion de vecinos.
-   **Intercambiar informacion de la base de datos**:   Los routers vecinos cambian informacion de sus **LSDB's** entre ellos.
-   **Elegir las mejores rutas**:   Cada router elije las mejores rutas para ser agregadas a su tabla de enrutamiento. Esto se lleva a cabo con el algoritmo Dijkstra o conocido como **SPF** luego de corroborar la informacion en sus **LSDB's**.

### Como se forman las relaciones vecinas?

Lo primero antes de establecer la relacion con el router vecino, cada router debe asignarse una router ID **RID**. El **RID** es un numero que sirve para identificar cada router individual, algo asi como los nombres OSPF. Esta **ID** está dada en formato IPv4 y puede ser seteada manualmente o dejar que cada router decida y asigne su ID.

-   **Esta es asignada de esta manera**:
    -   Si fue asignado de manera manual, el router tomara esta ID y la usara.
    -   Si en cambio no fue asignado manualmente, el router va a usar la direccion IP mas alta de la interfaz loopback.
    -   Si no hay interfaces loopback en el router, este asignara la direccion IP mas alta no-loopback.
    -   Esto hace que elija la IP mas alta en el router.

-   Una vez que OSPF esta configurado y tenemos las **RID's**, los routers intentan buscar potenciales vecinos enviando un *Hello* conteniendo informacion importante como el RID de emisor y la lista de vecinos. Este mensaje de saludo tambien sirve para avisar a los vecinos que siguen activos. 

![](/Imagenes_TP/Webp.net-gifmaker%20(1).gif "Estados por los que van pasando los routers")

-   Antes de iniciar el estado *init* en segundo router, este hara unos chequeos de requerimientos:
    -   Area ID. El ID de area tiene que ser igual, esto se usara cuando se escala el OSPF
    -   Subnet. Las conecciones entre los routers tienen que estar en la misma subnet
    -   Intervalo de Hello y Dead. Los tiempos de Hello y Dead tiene que ser iguales en ambos routers
    -   Autentificacion. La autentificacion debe ser la misma si esta esta siendo utilizada
    -   Bandera del area stub. Esta bandera debe ser igual en ambos
    -   RID unica. La ID no puede repetirse

-   Despues del estado **2-way** mostrado anteriormente viene el estado **EXSTART**, pero antes del mismo hay que ver los Router designados y los routers designados de back up o **DRs** y **BDRs** por sus siglas en ingles.
    
    -Estos routers son para que al enviar informacion no se sature la lineas y funcionan de la siguiente manera.
    
-   Armamos una red de seis routers, al no tener **DR** y **BDR** estos se hacen vecinos entre todos. Al caerse uno de los links, este es informado a todos los demas, pero cuando uno recibe, a su vez avisa a sus vecinos, y asi todos se avisan entre todos ya que estan todos escuchando lo que envian sus vecinos y estos retransmitiendo el mensaje, esto hace que la red se sature con mensajes.

![](/Imagenes_TP/Webp.net-gifmaker%20(2).gif "Network con 6 routers y ningun **DR**")

-   Ahora si esta red tiene **DR** y **BDR**, los routers solo se haran *full neighbours* con los **DRs** y **BDRs** haciendo que solo hagan caso a los updates de estos mismos, mientras los demas se mantendran en estado **2-Way**. Los **DR** y **BDR** son seleccionados por la prioridad OSPF mas alta que por defecto es uno, esta puede ser cambiada para influenciar la eleccion. Ahora si la prioridad se encuentra igual en 2 o más routers, la eleccion es basada en el **RID** mas alto.

![](/Imagenes_TP/Webp.net-gifmaker%20(3).gif "Network con 6 routers y **DR**")

### Estado Exchange

-   Luego de que empieze el estado EXCHANGE, los routers entran en estado EXSTART. En este punto se seleccionan un Slave y un Master basados en la **RID**, el MASTER controlara la secuencia de numeros y empezara el proceso EXCHANGE.
-   Ahora se pasa al estado EXCHANGE, los routers se envian las listas de **LSAs** siendo este proceso Database Description **DBD**
-   Despues del **DBD** se pasa al LOADING state donde cada router lista a traves del **DBD** y pediran cualquier informacion que no tengan en su **DB**, esto es hecho de esta manera para que evitar loops de querys de informacion.