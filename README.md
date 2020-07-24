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
