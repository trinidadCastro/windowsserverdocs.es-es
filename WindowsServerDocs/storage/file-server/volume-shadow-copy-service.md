---
title: Servicio de instantáneas de volumen
ms.date: 01/30/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: f2e8d3bfb5ef907ffb522b5b7be31d1def3001c8
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949683"
---
# <a name="volume-shadow-copy-service"></a>Servicio de instantáneas de volumen

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2, Windows Server 2008, Windows 10, Windows 8.1, Windows 8, Windows 7

La copia de seguridad y la restauración de datos empresariales críticos puede ser una operación muy compleja debido a los siguientes problemas:

  - Normalmente es necesario realizar una copia de seguridad de los datos mientras las aplicaciones que los generan siguen en ejecución. Esto significa que algunos de los archivos de datos pueden estar abiertos o que podrían estar en un estado incoherente.  
      
  - Si el conjunto de datos es de gran tamaño, puede resultar difícil realizar una copia de seguridad de todo el contenido al mismo tiempo.  
      

La realización correcta de las operaciones de copia de seguridad y restauración requiere una estrecha coordinación entre las aplicaciones de copia de seguridad, las aplicaciones de línea de negocio de las que se realiza la copia de seguridad, y el hardware y software de administración del almacenamiento. El Servicio de instantáneas de volumen (VSS), que se presentó en Windows Server® 2003, facilita la conversación entre estos componentes para permitirles funcionar mejor juntos. Cuando todos los componentes son compatibles con VSS, puedes usarlos para realizar una copia de seguridad de los datos de la aplicación sin desconectar las aplicaciones.

VSS coordina las acciones necesarias para crear una instantánea coherente (también conocida como copia de un momento específico) de los datos de los que se va a hacer una copia de seguridad. La instantánea se puede usar tal cual o en escenarios como los siguientes:

  - Quieres realizar una copia de seguridad de los datos de aplicación y la información de estado del sistema, incluido el archivado de datos en otra unidad de disco duro, cinta u otros medios extraíbles.  
      
  - Realizas minería de datos.  
      
  - Vas a realizar copias de seguridad de disco a disco.  
      
  - Necesitas recuperarte rápidamente de una pérdida de datos mediante la restauración de los datos en el LUN original o en un LUN completamente nuevo que reemplace al LUN original que produjo el error.  
      

