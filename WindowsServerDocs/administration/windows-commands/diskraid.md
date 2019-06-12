---
title: diskraid
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ebd65fb56114bff9e6ae4b6a76376561c686dfa
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439565"
---
# <a name="diskraid"></a>diskraid



DiskRAID es una herramienta de línea de comandos que le permite configurar y administrar la matriz redundante de los subsistemas de almacenamiento de discos independientes (o económicos) (RAID).

RAID es un método utilizado para estandarizar y clasificar los sistemas de disco tolerantes. Los niveles RAID ofrecen varias combinaciones de rendimiento, confiabilidad y costo. RAID se suele utilizar en los servidores. Algunos servidores ofrecen tres de los niveles RAID: Nivel 0 (secciones), nivel 1 (reflejo) y nivel 5 (creación de bandas con paridad).

Un subsistema RAID de hardware distingue las unidades de almacenamiento direccionable físicamente entre sí mediante el uso de un número de unidad lógica (LUN). Un objeto LUN debe tener al menos complejo y puede tener cualquier número de complejos adicionales. Cada complejos que contiene una copia de los datos en el objeto LUN. Complejos se pueden agregar a y quita de un objeto LUN.

La mayoría de los comandos de DiskRAID funcionan en un puerto de adaptador (HBA) del bus de host específico, adaptador de iniciador, portal de iniciador, proveedor, subsistema, controlador, puerto, unidad, LUN, portal de destino, destino o grupo de portales de destino. Utilice el comando SELECT para seleccionar un objeto. Se dice que el objeto seleccionado tiene el foco. El foco simplifica las tareas de configuración comunes, como la creación de varios LUN en el mismo subsistema.

> [!NOTE]
> La herramienta de línea de comandos DiskRAID solo funciona con los subsistemas de almacenamiento que admiten el servicio de disco Virtual (VDS).

## <a name="diskraid-commands"></a>Comandos DiskRAID

