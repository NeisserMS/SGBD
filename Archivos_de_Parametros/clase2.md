
##### ARQUITECTURA DE ORACLE DATABASE

![ARQUITECTURA](imagenes/arquitectura.png)

Por lo general cuando decimos Base de datos nos referimos al almacenamiento fisico donde se guarda los datos y cuando hablamos de instancia nos referimos al espacio de memoria  que se reserva al momento cuando iniciamos la base de datos. Se requiere un espacio de memoria por el motivo de que cuando queramos realizar una modificación y tratar los datos, necesitamos recuperarlos del disco, guardalos a memoria, realizar las modificaciones y luego volver a guardarlos y ademas necesitamos que se inicie ciertos procesos.

¿Qué tipos de procesos?

procesos que nos permitiran realizar todas las operaciones para q la BD funcione de forma adecuada. Cuando hablamos de instancia nos referimos de memoria y los procesos que trabajaen en conjunto para que la BD pueda recuperar o almacenar datos en disco.

Para que quede claro hemos creado una base de datos de instancia única, esto quiere decir q por cada BD especifica que tengas se va a iniciar una instancia. ¿Podemos la misma BD inciarla con diferentes estructuras de memoria o instancias?
Sí podemos, porque la instancia justamente lo define, por ejemplo yo reservo 500 megas para una BD, pero puede ser que esa misma BD lo incie con un espacio mayor e inclusive internamente la memoria tiene ciertos contenedores que nos permiten justamente el funcionamiento de la BD, en cuanto a los procesos tambien, porque cuando definimos la estructura de mememoria con la que incia la BD los procesos tambien van a trabajar.

##### EL DICCIONARIO DE DATOS

Cuando nosotros creamos una base de datos la finalidad es almacenar datos y realizar ciertas operaciones, entonces necesitamos tener el espacio en fisico para alamcenar estos datos antes de guardarlos a disco se tiene q almacenar temporalmente en la memoria donde realizamos la modificación y depende de los algoritmos, ciertos procesos que trabajan y luego se guardan en disco. Además también necesitamos grabar la metadata, es decir las deficiones acerca de los datos q vamos a almacenar, por ejemplo podemos tener el id del empleado, el apellido, el nombre, genero, etc. Entonces necesitamos tener estructuras que nos permitan almacenar los datos (001, jair, moreno, masculino) pero también necesitamos estrcturas para alacenar (id, nombre, apellido, sexo).

![METADATOS](imagenes/metadatos.png)


##### ¿Qué vistas existen para poder acceder al diccionario de datos?

![VISTAS](imagenes/VISTAS.png)

 Aquí hablamos de dos especificos:

- Vista de rendimeinto dinamico: Son aquellos que nos daran información acerca del funcionamiento de la base de datos, entonces todas estas variables también necesitamos almacenarlas, se almacena en las tablas del sistema, ¿a quien le pertenece estas tablas (esta informacion), osea a cual de los esquemas?

Recordemos que tenemos:

- Esquemas del usuario Sys
- Esquemas del usuario system
- Esquemas de ejemplos que agregamos como esquemas de ejemplos.

¿Entonces volviendo a la pregunta, en que esquema se almacena o mejor dicho a que usuario le pertenece este tipo de información?

Le pertenece al dba : administrador de base de datos. 


Entonces tenemos:

- Vistas Rendimiento dinámico: 
Asi como estadisticas, partes de memorias y otros. Entonces cuando hablamos de las vistas vemos el funcionamiento, entonces cuando nosotros queremos saber el nombre de nuestra base de datos, ¿a que tipo de vista estoy accediendo?, en este caso estoy accediendo a la vista de rendimiento dinamico. 

- Vistas Estructura de objetos:

En este caso nos brinda información de los objetos de la base de datos y aquí nosotros lo manejamos con niveles, dependiendo el usuario que accede (el q realice la consulta y el prefijo que se emplee) entonces se nos brinda la información.

Se maneja de la siguiente forma: 

