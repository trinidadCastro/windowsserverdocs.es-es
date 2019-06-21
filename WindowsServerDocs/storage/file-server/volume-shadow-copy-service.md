---
Title: Servicio de instantáneas de volumen
ms.date: 01/30/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 0a4af25723c6d1e796cd3255875c15faf21fb8be
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284385"
---
# <a name="volume-shadow-copy-service"></a>Servicio de instantáneas de volumen

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

Copia de seguridad y restaurar los datos críticos para el negocio pueden ser muy complejas debido a los problemas siguientes:

  - Los datos normalmente se necesita una copia de seguridad mientras las aplicaciones que producen los datos siguen en ejecución. Esto significa que algunos de los archivos de datos podrían estar abierto o podrían estar en un estado incoherente.  
      
  - Si el conjunto de datos es grande, puede ser difícil de realizar una copia de seguridad de todo al mismo tiempo.  
      

Realizar correctamente las operaciones de copia de seguridad y restauración requiere estrecha coordinación entre las aplicaciones de copia de seguridad, las aplicaciones de línea de negocio que se copia de seguridad, y el hardware de administración de almacenamiento y el software. El servicio de copia de instantáneas de volumen (VSS), que se introdujo en Windows Server® 2003, facilita la conversación entre estos componentes para que puedan trabajar mejor juntos. Cuando todos los componentes compatible con VSS, puede usarlos para realizar una copia de seguridad de los datos de la aplicación sin tener que desconectar las aplicaciones.

VSS coordina las acciones que son necesarias para crear una copia de instantáneas coherentes con los datos que es una copia de seguridad (también conocido como una instantánea o una copia en un momento). La instantánea se puede usar como-es, o puede usarse en escenarios como la siguiente:

  - Desea realizar copias de seguridad de la aplicación del sistema y datos información de estado, incluidos archivar los datos a otra unidad de disco duro, en cinta o en otro medio extraíble.  
      
  - Son la minería de datos.  
      
  - Va a realizar copias de seguridad de disco a disco.  
      
  - Necesita una recuperación rápida de la pérdida de datos mediante la restauración de datos para el LUN original o a un LUN completamente nuevo que reemplaza un LUN original que no se pudo.  
      

