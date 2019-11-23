---
title: Servicio de instantáneas de volumen
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 19e07504dad49c5e23cc49630015529e2a746aa7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394446"
---
# <a name="volume-shadow-copy-service"></a>Servicio de instantáneas de volumen

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

La copia de seguridad y la restauración de los datos empresariales críticos puede ser muy compleja debido a los siguientes problemas:

  - Normalmente es necesario realizar una copia de seguridad de los datos mientras las aplicaciones que producen los datos siguen en ejecución. Esto significa que algunos de los archivos de datos pueden estar abiertos o que podrían estar en un estado incoherente.  
      
  - Si el conjunto de datos es grande, puede resultar difícil realizar una copia de seguridad de todos ellos al mismo tiempo.  
      

La realización correcta de operaciones de copia de seguridad y restauración requiere una estrecha coordinación entre las aplicaciones de copia de seguridad, las aplicaciones de línea de negocio de las que se realiza la copia de seguridad y el hardware y software de administración del almacenamiento. El Servicio de instantáneas de volumen (VSS), que se presentó en Windows Server® 2003, facilita la conversación entre estos componentes para permitirles trabajar mejor juntos. Cuando todos los componentes admitan VSS, puede usarlos para realizar una copia de seguridad de los datos de la aplicación sin desconectar las aplicaciones.

VSS coordina las acciones necesarias para crear una instantánea coherente (también conocida como instantánea o una copia de un momento dado) de los datos de los que se va a hacer una copia de seguridad. La instantánea se puede usar tal cual o se puede usar en escenarios como los siguientes:

  - Desea realizar una copia de seguridad de los datos de aplicación y la información de estado del sistema, incluido el archivado de datos en otra unidad de disco duro, en cinta o en otros medios extraíbles.  
      
  - Es la minería de datos.  
      
  - Está realizando copias de seguridad de disco a disco.  
      
  - Necesita una recuperación rápida de la pérdida de datos mediante la restauración de los datos en el LUN original o en un LUN completamente nuevo que reemplaza a un LUN original que produjo un error.  
      