Para ver la sintaxis del comando, haga clic en un comando:
-   [add](#BKMK_1)
-   [associate](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [chap](#BKMK_5)
-   [create](#BKMK_6)
-   [delete](#BKMK_7)
-   [detail](#BKMK_8)
-   [dissociate](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [list](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [maintenance](#BKMK_22)
-   [name](#BKMK_23)
-   [offline](#BKMK_24)
-   [online](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerate](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [remove](#BKMK_30)
-   [replace](#BKMK_31)
-   [reset](#BKMK_32)
-   [select](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [standby](#BKMK_35)
-   [unmask](#BKMK_36)

### <a name="BKMK_1"></a>add

Agrega un LUN existente para el LUN seleccionado actualmente o agrega un portal de destino iSCSI para el grupo de portales de destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Parámetros

**lun complejo**=*n*

Especifica el número LUN para agregar como complejo al LUN actualmente seleccionado.

> [!CAUTION]
> Se eliminarán todos los datos en el LUN que se va a agregar como complejo.

**tpgroup tportal=** <em>n</em>

Especifica el número de portal de destino iSCSI para agregar al grupo de portales de destino iSCSI seleccionado actualmente.

**noerr**

Especifica que se omitirán los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

### <a name="BKMK_2"></a>Asociar

Establece la lista especificada de controlador de puertos como activo para el LUN seleccionado actualmente (otro controlador puertos están inactivos), o agrega los puertos de controlador especificado a la lista de puertos de controlador activo existente para el LUN seleccionado actualmente o asocia el destino de iSCSI especificado para el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Parámetros

**controllers**

Para su uso con proveedores de VDS 1.0 solo. Agrega o reemplaza la lista de controladores que están asociados con el LUN seleccionado actualmente.

**ports**

Para su uso con proveedores de VDS 1.1 sólo. Agrega o reemplaza la lista de puertos de controlador que están asociados con el LUN seleccionado actualmente.

**targets**

Para su uso con proveedores de VDS 1.1 sólo. Agrega o reemplaza la lista de destinos iSCSI que están asociados con el LUN seleccionado actualmente.

**add**

Para los proveedores de VDS 1.0, agrega los controladores especificados a la lista existente de controladores asociado con el LUN. Si no se especifica este parámetro, la lista de controladores reemplaza la lista de los controladores asociados con este LUN existente.

Para los proveedores de VDS 1.1, agrega los puertos de controlador especificado a la lista de puertos de controlador asociado con el LUN existente. Si no se especifica este parámetro, la lista de puertos de controlador reemplaza la lista de puertos de controlador asociados con este LUN existente.
```
<n>[,<n> [, ...]]
```
Para su uso con el **controladores** o **destinos** parámetro. Especifica los números de los controladores o los destinos iSCSI para establecer como activo o asociar.
```
<n-m>[,<n-m>[,…]]
```
Para su uso con el **puertos** parámetro. Especifica los puertos de controlador para establecer activo mediante una serie de controlador (*n*) y el número de puerto (*m*) par.

#### <a name="example"></a>Ejemplo

El ejemplo siguiente muestra cómo asociar y agregar puertos a un LUN que usa un proveedor de VDS 1.1:
```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)
```

### <a name="BKMK_3"></a>AUTOMAGIC

Establece o borra las marcas que ofrezcan pistas a proveedores sobre cómo configurar un LUN. Puede usar con ningún parámetro, el **automagic** operación, muestra una lista de marcas.

#### <a name="syntax"></a>Sintaxis

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Parámetros

**set**

Establece las marcas especificadas en los valores especificados.

**clear**

Borra las marcas especificadas. El **todas** palabra clave borra todas las marcas de automagic.

**apply**

Se aplica a las marcas actuales para el LUN seleccionado.

\<flag>

Las marcas se identifican mediante acrónimos de tres letras.

|Bandera|Descripción|
|----|-----------|
|FCR|Se requiere la recuperación rápida de bloqueo|
|FTL|Tolerante a errores|
|MSR|En su mayor parte de lecturas|
|MXD|Máximo de unidades|
|MXS|Tamaño máximo esperado|
|ORA|Alineación óptima de lectura|
|ORS|Tamaño óptimo de lectura|
|OSR|Optimizar para lecturas secuenciales|
|OSW|Optimizar para las escrituras secuenciales|
|OWA|Alineación de escritura óptimo|
|OWS|Tamaño de escritura óptimo|
|RBP|Volver a generar prioridad|
|RBV|Comprobar de nuevo lectura habilitado|
|RPM|Reasignación habilitada|
|STS|Tamaño de franja|
|WTC|Caché de escritura simultánea habilitado|
|YNK|Medios extraíbles|

### <a name="BKMK_4"></a>salto

Quita el complejo del LUN seleccionado actualmente. No se conservan el complejo y los datos que lo contiene, y se pueden reclamar las extensiones de unidad.

#### <a name="syntax"></a>Sintaxis

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Parámetros

**plex**

Especifica el número del complejo para quitar. No se conservarán el complejo y los datos que lo contiene y los recursos utilizados por este complejo que se recuperará. Los datos contenidos en el LUN no se garantiza que sea coherente. Si desea conservar este complejo, utilice el servicio de instantáneas de volumen (VSS).

**noerr**

Especifica que se omitirán los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

#### <a name="remarks"></a>Comentarios

> [!NOTE]
> En primer lugar debe seleccionar un LUN reflejado antes de usar el **salto** comando.

> [!CAUTION]
> Se eliminarán todos los datos en el complejo.

> [!CAUTION]
> No se garantiza que todos los datos contenidos en el LUN original sea coherente.

### <a name="BKMK_5"></a>CHAP

Establece el protocolo de autenticación de mutuo (CHAP) desafío compartido secreto para que los iniciadores de iSCSI y destinos iSCSI puedan comunicarse entre sí.

#### <a name="syntax"></a>Sintaxis

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Parámetros

**conjunto de iniciador**

Establece el secreto compartido en el servicio del iniciador iSCSI local utilizado para la autenticación CHAP mutua cuando el iniciador autentique al destino.

**Recuerde del iniciador**

Se comunica el secreto CHAP de un destino iSCSI para el servicio del iniciador iSCSI local para que el servicio del iniciador puede usar el secreto para autenticarse en el destino durante la autenticación CHAP.

**target set**

Establece el secreto compartido en el destino de iSCSI seleccionado actualmente utilizado para la autenticación CHAP cuando el destino autentica el iniciador.

**Recuerde de destino**

Se comunica el secreto CHAP del iniciador iSCSI al destino iSCSI de foco actual para que el destino puede usar el secreto para autenticarse en el iniciador durante la autenticación CHAP mutua.

**secret**

Especifica el secreto para usar. Si está vacío, se borrará el secreto.

**target**

Especifica un destino en el subsistema seleccionado actualmente para asociar el secreto. Esto es opcional cuando se establece un secreto en el iniciador y dejando indica que el secreto se utilizará para todos los destinos que no tengan ya un secreto asociado.

**initiatorname**

Especifica un nombre de iniciador iSCSI para asociar el secreto. Esto es opcional cuando se establece un secreto en un destino y dejando indica que el secreto se utilizará para todos los iniciadores que aún no tiene un secreto asociado.

### <a name="BKMK_6"></a>Crear

Crea un nuevo LUN o un destino iSCSI en el subsistema actualmente seleccionado, o crea un grupo de portales de destino en el destino seleccionado actualmente. Puede ver el enlace real mediante la **DiskRAID lista** comando.

#### <a name="syntax"></a>Sintaxis

```
create lun simple [size=<n>] [drives=<n>] [noerr]
create lun stripe [size=<n>] [drives=<n, n> [,...]]  [stripesize=<n>] [noerr]
create lun raid [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun mirror [size=<n>] [drives=<n, n> [,...]] [stripesize=<n>] [noerr]
create lun automagic size=<n> [noerr]
create target name=<name> [iscsiname=<iscsiname>] [noerr]
create tpgroup [noerr]
```

#### <a name="parameter"></a>Parámetro

**simple**

Crea un LUN simple.

**stripe**

Crea un LUN seccionado.

**RAID**

Crea un LUN seccionado con paridad.

**mirror**

Crea un LUN reflejado.

**automagic**

Crea un LUN mediante el *automagic* sugerencias en vigor actualmente. Consulte la **automagic** Sub comando para obtener más información.

**size**=

Especifica el tamaño total de LUN en megabytes. Si el **tamaño =** parámetro no se especifica, el LUN creado será el mayor tamaño posible permitido por todas las unidades especificadas.

Un proveedor normalmente crea un LUN al menos tan grande como el tamaño solicitado, pero el proveedor puede tener que redondear al alza al siguiente tamaño más grande en algunos casos. Por ejemplo, si se especifica el tamaño como.99 GB y el proveedor solo pueden asignar las extensiones de disco GB, el LUN resultante sería 1 GB.

Para especificar el tamaño en otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:
-   **B** de bytes.
-   **KB** para kilobytes.
-   **MB** para megabytes.
-   **GB** para gigabyte.
-   **TB** por terabyte.
-   **PB** para petabytes.

**unidades de disco**=

Especifica el *drive_number* para las unidades que se va a usar para crear un LUN. Si el **tamaño =** parámetro no se especifica, el LUN creado es el mayor tamaño posible permitido por todas las unidades especificadas. Si el **tamaño =** parámetro se especifica, los proveedores seleccionará las unidades de la lista de la unidad especificada para crear el LUN. Proveedores intentará usar las unidades de disco en el orden especificado cuando sea posible.

**stripesize**=

Especifica el tamaño en megabytes para un *bandas* o *RAID* LUN. No se puede cambiar el stripesize una vez creado el LUN.

Para especificar el tamaño en otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:
-   **B** de bytes.
-   **KB** para kilobytes.
-   **MB** para megabytes.
-   **GB** para gigabyte.
-   **TB** por terabyte.
-   **PB** para petabytes.

**target**

Crea un nuevo destino de iSCSI en el subsistema seleccionado actualmente.

**name**

Proporciona el nombre descriptivo para el destino.

**iscsiname**

Proporciona el nombre para el destino de iSCSI y puede omitirse para que el proveedor genera un nombre.

**tpgroup**

Crea un nuevo grupo de portal de destino de iSCSI en el destino seleccionado actualmente.

**noerr**

Especifica que se omitirán los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

#### <a name="remarks"></a>Comentarios

-   Ya sea el **tamaño**= o **unidades**= debe especificarse el parámetro. También puede usarse juntos.
-   No se puede cambiar el tamaño de franja para un LUN después de la creación.

### <a name="BKMK_7"></a>Eliminar

Elimina la LUN seleccionado actualmente, el destino iSCSI (siempre y cuando no hay ningún LUN asociado con el destino iSCSI) o el grupo de portales de destino iSCSI.

#### <a name="syntax"></a>Sintaxis

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Parámetros

**lun**

Elimina el LUN seleccionado actualmente y todos los datos en él.

**uninstall**

Especifica que el disco del sistema local asociado con el LUN se limpiarán antes de elimina el LUN.

**target**

Elimina el destino iSCSI actualmente seleccionado si ningún LUN está asociado con el destino.

**tpgroup**

Elimina el grupo de portales de destino iSCSI seleccionado actualmente.

**noerr**

Especifica que se omitirán los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

### <a name="BKMK_8"></a>Detalle

Muestra información detallada sobre el objeto seleccionado actualmente del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Parámetros

**hbaport**

Muestra información detallada sobre el puerto de adaptador (HBA) de bus host seleccionado actualmente.

**iadapter**

Muestra información detallada acerca del adaptador de iniciador iSCSI actualmente seleccionado.

**iportal**

Muestra información detallada sobre el portal de iniciador iSCSI actualmente seleccionado.

**provider**

Muestra información detallada acerca del proveedor seleccionado actualmente.

**subsystem**

Muestra información detallada acerca del subsistema seleccionado actualmente.

**controller**

Muestra información detallada acerca del controlador seleccionado actualmente.

**port**

Muestra información detallada sobre el puerto del controlador seleccionado actualmente.

**drive**

Muestra información detallada acerca de la unidad seleccionada, incluidos los LUN ocupa.

**lun**

Muestra información detallada sobre el LUN actualmente seleccionado, incluida la contribución unidades. La salida difiere ligeramente dependiendo de si el LUN forma parte de un subsistema iSCSI o canal de fibra. Si la lista de Hosts sin máscara contiene sólo un asterisco, esto significa que el LUN se desenmascara a todos los hosts.

**tportal**

Muestra información detallada sobre el portal de destino iSCSI seleccionado actualmente.

**target**

Muestra información detallada acerca del destino iSCSI seleccionado actualmente.

**tpgroup**

Muestra información detallada sobre el grupo de portales de destino iSCSI seleccionado actualmente.

**verbose**

Para su uso solo con el parámetro LUN. Muestra información adicional, incluida su complejos.

### <a name="BKMK_9"></a>dissociate

Conjuntos especificado lista de puertos de controlador como inactiva para el LUN seleccionado actualmente (otro controlador que no se ven afectados los puertos), o se anula la asociación de la lista especificada de destinos iSCSI para el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Parámetro

**controllers**

Para su uso con proveedores de VDS 1.0 solo. Quita los controladores de la lista de controladores que están asociados con el LUN seleccionado actualmente.

**ports**

Para su uso con proveedores de VDS 1.1 sólo. Quita los puertos de controlador de la lista de puertos de controlador que están asociados con el LUN seleccionado actualmente.

**targets**

Para su uso con proveedores de VDS 1.1 sólo. Quita los destinos de la lista de destinos iSCSI que están asociados con el LUN seleccionado actualmente.
```
<n> [,<n> [,…]]
```
Para su uso con el **controladores** o **destinos** parámetro. Especifica los números de los controladores o los destinos iSCSI para establecer como inactiva o desasociar.
```
<n-m>[,<n-m>[,…]]
```
Para su uso con el **puertos** parámetro. Especifica los puertos de controlador para establecer como inactiva mediante el uso de un número de controlador (*n*) y el número de puerto (*m*) par.

#### <a name="example"></a>Ejemplo

```
DISKRAID> SEL LUN 5
LUN 5 is now the selected LUN.

DISKRAID> ASSOCIATE PORTS 0-0,0-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1)

DISKRAID> ASSOCIATE PORTS ADD 1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 0, Ctlr 0 Port 1, Ctlr 1 Port 1)

DISKRAID> DISSOCIATE PORTS 0-0,1-1
Controller port associations changed.
(Controller ports active after this command: Ctlr 0 Port 1)
```

### <a name="BKMK_10"></a>exit

Sale de DiskRAID.

#### <a name="syntax"></a>Sintaxis

```
exit
```

### <a name="BKMK_11"></a>Ampliar

Extiende el LUN actualmente seleccionado mediante la adición de los sectores hasta el final de la LUN. No todos los proveedores admiten Extender LUN. No se extiende los volúmenes o sistemas de archivos contenidos en el LUN. Después de extender el LUN, debe extender las estructuras en disco asociadas con el **DiskPart extender** comando.

#### <a name="syntax"></a>Sintaxis

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Parámetros

**size=**

Especifica el tamaño en megabytes para extender el LUN. Si el **tamaño =** parámetro no se especifica, se amplía el LUN en el mayor tamaño posible permitido por todas las unidades especificadas. Si el **tamaño =** parámetro se especifica, los proveedores de seleccionar unidades en la lista especificada por el **unidades =** parámetro para crear el LUN.

Para especificar el tamaño en otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:
-   **B** de bytes.
-   **KB** para kilobytes.
-   **MB** para megabytes.
-   **GB** para gigabyte.
-   **TB** por terabyte
-   **PB** para petabytes

**drives=**

Especifica el \<drive_number > para las unidades que se va a usar al crear un LUN. Si el **tamaño =** parámetro no se especifica, el LUN creado es el mayor tamaño posible permitido por todas las unidades especificadas. Proveedores usan las unidades de disco en el orden especificado cuando sea posible.

**noerr**

Especifica que deben omitirse los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

#### <a name="remarks"></a>Comentarios

Ya sea el *tamaño* o \<unidad > debe especificar el parámetro. También puede usarse juntos.

### <a name="BKMK_12"></a>flushcache

Borra la memoria caché en el controlador seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
flushcache controller
```

### <a name="BKMK_13"></a>Ayuda

Muestra una lista de todos los comandos DiskRAID.

#### <a name="syntax"></a>Sintaxis

```
help
```

### <a name="BKMK_14"></a>importtarget

Recupera o establece el destino de importación de servicio de instantáneas de volumen (VSS) actual que se ha establecido para el subsistema seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Parámetro

**conjunto de destino**

Si se especifica, Establece el destino seleccionado actualmente en el destino de la importación VSS para el subsistema seleccionado actualmente. Si no se especifica, el comando recupera el destino actual de importación VSS que se establece para el subsistema seleccionado actualmente.

### <a name="BKMK_15"></a>Iniciador

Recupera información sobre el iniciador iSCSI local.

#### <a name="syntax"></a>Sintaxis

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

Invalida la memoria caché en el controlador seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

Establece la directiva de equilibrio de carga en el LUN actualmente seleccionado.

#### <a name="syntax"></a>Sintaxis

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Parámetros

**type**

Especifica la directiva de equilibrio de carga. Si no se especifica el tipo, el **ruta** debe especificarse el parámetro. Tipo puede ser uno de los siguientes:

**CONMUTACIÓN POR ERROR**: Usa una ruta de acceso principal con otras rutas de acceso que se va a rutas de acceso de copia de seguridad.

**ROUNDROBIN**: Usa todas las rutas de acceso en modo round-robin, que trata cada ruta de acceso de forma secuencial.

**SUBSETROUNDROBIN**: Usa todas las rutas de acceso principales en modo round-robin; rutas de acceso de copia de seguridad se usan solo si todas las principales provocan errores.

**DYNLQD**: Usa la ruta de acceso con el menor número de solicitudes activas.

**PONDERADO**: Usa la ruta de acceso con el menor peso (cada ruta de acceso debe tener asignado un peso).

**LEASTBLOCKS**: Usa la ruta de acceso con el menor número de bloques.

**VENDORSPECIFIC**: Usa una directiva específica del proveedor.

**paths**

Especifica si una ruta de acceso es **principal** o tiene un determinado \<peso >. No se especifica ninguna ruta de acceso se establece implícitamente como copia de seguridad. Las rutas de acceso deben ser una de las rutas de acceso de la LUN seleccionado actualmente.

### <a name="BKMK_19"></a>Lista

Muestra una lista de objetos del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Parámetros

**hbaports**

Muestra información de resumen acerca de todos los puertos HBA que sabe que VDS. El puerto HBA seleccionado está marcado con un asterisco (*).

**iadapters**

Muestra información de resumen acerca de todos los adaptadores de iniciador iSCSI sabe que VDS. El adaptador de iniciador seleccionado está marcado con un asterisco (*).

**iportals**

Muestra información de resumen acerca de todos los portales de iniciador iSCSI en el adaptador de iniciador seleccionado actualmente. El portal de iniciador seleccionado está marcado con un asterisco (*).

**providers**

Muestra información de resumen acerca de cada proveedor que se sabe que VDS. El proveedor seleccionado está marcado con un asterisco (*).

**subsystems**

Muestra información de resumen acerca de cada subsistema en el sistema. El subsistema actualmente seleccionado se marca con un asterisco (*).

**controllers**

Muestra información de resumen acerca de cada controlador en el subsistema seleccionado actualmente. El controlador seleccionado está marcado con un asterisco (*).

**ports**

Muestra información de resumen acerca de cada puerto de controlador en el controlador seleccionado actualmente. El puerto seleccionado está marcado con un asterisco (*).

**drives**

Muestra información de resumen acerca de cada unidad en el subsistema seleccionado actualmente. La unidad seleccionada actualmente está marcada con un asterisco (*).

**luns**

Muestra información de resumen sobre cada LUN en el subsistema seleccionado actualmente. El LUN actualmente seleccionado se marca con un asterisco (*).

**tportals**

Muestra información de resumen acerca de todos los portales de destino iSCSI en el subsistema seleccionado actualmente. El portal de destino seleccionado está marcado con un asterisco (*).

**targets**

Muestra información de resumen acerca de todos los destinos iSCSI en el subsistema seleccionado actualmente. El destino seleccionado está marcado con un asterisco (*).

**tpgroups**

Muestra información de resumen acerca de todos los grupos de portal de destino iSCSI en el destino seleccionado actualmente. El grupo de portal actualmente seleccionado se marca con un asterisco (*).

### <a name="BKMK_20"></a>inicio de sesión

Registra el adaptador de iniciador iSCSI especificado en el destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Parámetros

**type**

Especifica el tipo de inicio de sesión para realizar: **manual**, **persistente**, o **arranque**. Si no se especifica, se realizará un inicio de sesión manual.

**manual** -inicio de sesión manualmente.

**persistente** : automáticamente usa el mismo inicio de sesión cuando se reinicia el equipo.

**arranque** -(esta opción es para el desarrollo futuro y no se usa actualmente<em>.</em>)

**chap**

Especifica el tipo de autenticación que desea usar CHAP: **ninguno**, **oneway** CHAP, o **mutua** CHAP; si no se especifica, no se utilizará autenticación.

**tportal**

Especifica un portal de destino opcional en el subsistema seleccionado actualmente que se usará para el registro en.

**iportal**

Especifica un portal de iniciador opcional en el adaptador de iniciador especificado que se usará para el registro en.

\<flag>

Identificado por acrónimos de tres letras:

**IPS**: Requerir IPsec

**EMP**: Habilitar múltiples rutas

**EHD**: Habilitar la síntesis de encabezado

**EDD**: Habilitar el resumen de datos

### <a name="BKMK_21"></a>Cierre de sesión

Registra el adaptador de iniciador iSCSI especificado en el destino iSCSI actualmente seleccionado.

#### <a name="syntax"></a>Sintaxis

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Parámetros

**iadapter**

Especifica el adaptador de iniciador con una sesión de inicio de sesión a cierre de sesión desde.

### <a name="BKMK_22"></a>Mantenimiento

Realiza operaciones de mantenimiento en el objeto seleccionado actualmente del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Parámetros

\<object>

Especifica el tipo de objeto en el que se va a realizar la operación. El *objeto* tipo puede ser un **subsistema**, **controlador**, **puerto unidad** o **LUN**.

\<operation>

Especifica la operación de mantenimiento para realizar. El *operación* tipo puede ser **spinup**, **rotación**, **blink**, **Bip** o **ping** . Un *operación* debe especificarse.

**count=**

Especifica el número de veces que se repite el *operación*. Esto se suele utilizar con **blink**, **Bip**, o **ping**.

### <a name="BKMK_23"></a>name

Establece el nombre descriptivo del destino iSCSI, LUN o subsistema seleccionado actualmente en el nombre especificado.

#### <a name="syntax"></a>Sintaxis

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Parámetro

\<name>

Especifica un nombre para el subsistema, el LUN o el destino. El nombre debe ser inferior a 64 caracteres de longitud. Si se proporciona ningún nombre, el nombre existente, si existe, se elimina.

### <a name="BKMK_24"></a>Sin conexión

Establece el estado del objeto seleccionado actualmente del tipo especificado a **sin conexión**.

#### <a name="syntax"></a>Sintaxis

```
offline <object>
```

#### <a name="parameter"></a>Parámetro

\<object>

Especifica el tipo de objeto en el que se va a realizar esta operación. La \<objeto >

tipo puede ser **subsistema**, **controlador**, **unidad**, **LUN**, o **portal**.

### <a name="BKMK_25"></a>En línea

Establece el estado del objeto seleccionado del tipo especificado a **online**. Si el objeto es **hbaport**, cambia el estado de las rutas de acceso al puerto HBA seleccionado actualmente para **online**.

#### <a name="syntax"></a>Sintaxis

```
online <object> 
```

#### <a name="parameter"></a>Parámetro

\<object>

Especifica el tipo de objeto en el que se va a realizar esta operación. La \<objeto >

tipo puede ser **hbaport**, **subsistema**, **controlador**, **unidad**, **LUN**, o  **portal**.

### <a name="BKMK_26"></a>recover

Realiza las operaciones necesarias, como la resincronización o reserva activa reparar el LUN con tolerancia seleccionado actualmente. Por ejemplo, recuperación podría provocar una reserva activa para enlazarse a un conjunto RAID que tiene un disco con errores u otra reasignación de extensión del disco.

#### <a name="syntax"></a>Sintaxis

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerate

Reenumerates objetos del tipo especificado. Si usa el comando LUN de extender, debe usar el comando de actualización para actualizar el tamaño del disco antes de usar el comando enumerar.

#### <a name="syntax"></a>Sintaxis

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Parámetros

**subsystems**

Consulta el proveedor para detectar cualquier nuevos subsistemas que se agregaron en el proveedor seleccionado actualmente.

**drives**

Consulta los buses de E/S internos para descubrir las nuevas unidades que se agregaron en el subsistema seleccionado actualmente.

### <a name="BKMK_28"></a>refresh

Actualiza los datos internos para el proveedor seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
refresh provider
```

### <a name="BKMK_29"></a>rem

Se usa para las secuencias de comandos del comentario.

#### <a name="syntax"></a>Sintaxis

```
Rem <comment>
```

### <a name="BKMK_30"></a>remove

Quita el portal de destino iSCSI especificado desde el grupo de portales de destino seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Parámetro

**tpgroup tportal=** \<tportal>

Especifica el portal de destino iSCSI para quitar.

**noerr**

Especifica que deben omitirse los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

### <a name="BKMK_31"></a>replace

Reemplaza la unidad especificada con la unidad seleccionada actualmente.

#### <a name="syntax"></a>Sintaxis

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Parámetro

**drive=**

Especifica el \<drive_number > para la unidad que se debe reemplazar.

#### <a name="remarks"></a>Comentarios

-   La unidad especificada no puede ser la unidad seleccionada actualmente.

### <a name="BKMK_32"></a>reset

Restablece el controlador seleccionado actualmente o el puerto.

#### <a name="syntax"></a>Sintaxis

```
Reset {controller | port}
```

#### <a name="parameters"></a>Parámetros

**controller**

Restablece el controlador.

**port**

Restablece el puerto.

### <a name="BKMK_33"></a>Seleccione

Muestra o cambia el objeto seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Parámetros

**object**

Especifica el tipo de objeto para seleccionar. El \<objeto > puede ser tipo **proveedor**, **subsistema**, **controlador**, **unidad**, o **LUN**.

**hbaport** [\<n>]

Establece el foco en el puerto HBA local especificado. Si no se especifica ningún puerto HBA, el comando muestra el puerto HBA seleccionado actualmente (si existe). Especifica un índice no válido de puerto HBA da como resultado ningún puerto HBA de foco. Seleccionar un puerto HBA anula la selección de los adaptadores de iniciador seleccionado y portales de iniciador.

**iadapter** [\<n>]

Establece el foco en el adaptador de iniciador iSCSI local especificado. Si no se especifica ningún adaptador de iniciador, el comando muestra el adaptador de iniciador seleccionado actualmente (si existe). Especifica un índice de adaptador de iniciador no válida da lugar a ningún adaptador de iniciador de foco. Seleccionar un adaptador de iniciador anula la selección de los puertos HBA seleccionados y los portales de iniciador.

**iportal** [\<n>]

Establece el foco en el portal de iniciador iSCSI local especificado en el adaptador de iniciador iSCSI seleccionado. Si no se especifica ningún portal de iniciador, el comando muestra el portal de iniciador actualmente seleccionado (si existe). Especifica un índice de portal de iniciador no válida da lugar a ningún portal de iniciador seleccionado.

**provider** [\<n>]

Establece el foco en el proveedor especificado. Si se especifica ningún proveedor, el comando muestra el proveedor seleccionado actualmente (si existe). Especifica un índice de proveedor no válido provoca ningún proveedor de foco.

**subsystem** [\<n>]

Establece el foco en el subsistema especificado. Si no se especifica ningún subsistema, el comando muestra el subsistema de enfoque (si existe). Especifica un índice de subsistema no válido provoca ningún subsistema enfocado. Seleccione un subsistema implícitamente selecciona su proveedor asociado.

**controller** [\<n>]

Establece el foco en el controlador especificado en el subsistema seleccionado actualmente. Si no se especifica ningún controlador, el comando muestra el controlador seleccionado actualmente (si existe). Especifica un índice no válido del controlador da lugar a ningún controlador de foco. Al seleccionar un controlador anula la selección de cualquier controlador seleccionado puertos unidades, LUN, los portales de destino, destinos y grupos del portal de destino.

**port** [\<n>]

Establece el foco en el puerto de controlador especificado en el controlador seleccionado actualmente. Si no se especifica ningún puerto, el comando muestra el puerto seleccionado actualmente (si existe). Especifica un índice de puerto no válido, se produce ningún puerto seleccionado.

**drive** [\<n>]

Establece el foco en la unidad especificada, o eje físico, en el subsistema seleccionado actualmente. Si se especifica ninguna unidad, el comando muestra la unidad seleccionada actualmente (si existe). Especifica un índice de la unidad no válida da lugar a ninguna unidad de foco. Seleccionar una unidad anula la selección de cualquier controladores seleccionados los puertos de controlador, LUN, los portales de destino, destinos y grupos del portal de destino.

**lun** [\<n>]

Establece el foco en el LUN especificado dentro del subsistema seleccionado actualmente. Si no se especifica ningún LUN, el comando muestra el LUN actualmente seleccionado (si existe). Especifica un índice no válido de LUN da como resultado ningún LUN seleccionado. Anula la selección de un LUN cualquier controladores seleccionados los puertos de controlador, unidades, los portales de destino, destinos y grupos del portal de destino.

**tportal** [\<n>]

Establece el foco en el portal de destino iSCSI especificado dentro del subsistema seleccionado actualmente. Si no se especifica ningún portal de destino, el comando muestra el portal de destino seleccionado (si existe). Especifica un índice de portal de destino no válida da lugar a ningún portal de destino seleccionado. Anula la selección de un portal de destino cualquier controladores de puertos de controlador, unidades, LUN, destinos y grupos del portal de destino.

**target** [\<n>]

Establece el foco en el destino de iSCSI especificado en el subsistema seleccionado actualmente. Si no se especifica ningún destino, el comando muestra el destino seleccionado actualmente (si existe). Especifica un índice de destino no válida da como resultado ningún destino seleccionado. Anula la selección de un destino cualquier controladores, los puertos de controlador, unidades, LUN, los portales de destino y grupos del portal de destino.

**tpgroup** [\<n>]

Establece el foco en el grupo de portales de destino iSCSI especificado dentro del destino iSCSI actualmente seleccionado. Si no se especifica ningún grupo de portales de destino, el comando muestra el grupo de portales de destino seleccionado actualmente (si existe). Especifica un índice de grupo del portal de destino no válida da lugar a ningún grupo de portales de destino enfocado.

[\<n>]

Especifica el \<objeto número > para seleccionar. Si el <object number> especificado no es válido, se borran las selecciones existentes de los objetos del tipo especificado. Si no hay ningún <object number> se especifica, se muestra el objeto actual.

### <a name="BKMK_34"></a>setflag

Establece la unidad seleccionada actualmente como una reserva activa.

#### <a name="syntax"></a>Sintaxis

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Parámetros

**true**

Selecciona la unidad seleccionada actualmente como una reserva activa.

**false**

Anula la selección de la unidad seleccionada actualmente como una reserva activa.

#### <a name="remarks"></a>Comentarios

No se puede usar reservas activas para las operaciones de enlace de LUN normales. Están reservados para que sólo el control de errores. La unidad no debe estar enlazada actualmente a cualquier LUN existente.

### <a name="BKMK_shrink"></a>shrink

Reduce el tamaño del LUN seleccionado.

#### <a name="syntax"></a>Sintaxis

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Parámetros

**size=**

Especifica la cantidad deseada de espacio en megabytes (MB) para reducir el tamaño del LUN. Para especificar el tamaño en otras unidades, use uno de los sufijos reconocidos (B, KB, MB, GB, TB y PB) inmediatamente después del tamaño.

**noerr**

Especifica que se omitirán los errores que se producen al realizar esta operación. Esto es útil en el modo de secuencia de comandos.

### <a name="BKMK_35"></a>en espera

Cambia el estado de las rutas de acceso al puerto de adaptador (HBA) de bus host seleccionado actualmente en espera.

#### <a name="syntax"></a>Sintaxis

```
standby hbaport
```

#### <a name="parameters"></a>Parámetros

**hbaport**

Cambia el estado de las rutas de acceso al puerto de adaptador (HBA) de bus host seleccionado actualmente en espera.

### <a name="BKMK_36"></a>unmask

Hace que los LUN seleccionados actualmente sea accesible desde los hosts especificados.

#### <a name="syntax"></a>Sintaxis

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Parámetros

**all**

Especifica que el LUN debe estar accesible desde todos los hosts. Sin embargo, no puede desenmascarar el LUN a todos los destinos en un subsistema iSCSI.

> [!IMPORTANT]
> Debe cierre de sesión de destino antes de ejecutar el comando sin máscara todo.

**Ninguno**

Especifica que el LUN no debe ser accesible para cualquier host.

> [!IMPORTANT]
> Debe cerrar sesión en el destino antes de ejecutar el comando sin máscara LUN ninguno.

**add**

Especifica que se deben agregar los hosts especificados a la lista existente de hosts que es accesible desde este LUN. Si no se especifica este parámetro, la lista de hosts proporcionada reemplaza la lista existente de hosts que sea accesible desde este LUN.

**WWN=**

Especifica una lista de números hexadecimales que representan los nombres de todo el mundo desde la que los LUN o los hosts deben estar accesibles. Para máscara/sin máscara a un conjunto específico de hosts en un subsistema de canal de fibra, puede escribir una lista separada por punto y coma de WWN para los puertos en los equipos host de interés.

**initiator=**

Especifica una lista de a la que el LUN actualmente seleccionado debe ponerse a disposición de los iniciadores iSCSI. Para máscara/sin máscara a un conjunto específico de hosts en un subsistema iSCSI, puede escribir una lista separada por comas de nombres de iniciador iSCSI para los iniciadores en los equipos host de interés.

**uninstall**

Si se especifica, se desinstala el disco asociado con el LUN en el sistema local antes de que los LUN se enmascara.

## <a name="scripting-diskraid"></a>Secuencias de comandos DiskRAID

DiskRAID puede incluirse en scripts en cualquier equipo que ejecute Windows Server 2008 o Windows Server 2003 con un proveedor de hardware de VDS asociado. Para invocar una secuencia de comandos DiskRAID, en el símbolo del sistema, escriba:
```
diskraid /s <script.txt>
```
De forma predeterminada, DiskRAID detiene el procesamiento de comandos y devuelve un código de error si hay un problema en la secuencia de comandos. Para continuar la ejecución del script y omitir los errores, incluya el parámetro NOERR en el comando. Esto permite prácticas útiles, como el uso de una única secuencia de comandos para eliminar todos los LUN en un subsistema, independientemente del número total de LUN. No todos los comandos admiten el parámetro NOERR. Siempre se devuelven errores en los errores de sintaxis del comando, independientemente de si se incluyó el parámetro NOERR,

### <a name="diskraid-error-codes"></a>Códigos de error de DiskRAID

|Código de error|Descripción del error|
|----------|-----------------|
|0|Se produjo ningún error. El script completo se ejecutó sin errores.|
|1|Se ha producido una excepción grave.|
|2|Los argumentos especificados en una línea de comandos DiskRAID eran incorrectos.|
|3|DiskRAID no pudo abrir el archivo de salida o el script especificado.|
|4|Uno de los servicios que usa DiskRAID devolvió un error.|
|5|Se ha producido un error de sintaxis de comando. La secuencia de comandos no se pudo porque un objeto se seleccionó incorrectamente o no era válido para su uso con ese comando.|

## <a name="example-interactively-view-status-of-subsystem"></a>Por ejemplo: Ver estado del subsistema de forma interactiva

Si desea ver el estado del subsistema de 0 en el equipo, escriba lo siguiente en la línea de comandos:
```
diskraid
```
Presione ENTRAR. Se muestra lo siguiente:
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Para seleccionar el subsistema de 0, escriba lo siguiente en el símbolo del sistema de DiskRAID:
```
select subsystem 0
```
Presione ENTRAR. Se muestra el resultado similar al siguiente:
```
Subsystem 0 is now the selected subsystem.

DISKRAID> list drives

  Drive ###  Status      Health          Size      Free    Bus  Slot  Flags
  ---------  ----------  ------------  --------  --------  ---  ----  -----
  Drive 0    Online      Healthy         107 GB    107 GB    0     1
  Drive 1    Offline     Healthy          29 GB     29 GB    1     0
  Drive 2    Online      Healthy         107 GB    107 GB    0     2
  Drive 3    Not Ready   Healthy          19 GB     19 GB    1     1
```
Para salir de DiskRAID, escriba lo siguiente en el símbolo del sistema de DiskRAID:
```
exit
```