Tenemos `user_xxx`  que quiere decir que la xxx lo vamos a reemplazar por el nombre del objeto, digamos que son tablas por ejemplo, entonces queremos acceder a la informacion de las tablas de la base de datos, entonces para eso usaremos el prefijo user_servicies. Siempre el objeto va a ir en plural, entonces si yo como usuario, cualquier usuario de mi base de datos, quiero acceder a las tablas que me pertenece entonces empleo el prefijo user para realizar la consulta.

Si como usuario accedo con el prefijo `all_xxx`, quiere decir que voy a poder observar las tablas que me pertenecen como los objetos que me pertenecen  a los cuales tengo permiso de acceder, entonces el all_xxx siempre se encuentra por encima o incluye al user.

Entonces el `dba_xxx` es el que tiene el completo control de la base de datos, entonces el dba va a poder acceder a la información que nos de el all y el user.

Entonces cuando usemos el prefijo dba solo debe de ser un usuario que sea adm de base de datos, es decir que cuando cuando nos conectamos empleamos sys_dba, que eso nos indica que el usuario tiene categoria de adm de base de datos.

Cuando nosotros empleamos rendimiento dinamico, queremos acceder a las vistas de rendimiento dinamico, casi siempre empezamos con v$ y luego el termino que viene esta relacionado con la estructura a la cual queremos acceder. 

##### COMANDOS

> sqlplus /nolog
> conn sys/oracle@pro as sysdba

una vista de rendimiento dinamico es:
> v$database

Entonces, ¿cómo se cual es la estructura de una tabla, campOs  o una lista?

> DESCRIBE v$database

Entonces oracle nos muestra la vista de rendimiento y la informacion es de la base de datos, por ejemplo el ID de la base de datos, el nombre. Esta es información de la base de datos. Entonces si en algún momento queremos saber el modo de apertura de la BD ahi esta open mode por ejemplo.

Si queremos saber el nombre de la base de datos:

> select name from v$database;

Esto es con respecto a las vistas de rendimiento, igual si queremos consultar a alguna especifica, podemos ir a la guia de referencia de oracle y ahí nos brinda la información detallada de cada una de las vistas.

También podemos consultar información acerca de la instancia:

> desc v$instance

Si nos damos cuenta en las vistas de rendimiento dinamico va en singular, en cambio en la vista de los objetos va en plural. // desc es igual a describe.

Entonces con ese comando nos muestra los campos que nos brinda alguna informacion de la instancia, como el numero de instancia, nombre, host, version, etc, y por ejemplo tbn esta el estado de la BD.

Esto es con respecto a las vistas de rendimiento dinamico. Entonces cuando veamos que se accede a alguna vista que empieza con v$ ya sabemos que corresponde a la vista de rendimiento dinamico. 

¿Cómo se con que usuario estoy conectado?

> SHOW USER

Ahora como hemos agregado los esquemas de ejemplos. vamos a acceder como ese usuario, como por ejemplo el usuario scott.

Abrimos otro terminal donde nos conectemos como scrott o hr, que son los esquemas de ejemplos que nos proporciona oracle, que son base de datos de ejemplo que también tienen tablas de diferentes objetos.

Entonces nos conectamos con el usuario scott, para ello escribimos:

> SQLPLUS /NOLOG
> conn scott/tiger@pro

NT: conn es lo mismo que connect, luego va el usuario/la contraseña@la base de datos a la cual queremos acceder y no lo colocamos dba porq no es un administrador de base de datos. Scott si tiene una contraseña que es tiger y hr tiene una contraseña llamada hrn, scott fue creado por un empleado de oracle.

ERROR: ORA-28000: the account is locked
nos dice que el usuario esta bloqueado. 

Claro desde la nueva ventana no se puede porque es otro usuario, entonces pasamos a la otra ventana del usuarios sys y escribimos lo siguiente para desbloquearlo:

> alter user scott account unlock;

