### ¿QUÉ ES UNDO?

![GitHub Logo](img/undo.png)

- Información usada para deshacer los cambios en la base de datos, pero en la data, y si existiera cambios en la estructura de la bd se guarda en los archivos de control. También se podría decir deshacer las transacciones que no han sido transformadas.

- Copia de datos originales, nosotros sabemos que los datos se encuentran en los datafiles, fisicamente ahí y es necesario hacer una copia cada vez que se lee estos archivos.

- Capturado por cada transacción que cambia datos

![GitHub Logo](img/quees.png)

Los tablespace soporta estas operaciones:

 - Las operaciones Rollback nos permite deshacer
 - Consistencia de Lectura
 - Recuperación de transacciones fallidas

 ### TRANSACCIONES Y DATOS UNDO

 Cada vez que realizemos alguna operación que puede ser actualización o transacción de ciertos regitros, en este ejemplo se ve que realizemos algunso cambios, ese cambio es ese pequeño bloque imagen nuevo, sin embargo la imagen nueva se va a guardar en el segmento Undo.

Si la transacción no la finaliza correctamente no debería aparecer que ese asiento a sido ocupado, a esto se denomina consistencia en la lecttura.

![GitHub Logo](img/transacciones.png)


### TAREAS DE GESTIÓN DE TABLESPACE UNDO

Visualizar el nombre del tablespace UNDO de la base de datos:

> SHOW PARAMETER UNDO_TABLESPACE  // Con esto podemos saber el tablespace que emplea la base de datos, siempre lo va a tener.

Visualizar tipo de gestión de tablespace UNDO (Manual/Automática):

> SHOW PARAMETER UNDO_MANAGEMENT  // lo adecuado es que sea automático

Modificar tablespace UNDO de base de datos:

> ALTER SYSTEM SET UNDO_TABLESPACE=nombreTBUndo;

Nosotros podemos crear un tablespace de tipo undo y que la bd lo utilice para que realice esas operaciones. 

Creamos el tablespace logicamente y luego le damos ALTER SYSTEM SET UNDO_TABLESPACE=nombreTBUndo; y cambiamos el tablespace de tipo undo que es usado por la BD. Toda BD tiene al menos un tablespace de tipo ando y ese se define para toda la base de datos.

![GitHub Logo](img/tareas_undo.png)

### RETENCIÓN PARA UNDO

![GitHub Logo](img/retencion.png)

- UNDO_RETENTION

Al consultar este parametro UNDO_RETENTION y con esto nos indica el tiempo de retención que está justamente la información antigua se esta guardando en este tablespace.

- CLÁUSULA RETENTION GUARANTEE 

Puede que cuando creemos el tablespace no aseguren la retención, porque cuando creamos el tablespace al final debemos indicar la cláusula RETENTION GUARANTEE y con eso le decimos que aseguramos que esos datos se encuentren por cierto tiempo en el tablespace.

### TAREAS DE GESTIÓN DE TABLESPACE UNDO

![GitHub Logo](img/tareas2.png)

- CREAR TABLESPACE UNDO:

> CREATE UNDO TABLESPACE nombreTBUndo
> DATAFILE 'nombreDataFile' SIZE tamDataFile
> AUTOEXTEND ON NEXT tamCrecimiento MAXSIZE tamMax
[RETENTION NOGUARANTEE | RETENTION GUARANTEE];  

También se puede indicar un tamaño máximo limitado hasta agotar el disco (Buscar el comando), aunque no es recomendable.

Al final colocamos la última cláusula, para garantizar o no la retención (hablamos de lo que está en corchetes).

- AGREGAR DATAFILE A TABLESPACE UNDO:

> ALTER TABLESPACE nombreTBUndo
> ADD DATAFILE 'nombreDataFile' SIZE tamDataFile
> AUTOEXTEND ON NEXT tamCrecimiento MAXSIZE tamMax;


### TABLAS

![GitHub Logo](img/tablas.png)

- TABLAS ORDINARIAS:

Las tablas que por lo general creamos vienen a ser las tablas ordinarias.
Se guardan en los tablespace permanentes.

- TABLAS CTAS:

Estas son aquellas que en el momento en el que indicamos la intrucción, no solamente crea la estructura (tabla), si no que también guarda la carga de datos.

Está es una tablas se crean apartir de una consulta que puede ser a otras tablas

- TABLAS TEMPORALES:

Son justamente las que se guardan en los tablespace temporales.
La caracteristica de las tablas temporales el tiempo que la data se va a encontrar ahí es lo que dura por lo general una transacción o el tiemp oque el usuario está conectado, pasado el tiempo se elimina la data de estas tablas temporales. Entonces necesariamente una tabla temporal va a ser creada en un tablespace temporal.





