Entre las características y aplicaciones de Windows que usan VSS se incluyen las siguientes:

  - [Copias de seguridad de Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Instantáneas de carpetas compartidas](http://go.microsoft.com/fwlink/?linkid=142874) (http://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](http://go.microsoft.com/fwlink/?linkid=180892) (http://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Restaurar sistema](http://go.microsoft.com/fwlink/?linkid=180893) (http://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Cómo funciona Servicio de instantáneas de volumen

Una solución completa de VSS requiere todos los elementos básicos siguientes:

El **servicio VSS**   parte del sistema operativo Windows que garantiza que los demás componentes puedan comunicarse entre sí correctamente y trabajar juntos.

El **solicitante de VSS**   el software que solicita la creación real de instantáneas (u otras operaciones de alto nivel, como la importación o la eliminación). Normalmente, se trata de la aplicación de copia de seguridad. La utilidad Copias de seguridad de Windows Server y la aplicación de Data Protection Manager de System Center son solicitantes VSS. Los solicitantes de VSS que no son de® Microsoft incluyen casi todo el software de copia de seguridad que se ejecuta en Windows.

**VSS Writer**   el componente que garantiza que tenemos un conjunto de datos coherente para realizar la copia de seguridad. Esto se proporciona normalmente como parte de una aplicación de línea de negocio, como SQL Server® o Exchange Server. Los escritores de VSS para varios componentes de Windows, como el registro, se incluyen con el sistema operativo Windows. Los escritores de VSS que no son de Microsoft se incluyen con muchas aplicaciones para Windows que necesitan garantizar la coherencia de los datos durante la copia de seguridad.

**Proveedor VSS**   el componente que crea y mantiene las instantáneas. Esto puede ocurrir en el software o en el hardware. El sistema operativo Windows incluye un proveedor VSS que usa la operación de copia en escritura. Si utiliza una red de área de almacenamiento (SAN), es importante que instale el proveedor de hardware de VSS para la SAN, si se proporciona uno. Un proveedor de hardware descarga la tarea de creación y mantenimiento de una instantánea del sistema operativo host.

En el diagrama siguiente se ilustra cómo el servicio VSS se coordina con solicitantes, escritores y proveedores para crear una instantánea de un volumen.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagrama arquitectónico de servicio de instantáneas de volumen

### <a name="how-a-shadow-copy-is-created"></a>Cómo se crea una instantánea

En esta sección se colocan los distintos roles del solicitante, el escritor y el proveedor en el contexto mediante la descripción de los pasos que hay que realizar para crear una instantánea. En el diagrama siguiente se muestra cómo el Servicio de instantáneas de volumen controla la coordinación general del solicitante, el escritor y el proveedor.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Ilustración 2** Proceso de creación de instantáneas

Para crear una instantánea, el solicitante, el escritor y el proveedor realizan las siguientes acciones:

1.  El solicitante pide al Servicio de instantáneas de volumen que Enumere los escritores, recopile los metadatos del escritor y prepárese para la creación de la instantánea.  
      
2.  Cada escritor crea una descripción XML de los componentes y almacenes de datos de los que es necesario hacer una copia de seguridad y proporcionarle el Servicio de instantáneas de volumen. El escritor también define un método de restauración, que se utiliza para todos los componentes. El Servicio de instantáneas de volumen proporciona la descripción del escritor al solicitante, que selecciona los componentes de los que se va a hacer una copia de seguridad.  
      
3.  El Servicio de instantáneas de volumen notifica a todos los escritores que prepare sus datos para realizar una instantánea.  
      
4.  Cada escritor prepara los datos según corresponda, como la finalización de todas las transacciones abiertas, los registros de transacciones graduales y el vaciado de las cachés. Cuando los datos están listos para la copia sombra, el escritor informa al Servicio de instantáneas de volumen.  
      
5.  El Servicio de instantáneas de volumen indica a los escritores que inmovilizar temporalmente las solicitudes de e/s de escritura de la aplicación (las solicitudes de e/s de lectura siguen siendo posibles) durante los segundos que se requieren para crear la instantánea del volumen o volúmenes. No se permite que la inmovilización de la aplicación tarde más de 60 segundos. El Servicio de instantáneas de volumen vacía los búferes del sistema de archivos y, a continuación, inmoviliza el sistema de archivos, lo que garantiza que los metadatos del sistema de archivos se graban correctamente y que los datos de los que se va a realizar una copia sombra se escriben en un orden coherente.  
      
6.  El Servicio de instantáneas de volumen indica al proveedor que cree la instantánea. El período de creación de instantáneas no dura más de 10 segundos, durante el cual todas las solicitudes de e/s de escritura en el sistema de Archivos permanecen inmovilizadas.  
      
7.  El Servicio de instantáneas de volumen libera las solicitudes de e/s de escritura del sistema de archivos.  
      
8.  VSS informa a los escritores para reanudar las solicitudes de e/s de escritura de la aplicación. En este momento, las aplicaciones pueden reanudar la escritura de datos en el disco en el que se realiza la instantánea.  
      

> [!NOTE]
> Se puede anular la creación de instantáneas si los escritores se mantienen en estado de inmovilización durante más de 60 segundos o si los proveedores tardan más de 10 segundos en confirmar la instantánea. 
<br>

9. El solicitante puede reintentar el proceso (volver al paso 1) o notificar al administrador que vuelva a intentarlo más tarde.  
      
10. Si la instantánea se crea correctamente, el Servicio de instantáneas de volumen devuelve la información de ubicación de la instantánea al solicitante. En algunos casos, la instantánea puede estar disponible temporalmente como volumen de lectura y escritura para que VSS y una o varias aplicaciones puedan modificar el contenido de la instantánea antes de que finalice la instantánea. Una vez que VSS y las aplicaciones realizan sus modificaciones, la instantánea se convierte en de solo lectura. Esta fase se denomina recuperación automática y se usa para deshacer cualquier transacción del sistema de archivos o de la aplicación en el volumen de instantáneas que no se completaron antes de que se creara la instantánea.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Cómo el proveedor crea una instantánea

Un proveedor de instantáneas de hardware o software usa uno de los métodos siguientes para crear una instantánea:

**Copia completa**   este método realiza una copia completa (denominada "copia completa" o "clon") del volumen original en un momento dado. Esta copia es de solo lectura.

**Copiar en escritura**   este método no copia el volumen original. En su lugar, realiza una copia diferencial copiando todos los cambios (solicitudes de e/s de escritura completadas) que se realizan en el volumen después de un momento dado.

**Redireccionamiento** de la escritura   este método no copia el volumen original y no realiza ningún cambio en el volumen original después de un momento dado en el tiempo. En su lugar, realiza una copia diferencial redirigiendo todos los cambios a un volumen diferente.

## <a name="complete-copy"></a>Copia completa

Normalmente se crea una copia completa mediante la creación de un "reflejo dividido" como se indica a continuación:

1.  El volumen original y el volumen de la instantánea son un conjunto de volúmenes reflejado.  
      
2.  El volumen de la instantánea está separado del volumen original. Esto interrumpe la conexión del reflejo.  
      

Una vez interrumpida la conexión del reflejo, el volumen original y el volumen de la instantánea son independientes. El volumen original continúa aceptando todos los cambios (solicitudes de e/s de escritura), mientras que el volumen de la instantánea sigue siendo una copia exacta de solo lectura de los datos originales en el momento de la interrupción.

### <a name="copy-on-write-method"></a>Método de copia en escritura

En el método de copia en escritura, cuando se produce un cambio en el volumen original (pero antes de que se complete la solicitud de e/s de escritura), cada bloque que se va a modificar se lee y se escribe en el área de almacenamiento de instantáneas del volumen (también denominada "área de diferencia"). El área de almacenamiento de instantáneas puede estar en el mismo volumen o en un volumen diferente. Esto conserva una copia del bloque de datos en el volumen original antes de que el cambio lo sobrescriba.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Tiempo</th>
<th>Datos de origen (estado y datos)</th>
<th>Instantánea (estado y datos)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Datos originales: 1 2 3 4 5</p></td>
<td><p>Sin copia::</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Datos cambiados en caché: 3 a 3 '</p></td>
<td><p>Instantánea creada (solo diferencias): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Datos originales sobrescritos: 1 2 3 ' 4 5 '</p></td>
<td><p>Diferencias e índice almacenados en la instantánea: 3</p></td>
</tr>
</tbody>
</table>

**Tabla 1**   el método de copia en escritura de creación de instantáneas

El método de copia en escritura es un método rápido para crear una instantánea, ya que solo copia los datos que se han modificado. Los bloques copiados en el área de diferencia se pueden combinar con los datos modificados en el volumen original para restaurar el volumen a su estado anterior a los cambios realizados. Si hay muchos cambios, el método de copia en escritura puede ser caro.

### <a name="redirect-on-write-method"></a>Redirect-on-Write (método)

En el método de redireccionamiento de escritura, siempre que el volumen original recibe un cambio (solicitud de e/s de escritura), el cambio no se aplica al volumen original. En su lugar, el cambio se escribe en el área de almacenamiento de instantáneas de otro volumen.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Tiempo</th>
<th>Datos de origen (estado y datos)</th>
<th>Instantánea (estado y datos)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>T0</p></td>
<td><p>Datos originales: 1 2 3 4 5</p></td>
<td><p>Sin copia::</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Datos cambiados en caché: 3 a 3 '</p></td>
<td><p>Instantánea creada (solo diferencias): 3 '</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Datos originales sin cambios: 1 2 3 4 5</p></td>
<td><p>Diferencias e índice almacenados en la instantánea: 3 '</p></td>
</tr>
</tbody>
</table>

**Tabla 2**   el método de redirección de creación de instantáneas

Al igual que el método de copia en escritura, el método de redireccionamiento de escritura es un método rápido para crear una instantánea, ya que solo copia los cambios en los datos. Los bloques copiados en el área de diferencia se pueden combinar con los datos sin modificar en el volumen original para crear una copia completa y actualizada de los datos. Si hay muchas solicitudes de e/s de lectura, el método de redirección de escritura puede ser costoso.

## <a name="shadow-copy-providers"></a>Proveedores de instantáneas

Hay dos tipos de proveedores de instantáneas: proveedores basados en hardware y proveedores basados en software. También hay un proveedor del sistema, que es un proveedor de software integrado en el sistema operativo Windows.

### <a name="hardware-based-providers"></a>Proveedores basados en hardware

Los proveedores de instantáneas basados en hardware actúan como una interfaz entre el Servicio de instantáneas de volumen y el nivel de hardware al trabajar junto con un controlador o adaptador de almacenamiento de hardware. La matriz de almacenamiento realiza el trabajo de creación y mantenimiento de la instantánea.

Los proveedores de hardware siempre toman la instantánea de un LUN completo, pero el Servicio de instantáneas de volumen solo expone la instantánea del volumen o los volúmenes que se solicitaron.

Un proveedor de instantáneas basado en hardware hace uso de la funcionalidad Servicio de instantáneas de volumen que define el momento dado, permite la sincronización de datos, administra la instantánea y proporciona una interfaz común con las aplicaciones de copia de seguridad. Sin embargo, el Servicio de instantáneas de volumen no especifica el mecanismo subyacente por el que el proveedor basado en hardware genera y mantiene instantáneas.

### <a name="software-based-providers"></a>Proveedores basados en software

Los proveedores de instantáneas basadas en software suelen interceptar y procesar solicitudes de e/s de lectura y escritura en una capa de software entre el sistema de archivos y el software del administrador de volúmenes.

Estos proveedores se implementan como un componente DLL de modo usuario y al menos un controlador de dispositivo de modo kernel, normalmente un controlador de filtro de almacenamiento. A diferencia de los proveedores basados en hardware, los proveedores basados en software crean instantáneas en el nivel de software, no en el nivel de hardware.

Un proveedor de instantáneas basado en software debe mantener una vista "a un momento dado" de un volumen mediante el acceso a un conjunto de datos que se puede usar para volver a crear el estado del volumen antes de la hora de creación de la instantánea. Un ejemplo es la técnica de copia en escritura del proveedor del sistema. Sin embargo, el Servicio de instantáneas de volumen no impone ninguna restricción sobre la técnica que los proveedores basados en software utilizan para crear y mantener instantáneas.

Un proveedor de software es aplicable a una amplia gama de plataformas de almacenamiento que un proveedor basado en hardware, y debe funcionar con discos básicos o volúmenes lógicos igualmente bien. (Un volumen lógico es un volumen que se crea mediante la combinación de espacio libre de dos o más discos). A diferencia de las instantáneas de hardware, los proveedores de software consumen recursos del sistema operativo para mantener la instantánea.

Para obtener más información acerca de los discos básicos, consulte [¿Qué son los discos y volúmenes básicos?](http://go.microsoft.com/fwlink/?linkid=180894) (http://go.microsoft.com/fwlink/?LinkId=180894) en TechNet.

### <a name="system-provider"></a>Proveedor del sistema

Un proveedor de instantáneas, el proveedor del sistema, se proporciona en el sistema operativo Windows. Aunque se proporciona un proveedor predeterminado en Windows, otros proveedores pueden proporcionar implementaciones que están optimizadas para sus aplicaciones de software y hardware de almacenamiento.

Para mantener la vista "a un momento dado" de un volumen que se encuentra en una instantánea, el proveedor del sistema utiliza una técnica de copia en escritura. Las copias de los bloques en el volumen que se han modificado desde el inicio de la creación de instantáneas se almacenan en un área de almacenamiento de instantáneas.

El proveedor del sistema puede exponer el volumen de producción, que se puede escribir y leer de la forma habitual. Cuando se necesita la instantánea, aplica lógicamente las diferencias a los datos en el volumen de producción para exponer la instantánea completa.

En el caso del proveedor del sistema, el área de almacenamiento de instantáneas debe estar en un volumen NTFS. No es necesario que el volumen del que se va a realizar la instantánea sea un volumen NTFS, pero al menos un volumen montado en el sistema debe ser un volumen NTFS.

Los archivos de componentes que componen el proveedor del sistema son swprv. dll y volsnap. sys.

### <a name="in-box-vss-writers"></a>Escritores de VSS en la caja

El sistema operativo Windows incluye un conjunto de escritores de VSS que son responsables de enumerar los datos que requieren diversas características de Windows.

Para obtener más información acerca de estos escritores, vea los siguientes sitios web de Microsoft:

  - [Escritores de VSS integrados](http://go.microsoft.com/fwlink/?linkid=180895) (http://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Nuevos escritores de VSS integrados para Windows Server 2008 y Windows Vista SP1](http://go.microsoft.com/fwlink/?linkid=180896) (http://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Nuevos escritores de VSS integrados para Windows Server 2008 R2 y Windows 7](http://go.microsoft.com/fwlink/?linkid=180897) (http://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Cómo se usan las instantáneas

Además de realizar copias de seguridad de los datos de la aplicación y de la información de estado del sistema, las instantáneas se pueden usar para varios propósitos, entre los que se incluyen los siguientes:

  - Restauración de LUN (resincronización de LUN y intercambio de LUN)  
      
  - Restaurar archivos individuales (Instantáneas para carpetas compartidas)  
      
  - Minería de datos mediante instantáneas transportables  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restauración de LUN (resincronización de LUN y intercambio de LUN)

En Windows Server 2008 R2 y Windows 7, los solicitantes de VSS pueden usar una característica de proveedor de instantáneas de hardware llamada resincronización de LUN (o "resincronización de LUN"). Se trata de un esquema de recuperación rápida que permite a un administrador de aplicaciones restaurar datos de una instantánea en el LUN original o en un LUN nuevo.

La instantánea puede ser un clon completo o una instantánea diferencial. En cualquier caso, al final de la operación de resincronización, el LUN de destino tendrá el mismo contenido que el LUN de instantánea. Durante la operación de resincronización, la matriz realiza una copia de nivel de bloque de la instantánea en el LUN de destino.


> [!NOTE]
> La instantánea debe ser una instantánea de hardware transportable. 
<br>


La mayoría de las matrices permiten que las operaciones de e/s de producción se reanuden poco después de que se inicie la operación de resincronización. Mientras la operación de resincronización está en curso, las solicitudes de lectura se redirigen al LUN de instantáneas y las solicitudes de escritura al LUN de destino. Esto permite a las matrices recuperar conjuntos de datos muy grandes y reanudar las operaciones normales en varios segundos.

La resincronización de LUN es diferente del intercambio de LUN. Un intercambio de LUN es un escenario de recuperación rápida que admite VSS desde Windows Server 2003 SP1. En un intercambio de LUN, la instantánea se importa y luego se convierte en un volumen de lectura y escritura. La conversión es una operación irreversible, y el volumen y el LUN subyacente no se pueden controlar con las API de VSS después. En la lista siguiente se describe cómo se compara la resincronización de LUN con el intercambio de LUN:

  - En la resincronización de LUN, la instantánea no se modifica, por lo que se puede usar varias veces. En el intercambio de LUN, la instantánea solo se puede usar una vez para una recuperación. Para los administradores más conscientes de la seguridad, esto es importante. Cuando se utiliza la resincronización de LUN, el solicitante puede reintentar toda la operación de restauración si se produce un error por primera vez.  
      
  - Al final de un intercambio de LUN, el LUN de instantánea se usa para las solicitudes de e/s de producción. Por esta razón, el LUN de instantánea debe usar la misma calidad de almacenamiento que el LUN de producción original para garantizar que el rendimiento no se vea afectado después de la operación de recuperación. Si en su lugar se usa la resincronización de LUN, el proveedor de hardware puede mantener la instantánea en el almacenamiento que es más económica que el almacenamiento de calidad de producción.  
      
  - Si el LUN de destino no se puede usar y debe volver a crearse, el intercambio de LUN puede ser más económico porque no requiere un LUN de destino.  
      


> [!WARNING]
> Todas las operaciones enumeradas son operaciones de nivel de LUN. Si intenta recuperar un volumen específico mediante la resincronización de LUN, inadvertidamente revierte todos los demás volúmenes que comparten el LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restaurar archivos individuales (Instantáneas para carpetas compartidas)

Instantáneas para carpetas compartidas usa el Servicio de instantáneas de volumen para proporcionar copias en un momento dado de los archivos que se encuentran en un recurso compartido de red, como un servidor de archivos. Con Instantáneas para carpetas compartidas, los usuarios pueden recuperar rápidamente archivos eliminados o modificados que se almacenan en la red. Dado que pueden hacerlo sin la ayuda del administrador, Instantáneas para carpetas compartidas pueden aumentar la productividad y reducir los costos administrativos.

Para obtener más información acerca de instantáneas para carpetas compartidas, vea [instantáneas para carpetas compartidas](http://go.microsoft.com/fwlink/?linkid=180898) (http://go.microsoft.com/fwlink/?LinkId=180898) en TechNet).

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Minería de datos mediante instantáneas transportables

Con un proveedor de hardware diseñado para su uso con el Servicio de instantáneas de volumen, puede crear instantáneas transportables que se pueden importar en servidores del mismo subsistema (por ejemplo, una SAN). Estas instantáneas se pueden usar para inicializar una instalación de producción o de prueba con datos de solo lectura para la minería de datos.

Con el Servicio de instantáneas de volumen y una matriz de almacenamiento con un proveedor de hardware diseñado para su uso con la Servicio de instantáneas de volumen, es posible crear una instantánea del volumen de datos de origen en un servidor y, a continuación, importar la instantánea en otro servidor.  (o volver al mismo servidor). Este proceso se lleva a cabo en unos minutos, independientemente del tamaño de los datos. El proceso de transporte se realiza a través de una serie de pasos que usan un solicitante de instantáneas (una aplicación de administración de almacenamiento) que admite instantáneas transportables.

## <a name="to-transport-a-shadow-copy"></a>Para transportar una instantánea

1.  Cree una instantánea transportable de los datos de origen en un servidor.

2.  Importe la instantánea a un servidor que esté conectado a la SAN (puede importar en un servidor diferente o en el mismo servidor).

3.  Los datos ya están listos para usarse.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   creación y transporte de instantáneas entre dos servidores


> [!NOTE]
> Las instantáneas transportables que se crean en Windows Server 2003 no se pueden importar en un servidor que ejecute Windows Server 2008 o Windows Server 2008 R2. Las instantáneas transportables que se crearon en Windows Server 2008 o Windows Server 2008 R2 no se pueden importar en un servidor que ejecute Windows Server 2003. Sin embargo, una instantánea creada en Windows Server 2008 se puede importar en un servidor que ejecute Windows Server 2008 R2 y viceversa. 
<br>


Las instantáneas son de solo lectura. Si desea convertir una instantánea en un LUN de lectura/escritura, puede usar una aplicación de administración de almacenamiento basada en servicio de disco virtual (incluidos algunos solicitantes) además de la Servicio de instantáneas de volumen. Con esta aplicación, puede quitar la instantánea de Servicio de instantáneas de volumen administración y convertirla en un LUN de lectura/escritura.

Servicio de instantáneas de volumen transporte es una solución avanzada en equipos que ejecutan Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 o Windows Server 2008 R2. Solo funciona si hay un proveedor de hardware en la matriz de almacenamiento. El transporte de instantáneas se puede utilizar para varios propósitos, como copias de seguridad en cinta, minería de datos y pruebas.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Estas preguntas más frecuentes responden a preguntas sobre Servicio de instantáneas de volumen (VSS) para los administradores del sistema. Para obtener información sobre las interfaces de programación de aplicaciones de VSS, vea [servicio de instantáneas de volumen](http://go.microsoft.com/fwlink/?linkid=180899) (http://go.microsoft.com/fwlink/?LinkId=180899) en la biblioteca del centro para desarrolladores de Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>¿Cuándo se ha Servicio de instantáneas de volumen introducido? ¿En qué versiones del sistema operativo Windows está disponible?

VSS se presentó en Windows XP. Está disponible en Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 y Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>¿Cuál es la diferencia entre una instantánea y una copia de seguridad?

En el caso de la copia de seguridad de una unidad de disco duro, la instantánea creada también es la copia de seguridad. Los datos se pueden copiar fuera de la instantánea para una restauración o se puede usar la instantánea para un escenario de recuperación rápida, por ejemplo, resincronización de LUN o intercambio de LUN.

Cuando se copian datos de la instantánea en cinta u otro medio extraíble, el contenido que se almacena en el medio constituye la copia de seguridad. La instantánea se puede eliminar una vez copiados los datos de la misma.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>¿Cuál es el volumen de mayor tamaño que admite Servicio de instantáneas de volumen?

Servicio de instantáneas de volumen admite un tamaño de volumen de hasta 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>He realizado una copia de seguridad en Windows Server 2008. ¿Puedo restaurarlo en Windows Server 2008 R2?

Depende del software de copia de seguridad que haya usado. La mayoría de los programas de copia de seguridad admiten este escenario para datos pero no para copias de seguridad de estado del sistema.

Las instantáneas que se crean en cualquiera de estas versiones de Windows se pueden usar en la otra.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>He realizado una copia de seguridad en Windows Server 2003. ¿Puedo restaurarlo en Windows Server 2008?

Depende del software de copia de seguridad que se use. Si crea una instantánea en Windows Server 2003, no podrá usarla en Windows Server 2008. Además, si crea una instantánea en Windows Server 2008, no podrá restaurarla en Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>¿Cómo se puede deshabilitar VSS?

Es posible deshabilitar el Servicio de instantáneas de volumen mediante Microsoft Management Console. Sin embargo, no debería hacerlo. Deshabilitar VSS afecta negativamente a cualquier software que se use y que dependa de él, como Restaurar sistema y Copias de seguridad de Windows Server.

Para obtener más información, vea los siguientes sitios web de Microsoft TechNet:

  - [Restaurar sistema](http://go.microsoft.com/fwlink/?linkid=157113) (http://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Copias de seguridad de Windows Server](http://go.microsoft.com/fwlink/?linkid=180891) (http://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>¿Puedo excluir archivos de una instantánea para ahorrar espacio?

VSS está diseñado para crear instantáneas de volúmenes completos. Los archivos temporales, como los archivos de paginación, se omiten automáticamente en instantáneas para ahorrar espacio.

Para excluir archivos específicos de instantáneas, use la siguiente clave del registro: **FilesNotToSnapshot**.


> [!NOTE]
> La clave del registro <STRONG>FilesNotToSnapshot</STRONG> está pensada para que la usen únicamente las aplicaciones. Los usuarios que intenten usarlo tendrán limitaciones como las siguientes:
> <br>
> <UL>
> <LI>No puede eliminar archivos de una instantánea creada en un servidor de Windows mediante la característica versiones anteriores.<BR><BR>
> <LI>No puede eliminar archivos de instantáneas para carpetas compartidas.<BR><BR>
> <LI>Puede eliminar archivos de una instantánea que se haya creado con la utilidad <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">DiskShadow</a> , pero no puede eliminar archivos de una instantánea creada con la utilidad <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">vssadmin</a> .<BR><BR>
> <LI>Los archivos se eliminan de una instantánea en función del mejor esfuerzo. Esto significa que no se garantiza que se eliminen.<BR><BR></LI></UL>


Para obtener más información, vea [excluir archivos de instantáneas](http://go.microsoft.com/fwlink/?linkid=180904) (http://go.microsoft.com/fwlink/?LinkId=180904) en MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Error de VSS en mi programa de copia de seguridad que no es de Microsoft. ¿Qué puedo hacer?

Consulte la sección de soporte técnico del sitio web de la compañía que creó el programa de copia de seguridad. Puede que haya una actualización de producto que puede descargar e instalar para solucionar el problema. Si no es así, póngase en contacto con el Departamento de soporte técnico de la empresa.

Los administradores del sistema pueden usar la información de solución de problemas de VSS en el siguiente sitio web de la biblioteca de Microsoft TechNet para recopilar información de diagnóstico acerca de los problemas relacionados con VSS.

Para obtener más información, vea [servicio de instantáneas de volumen](http://go.microsoft.com/fwlink/?linkid=180905) (http://go.microsoft.com/fwlink/?LinkId=180905) en TechNet).

### <a name="what-is-the-diff-area"></a>¿Qué es el "área de diferencias"?

El área de almacenamiento de instantáneas (o "área de diferencia") es la ubicación donde se almacenan los datos de la instantánea que crea el proveedor de software del sistema.

### <a name="where-is-the-diff-area-located"></a>¿Dónde se encuentra el área de diferencia?

El área de diferencia se puede ubicar en cualquier volumen local. Sin embargo, debe encontrarse en un volumen NTFS que tenga espacio suficiente para almacenarlo.

### <a name="how-is-the-diff-area-location-determined"></a>¿Cómo se determina la ubicación del área de diferencia?

Los siguientes criterios se evalúan, en este orden, para determinar la ubicación del área de diferencia:

  - Si un volumen ya tiene una instantánea existente, se utiliza esa ubicación.  
      
  - Si hay una asociación manual preconfigurada entre el volumen original y la ubicación del volumen de instantáneas, se utiliza esa ubicación.  
      
  - Si los dos criterios anteriores no proporcionan una ubicación, el servicio de instantáneas elige una ubicación basada en el espacio libre disponible. Si se está realizando una instantánea de más de un volumen, el servicio de instantáneas crea una lista de posibles ubicaciones de instantáneas en función del tamaño del espacio libre, en orden descendente. El número de ubicaciones proporcionadas es igual al número de volúmenes de los que se está realizando la instantánea.  
      
  - Si el volumen del que se está realizando la instantánea es una de las ubicaciones posibles, se crea una asociación local. De lo contrario, se crea una asociación con el volumen con el mayor espacio disponible.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>¿Puede VSS crear instantáneas de volúmenes que no son NTFS?

Sí. Sin embargo, las instantáneas persistentes solo se pueden realizar para volúmenes NTFS. Además, al menos un volumen montado en el sistema debe ser un volumen NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>¿Cuál es el número máximo de instantáneas que se pueden crear al mismo tiempo?

El número máximo de volúmenes de instantáneas en un único conjunto de instantáneas es 64. Tenga en cuenta que esto no es lo mismo que el número de instantáneas.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>¿Cuál es el número máximo de instantáneas de software creadas por el proveedor del sistema que puedo mantener para un volumen?

El número máximo de instantáneas de software para cada volumen es 512. Sin embargo, de forma predeterminada solo se pueden mantener las instantáneas 64 utilizadas por la característica instantáneas de carpetas compartidas. Para cambiar el límite de la característica instantáneas de carpetas compartidas, utilice la siguiente clave del registro: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>¿Cómo puedo controlar el espacio que se usa para el espacio de almacenamiento de instantáneas?

Escriba el comando **vssadmin Resize shadowstorage** .

Para obtener más información, vea [vssadmin Resize shadowstorage](http://go.microsoft.com/fwlink/?linkid=180906) (http://go.microsoft.com/fwlink/?LinkId=180906) en TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>¿Qué ocurre cuando se queda sin espacio?

Se eliminan las instantáneas del volumen a partir de la instantánea más antigua.

## <a name="volume-shadow-copy-service-tools"></a>Herramientas de Servicio de instantáneas de volumen

El sistema operativo Windows proporciona las siguientes herramientas para trabajar con VSS:

  - [DiskShadow](http://go.microsoft.com/fwlink/?linkid=180907) (http://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](http://go.microsoft.com/fwlink/?linkid=84008) (http://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow es un solicitante de VSS que puede usar para administrar todas las instantáneas de hardware y software que puede tener en un sistema. DiskShadow incluye comandos como los siguientes:

  - **lista**: enumera escritores de VSS, proveedores VSS y instantáneas  
      
  - **crear**: crea una nueva instantánea.  
      
  - **importar**: importa una instantánea transportable  
      
  - **exponer**: expone una instantánea persistente (como una letra de unidad, por ejemplo).  
      
  - **revertir**: revierte un volumen a una instantánea especificada.  
      

Esta herramienta está pensada para que la usen los profesionales de ti, pero los desarrolladores también pueden encontrarla útil al probar un proveedor de VSS Writer o VSS.

DiskShadow solo está disponible en los sistemas operativos Windows Server. No está disponible en los sistemas operativos de cliente de Windows.

### <a name="vssadmin"></a>List

VssAdmin se usa para crear, eliminar y Mostrar información sobre las instantáneas. También se puede usar para cambiar el tamaño del área de almacenamiento de instantáneas ("área de diferencia").

VssAdmin incluye comandos como los siguientes:

  - **crear sombra**: crea una nueva instantánea.  
      
  - **eliminar sombras**: elimina instantáneas  
      
  - **enumerar proveedores**: enumera todos los proveedores VSS registrados  
      
  - enumerar **escritores**: enumera todos los escritores de VSS suscritos  
      
  - **cambiar el tamaño de shadowstorage**: cambia el tamaño máximo del área de almacenamiento de instantáneas  
      

VssAdmin solo se puede usar para administrar las instantáneas creadas por el proveedor de software del sistema.

VssAdmin está disponible en las versiones de sistema operativo cliente de Windows y Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Claves del registro de Servicio de instantáneas de volumen

Las siguientes claves del registro están disponibles para su uso con VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Esta clave se usa para especificar qué usuarios tienen acceso a las instantáneas.

Para obtener más información, vea las siguientes entradas en el sitio web de MSDN:

  - [Consideraciones de seguridad para escritores](http://go.microsoft.com/fwlink/?linkid=157739) (http://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Consideraciones de seguridad para los solicitantes](http://go.microsoft.com/fwlink/?linkid=180908) (http://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Esta clave especifica el número máximo de instantáneas accesibles para el cliente que se pueden almacenar en cada volumen del equipo. Instantáneas para carpetas compartidas usa las instantáneas accesibles para el cliente.

Para obtener más información, vea la siguiente entrada en el sitio web de MSDN:

**MaxShadowCopies** en [las claves del registro para copias de seguridad y restauración](http://go.microsoft.com/fwlink/?linkid=180909) (http://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Esta clave especifica el tamaño mínimo inicial, en MB, del área de almacenamiento de instantáneas.

Para obtener más información, vea la siguiente entrada en el sitio web de MSDN:

**MinDiffAreaFileSize** en [las claves del registro para copias de seguridad y restauración](http://go.microsoft.com/fwlink/?linkid=180910) (http://go.microsoft.com/fwlink/?LinkId=180910)

`##`# ' versiones admitidas del sistema operativo

En la tabla siguiente se enumeran las versiones de sistema operativo mínimas admitidas para las características de VSS.


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
<td><p>Clave del registro <strong>FilesNotToSnapshot</strong></p></td>
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
<td><p>Recuperación rápida mediante el intercambio de LUN</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2003 con SP1</p></td>
</tr>
<tr class="odd">
<td><p>Varias importaciones de instantáneas de hardware</p>
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
<td>Se trata de la capacidad de importar una instantánea más de una vez. Solo se puede realizar una operación de importación a la vez.
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
<td><p>Instantáneas de recuperación automática transportables</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sesiones de copia de seguridad simultáneas (hasta 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Sesión de restauración única simultánea con copias de seguridad</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 con SP2</p></td>
</tr>
<tr class="even">
<td><p>Hasta 8 sesiones de restauración simultáneas con copias de seguridad</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Consulte también

[Servicio de instantáneas de volumen en el centro para desarrolladores de Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)