ya con esto nos aparece usuario modificado (desbloqueado) y ahora si intento conectarme como scott en el otro cmd: `conn scott/tiger@pro`y me pide cambiar contraseña, le ponemos una.

Pero si no nos aparece lo de colocar nueva contraseña, entonces escribimos en la consola SYS:

> alter user scott identified by sistemas;

Cuando nosotros nos conectamos windows detecta que nosotros como usuarios hemos creado la base de datos  nos conectamos al servidor, no nos pide que tenga contraseña, mosotros podemos ingresar directamente, vamos a probarlo.

Para desconectarnos escribimos:  `SQL> diconnect`

Nos volvemos a conectar de otra manera:
> conn / as sysdba

Y como vemos no nos a pedido contraseña, y nos conectamos de manera normal, porque estpa detectando que este usuario a creado la base de datosy nos conectamos de forma directa (de forma remota no permite).

NT: Esto es una vulnerabilidad, en linux no sucede, tengamoslo en cuenta!

Entonces nos podemos conectar con solo indicar q somos adm de base de datos.

Comando para limpiar Pantalla: `SQL> HOST CLS`

Por otro lado con respecto a las instrucciones, se clasifican instrucciones de `SQL` y instrucciones de `PLSQL` y `SQLPLUS`.

Un pequeño ejemplo es cuando indicamos la instrucción de tipo SELECT:

> select name from v$database

Estás instrucciones permite que va directo al servidor, nos conectamos y recuperamos la información, por ejemplo. Este tipo de instruccion viene del lenguaje de `SQL` (lo sabemos porque estamos usando service), todas aquellas que viene de SQL para que sean enviadas al servidor es necesario q coloquemos el ";" , no interesa en cuanta lineas lo escribamos, solo interesa q este el ; porq esto indica que ahí termina la instruccion, entonces se envia y recupera la información. Si no ponemos el ; este jamas llegará al servidor.

Ahora las instrucciones `PLSQL`, son aquellas que nos permiten programar en un servidor base de datos. Este tipo de instrucciones por lo general se trabaja en bloques y estos también requieren que termine en ;

Pero también existe instrucciones que nunca llega de cierta manera al servidor, porque son comandos que solo se trabaja en el entorno de SQLPLUS, como por ejemplo: `describe` o `connect` estas no necesitan ";" no requiere.

> NT: el / accede al backup (repite la ultima consulta).

Seguimso con las estructuras de los objetos: 
Ya nos conectamos con el usuario scott y queremos ahora conectarnos a las tablas:

> SQL> DESCRIBE USER_TABLES

Escribimos describe, a la vista user porque justamente somos un usuario que quiero acceder a mis tablas, a los objetos que me pertence, luego indico el nombre del objeto TABLES, con esto indicamos que queremos ver los campos que tienen dicha vista.

Ahora si queremos acceder al nombre de las tablas tenemos ahí el TABLE NAME que aparece en la lista de la consulta de la vista.

Quiero saber el nombre de las tablas que pertenece al usuario scott.

> SQL> SELECT TABLE_NAME FROM USER_TABLES;

Nos aparecerá que tiene las tablas: dept, emp, bonus, salgrade.

Ahora quiero acceder no solo a las tablas que me pertenecen, si no también a las que tengo permiso de uso, algunos de los usuarios me dio permiso sobre algunos objetos, asi que queremos las tablas.

> SQL> SELECT TABLE_NAME FROM ALL_TABLES;

Y nos aparecerá toda una lista, 103 filas seleccionadas, alrededor de 99 tablas tiene permiso el usuario scott.

Ahora quiero visualizar todas las tablas de toda la base de datos, pero scott no podrá porque no tiene permiso.

> SQL > SELECT TABLE_NAME FROM DBA_TABLES;

Nos dirá que la tabla o vista no existe, pero si lo hacemos desde el cmd de SYS veremos que si aparece ya que el es el adm de la BD. Nos muestra 2783 filas seleccionada, pero esto se hace usando el prefijo DBA.

























