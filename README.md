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