Entre las características y aplicaciones de Windows que usan VSS, se incluyen las siguientes:

  - [Copias de seguridad de Windows Server](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkId=180891)  
      
  - [Instantáneas de carpetas compartidas](https://go.microsoft.com/fwlink/?linkid=142874) (https://go.microsoft.com/fwlink/?LinkId=142874)  
      
  - [System Center Data Protection Manager](https://go.microsoft.com/fwlink/?linkid=180892) (https://go.microsoft.com/fwlink/?LinkId=180892)  
      
  - [Restaurar sistema](https://go.microsoft.com/fwlink/?linkid=180893) (https://go.microsoft.com/fwlink/?LinkId=180893)  
      

## <a name="how-volume-shadow-copy-service-works"></a>Funcionamiento del Servicio de instantáneas de volumen

Una solución completa de VSS requiere todos los elementos básicos a continuación:

**Servicio VSS**   parte del sistema operativo Windows que garantiza que los demás componentes pueden comunicarse entre sí correctamente y funcionar juntos.

**Solicitante de VSS**   software que solicita realmente la creación de instantáneas (u otras operaciones de alto nivel, como su importación o eliminación). Normalmente, se trata de la aplicación de copia de seguridad. La utilidad Copias de seguridad de Windows Server y la aplicación System Center Data Protection Manager son solicitantes de VSS. Entre los solicitantes de VSS que no son de Microsoft® se incluye casi todo el software de copia de seguridad que se ejecuta en Windows.

**VSS Writer**   componente que garantiza que existe un conjunto de datos coherente para realizar la copia de seguridad. Normalmente, se proporciona como parte de una aplicación de línea de negocio, como SQL Server® o Exchange Server. Las instancias de VSS Writer para varios componentes de Windows, como el registro, se incluyen con el sistema operativo Windows. Las instancias de VSS Writer que no son de Microsoft se incluyen con muchas aplicaciones para Windows que necesitan garantizar la coherencia de los datos durante la copia de seguridad.

**Proveedor de VSS**   componente que crea y mantiene las instantáneas. Puede tener lugar en el software o en el hardware. El sistema operativo Windows incluye un proveedor de VSS que usa la operación de copia en escritura. Si utilizas una red de área de almacenamiento (SAN), es importante que instales el proveedor de hardware de VSS para la SAN, si se proporciona uno. Un proveedor de hardware retira la tarea de creación y mantenimiento de una instantánea del sistema operativo host.

En el diagrama siguiente se ilustra cómo el servicio VSS se coordina con solicitantes, escritores y proveedores para crear la instantánea de un volumen.

![](media/volume-shadow-copy-service/Ee923636.94dfb91e-8fc9-47c6-abc6-b96077196741(WS.10).jpg)

**Figura 1**   diagrama arquitectónico del Servicio de instantáneas de volumen

### <a name="how-a-shadow-copy-is-created"></a>Creación de una instantánea

En esta sección se contextualizan los distintos roles del solicitante, el escritor y el proveedor mediante la descripción de los pasos que hay que realizar para crear una instantánea. En el diagrama siguiente se muestra cómo el Servicio de instantáneas de volumen controla la coordinación general entre el solicitante, el escritor y el proveedor.

![](media/volume-shadow-copy-service/Ee923636.1c481a14-d6bc-4796-a3ff-8c6e2174749b(WS.10).jpg)

**Figura 2** Proceso de creación de instantáneas

Para crear una instantánea, el solicitante, el escritor y el proveedor realizan las siguientes acciones:

1.  El solicitante pide al Servicio de instantáneas de volumen que enumere a los escritores, recopile los metadatos del escritor y se prepare para la creación de la instantánea.  
      
2.  Cada escritor crea una descripción XML de los componentes y almacenes de datos de los que es necesario hacer una copia de seguridad y se la proporciona al Servicio de instantáneas de volumen. El escritor también define un método de restauración, que se utiliza para todos los componentes. El Servicio de instantáneas de volumen proporciona la descripción del escritor al solicitante, que selecciona los componentes para los que se hará una copia de seguridad.  
      
3.  El Servicio de instantáneas de volumen notifica a todos los escritores que preparen sus datos para realizar una instantánea.  
      
4.  Cada escritor prepara los datos según corresponda; por ejemplo, finaliza todas las transacciones abiertas, actualiza los registros de transacciones y vacía las memorias caché. Cuando los datos están listos para crear una instantánea de ellos, el escritor informa al Servicio de instantáneas de volumen.  
      
5.  El Servicio de instantáneas de volumen indica a los escritores que inmovilicen temporalmente las solicitudes de E/S de escritura de la aplicación (las solicitudes de E/S de lectura siguen estando disponibles) durante los segundos que se requieren para crear la instantánea del volumen o volúmenes. No se permite que la inmovilización de la aplicación dure más de 60 segundos. El Servicio de instantáneas de volumen vacía los búferes del sistema de archivos y, a continuación, inmoviliza el sistema de archivos, lo que garantiza que los metadatos del sistema de archivos se registran correctamente y que los datos de los que se va a realizar una instantánea se escriben en un orden coherente.  
      
6.  El Servicio de instantáneas de volumen indica al proveedor que cree la instantánea. El proceso de creación de instantáneas no dura más de 10 segundos, durante los cuales todas las solicitudes de E/S de escritura en el sistema de archivos permanecen inmovilizadas.  
      
7.  El Servicio de instantáneas de volumen libera las solicitudes de E/S de escritura del sistema de archivos.  
      
8.  VSS indica a los escritores que reanuden las solicitudes de E/S de escritura de la aplicación. En este momento, las aplicaciones pueden reanudar la escritura de datos en el disco del que se ha realizado la instantánea.  
      

> [!NOTE]
> Se puede anular la creación de instantáneas si los escritores se mantienen en estado de inmovilización durante más de 60 segundos o si los proveedores tardan más de 10 segundos en confirmar la instantánea. 
<br>

9. El solicitante puede reintentar el proceso (volver al paso 1) o notificar al administrador que vuelva a intentarlo más tarde.  
      
10. Si la instantánea se crea de forma correcta, el Servicio de instantáneas de volumen devuelve la información de ubicación de la instantánea al solicitante. En algunos casos, la instantánea puede estar disponible temporalmente como un volumen de lectura y escritura para que VSS y una o varias aplicaciones puedan modificar su contenido antes de que esté terminada. Una vez que VSS y las aplicaciones realizan las modificaciones, la instantánea cambia al modo de solo lectura. Esta fase se denomina Recuperación automática y se usa para deshacer cualquier transacción del sistema de archivos o de la aplicación en el volumen de instantáneas que no se haya completado antes de crear la instantánea.  
      

### <a name="how-the-provider-creates-a-shadow-copy"></a>Cómo el proveedor crea una instantánea

Un proveedor de instantáneas de hardware o software usa alguno de los métodos siguientes para crear una instantánea:

**Copia completa**   este método realiza una copia completa (denominada "clon") del volumen original en un momento específico. Esta copia es de solo lectura.

**Copia en escritura**   este método no copia el volumen original. En su lugar, realiza una copia diferencial al copiar todos los cambios (solicitudes de E/S de escritura completadas) que se realizan en el volumen después de un momento específico.

**Redirección en escritura**   este método no copia el volumen original y no realiza ningún cambio en el volumen original después de un momento específico. En su lugar, realiza una copia diferencial al redirigir todos los cambios a un volumen diferente.

## <a name="complete-copy"></a>Copia completa

Normalmente se crea una copia completa mediante la creación de un "reflejo dividido" como se indica a continuación:

1.  El volumen original y el volumen de la instantánea son un conjunto de volúmenes reflejado.  
      
2.  El volumen de la instantánea se separa del volumen original. De este modo, se interrumpe la conexión de reflejo.  
      

Una vez interrumpida la conexión de reflejo, el volumen original y el volumen de la instantánea son independientes. El volumen original continúa aceptando todos los cambios (solicitudes de E/S de escritura), mientras que el volumen de la instantánea sigue siendo una copia exacta de solo lectura de los datos originales en el momento de la interrupción.

### <a name="copy-on-write-method"></a>Método de copia en escritura

En el método de copia en escritura, cuando se produce un cambio en el volumen original (aunque antes de que se complete la solicitud de E/S de escritura), cada bloque que se va a modificar se lee y se escribe en el área de almacenamiento de instantáneas del volumen (también denominada "área de diferencia"). El área de almacenamiento de instantáneas puede estar en el mismo volumen o en uno diferente. Esta conserva una copia del bloque de datos en el volumen original antes de que el cambio lo sobrescriba.


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
<td><p>Datos originales: 1 2 3 4 5</p></td>
<td><p>Sin copia: —</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Datos cambiados en la caché: 3 a 3'</p></td>
<td><p>Instantánea creada (solo diferencias): 3</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Datos originales sobrescritos: 1 2 3' 4 5</p></td>
<td><p>Diferencias e índice almacenados en la instantánea: 3</p></td>
</tr>
</tbody>
</table>

**Tabla 1**   Método de copia en escritura para la creación de instantáneas

El método de copia en escritura es un método rápido para crear una instantánea, ya que solo copia los datos que se han modificado. Los bloques copiados en el área de diferencia se pueden combinar con los datos modificados en el volumen original para restaurar el volumen al estado anterior a los cambios realizados. Si hay muchos cambios, el método de copia en escritura puede resultar costoso.

### <a name="redirect-on-write-method"></a>Método de redirección en escritura

En el método de redirección en escritura, siempre que el volumen original recibe un cambio (solicitud de E/S de escritura), el cambio no se aplica al volumen original. En su lugar, el cambio se escribe en el área de almacenamiento de instantáneas de otro volumen.


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
<td><p>Datos originales: 1 2 3 4 5</p></td>
<td><p>Sin copia: —</p></td>
</tr>
<tr class="even">
<td><p>T1</p></td>
<td><p>Datos cambiados en la caché: 3 a 3'</p></td>
<td><p>Instantánea creada (solo diferencias): 3'</p></td>
</tr>
<tr class="odd">
<td><p>T2</p></td>
<td><p>Datos originales sin cambios: 1 2 3 4 5</p></td>
<td><p>Diferencias e índice almacenados en la instantánea: 3'</p></td>
</tr>
</tbody>
</table>

**Tabla 2**   Método de redirección en escritura para la creación de instantáneas

Al igual que el método de copia en escritura, el método de redirección en escritura es un método rápido para crear una instantánea, ya que solo copia las modificaciones de los datos. Los bloques copiados en el área de diferencia se pueden combinar con los datos sin modificar en el volumen original para crear una copia completa y actualizada de los datos. Si hay muchas solicitudes de E/S de lectura, el método de redirección en escritura puede resultar costoso.

## <a name="shadow-copy-providers"></a>Proveedores de instantáneas

Hay dos tipos de proveedores de instantáneas: los proveedores basados en hardware y los proveedores basados en software. También hay un proveedor del sistema, que es un proveedor de software integrado en el sistema operativo Windows.

### <a name="hardware-based-providers"></a>Proveedores basados en hardware

Los proveedores de instantáneas basados en hardware actúan como una interfaz entre el Servicio de instantáneas de volumen y el nivel de hardware, ya que trabajan junto con un controlador o adaptador de almacenamiento de hardware. La matriz de almacenamiento realiza las tareas de creación y mantenimiento de la instantánea.

Los proveedores de hardware siempre realizan la instantánea a partir de un LUN completo, pero el Servicio de instantáneas de volumen solo expone la instantánea del volumen o volúmenes solicitados.

Un proveedor de instantáneas basado en hardware usa la funcionalidad de Servicio de instantáneas de volumen que define el momento específico, permite la sincronización de datos, administra la instantánea y proporciona una interfaz común con las aplicaciones de copia de seguridad. Sin embargo, el Servicio de instantáneas de volumen no especifica el mecanismo subyacente a través del cual el proveedor basado en hardware genera y mantiene instantáneas.

### <a name="software-based-providers"></a>Proveedores basados en software

Los proveedores de instantáneas basados en software suelen interceptar y procesar las solicitudes de E/S de lectura y escritura en un nivel de software entre el sistema de archivos y el software del administrador de volúmenes.

Estos proveedores se implementan como un componente DLL de modo usuario y, como mínimo, un controlador de dispositivo de modo kernel, normalmente un controlador de filtro de almacenamiento. A diferencia de los proveedores basados en hardware, los proveedores basados en software crean instantáneas en el nivel de software, no en el de hardware.

Un proveedor de instantáneas basado en software debe mantener una vista de "un momento específico" de un volumen al proporcionar acceso a un conjunto de datos que se pueda usar para volver a crear el estado del volumen antes de la hora de creación de la instantánea. Un ejemplo de ello es la técnica de copia en escritura del proveedor del sistema. Sin embargo, el Servicio de instantáneas de volumen no tiene ninguna restricción sobre la técnica que los proveedores basados en software utilizan para crear y mantener instantáneas.

En comparación con un proveedor basado en hardware, un proveedor de software se puede aplicar a una gama de plataformas de almacenamiento más amplia, y debe funcionar igual de bien con discos básicos o volúmenes lógicos. (Un volumen lógico es un volumen que se crea al combinar espacio libre en dos o más discos). A diferencia de las instantáneas de hardware, los proveedores de software consumen recursos del sistema operativo para conservar la instantánea.

Para obtener más información acerca de los discos básicos, consulta [¿Qué son los discos y volúmenes básicos?](https://go.microsoft.com/fwlink/?linkid=180894) (https://go.microsoft.com/fwlink/?LinkId=180894) en TechNet.

### <a name="system-provider"></a>Proveedor del sistema

Un proveedor de instantáneas, el proveedor del sistema, se incluye en el sistema operativo Windows. Aunque se proporciona un proveedor predeterminado en Windows, otros proveedores pueden proporcionar implementaciones optimizadas para sus aplicaciones de software y hardware de almacenamiento.

Para mantener la vista de "un momento específico" de un volumen que se encuentra en una instantánea, el proveedor del sistema utiliza una técnica de copia en escritura. En el área de almacenamiento de una instantánea se almacenan copias de los bloques del volumen que se han modificado desde que inició la creación de la instantánea.

El proveedor del sistema puede exponer el volumen de producción, en el que se puede escribir y leer de la forma habitual. Cuando la instantánea se necesita, dicho proveedor aplica de forma lógica las diferencias a los datos en el volumen de producción para exponer la instantánea completa.

En el caso del proveedor del sistema, el área de almacenamiento de instantáneas debe encontrarse en un volumen NTFS. No es necesario que el volumen del que se va a realizar la instantánea sea un volumen NTFS, pero al menos un volumen montado en el sistema debe ser de este tipo.

Los archivos de componentes que componen al proveedor del sistema son swprv.dll y volsnap.sys.

### <a name="in-box-vss-writers"></a>Instancias de VSS Writer incluidas

El sistema operativo Windows incluye un conjunto de VSS Writer que son responsables de enumerar los datos que diversas características de Windows requieren.

Para obtener más información acerca de dichos escritores, visita los siguientes sitios web de Microsoft:

  - [Instancias de VSS Writer incluidas](https://go.microsoft.com/fwlink/?linkid=180895) (https://go.microsoft.com/fwlink/?LinkId=180895)  
      
  - [Nuevos VSS Writer incluidos para Windows Server 2008 y Windows Vista SP1](https://go.microsoft.com/fwlink/?linkid=180896) (https://go.microsoft.com/fwlink/?LinkId=180896)  
      
  - [Nuevos VSS Writer incluidos para Windows Server 2008 R2 y Windows 7](https://go.microsoft.com/fwlink/?linkid=180897) (https://go.microsoft.com/fwlink/?LinkId=180897)  
      

## <a name="how-shadow-copies-are-used"></a>Uso de las instantáneas

Además de realizar copias de seguridad de los datos de la aplicación y de la información de estado del sistema, las instantáneas se pueden usar con varios fines, entre los que se incluyen los siguientes:

  - Restauración de LUN (resincronización e intercambio de LUN)  
      
  - Restauración de archivos individuales (Instantáneas para carpetas compartidas)  
      
  - Minería de datos mediante instantáneas transportables  
      

### <a name="restoring-luns-lun-resynchronization-and-lun-swapping"></a>Restauración de LUN (resincronización e intercambio de LUN)

En Windows Server 2008 R2 y Windows 7, los solicitantes de VSS pueden usar una característica del proveedor de instantáneas de hardware llamada resincronización de LUN. Se trata de un esquema de recuperación rápida que permite a un administrador de aplicaciones restaurar datos desde una instantánea en el LUN original o en un LUN nuevo.

La instantánea puede ser un clon completo o una instantánea diferencial. En ambos casos, al final de la operación de resincronización, el LUN de destino tendrá el mismo contenido que el LUN de instantánea. Durante la operación de resincronización, la matriz realiza una copia del nivel de bloque de la instantánea en el LUN de destino.


> [!NOTE]
> La instantánea debe ser de hardware transportable. 
<br>


La mayoría de las matrices permiten que las operaciones de E/S de producción se reanuden poco después de que se ha iniciado la operación de resincronización. Mientras la operación de resincronización está en curso, las solicitudes de lectura se redirigen al LUN de la instantánea y las solicitudes de escritura al LUN de destino. Esto permite a las matrices recuperar conjuntos de datos de gran tamaño y reanudar las operaciones normales en varios segundos.

La resincronización de LUN es diferente del intercambio de LUN. Un intercambio de LUN es un escenario de recuperación rápida que es compatible con VSS desde Windows Server 2003 SP1. En un intercambio de LUN, la instantánea se importa y luego se convierte en un volumen de lectura y escritura. La conversión es una operación irreversible, y tanto el volumen como el LUN subyacente no se pueden controlar con las API de VSS posteriormente. En la lista siguiente se describen las diferencias entre la resincronización de LUN y el intercambio de LUN:

  - En la resincronización de LUN, la instantánea no se modifica, por lo que se puede usar varias veces. En el intercambio de LUN, la instantánea solo se puede usar una vez para una recuperación. Para los administradores más preocupados por la seguridad, esto es importante. Cuando se usa la resincronización de LUN, el solicitante puede reintentar toda la operación de restauración si se produce un error la primera vez.  
      
  - Al final de un intercambio de LUN, el LUN de instantánea se usa para las solicitudes de E/S de producción. Por este motivo, el LUN de instantánea debe usar la misma calidad de almacenamiento que el LUN de producción original, para garantizar que el rendimiento no se vea afectado después de la operación de recuperación. Si en su lugar se usa la resincronización de LUN, el proveedor de hardware puede mantener la instantánea en un almacenamiento que sea más económico que el almacenamiento de calidad de producción.  
      
  - Si el LUN de destino no se puede usar y debe volver a crearse, el intercambio de LUN puede resultar más económico, ya que no requiere un LUN de destino.  
      


> [!WARNING]
> Todas las operaciones mencionadas son operaciones de nivel de LUN. Si intentas recuperar un volumen específico mediante la resincronización de LUN, revertirás involuntariamente todos los demás volúmenes que comparten el LUN. 
<br>


### <a name="restoring-individual-files-shadow-copies-for-shared-folders"></a>Restauración de archivos individuales (Instantáneas para carpetas compartidas)

Instantáneas para carpetas compartidas usa el Servicio de instantáneas de volumen para proporcionar copias de un momento específico de los archivos que se encuentran en un recurso compartido de red, como un servidor de archivos. Con Instantáneas para carpetas compartidas, los usuarios pueden recuperar rápidamente los archivos eliminados o modificados que están almacenados en la red. Dado que pueden hacerlo sin la ayuda del administrador, Instantáneas para carpetas compartidas puede aumentar la productividad y reducir los costos administrativos.

Para obtener más información acerca de Instantáneas para carpetas compartidas, consulta [Instantáneas para carpetas compartidas](https://go.microsoft.com/fwlink/?linkid=180898) (https://go.microsoft.com/fwlink/?LinkId=180898) en TechNet.

### <a name="data-mining-by-using-transportable-shadow-copies"></a>Minería de datos mediante instantáneas transportables

Con un proveedor de hardware diseñado para usarse con el Servicio de instantáneas de volumen, puedes crear instantáneas transportables que se pueden importar en servidores del mismo subsistema (por ejemplo, una SAN). Estas instantáneas se pueden usar para inicializar una instalación de producción o de pruebas con datos de solo lectura para la minería de datos.

Con el Servicio de instantáneas de volumen y una matriz de almacenamiento con un proveedor de hardware diseñado para usarse con el Servicio de instantáneas de volumen, puedes crear una instantánea del volumen de datos de origen en un servidor y, a continuación, importarla a otro servidor (o al mismo servidor). Este proceso se lleva a cabo en unos minutos, independientemente del tamaño de los datos. El proceso de transporte se realiza a mediante una serie de pasos que usan un solicitante de instantáneas (una aplicación de administración de almacenamiento) que es compatible con las instantáneas transportables.

## <a name="to-transport-a-shadow-copy"></a>Para transportar una instantánea

1.  Crea una instantánea transportable de los datos de origen en un servidor.

2.  Importa la instantánea a un servidor que esté conectado a la SAN (puedes importarla a un servidor diferente o al mismo).

3.  Los datos ya están listos para usarse.

![](media/volume-shadow-copy-service/Ee923636.633752e0-92f6-49a7-9348-f451b1dc0ed7(WS.10).jpg)

**Figura 3**   Creación y transporte de instantáneas entre dos servidores


> [!NOTE]
> Las instantáneas transportables que se crean en Windows Server 2003 no se pueden importar a un servidor que ejecute Windows Server 2008 o Windows Server 2008 R2. Las instantáneas transportables que se crearon en Windows Server 2008 o Windows Server 2008 R2 no se pueden importar a un servidor que ejecuta Windows Server 2003. No obstante, una instantánea creada en Windows Server 2008 no se puede importar a un servidor que ejecute Windows Server 2008 R2 y viceversa. 
<br>


Las instantáneas son de solo lectura. Si quieres convertir una instantánea en un LUN de lectura o escritura, puedes usar una aplicación de administración de almacenamiento basada en Servicio de disco virtual (incluidos algunos solicitantes) junto con el Servicio de instantáneas de volumen. Con esta aplicación, puedes quitar la instantánea de la administración de Servicio de instantáneas de volumen y convertirla en un LUN de lectura o escritura.

El transporte del Servicio de instantáneas de volumen es una solución avanzada de los equipos que ejecutan Windows Server 2003 Enterprise Edition, Windows Server 2003 Datacenter Edition, Windows Server 2008 o Windows Server 2008 R2. Solo funciona si hay un proveedor de hardware en la matriz de almacenamiento. El transporte de instantáneas se puede utilizar con varios fines, como copias de seguridad en cinta, minería de datos y pruebas.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

Estas preguntas más frecuentes responden a preguntas sobre el Servicio de instantáneas de volumen (VSS) para los administradores del sistema. Para obtener información sobre las interfaces de programación de aplicaciones de VSS, consulta [Servicio de instantáneas de volumen](https://go.microsoft.com/fwlink/?linkid=180899) (https://go.microsoft.com/fwlink/?LinkId=180899) en la biblioteca del Centro para desarrolladores de Windows.

### <a name="when-was-volume-shadow-copy-service-introduced-on-which-windows-operating-system-versions-is-it-available"></a>¿Cuándo se introdujo el Servicio de instantáneas de volumen? ¿En qué versiones del sistema operativo Windows está disponible?

VSS se introdujo en Windows XP. Está disponible en Windows XP, Windows Server 2003, Windows Vista®, Windows Server 2008, Windows 7 y Windows Server 2008 R2.

### <a name="what-is-the-difference-between-a-shadow-copy-and-a-backup"></a>¿Qué diferencia hay entre una instantánea y una copia de seguridad?

En el caso de la copia de seguridad de una unidad de disco duro, la instantánea creada también es una copia de seguridad. Los datos se pueden copiar desde de la instantánea para una restauración o se puede usar la instantánea en un escenario de recuperación rápida; por ejemplo, resincronización o intercambio de LUN.

Cuando se copian datos de la instantánea a una cinta u otro medio extraíble, el contenido que se almacena en el medio constituye la copia de seguridad. La instantánea se puede eliminar una vez copiados los datos de la misma.

### <a name="what-is-the-largest-size-volume-that-volume-shadow-copy-service-supports"></a>¿Cuál es el mayor tamaño de volumen compatible con el Servicio de instantáneas de volumen?

El Servicio de instantáneas de volumen admite un tamaño de volumen de hasta 64 TB.

### <a name="i-made-a-backup-on-windows-server2008-can-i-restore-it-on-windows-server2008r2"></a>Realicé una copia de seguridad en Windows Server 2008. ¿Puedo restaurarla en Windows Server 2008 R2?

Depende del software de copia de seguridad que hayas usado. La mayoría de los programas de copia de seguridad admiten este escenario para los datos, pero no para las copias de seguridad del estado del sistema.

Las instantáneas creadas en cualquiera de estas versiones de Windows se pueden usar en la otra.

### <a name="i-made-a-backup-on-windows-server2003-can-i-restore-it-on-windows-server2008"></a>Realicé una copia de seguridad en Windows Server 2003. ¿Puedo restaurarla en Windows Server 2008?

Depende del software de copia de seguridad que usaste. Si creas una instantánea en Windows Server 2003, no podrás usarla en Windows Server 2008. Además, si creas una instantánea en Windows Server 2008, no podrás restaurarla en Windows Server 2003.

### <a name="how-can-i-disable-vss"></a>¿Cómo se puede deshabilitar VSS?

Es posible deshabilitar el Servicio de instantáneas de volumen mediante Microsoft Management Console. Sin embargo, no deberías hacerlo. La desactivación de VSS afecta negativamente a todo el software en uso que dependa de dicho servicio, como Restaurar sistema y Copias de seguridad de Windows Server.

Para obtener más información, visita los siguientes sitios web de Microsoft TechNet:

  - [Restaurar sistema](https://go.microsoft.com/fwlink/?linkid=157113) (https://go.microsoft.com/fwlink/?LinkID=157113)  
      
  - [Copias de seguridad de Windows Server](https://go.microsoft.com/fwlink/?linkid=180891) (https://go.microsoft.com/fwlink/?LinkID=180891)  
      

### <a name="can-i-exclude-files-from-a-shadow-copy-to-save-space"></a>¿Puedo excluir archivos de una instantánea para ahorrar espacio?

VSS está diseñado para crear instantáneas de volúmenes completos. Los archivos temporales, como los archivos de paginación, se omiten automáticamente en las instantáneas para ahorrar espacio.

Para excluir archivos específicos de las instantáneas, usa la siguiente clave del registro: **FilesNotToSnapshot**.


> [!NOTE]
> La clave del registro <STRONG>FilesNotToSnapshot</STRONG> está pensada para que solo las aplicaciones la usen. Los usuarios que intenten usarla tendrán limitaciones como las siguientes:
> <br>
> <UL>
> <LI>No puede eliminar archivos de una instantánea creada en Windows Server mediante la característica Versiones anteriores.<BR><BR>
> <LI>No puede eliminar archivos de las instantáneas para carpetas compartidas.<BR><BR>
> <LI>Puede eliminar archivos de una instantánea creada con la utilidad <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow" data-raw-source="[Diskshadow](https://docs.microsoft.com/windows-server/administration/windows-commands/diskshadow)">DiskShadow</a>, pero no de una que se haya creado con la utilidad <a href="https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin" data-raw-source="[Vssadmin](https://docs.microsoft.com/windows-server/administration/windows-commands/vssadmin)">Vssadmin</a>.<BR><BR>
> <LI>Los archivos se eliminan de una instantánea en función de la mejor opción. Como resultado, no se garantiza que se eliminen.<BR><BR></LI></UL>


Para obtener más información, consulta [Exclusión de archivos de las instantáneas](https://go.microsoft.com/fwlink/?linkid=180904) (https://go.microsoft.com/fwlink/?LinkId=180904) en MSDN.

### <a name="my-non-microsoft-backup-program-failed-with-a-vss-error-what-can-i-do"></a>Hubo un error de VSS en mi programa de copia de seguridad que no es de Microsoft. ¿Qué se puede hacer?

Consulta la sección de soporte técnico del sitio web de la empresa que creó el programa de copia de seguridad. Puede que haya una actualización de producto que puedas descargar e instalar para solucionar el problema. Si no es así, ponte en contacto con el departamento de soporte técnico de la empresa.

Los administradores del sistema pueden usar la información de solución de problemas de VSS en el siguiente sitio web de la biblioteca de Microsoft TechNet para recopilar información de diagnóstico acerca de los problemas relacionados con VSS.

Para obtener más información, consulta [Servicio de instantáneas de volumen](https://go.microsoft.com/fwlink/?linkid=180905) (https://go.microsoft.com/fwlink/?LinkId=180905) en TechNet.

### <a name="what-is-the-diff-area"></a>¿Qué es el "área de diferencia"?

El área de almacenamiento de instantáneas (o "área de diferencia") es la ubicación donde se almacenan los datos de la instantánea que crea el proveedor de software del sistema.

### <a name="where-is-the-diff-area-located"></a>¿Dónde se encuentra el área de diferencia?

El área de diferencia puede encontrarse en cualquier volumen local. Sin embargo, debe encontrarse en un volumen NTFS que tenga espacio suficiente para almacenarla.

### <a name="how-is-the-diff-area-location-determined"></a>¿Cómo se define la ubicación del área de diferencia?

Se evalúan los siguientes criterios, en este orden, para determinar la ubicación del área de diferencia:

  - Si un volumen ya tiene una instantánea, se utiliza esa ubicación.  
      
  - Si hay una asociación manual preconfigurada entre el volumen original y la ubicación del volumen de instantáneas, se utiliza esa ubicación.  
      
  - Si los dos criterios anteriores no proporcionan una ubicación, el servicio de instantáneas elige una ubicación en función del espacio libre disponible. Si se va a realizar una instantánea de más de un volumen, el servicio de instantáneas crea una lista de posibles ubicaciones de instantáneas en función de la cantidad de espacio libre, en orden descendente. El número de ubicaciones proporcionadas es igual al número de volúmenes de los que se está realizando una instantánea.  
      
  - Si el volumen del que se está realizando la instantánea es una de las ubicaciones posibles, se crea una asociación local. De lo contrario, se crea una asociación con el volumen que tiene el mayor espacio disponible.  
      

### <a name="can-vss-create-shadow-copies-of-non-ntfs-volumes"></a>¿VSS puede crear instantáneas de volúmenes que no son NTFS?

Sí. No obstante, las instantáneas persistentes solo se pueden realizar para volúmenes NTFS. Además, al menos un volumen montado en el sistema debe ser NTFS.

### <a name="whats-the-maximum-number-of-shadow-copies-i-can-create-at-one-time"></a>¿Cuál es el número máximo de instantáneas que se pueden crear al mismo tiempo?

El número máximo de volúmenes con instantáneas en un único conjunto de instantáneas es 64. Ten en cuenta que esta cifra no es lo misma que el número de instantáneas.

### <a name="whats-the-maximum-number-of-software-shadow-copies-created-by-the-system-provider-that-i-can-maintain-for-a-volume"></a>¿Cuál es el número máximo de instantáneas de software creadas por el proveedor del sistema que se puede mantener para un volumen?

El número máximo de instantáneas de software para cada volumen es 512. Sin embargo, de forma predeterminada solo se pueden mantener 64 instantáneas utilizadas por la característica Instantáneas de carpetas compartidas. Para cambiar el límite de la característica Instantáneas para carpetas compartidas, usa la siguiente clave del registro: **MaxShadowCopies**.

### <a name="how-can-i-control-the-space-that-is-used-for-shadow-copy-storage-space"></a>¿Cómo puedo controlar el espacio de almacenamiento que se usa para las instantáneas?

Escribe el comando **vssadmin resize shadowstorage**.

Para obtener más información, consulta [Vssadmin resize shadowstorage](https://go.microsoft.com/fwlink/?linkid=180906) (https://go.microsoft.com/fwlink/?LinkId=180906) en TechNet.

### <a name="what-happens-when-i-run-out-of-space"></a>¿Qué sucede cuando se agota el espacio?

Se eliminan las instantáneas del volumen, comenzando por la instantánea más antigua.

## <a name="volume-shadow-copy-service-tools"></a>Herramientas del Servicio de instantáneas de volumen

El sistema operativo Windows proporciona las siguientes herramientas para trabajar con VSS:

  - [DiskShadow](https://go.microsoft.com/fwlink/?linkid=180907) (https://go.microsoft.com/fwlink/?LinkId=180907)  
      
  - [VssAdmin](https://go.microsoft.com/fwlink/?linkid=84008) (https://go.microsoft.com/fwlink/?LinkId=84008)  
      

### <a name="diskshadow"></a>DiskShadow

DiskShadow es un solicitante de VSS que puedes usar para administrar todas las instantáneas de hardware y software que se pueden tener en un sistema. DiskShadow incluye comandos como los siguientes:

  - **list**: enumera los escritores y proveedores de VSS, además de las instantáneas.  
      
  - **create**: crea una nueva instantánea.  
      
  - **import**: importa una instantánea transportable.  
      
  - **expose**: expone una instantánea persistente (como una letra de unidad, por ejemplo).  
      
  - **revert**: revierte un volumen a una instantánea especificada.  
      

Esta herramienta está pensada para que la usen los profesionales de TI, aunque también puede resultar útil para los desarrolladores cuando realizan pruebas para VSS Writer o un proveedor de VSS.

DiskShadow solo está disponible en los sistemas operativos Windows Server. No está disponible en sistemas operativos de cliente de Windows.

### <a name="vssadmin"></a>VssAdmin

VssAdmin se usa para crear, eliminar y mostrar información sobre las instantáneas. También se puede usar para cambiar el tamaño del área de almacenamiento de instantáneas ("área de diferencia").

VssAdmin incluye comandos como los siguientes:

  - **create shadow**: crea una nueva instantánea.  
      
  - **delete shadows**: elimina las instantáneas.  
      
  - **list providers**: enumera todos los proveedores de VSS registrados.  
      
  - **list writers**: enumera todos los VSS Writer suscritos.  
      
  - **resize shadowstorage**: cambia el tamaño máximo del área de almacenamiento de instantáneas.  
      

VssAdmin solo se puede usar para administrar las instantáneas creadas por el proveedor de software del sistema.

VssAdmin está disponible en las versiones de sistema operativo cliente de Windows y Windows Server.

## <a name="volume-shadow-copy-service-registry-keys"></a>Claves del registro de Servicio de instantáneas de volumen remoto

Las siguientes claves del registro están disponibles para usarse con VSS:

  - **VssAccessControl**  
      
  - **MaxShadowCopies**  
      
  - **MinDiffAreaFileSize**  
      

### <a name="vssaccesscontrol"></a>VssAccessControl

Esta clave se usa para especificar qué usuarios tienen acceso a las instantáneas.

Para obtener más información, consulta las siguientes publicaciones en el sitio web de MSDN:

  - [Consideraciones de seguridad para escritores](https://go.microsoft.com/fwlink/?linkid=157739) (https://go.microsoft.com/fwlink/?LinkId=157739)  
      
  - [Consideraciones de seguridad para solicitantes](https://go.microsoft.com/fwlink/?linkid=180908) (https://go.microsoft.com/fwlink/?LinkId=180908)  
      

### <a name="maxshadowcopies"></a>MaxShadowCopies

Esta clave especifica el número máximo de instantáneas accesibles para el cliente que se pueden almacenar en cada volumen del equipo. La característica Instantáneas para carpetas compartidas usa las instantáneas accesibles para el cliente.

Para obtener más información, consulta la siguiente publicación en el sitio web de MSDN:

**MaxShadowCopies** en [claves del registro para Copias de seguridad y restauración](https://go.microsoft.com/fwlink/?linkid=180909) (https://go.microsoft.com/fwlink/?LinkId=180909)

### <a name="mindiffareafilesize"></a>MinDiffAreaFileSize

Esta clave especifica el tamaño mínimo inicial, en MB, del área de almacenamiento de instantáneas.

Para obtener más información, consulta la siguiente publicación en el sitio web de MSDN:

**MinDiffAreaFileSize** en [claves del registro para Copias de seguridad y restauración](https://go.microsoft.com/fwlink/?linkid=180910) (https://go.microsoft.com/fwlink/?LinkId=180910)

`##`#` Versiones de sistema operativo compatibles

En la tabla siguiente se enumeran las versiones de sistema operativo mínimas compatibles con las características de VSS.


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Característica de VSS</th>
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
<td><p>Versiones anteriores de Windows Server</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="even">
<td><p>Recuperación rápida mediante intercambios de LUN</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2003 con SP1</p></td>
</tr>
<tr class="odd">
<td><p>Varias importaciones para instantáneas de hardware</p>
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
<td><p>Instantáneas con recuperación automática transportables</p></td>
<td><p>No se admite ninguno</p></td>
<td><p>Windows Server 2008</p></td>
</tr>
<tr class="even">
<td><p>Sesiones de copia de seguridad simultáneas (hasta 64)</p></td>
<td><p>Windows XP</p></td>
<td><p>Windows Server 2003</p></td>
</tr>
<tr class="odd">
<td><p>Sesión de restauración única simultánea con las copias de seguridad</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows Server 2003 con SP2</p></td>
</tr>
<tr class="even">
<td><p>Hasta 8 sesiones de restauración simultáneas con las copias de seguridad</p></td>
<td><p>Windows 7</p></td>
<td><p>Windows Server 2003 R2</p></td>
</tr>
</tbody>
</table>

## <a name="see-also"></a>Consulta también

[Servicio de instantáneas de volumen en el Centro para desarrolladores de Windows](https://docs.microsoft.com/windows/desktop/vss/volume-shadow-copy-service-overview)