Características de Windows y las aplicaciones que usan VSS incluyen lo siguiente:

  - [Copia de seguridad de Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) ()http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Instantáneas de carpetas compartidas](http://go.microsoft.com/fwlink/?linkid=142874) ()http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) ()http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Restaurar sistema](http://go.microsoft.com/fwlink/?linkid=180893) ()http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Cómo funciona el servicio de instantáneas de volumen

Una solución completa de VSS requiere que todos los componentes básicos siguientes:

**El servicio VSS**   parte del sistema operativo Windows que garantiza que los demás componentes puede comunicarse correctamente entre sí y trabajar juntos.

**Solicitante VSS**   el software que solicita la creación real de copias instantáneas (u otras operaciones de alto nivel, como importar o eliminarlos). Normalmente, esta es la aplicación de copia de seguridad. La utilidad de copia de seguridad de Windows Server y la aplicación de System Center Data Protection Manager son los solicitantes VSS. Los solicitantes no sean de Microsoft® VSS incluyen casi todo el software de copia de seguridad que se ejecuta en Windows.

**El escritor VSS**   el componente que garantiza que tenemos un conjunto de datos coherente para realizar copias de seguridad. Esto normalmente se proporciona como parte de una aplicación de línea de negocio, como SQL Server® o Exchange Server. Los escritores VSS para diversos componentes de Windows, como el registro, se incluyen con el sistema operativo de Windows. Los escritores de VSS no sean de Microsoft se incluyen con muchas aplicaciones para Windows que necesitan para garantizar la coherencia de datos durante la copia seguridad.

**Proveedor de VSS**   el componente que crea y mantiene las instantáneas de volumen. Esto puede ocurrir en el software o en el hardware. El sistema operativo de Windows incluye un proveedor VSS que usa la copia en escritura. Si usa una red de área de almacenamiento (SAN), es importante que instale al proveedor de hardware VSS para la red SAN, se proporciona uno. Un proveedor de hardware de descarga de la tarea de crear y mantener una copia de instantáneas desde el sistema operativo host.

El siguiente diagrama ilustra cómo el servicio VSS coordina con los solicitantes, escritores y proveedores para crear una instantánea de un volumen.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagrama de arquitectura de servicio de instantáneas de volumen

### <a name="how-a-shadow-copy-is-created"></a>¿Cómo se crea una instantánea

En esta sección se coloca las diversas funciones del solicitante, escritor y proveedor en contexto con una lista de los pasos que deben realizarse para crear una instantánea. El diagrama siguiente muestra cómo el servicio de instantáneas de volumen controla la coordinación del solicitante, escritor y proveedor general.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** proceso de creación de instantáneas

Para crear una instantánea, el solicitante, el escritor y el proveedor realizan las siguientes acciones:

1.  El solicitante solicita el servicio de instantáneas de volumen para enumerar los sistemas de escritura, los metadatos del escritor de recopilar y preparar para la creación de instantáneas.  
      
2.  Cada sistema de escritura crea una descripción XML de los almacenes de datos y los componentes que necesita una copia de seguridad y proporciona para el servicio de instantáneas de volumen. El sistema de escritura también define un método de restauración, que se usa para todos los componentes. El servicio de instantáneas de volumen proporciona la descripción del escritor al solicitante, que selecciona los componentes que se realizará una copia.  
      
3.  El servicio de instantáneas de volumen se notifica a todos los escritores para preparar sus datos para realizar una copia sombra.  
      
4.  Cada escritor prepara los datos según corresponda, como todas las completar transacciones, revertir los registros de transacciones y vaciar las memorias caché abiertas. Cuando los datos están listos para que se copiará una instantánea, el escritor notifica el servicio de instantáneas de volumen.  
      
5.  El servicio de instantáneas de volumen indica a los escritores para inmovilizar temporalmente las solicitudes de E/S de escritura de aplicaciones (siguen siendo posibles las solicitudes de E/S de lectura) para los segundos que son necesarios para crear la instantánea del volumen o volúmenes. No se permite la inmovilización de la aplicación puede tardar más de 60 segundos. El servicio de instantáneas de volumen vacía los búferes del sistema de archivos y, a continuación, se bloquea el sistema de archivos, lo que garantiza que los metadatos del sistema de archivos se ha registrado correctamente y los datos se copia sombra está escritos en un orden coherente.  
      
6.  El servicio de instantáneas de volumen indica al proveedor para crear la instantánea. El período de creación de copia sombra prolonga no más de 10 segundos, durante el cual escriben todas las solicitudes de E/S en el sistema de archivos permanecen inmovilizadas.  
      
7.  El servicio de instantáneas de volumen libera las solicitudes de E/S de escritura de archivo del sistema.  
      
8.  VSS indica a los escritores para reanudar las solicitudes de E/S de escritura de aplicaciones. En este momento las aplicaciones son gratis reanudar la escritura de datos en el disco que se está usando una copia sombra.  
      

> [!NOTE]
> Si los sistemas de escritura se mantienen en el estado de inmovilización durante más de 60 segundos o si los proveedores tardan más de 10 segundos para confirmar la instantánea, se puede anular la creación de instantáneas. 
<br>

9. El solicitante puede volver a intentar el proceso (vaya al paso 1) o notificar al administrador que vuelva a intentarlo más tarde.  
      
10. Si la instantánea se crea correctamente, el servicio de instantáneas de volumen devuelve la información de ubicación de la instantánea al solicitante. En algunos casos, la instantánea puede ser temporalmente disponible como un volumen de lectura y escritura para que VSS y una o varias aplicaciones pueden modificar el contenido de la instantánea antes de que termine la instantánea. Después de VSS y las aplicaciones realizar sus modificaciones, se realiza la instantánea de solo lectura. Esta fase se denomina recuperación automática, y se usa para deshacer las transacciones del sistema de archivos o una aplicación en el volumen de instantáneas que no se completaron antes de que se creó la instantánea.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Cómo el proveedor crea una instantánea

Un hardware o software de proveedor de instantáneas utiliza uno de los métodos siguientes para crear una instantánea:

**Completar la copia**   este método realiza una copia completa (denominada "copia completa" o "clonar") del volumen original en un momento dado en el tiempo. Esta copia es de solo lectura.

**Copia en escritura**   este método no copia el volumen original. En su lugar, realiza una copia diferencial mediante la copia de todos los cambios (las solicitudes de E/S de escritura completadas) que se realizan en el volumen después de un momento dado en el tiempo.

**Redireccionamiento basado en escritura**   este método no copia el volumen original y no realizar cambios en el volumen original después de un momento dado en el tiempo. En su lugar, realiza una copia diferencial mediante la redirección de todos los cambios en un volumen diferente.

## <a name="complete-copy"></a>Copia completa

Normalmente, se crea una copia completa haciendo "espejada" como sigue:

1.  El volumen original y el volumen de instantáneas son un conjunto de volúmenes reflejados.  
      
2.  El volumen de instantáneas está separado del volumen original. Esto interrumpe la conexión de reflejo.  
      

Después de la conexión de reflejo se rompe, el volumen original y el volumen de instantáneas son independientes. El volumen original sigue aceptando todos los cambios (solicitudes de E/S de escritura), mientras las instantáneas de volumen sigue siendo una copia exacta de solo lectura de los datos originales en el momento de la interrupción.

### <a name="copy-on-write-method"></a>Método de copia por escritura

En el método de copia en escritura, cuando se produce un cambio en el volumen original (pero antes de la escritura se completa la solicitud de E/S), cada bloque que se va a modificar es leer y, a continuación, se escriben en el área de almacenamiento del volumen sombra copia (denominado también su área de diferencia""). El área de almacenamiento puede estar en el mismo volumen o un volumen diferente. Esto conserva una copia del bloque de datos en el volumen original antes de que el cambio lo sobrescribe.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Origen de datos (estado y datos)</th>
<th>Instantánea (estado y datos)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Datos originales: 1 2 3 4 5</p></td>
<td><p>Ninguna copia::</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Datos cambiados en memoria caché: 3 a 3'</p></td>
<td><p>Sombra copia creada (solo diferencias): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Datos originales se sobrescriben: 1 2 3’ 4 5</p></td>
<td><p>Las diferencias y el índice almacenado en la instantánea: 3</p></td>
</tr>
</tbody>
</table>

**Tabla 1**   el método de copia en escritura de creación de instantáneas

El método de copia en escritura es un método rápido para crear una instantánea, ya que copian solo los datos que se ha modificado. Los bloques copiados en el área de diferencia se pueden combinar con los datos modificados en el volumen original para restaurar el volumen a su estado antes de cualquiera de los cambios realizado. Si hay muchos cambios, el método de copia en escritura puede resultar caros.

### <a name="redirect-on-write-method"></a>Método de redirección al escribir

En el método de redirección en la escritura, siempre que el volumen original reciba un cambio (solicitud de E/S de escritura), el cambio no se aplica en el volumen original. En su lugar, el cambio se escribe en el área de almacenamiento de instantáneas del volumen de otro.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Time</th>
<th>Origen de datos (estado y datos)</th>
<th>Instantánea (estado y datos)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Datos originales: 1 2 3 4 5</p></td>
<td><p>Ninguna copia::</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Datos cambiados en memoria caché: 3 a 3'</p></td>
<td><p>Sombra copia creada (solo diferencias): 3’</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Datos originales sin cambios: 1 2 3 4 5</p></td>
<td><p>Las diferencias y el índice almacenado en la instantánea: 3’</p></td>
</tr>
</tbody>
</table>

**Tabla 2**   el método de redireccionamiento en la escritura de la creación de instantáneas

Como el método de copia en escritura, el método de redireccionamiento basado en escritura es un método rápido para crear una instantánea, ya que copia únicamente los cambios a los datos. Los bloques copiados en el área de diferencia se pueden combinar con los datos sin cambios en el volumen original para crear una copia completa y actualizada de los datos. Si hay muchas solicitudes de E/S de lectura, el método de redireccionamiento basado en escritura puede resultar caros.

## <a name="shadow-copy-providers"></a>Proveedores de instantáneas

Hay dos tipos de proveedores de instantáneas: proveedores de hardware y basados en software. También es un proveedor del sistema, que es un proveedor de software que está integrado en el sistema operativo Windows.

### <a name="hardware-based-providers"></a>Proveedores de hardware

Acto de proveedores de instantáneas basadas en hardware copia como una interfaz entre el servicio de instantáneas de volumen y el nivel de hardware por trabajar junto con un adaptador de almacenamiento de hardware o un controlador. El trabajo de creación y mantenimiento de la instantánea se realiza mediante la matriz de almacenamiento.

Los proveedores de hardware siempre realizar la copia sombra de un LUN todo, pero el servicio de instantáneas de volumen expone sólo la instantánea del volumen o volúmenes que se solicitaron.

Un proveedor de instantáneas basada en hardware siguiente utiliza el servicio de instantáneas de volumen funcionalidad que define el punto en el tiempo, permite la sincronización de datos, administra la instantánea y proporciona una interfaz común con aplicaciones de copia de seguridad. Sin embargo, el servicio de instantáneas de volumen no especifica el mecanismo subyacente por el cual el proveedor basado en hardware genera y mantiene las instantáneas.

### <a name="software-based-providers"></a>Proveedores de software

Proveedores de instantáneas basada en software normalmente interceptar y procesar leer y escribir las solicitudes de E/S en una capa de software entre el sistema de archivos y el software de administrador de volúmenes.

Estos proveedores se implementan como un componente DLL de modo de usuario y el controlador en modo kernel al menos un dispositivo, normalmente un controlador de filtro de almacenamiento. A diferencia de los proveedores basados en hardware, proveedores de software creación instantáneas en el nivel de software, no en el nivel de hardware.

Un proveedor de instantáneas basada en software debe mantener una vista de "punto-in-time" de un volumen al tener acceso a un conjunto de datos que se puede usar para volver a crear el estado del volumen antes de la hora de creación de copia sombra. Un ejemplo es la técnica de copia en escritura de proveedor del sistema. Sin embargo, el servicio de instantáneas de volumen coloca ninguna restricción en qué técnica usan los proveedores de software para crear y mantener las instantáneas.

Un proveedor de software es aplicable a una amplia variedad de plataformas de almacenamiento de un proveedor de hardware, y debería funcionar igualmente bien con volúmenes lógicos o discos básicos. (Un volumen lógico es un volumen que se crea mediante la combinación de espacio libre de dos o más discos). A diferencia de instantáneas de hardware, proveedores de software de consumen recursos del sistema operativo para mantener la instantánea.

Para obtener más información acerca de los discos básicos, consulte [¿cuáles son los discos y volúmenes básicos?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) en TechNet.

### <a name="system-provider"></a>Proveedor del sistema

Un proveedor de instantáneas, el proveedor del sistema, se proporciona en el sistema operativo Windows. Aunque se proporciona un proveedor predeterminado en Windows, otros proveedores son gratis para proporcionar implementaciones optimizadas para sus aplicaciones de software y hardware de almacenamiento.

Para mantener la vista de "punto-in-time" de un volumen que se encuentra en una instantánea, el proveedor del sistema usa una técnica de copia en escritura. Copias de los bloques en el volumen que se han modificado desde el principio de la creación de instantáneas se almacenan en un área de almacenamiento de instantáneas.

El proveedor del sistema puede exponer el volumen de producción, que puede escribir y leen normalmente. Cuando se necesita la instantánea, se aplica lógicamente las diferencias en los datos en el volumen de producción para exponer la instantánea completa.

Para el proveedor del sistema, el área de almacenamiento debe estar en un volumen NTFS. El volumen de instantáneas no necesita estar en un volumen NTFS, pero al menos un volumen montado en el sistema debe ser un volumen NTFS.

Los archivos de componentes que constituyen el proveedor del sistema son swprv.dll y volsnap.sys.

### <a name="in-box-vss-writers"></a>Los escritores VSS en el equipo

El sistema operativo de Windows incluye un conjunto de los escritores VSS que son responsables de la enumeración de los datos que se necesitan por diversas características de Windows.

Para obtener más información acerca de estos sistemas de escritura, consulte los siguientes sitios Web de Microsoft:

  - [Los escritores VSS en el cuadro](http://go.microsoft.com/fwlink/?linkid=180895) ()http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Los escritores VSS incluido con nueva para Windows Server 2008 y Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) ()http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Los escritores VSS incluido con nueva para Windows Server 2008 R2 y Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) ()http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Usan de instantáneas

Además de la copia de seguridad de información de estado de sistema y los datos de aplicación, las instantáneas pueden usarse para varios fines, incluidos los siguientes:

  - Restauración de LUN (resincronización LUN y su intercambio de LUN)  
      
  - Restaurar archivos individuales (instantáneas para carpetas compartidas)  
      
  - Minería de datos mediante el uso de instantáneas transportables  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restauración de LUN (resincronización LUN y su intercambio de LUN)

En Windows Server 2008 R2 y Windows 7, los solicitantes VSS pueden utilizar una función de proveedor de copia de instantáneas de hardware denominada resincronización LUN (o "Resincronización LUN"). Se trata de un esquema de recuperación rápida que permite que un administrador de la aplicación restaurar datos desde una instantánea en el LUN original o a un nuevo LUN.

La instantánea puede ser un clon completo o una instantánea diferencial. En cualquier caso, al final de la operación de resincronización, el LUN de destino tendrá el mismo contenido que la instantánea LUN. Durante la operación de resincronización, la matriz realiza una copia de nivel de bloque de la instantánea para el LUN de destino.


> [!NOTE]
> La instantánea debe ser una copia de instantáneas transportables de hardware. 
<br>


La mayoría de las matrices permiten las operaciones de E/S de producción reanudar poco después de que comience la operación de resincronización. Mientras la operación de resincronización está en curso, lea las solicitudes se redirigen a la copia sombra LUN y las solicitudes de escritura para el LUN de destino. Esto permite que las matrices recuperar conjuntos de datos muy grandes y reanudar las operaciones normales en varios segundos.

La resincronización de LUN difiere de intercambio de LUN. Un intercambio de LUN es un escenario de recuperación rápida que VSS ha admitido desde Windows Server 2003 SP1. En un intercambio de LUN, la instantánea se importan y, a continuación, se convierte en un volumen de lectura y escritura. La conversión es una operación irreversible y el LUN subyacente y el volumen no se puede controlar con las API de VSS después de eso. La lista siguiente describe cómo se compara la resincronización de LUN con intercambio de LUN:

  - En la resincronización de LUN, la instantánea no se modifica, por lo que puede usarse varias veces. En el intercambio de LUN, la instantánea se puede usar una sola vez para una recuperación. Para los administradores más preocupados por la seguridad, esto es importante. Cuando se usa la resincronización de LUN, el solicitante puede volver a intentar la operación de restauración completa si algo va mal la primera vez.  
      
  - Al final de un intercambio de LUN, la copia sombra LUN se utiliza para las solicitudes de E/S de producción. Por este motivo, la copia sombra LUN debe usar la misma calidad de almacenamiento como el LUN de producción original para asegurarse de que el rendimiento no se ve afectado después de la operación de recuperación. Si la resincronización de LUN se usa en su lugar, el proveedor de hardware puede mantener la instantánea de almacenamiento que es menos costoso que el almacenamiento de calidad de producción.  
      
  - Si el destino LUN es inutilizable y debe volver a crear, intercambio de LUN puede ser más económico ya que no requiere un LUN de destino.  
      


> [!WARNING]
> Todas las operaciones enumeradas son operaciones de nivel de LUN. Si se intenta recuperar un volumen específico mediante la resincronización de LUN, sin darse cuenta va a revertir todos los otros volúmenes que comparten el LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restaurar archivos individuales (instantáneas para carpetas compartidas)

Las instantáneas para carpetas compartidas se usa el servicio de instantáneas de volumen para proporcionar copias instantáneas de archivos que se encuentran en un recurso compartido de red, como un servidor de archivos. Las instantáneas para carpetas compartidas, los usuarios pueden recuperar rápidamente los archivos eliminados o modificados que están almacenados en la red. Dado que puede hacerlo sin ayuda del administrador, instantáneas para carpetas compartidas puede aumentar la productividad y reducir los costos administrativos.

Para obtener más información acerca de las instantáneas para carpetas compartidas, consulte [instantáneas para carpetas compartidas](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) en TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Minería de datos mediante el uso de instantáneas transportables

Con un proveedor de hardware que está diseñado para su uso con el servicio de instantáneas de volumen, puede crear instantáneas transportables que pueden importarse en servidores en el mismo subsistema (por ejemplo, una SAN). Estas instantáneas se pueden usar para inicializar una producción o prueba de la instalación con datos de solo lectura para la minería de datos.

Con el servicio de instantáneas de volumen y una matriz de almacenamiento con un proveedor de hardware que está diseñado para su uso con el servicio de instantáneas de volumen, es posible crear una instantánea del volumen de datos de origen en un servidor y, a continuación, importar la instantánea en otro servidor  (o realizar una copia en el mismo servidor). Este proceso se consigue en unos minutos, independientemente del tamaño de los datos. El proceso de transporte se logra a través de una serie de pasos que usan a un solicitante de copia de instantáneas (una aplicación de administración de almacenamiento) que admita instantáneas transportables.

## <a name="to-transport-a-shadow-copy"></a>Para el transporte de una instantánea

1.  Crear una copia de instantáneas transportables de los datos de origen en un servidor.

2.  Importar la instantánea en un servidor que está conectado a la SAN (puede importar en un servidor diferente o en el mismo servidor).

3.  Los datos ahora están listos para usarse.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   transporte entre dos servidores y creación de instantáneas


> [!NOTE]
> No se puede importar una copia de instantáneas transportables que se crea en Windows Server 2003 en un servidor que ejecuta Windows Server 2008 o Windows Server 2008 R2. No se puede importar una copia de instantáneas transportables que se creó en Windows Server 2008 o Windows Server 2008 R2 en un servidor que ejecuta Windows Server 2003. Sin embargo, se puede importar una copia sombra que se crea en Windows Server 2008 en un servidor que ejecuta Windows Server 2008 R2, y viceversa. 
<br>


Las instantáneas son de solo lectura. Si desea convertir una instantánea en un LUN de lectura/escritura, puede usar una aplicación de administración de almacenamiento basada en el servicio de disco Virtual (incluidos algunos de los solicitantes) además del servicio de instantáneas de volumen. Con esta aplicación, puede quitar la instantánea de la administración de servicio de instantáneas de volumen y convertirlo en un LUN de lectura/escritura.

Transporte de servicio de instantáneas de volumen es una solución avanzada en equipos que ejecutan Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 o Windows Server 2008 R2. Sólo funciona si hay un proveedor de hardware en la matriz de almacenamiento. Transporte de copia sombra puede usarse para varios fines, incluidas copias de seguridad de cinta, los datos de minería de datos y las pruebas.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Estas P+F responden a preguntas acerca de Volume Shadow Copy Service (VSS) para los administradores del sistema. Para obtener información acerca de las interfaces de programación de aplicaciones de VSS, consulte [Volume Shadow Copy Service](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) en la biblioteca de centro para desarrolladores de Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>¿Cuándo se introdujo Volume Shadow Copy Service? ¿En las versiones de sistema operativo de Windows está disponible?

VSS se introdujo en Windows XP. Está disponible en Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 y Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>¿Qué es la diferencia entre una instantánea y una copia de seguridad?

En el caso de una copia de seguridad de la unidad de disco duro, la instantánea crea también es la copia de seguridad. Se pueden copiar datos desactivada la instantánea para una restauración o la instantánea se puede usar para un escenario de recuperación rápida, por ejemplo, la resincronización de LUN o cambiar el LUN.

Cuando se copian datos de la instantánea en cinta u otro medio extraíble, el contenido que se almacena en el medio que constituye la copia de seguridad. Después de que los datos se copian desde él, se puede eliminar la instantánea propio.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>¿Qué es el volumen mayor tamaño que admita el servicio de instantáneas de volumen?

Servicio de instantáneas de volumen es compatible con un tamaño de volumen de hasta 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>He realizado una copia de seguridad en Windows Server 2008. ¿Puedo restaurarla en Windows Server 2008 R2?

Depende de la copia de seguridad software que usa. La mayoría de programas de copia de seguridad admiten este escenario para los datos, pero no para copias de seguridad de estado del sistema.

Las instantáneas que se crean en cualquiera de estas versiones de Windows pueden usarse en el otro.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>He realizado una copia de seguridad en Windows Server 2003. ¿Puedo restaurarla en Windows Server 2008?

Depende de la copia de seguridad software usado. Si crea una instantánea en Windows Server 2003, no puede usar en Windows Server 2008. Además, si crea una instantánea en Windows Server 2008, no puede restaurar en Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>¿Cómo puedo deshabilitar VSS?

Es posible deshabilitar el servicio de instantáneas de volumen mediante la consola de administración de Microsoft. Sin embargo, no debe hacer esto. Deshabilitar VSS negativamente, afecta a cualquier software que utiliza y que depende de él, como restaurar sistema y copia de seguridad de Windows Server.

Para obtener más información, consulte los siguientes sitios Web de Microsoft TechNet:

  - [Restaurar sistema](http://go.microsoft.com/fwlink/?linkid=157113) ()http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Copia de seguridad de Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) ()http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>¿Se puede excluir archivos desde una instantánea para ahorrar espacio?

VSS está diseñado para crear instantáneas de volúmenes completos. Los archivos temporales, como los archivos de paginación, automáticamente se omiten de instantáneas para ahorrar espacio.

Para excluir determinados archivos de instantáneas, use la siguiente clave del registro: **FilesNotToSnapshot**.


> [!NOTE]
> El <STRONG>FilesNotToSnapshot</STRONG> clave del registro está pensada para usarse únicamente por las aplicaciones. Los usuarios que intentan usar encontrarán limitaciones como los siguientes:
> <br>
> <UL>
> <LI>No puede eliminar archivos desde una instantánea que se creó en un servidor de Windows mediante la característica versiones anteriores.<BR><BR>
> <LI>No puede eliminar los archivos de instantáneas para carpetas compartidas.<BR><BR>
> <LI>Puede eliminar archivos desde una instantánea que se creó mediante la <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">Diskshadow</a> utilidad, pero no se puede eliminar archivos desde una instantánea que se creó mediante la <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a> utilidad.<BR><BR>
> <LI>Se eliminan los archivos desde una copia sombra de forma óptima. Esto significa que no se garantiza que se va a eliminar.<BR><BR></LI></UL>


Para obtener más información, consulte [excluir archivos de instantáneas](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) en MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>No se pudo mi programa de copia de seguridad que no sean de Microsoft con un error de VSS. ¿Qué puedo hacer?

Consulte la sección de soporte técnico de producto del sitio Web de la compañía que creó el programa de copia de seguridad. Puede haber una actualización de producto que puede descargar e instalar para corregir el problema. Si no es así, póngase en contacto con el departamento de soporte técnico de productos de la empresa.

Los administradores del sistema pueden utilizar la información de solución de problemas de VSS en el siguiente sitio Web de Microsoft TechNet Library para recopilar información de diagnóstico sobre problemas relacionados con el VSS.

Para obtener más información, consulte [Volume Shadow Copy Service](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) en TechNet.

### <a name="what-is-the-diff-area"></a>¿Qué es el "área de diferencia"?

El área de almacenamiento de instantáneas (o "área de diferencia") es la ubicación donde se almacenan los datos para la copia sombra que se crea mediante el proveedor de software del sistema.

### <a name="where-is-the-diff-area-located"></a>¿Dónde se encuentra el área de diferencia?

El área de diferencia puede encontrarse en cualquier volumen local. Sin embargo, debe estar ubicado en un volumen NTFS que tenga espacio suficiente para almacenarlo.

### <a name="how-is-the-diff-area-location-determined"></a>¿Cómo es la ubicación del área de diferencia determinada?

Se evalúan los criterios siguientes, en este orden, para determinar la ubicación del área de diferencias:

  - Si un volumen ya tiene una instantánea existente, se usa esa ubicación.  
      
  - Si hay una asociación manual preconfigurada entre el volumen original y la ubicación del volumen de copia de instantáneas, se usa esa ubicación.  
      
  - Si los dos criterios anteriores no proporcionan una ubicación, el servicio de instantáneas elige una ubicación basada en el espacio libre disponible. Si más de un volumen se están creando instantáneas, el servicio de instantáneas crea una lista de ubicaciones posibles de instantánea en función del tamaño de espacio disponible, en orden descendente. El número de ubicaciones proporcionado es igual al número de volúmenes que se va a realiza la copia sombra.  
      
  - Si el volumen que se va a realiza la copia sombra es una de las ubicaciones posibles, se crea una asociación local. En caso contrario, se crea una asociación con el volumen con más espacio disponible.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>¿Puede VSS crea instantáneas de volúmenes no NTFS?

Sí. Sin embargo, se pueden realizar copias sombra persistente solo para los volúmenes NTFS. Además, al menos un volumen montado en el sistema debe ser un volumen NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>¿Qué es el número máximo de instantáneas, que puedo crear al mismo tiempo?

El número máximo de volúmenes de instantáneas en un único conjunto de instantáneas es 64. Tenga en cuenta que no es el mismo que el número de instantáneas.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>¿Lo que se crea el número máximo de instantáneas de software por el proveedor del sistema que puedo mantener la para un volumen?

El número máximo es de instantáneas de software para cada volumen es 512. Sin embargo, de forma predeterminada sólo puede mantener 64 instantáneas que se usan con la característica de instantáneas de carpetas compartidas. Para cambiar el límite para la característica de instantáneas de carpetas compartidas, use la siguiente clave del registro: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>¿Cómo se puede controlar el espacio que se usa para el espacio de almacenamiento de instantáneas?

Tipo de la **vssadmin cambiar el tamaño de shadowstorage** comando.

Para obtener más información, consulte [Vssadmin cambiar el tamaño de shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) en TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>¿Qué ocurre cuando me he quedado sin espacio?

Se eliminan las instantáneas para el volumen, empezando por las instantáneas más antiguas.

## <a name="volume-shadow-copy-service-tools"></a>Herramientas de servicio de copia de instantáneas de volumen

El sistema operativo de Windows proporciona las siguientes herramientas para trabajar con VSS:

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow es un solicitante VSS que puede usar para administrar todas las instantáneas de hardware y software que puede haber en un sistema. DiskShadow incluye comandos como la siguiente:

  - **list**: Enumera los escritores de VSS, los proveedores de VSS e instantáneas  
      
  - **create**: Crea una nueva instantánea.  
      
  - **import**: Importa una copia de instantáneas transportables  
      
  - **expose**: Expone una instantánea persistente (como una letra de unidad, por ejemplo)  
      
  - **revert**: Revierte un volumen en una copia de instantáneas especificada  
      

Esta herramienta está diseñada para su uso por los profesionales de TI, pero los desarrolladores también le resultará útil cuando se prueba un escritor de VSS o el proveedor de VSS.

DiskShadow está disponible sólo en sistemas operativos Windows Server. No está disponible en los sistemas operativos de cliente de Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin se usa para crear, eliminar y mostrar información acerca de las instantáneas. También puede usarse para cambiar el tamaño del área de almacenamiento de instantáneas ("área de diferencia").

VssAdmin incluye comandos como la siguiente:

  - **crear instantáneas**: Crea una nueva instantánea.  
      
  - **eliminar las sombras**: Eliminaciones de instantáneas  
      
  - **enumerar proveedores**: Enumera todos los proveedores registrados de VSS  
      
  - **lista de escritores**: Enumera todos los escritores de VSS de suscrito  
      
  - **cambiar el tamaño de shadowstorage**: Cambia el tamaño máximo del área de almacenamiento de copia de instantáneas  
      

VssAdmin solo puede usarse para administrar las instantáneas que se crean mediante el proveedor de software del sistema.

VssAdmin está disponible en el cliente de Windows y las versiones de sistema operativo Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Claves del registro de volumen Shadow Copy Service

Las claves del registro siguientes están disponibles para su uso con VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Esta clave se utiliza para especificar qué usuarios tienen acceso a las instantáneas.

Para obtener más información, consulte las siguientes entradas en el sitio Web de MSDN:

  - [Consideraciones de seguridad para escritores](http://go.microsoft.com/fwlink/?linkid=157739) ()http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Consideraciones de seguridad para los solicitantes](http://go.microsoft.com/fwlink/?linkid=180908) ()http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Esta clave especifica el número máximo de instantáneas accesibles para el cliente que se pueden almacenar en cada volumen del equipo. Instantáneas accesibles para el cliente se usan las instantáneas para carpetas compartidas.

Para obtener más información, vea la siguiente entrada en el sitio Web de MSDN:

**MaxShadowCopies** en [las claves del registro de copia de seguridad y restauración](http://go.microsoft.com/fwlink/?linkid=180909) ()http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Esta clave especifica el tamaño mínimo inicial, en MB, del área de almacenamiento de copia de instantáneas.

Para obtener más información, vea la siguiente entrada en el sitio Web de MSDN:

**MinDiffAreaFileSize** en [las claves del registro de copia de seguridad y restauración](http://go.microsoft.com/fwlink/?linkid=180910) ()http://go.microsoft.com/fwlink/?LinkId=180910)

`##`#' Admite versiones de sistema operativo

En la tabla siguiente se enumera las versiones de sistema operativo mínimo compatible para características VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Característica VSS</th>
<th>Cliente mínimo compatible</th>
<th>Servidor mínimo compatible</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Resincronización de LUN</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2008 R2</p></td>
</tr>
<tr class="even">
<td><p><strong>FilesNotToSnapshot</strong> clave del registro</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="odd">
<td><p>Instantáneas transportables</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2003 con SP1</p></td>
</tr>
<tr class="even">
<td><p>Instantáneas de hardware</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Versiones anteriores de Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Recuperación rápida mediante el intercambio LUN</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2003 con SP1</p></td>
</tr>
<tr class="odd">
<td><p>Varias importación de instantáneas de hardware</p>
<div class="alert">
<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<thead>
<tr class="header">
<th><img src="media/volume-shadow-copy-service/Dd560667.note(WS.10).gif" />Nota</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Esta es la capacidad para importar una instantánea de más de una vez. Operación de solo importación puede realizarse en un momento.
<p></p></td>
</tr>
</tbody>
</table>
<p></p>
</div></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Instantáneas para carpetas compartidas</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Instantáneas transportables recuperada automáticamente</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sesiones de copia de seguridad simultáneas (hasta 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Sesión de solo restauración simultánea con las copias de seguridad</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 con SP2</p></td>
</tr>
<tr class="even">
<td><p>Hasta 8 sesiones de restauración simultáneas con las copias de seguridad</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Vea también

[Servicio de instantáneas de volumen en el Centro para desarrolladores de Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)