---
title: diskraid
description: 'Tema de comandos de Windows para * * * *- '
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
ms.openlocfilehash: f2dfda058a7ca266adedbacf8860137c5d1782c7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867079"
---
# <a name="diskraid"></a>diskraid



Diskraid es una herramienta de línea de comandos que permite configurar y administrar matrices redundantes de subsistemas de almacenamiento de discos independientes (o económicos).

RAID es un método que se usa para estandarizar y clasificar los sistemas de disco tolerantes a errores. Los niveles RAID proporcionan varias combinaciones de rendimiento, confiabilidad y costo. RAID se usa normalmente en los servidores. Algunos servidores proporcionan tres niveles de RAID: Nivel 0 (seccionado), nivel 1 (creación de reflejo) y nivel 5 (seccionamiento con paridad).

Un subsistema RAID de hardware distingue las unidades de almacenamiento físicamente direccionable entre sí mediante un número de unidad lógica (LUN). Un objeto LUN debe tener al menos un Plex y puede tener cualquier número de Plex adicionales. Cada Plex contiene una copia de los datos en el objeto LUN. Los Plex se pueden agregar y quitar de un objeto LUN.

La mayoría de los comandos de Diskraid funcionan en un puerto de adaptador de bus host (HBA) específico, un adaptador de iniciador, un portal de iniciador, un proveedor, un subsistema, un controlador, un puerto, una unidad, un LUN, un portal de destino, un destino o un grupo de portal de destino. Use el comando Seleccionar para seleccionar un objeto. Se dice que el objeto seleccionado tiene el foco. El enfoque simplifica las tareas de configuración comunes, como la creación de varios LUN en el mismo subsistema.

> [!NOTE]
> La herramienta de línea de comandos Diskraid solo funciona con subsistemas de almacenamiento compatibles con el servicio de disco virtual (VDS).

## <a name="diskraid-commands"></a>Comandos de Diskraid

Para ver la sintaxis del comando, haga clic en un comando:
-   [add](#BKMK_1)
-   [asócielos](#BKMK_2)
-   [automagic](#BKMK_3)
-   [break](#BKMK_4)
-   [CHAPv2](#BKMK_5)
-   [create](#BKMK_6)
-   [elimínelos](#BKMK_7)
-   [detalle](#BKMK_8)
-   [Desasociar](#BKMK_9)
-   [exit](#BKMK_10)
-   [extend](#BKMK_11)
-   [flushcache](#BKMK_12)
-   [help](#BKMK_13)
-   [importtarget](#BKMK_14)
-   [Initiator](#BKMK_15)
-   [invalidatecache](#BKMK_16)
-   [lbpolicy](#BKMK_18)
-   [lista](#BKMK_19)
-   [login](#BKMK_20)
-   [logout](#BKMK_21)
-   [alimenticia](#BKMK_22)
-   [name](#BKMK_23)
-   [n](#BKMK_24)
-   [pantalla](#BKMK_25)
-   [recover](#BKMK_26)
-   [reenumerar](#BKMK_27)
-   [refresh](#BKMK_28)
-   [rem](#BKMK_29)
-   [retirar](#BKMK_30)
-   [replace](#BKMK_31)
-   [determinado](#BKMK_32)
-   [no](#BKMK_33)
-   [setflag](#BKMK_34)
-   [shrink](#BKMK_shrink)
-   [secundarios](#BKMK_35)
-   [Mask](#BKMK_36)

### <a name="BKMK_1"></a>agréguela

Agrega un LUN existente al LUN seleccionado actualmente o agrega un portal de destino iSCSI al grupo del portal de destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

#### <a name="parameters"></a>Parámetros

**LUN de Plex n**=

Especifica el número de LUN que se va a agregar como complejo al LUN seleccionado actualmente.

> [!CAUTION]
> Se eliminarán todos los datos del LUN que se va a agregar como complejo.

**tpgroup TPORTAL =** <em>n</em>

Especifica el número de portal de destino iSCSI que se va a agregar al grupo de portal de destino iSCSI seleccionado actualmente.

**Noerr**

Especifica que se omitirán los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

### <a name="BKMK_2"></a>asócielos

Establece la lista especificada de puertos de controlador como activo para el LUN seleccionado actualmente (otros puertos de controlador se establecen como inactivos), o agrega los puertos de controlador especificados a la lista de puertos de controlador activos existentes para el LUN seleccionado actualmente o asocia el destino iSCSI especificado para el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

#### <a name="parameters"></a>Parámetros

**Controladores**

Solo se usa con proveedores de VDS 1,0. Agrega o reemplaza la lista de controladores asociados al LUN seleccionado actualmente.

**Puerto**

Solo se usa con proveedores de VDS 1,1. Agrega o reemplaza la lista de puertos de controlador asociados al LUN seleccionado actualmente.

**destinos**

Solo se usa con proveedores de VDS 1,1. Agrega o reemplaza la lista de destinos iSCSI asociados al LUN seleccionado actualmente.

**add**

Para los proveedores de VDS 1,0, agrega los controladores especificados a la lista existente de controladores asociados con el LUN. Si no se especifica este parámetro, la lista de controladores reemplazará a la lista existente de controladores asociados a este LUN.

Para los proveedores de VDS 1,1, agrega los puertos de controlador especificados a la lista existente de puertos de controlador asociados con el LUN. Si no se especifica este parámetro, la lista de puertos de controlador reemplaza la lista existente de puertos de controlador asociados a este LUN.
```
<n>[,<n> [, ...]]
```
Para su uso con el parámetro **Controllers** o **targets** . Especifica los números de los controladores o destinos iSCSI que se van a establecer en activo o asociado.
```
<n-m>[,<n-m>[,…]]
```
Para su uso con el parámetro **ports** . Especifica los puertos del controlador para establecer activos mediante un par de número de controlador (*n*) y número de puerto (*m*).

#### <a name="example"></a>Ejemplo

En el ejemplo siguiente se muestra cómo asociar y agregar puertos a un LUN que usa un proveedor de VDS 1,1:
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

### <a name="BKMK_3"></a>automagic

Establece o borra las marcas que proporcionan sugerencias a los proveedores sobre cómo configurar un LUN. Cuando se usa sin parámetros, la operación **automagic** muestra una lista de marcas.

#### <a name="syntax"></a>Sintaxis

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

#### <a name="parameters"></a>Parámetros

**set**

Establece las marcas especificadas en los valores especificados.

**claridad**

Borra las marcas especificadas. La palabra clave **All** borra todas las marcas automágicas.

**aplica**

Aplica las marcas actuales al LUN seleccionado.

\<marca >

Las marcas se identifican mediante acrónimos de tres letras.

|Marcar|Descripción|
|----|-----------|
|FCR|Se requiere recuperación rápida tras bloqueo|
|FTL|Tolerante a errores|
|MSR|Lecturas principalmente|
|MXD|Unidades máximas|
|MXS|Se esperaba un tamaño máximo|
|ORA|Alineación de lectura óptima|
|OR|Tamaño de lectura óptimo|
|OSR|Optimizar para lecturas secuenciales|
|OSW|Optimizar para escrituras secuenciales|
|OUTLOOK|Alineación de escritura óptima|
|OWS|Tamaño de escritura óptimo|
|RBP|Prioridad de recompilación|
|RBV|Comprobación de la lectura de la versión habilitada|
|RMP|Reasignación habilitada|
|STS|Tamaño de franja|
|WTC|Almacenamiento en caché de escritura a través habilitado|
|YNK|Quitar|

### <a name="BKMK_4"></a>eliminar

Quita el Plex del LUN seleccionado actualmente. El Plex y los datos que contiene no se conservan y se pueden recuperar las extensiones de unidad.

#### <a name="syntax"></a>Sintaxis

```
break plex=<plex_number> [noerr]
```

#### <a name="parameters"></a>Parámetros

**complejos**

Especifica el número del complejo que se va a quitar. El Plex y los datos que contiene no se conservarán y se recuperarán los recursos usados por este complejo. No se garantiza que los datos contenidos en el LUN sean coherentes. Si desea conservar este complejo, use el Servicio de instantáneas de volumen (VSS).

**Noerr**

Especifica que se omitirán los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

#### <a name="remarks"></a>Comentarios

> [!NOTE]
> Primero debe seleccionar un LUN reflejado antes de usar el comando **break** .

> [!CAUTION]
> Se eliminarán todos los datos del complejo.

> [!CAUTION]
> No se garantiza que todos los datos contenidos en el LUN original sean coherentes.

### <a name="BKMK_5"></a>CHAPv2

Establece el secreto compartido del Protocolo de autenticación por desafío mutuo (CHAP) para que los iniciadores iSCSI y los destinos iSCSI puedan comunicarse entre sí.

#### <a name="syntax"></a>Sintaxis

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

#### <a name="parameters"></a>Parámetros

**conjunto de iniciadores**

Establece el secreto compartido en el servicio del iniciador iSCSI local que se usa para la autenticación CHAP mutua cuando el iniciador autentica el destino.

**se recuerda al iniciador**

Comunica el secreto CHAP de un destino iSCSI con el servicio del iniciador iSCSI local para que el servicio iniciador pueda usar el secreto con el fin de autenticarse en el destino durante la autenticación CHAP.

**conjunto de destinos**

Establece el secreto compartido en el destino iSCSI seleccionado actualmente que se usa para la autenticación CHAP cuando el destino autentica al iniciador.

**objetivo recordar**

Comunica el secreto CHAP de un iniciador iSCSI con el destino iSCSI actual enfocado para que el destino pueda usar el secreto con el fin de autenticarse en el iniciador durante la autenticación CHAP mutua.

**secreto**

Especifica el secreto que se va a usar. Si está vacío, se borrará el secreto.

**dirigir**

Especifica un destino en el subsistema seleccionado actualmente para asociarlo con el secreto. Esto es opcional cuando se establece un secreto en el iniciador y se deja indica que el secreto se usará para todos los destinos que aún no tienen un secreto asociado.

**initiatorname**

Especifica un nombre iSCSI del iniciador que se va a asociar con el secreto. Esto es opcional cuando se establece un secreto en un destino y se deja indica que el secreto se usará para todos los iniciadores que aún no tienen un secreto asociado.

### <a name="BKMK_6"></a>a

Crea un nuevo LUN o destino iSCSI en el subsistema seleccionado actualmente o crea un grupo de portal de destino en el destino seleccionado actualmente. Puede ver el enlace real mediante el comando de **lista de Diskraid** .

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

**creación**

Crea un LUN reflejado.

**automagic**

Crea un LUN con las sugerencias de *automagic* que están actualmente en vigor. Vea el subcomando **automagic** para obtener más información.

**ajusta**=

Especifica el tamaño total del LUN en megabytes. Si no se especifica el parámetro **size =** , el LUN creado será el tamaño más grande posible permitido por todas las unidades especificadas.

Normalmente, un proveedor crea un LUN al menos tan grande como el tamaño solicitado, pero es posible que el proveedor tenga que redondear al siguiente tamaño más grande en algunos casos. Por ejemplo, si el tamaño se especifica como. 99 GB y el proveedor solo puede asignar GB de extensiones de disco, el LUN resultante sería 1 GB.

Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:
-   **B** para byte.
-   **KB** para kilobytes.
-   **MB** para megabyte.
-   **GB** para Gigabyte.
-   **TB** para terabyte.
-   **PB** para petabytes.

**SATA**=

Especifica el *drive_number* de las unidades que se van a usar para crear un LUN. Si no se especifica el parámetro **size =** , el LUN creado es el tamaño más grande posible permitido por todas las unidades especificadas. Si se especifica el parámetro **size =** , los proveedores seleccionarán las unidades de la lista de unidades especificada para crear el LUN. Los proveedores intentarán usar las unidades en el orden especificado cuando sea posible.

**stripesize**=

Especifica el tamaño en megabytes para un LUN *secciona* o *RAID* . La StripeSize no se puede cambiar una vez creado el LUN.

Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:
-   **B** para byte.
-   **KB** para kilobytes.
-   **MB** para megabyte.
-   **GB** para Gigabyte.
-   **TB** para terabyte.
-   **PB** para petabytes.

**dirigir**

Crea un nuevo destino iSCSI en el subsistema seleccionado actualmente.

**name**

Proporciona el nombre descriptivo para el destino.

**iscsiname**

Proporciona el nombre iSCSI del destino y se puede omitir para que el proveedor genere un nombre.

**tpgroup**

Crea un nuevo grupo de portales de destino iSCSI en el destino seleccionado actualmente.

**Noerr**

Especifica que se omitirán los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

#### <a name="remarks"></a>Comentarios

-   Se debe especificar el parámetro **size**= o **Drives**=. También se pueden usar juntos.
-   No se puede cambiar el tamaño de franja para un LUN tras su creación.

### <a name="BKMK_7"></a>elimínelos

Elimina el LUN seleccionado actualmente, el destino iSCSI (siempre y cuando no haya ningún LUN asociado con el destino iSCSI) o el grupo del portal de destino iSCSI.

#### <a name="syntax"></a>Sintaxis

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

#### <a name="parameters"></a>Parámetros

**LUN**

Elimina el LUN seleccionado actualmente y todos los datos que hay en él.

**desinstalar**

Especifica que se limpiará el disco del sistema local asociado con el LUN antes de que se elimine el LUN.

**dirigir**

Elimina el destino iSCSI seleccionado actualmente si no hay ningún LUN asociado al destino.

**tpgroup**

Elimina el grupo de portal de destino iSCSI seleccionado actualmente.

**Noerr**

Especifica que se omitirán los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

### <a name="BKMK_8"></a>detalle

Muestra información detallada sobre el objeto seleccionado actualmente del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
Detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

#### <a name="parameters"></a>Parámetros

**hbaport**

Muestra información detallada acerca del puerto del adaptador de bus host (HBA) seleccionado actualmente.

**iadapter**

Muestra información detallada acerca del adaptador de iniciador iSCSI seleccionado actualmente.

**IPORTAL**

Muestra información detallada acerca del portal del iniciador iSCSI seleccionado actualmente.

**provider**

Muestra información detallada acerca del proveedor seleccionado actualmente.

**subsystem**

Muestra información detallada acerca del subsistema seleccionado actualmente.

**módem**

Muestra información detallada acerca del controlador seleccionado actualmente.

**casilla**

Muestra información detallada acerca del puerto del controlador seleccionado actualmente.

**drive**

Muestra información detallada acerca de la unidad seleccionada actualmente, incluidos los LUN de ocupante.

**LUN**

Muestra información detallada acerca del LUN seleccionado actualmente, incluidas las unidades contribuyentes. El resultado varía ligeramente en función de si el LUN forma parte de un subsistema de Canal de fibra o iSCSI. Si la lista de hosts sin máscara solo contiene un asterisco, significa que el LUN no se enmascara en todos los hosts.

**Portal**

Muestra información detallada acerca del portal de destino iSCSI seleccionado actualmente.

**dirigir**

Muestra información detallada acerca del destino iSCSI seleccionado actualmente.

**tpgroup**

Muestra información detallada acerca del grupo de portal de destino iSCSI seleccionado actualmente.

**detallado**

Solo se usa con el parámetro LUN. Muestra información adicional, incluidos sus Plex.

### <a name="BKMK_9"></a>Desasociar

Establece la lista especificada de puertos de controlador como inactivo para el LUN seleccionado actualmente (otros puertos de controlador no se ven afectados) o desasocia la lista especificada de destinos iSCSI para el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

#### <a name="parameter"></a>Parámetro

**Controladores**

Solo se usa con proveedores de VDS 1,0. Quita los controladores de la lista de controladores asociados al LUN seleccionado actualmente.

**Puerto**

Solo se usa con proveedores de VDS 1,1. Quita los puertos del controlador de la lista de puertos de controlador asociados al LUN seleccionado actualmente.

**destinos**

Solo se usa con proveedores de VDS 1,1. Quita los destinos de la lista de destinos iSCSI asociados al LUN seleccionado actualmente.
```
<n> [,<n> [,…]]
```
Para su uso con el parámetro **Controllers** o **targets** . Especifica los números de los controladores o destinos iSCSI que se van a establecer como inactivos o desasociar.
```
<n-m>[,<n-m>[,…]]
```
Para su uso con el parámetro **ports** . Especifica los puertos de controlador que se van a establecer como inactivos mediante un par de número de controlador (*n*) y número de puerto (*m*).

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

### <a name="BKMK_10"></a>abandonar

Sale de Diskraid.

#### <a name="syntax"></a>Sintaxis

```
exit
```

### <a name="BKMK_11"></a>allá

Extiende el LUN seleccionado actualmente agregando sectores al final del LUN. No todos los proveedores admiten la ampliación de LUN. No extiende ningún volumen ni sistema de archivos contenidos en el LUN. Después de extender el LUN, debe extender las estructuras en disco asociadas mediante el comando de **extensión DiskPart** .

#### <a name="syntax"></a>Sintaxis

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

#### <a name="parameters"></a>Parámetros

**tamaño =**

Especifica el tamaño en megabytes para extender el LUN. Si no se especifica el parámetro **size =** , el LUN se extiende por el tamaño más grande posible permitido por todas las unidades especificadas. Si se especifica el parámetro **size =** , los proveedores seleccionan las unidades de la lista especificada por el parámetro **Drives =** para crear el LUN.

Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:
-   **B** para byte.
-   **KB** para kilobytes.
-   **MB** para megabyte.
-   **GB** para Gigabyte.
-   **TB** para terabytes
-   **PB** para petabyte

**unidades =**

Especifica el \<> drive_number para las unidades que se van a usar al crear un LUN. Si no se especifica el parámetro **size =** , el LUN creado es el tamaño más grande posible permitido por todas las unidades especificadas. Los proveedores usan las unidades en el orden especificado cuando sea posible.

**Noerr**

Especifica que se deben omitir los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

#### <a name="remarks"></a>Comentarios

Se debe especificar el tamaño \<o la unidad > parámetro. También se pueden usar juntos.

### <a name="BKMK_12"></a>flushcache

Borra la memoria caché del controlador seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
flushcache controller
```

### <a name="BKMK_13"></a>Ayuda

Muestra una lista de todos los comandos de Diskraid.

#### <a name="syntax"></a>Sintaxis

```
help
```

### <a name="BKMK_14"></a>importtarget

Recupera o establece el destino de importación de Servicio de instantáneas de volumen (VSS) actual que se establece para el subsistema seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
importtarget subsystem [set target]
```

#### <a name="parameter"></a>Parámetro

**establecer destino**

Si se especifica, establece el destino seleccionado actualmente en el destino de importación de VSS para el subsistema seleccionado actualmente. Si no se especifica, el comando recupera el destino de importación de VSS actual que se establece para el subsistema seleccionado actualmente.

### <a name="BKMK_15"></a>Initiator

Recupera información sobre el iniciador iSCSI local.

#### <a name="syntax"></a>Sintaxis

```
initiator
```

### <a name="BKMK_16"></a>invalidatecache

Invalida la caché en el controlador seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
invalidatecache controller
```

### <a name="BKMK_18"></a>lbpolicy

Establece la Directiva de equilibrio de carga en el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

#### <a name="parameters"></a>Parámetros

**type**

Especifica la Directiva de equilibrio de carga. Si no se especifica el tipo, se debe especificar el parámetro **path** . El tipo puede ser uno de los siguientes:

**CONMUTACIÓN POR ERROR**: Usa una ruta de acceso principal con otras rutas de acceso de copia de seguridad.

**ROUNDROBIN**: Usa todas las rutas de acceso en modo Round-Robin, que prueba cada ruta de forma secuencial.

**SUBSETROUNDROBIN**: Usa todas las rutas de acceso primarias en modo Round-Robin; las rutas de acceso de copia de seguridad se usan solo si se producen errores en todas las rutas principales

**DYNLQD**: Usa la ruta de acceso con el menor número de solicitudes activas.

**PONDERADO**: Usa la ruta de acceso con el menor peso (cada ruta de acceso debe tener asignada una ponderación).

**LEASTBLOCKS**: Usa la ruta de acceso con los bloques mínimos.

**VENDORSPECIFIC**: Usa una directiva específica del proveedor.

**rutas**

Especifica si una ruta de acceso es **principal** o tiene \<una ponderación determinada >. Las rutas de acceso no especificadas se establecen implícitamente como copia de seguridad. Cualquier ruta de acceso enumerada debe ser una de las rutas de acceso de LUN seleccionadas actualmente.

### <a name="BKMK_19"></a>lista

Muestra una lista de objetos del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
List {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

#### <a name="parameters"></a>Parámetros

**hbaports**

Muestra información de resumen sobre todos los puertos HBA conocidos para VDS. El puerto HBA seleccionado actualmente se marca con un asterisco (*).

**iadapters**

Muestra información de resumen sobre todos los adaptadores de iniciador iSCSI conocidos para VDS. El adaptador de iniciador seleccionado actualmente se marca con un asterisco (*).

**iportals**

Muestra información de resumen sobre todos los portales del iniciador iSCSI en el adaptador de iniciador seleccionado actualmente. El portal iniciador seleccionado actualmente se marca con un asterisco (*).

**presta**

Muestra información de resumen acerca de cada proveedor conocido para VDS. El proveedor seleccionado actualmente se marca con un asterisco (*).

**subsistemas**

Muestra información de resumen acerca de cada subsistema del sistema. El subsistema seleccionado actualmente está marcado con un asterisco (*).

**Controladores**

Muestra información de resumen sobre cada controlador del subsistema seleccionado actualmente. El controlador seleccionado actualmente se marca con un asterisco (*).

**Puerto**

Muestra información de resumen sobre cada puerto del controlador en el controlador seleccionado actualmente. El puerto seleccionado actualmente se marca con un asterisco (*).

**SATA**

Muestra información de resumen sobre cada unidad del subsistema seleccionado actualmente. La unidad seleccionada actualmente se marca con un asterisco (*).

**soporta**

Muestra información de resumen sobre cada LUN del subsistema seleccionado actualmente. El LUN seleccionado actualmente se marca con un asterisco (*).

**tportals**

Muestra información de resumen sobre todos los portales de destino iSCSI en el subsistema seleccionado actualmente. El portal de destino seleccionado actualmente se marca con un asterisco (*).

**destinos**

Muestra información de resumen sobre todos los destinos iSCSI en el subsistema seleccionado actualmente. El destino seleccionado actualmente se marca con un asterisco (*).

**tpgroups**

Muestra información de resumen sobre todos los grupos del portal de destino iSCSI del destino seleccionado actualmente. El grupo de portal seleccionado está marcado con un asterisco (*).

### <a name="BKMK_20"></a>Inicio

Registra el adaptador de iniciador iSCSI especificado en el destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

#### <a name="parameters"></a>Parámetros

**type**

Especifica el tipo de inicio de sesión que se va a realizar: **manual**, **persistente**o de **arranque**. Si no se especifica, se realizará un inicio de sesión manual.

Inicio de sesión **manual** manualmente.

**persistente** : Use automáticamente el mismo inicio de sesión cuando se reinicie el equipo.

**arranque** : (esta opción es para el desarrollo futuro y no se usa actualmente)<em>.</em>

**CHAPv2**

Especifica el tipo de autenticación CHAP que se va a usar: **ninguno**, el CHAP **unidireccional** o el CHAP **mutuo** ; Si no se especifica, no se utilizará ninguna autenticación.

**Portal**

Especifica un portal de destino opcional en el subsistema seleccionado actualmente que se va a usar para el inicio de sesión.

**IPORTAL**

Especifica un portal de iniciador opcional en el adaptador de iniciador especificado que se usará para el inicio de sesión.

\<marca >

Identificado por acrónimos de tres letras:

**DIRECCIONES IP**: Requerir IPsec

**EMP**: Habilitar múltiples rutas

**EHD**: Habilitar síntesis de encabezado

**EDD**: Habilitar síntesis de datos

### <a name="BKMK_21"></a>cierre

Registra el adaptador de iniciador iSCSI especificado fuera del destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
logout target iadapter= <iadapter>
```

#### <a name="parameters"></a>Parámetros

**iadapter**

Especifica el adaptador de iniciador con una sesión de inicio de sesión de.

### <a name="BKMK_22"></a>alimenticia

Realiza operaciones de mantenimiento en el objeto seleccionado actualmente del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
maintenance <object operation> [count=<iteration>]
```

#### <a name="parameters"></a>Parámetros

\<> de objeto

Especifica el tipo de objeto en el que se va a realizar la operación. El tipo de *objeto* puede ser un **subsistema**, un **controlador**, un **Puerto, una unidad** o un **LUN**.

\<operación >

Especifica la operación de mantenimiento que se va a realizar. El tipo de *operación* puede ser **Spinup**, **SpinDown**, **Blink**, **beep** o **ping**. Se debe especificar una *operación* .

**recuento =**

Especifica el número de veces que se va a repetir la *operación*. Normalmente se usa con **parpadeo**, **bip**o **ping**.

### <a name="BKMK_23"></a>Name

Establece el nombre descriptivo del subsistema, LUN o destino iSCSI seleccionado actualmente en el nombre especificado.

#### <a name="syntax"></a>Sintaxis

```
name {subsystem | lun | target} [<name>]
```

#### <a name="parameter"></a>Parámetro

\<name>

Especifica un nombre para el subsistema, LUN o destino. El nombre debe tener menos de 64 caracteres de longitud. Si no se proporciona ningún nombre, se elimina el nombre existente, si existe.

### <a name="BKMK_24"></a>n

Establece el estado del objeto actualmente seleccionado del tipo especificado en **offline**.

#### <a name="syntax"></a>Sintaxis

```
offline <object>
```

#### <a name="parameter"></a>Parámetro

\<> de objeto

Especifica el tipo de objeto en el que se va a realizar esta operación. \<Objeto >

el tipo puede ser **Subsystem**, **Controller**, **Drive**, **LUN**o **TPORTAL**.

### <a name="BKMK_25"></a>pantalla

Establece el estado del objeto seleccionado del tipo especificado en **online**. Si el objeto es **hbaport**, cambia el estado de las rutas de acceso al puerto HBA seleccionado actualmente a **en línea**.

#### <a name="syntax"></a>Sintaxis

```
online <object> 
```

#### <a name="parameter"></a>Parámetro

\<> de objeto

Especifica el tipo de objeto en el que se va a realizar esta operación. \<Objeto >

el tipo puede ser **hbaport**, **Subsystem**, **Controller**, **Drive**, **LUN**o **TPORTAL**.

### <a name="BKMK_26"></a>corregir

Realiza las operaciones necesarias, como la resincronización o el reemplazo activo, para reparar el LUN tolerante a errores seleccionado actualmente. Por ejemplo, la recuperación puede hacer que una reserva activa se enlace a un conjunto RAID que tenga un disco con errores u otra reasignación de la extensión de disco.

#### <a name="syntax"></a>Sintaxis

```
recover <lun>
```

### <a name="BKMK_27"></a>reenumerar

Reenumera los objetos del tipo especificado. Si usa el comando Extend LUN, debe usar el comando Refresh para actualizar el tamaño del disco antes de usar el comando reenumerate.

#### <a name="syntax"></a>Sintaxis

```
reenumerate {subsystems | drives}
```

#### <a name="parameters"></a>Parámetros

**subsistemas**

Consulta el proveedor para detectar los nuevos subsistemas que se agregaron en el proveedor seleccionado actualmente.

**SATA**

Consulta los buses de e/s internos para detectar las unidades nuevas que se agregaron en el subsistema seleccionado actualmente.

### <a name="BKMK_28"></a>actualizaciones

Actualiza los datos internos para el proveedor seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
refresh provider
```

### <a name="BKMK_29"></a>real

Se usa para comentar scripts.

#### <a name="syntax"></a>Sintaxis

```
Rem <comment>
```

### <a name="BKMK_30"></a>retirar

Quita el portal de destino iSCSI especificado del grupo de portal de destino seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
remove tpgroup tportal=<tportal> [noerr]
```

#### <a name="parameter"></a>Parámetro

**tpgroup TPORTAL =** \<> TPORTAL

Especifica el portal de destino iSCSI que se va a quitar.

**Noerr**

Especifica que se deben omitir los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

### <a name="BKMK_31"></a>Reemplace

Reemplaza la unidad especificada por la unidad seleccionada actualmente.

#### <a name="syntax"></a>Sintaxis

```
replace drive=<drive_number>
```

#### <a name="parameter"></a>Parámetro

**unidad =**

Especifica el \<> de drive_number para la unidad que se va a reemplazar.

#### <a name="remarks"></a>Comentarios

-   Es posible que la unidad especificada no sea la unidad seleccionada actualmente.

### <a name="BKMK_32"></a>determinado

Restablece el controlador o el puerto seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
Reset {controller | port}
```

#### <a name="parameters"></a>Parámetros

**módem**

Restablece el controlador.

**casilla**

Restablece el puerto.

### <a name="BKMK_33"></a>no

Muestra o cambia el objeto seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
Select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

#### <a name="parameters"></a>Parámetros

**objeto**

Especifica el tipo de objeto que se va a seleccionar. El \<tipo de > del objeto puede ser **proveedor**, **subsistema**, **controlador**, **unidad**o **LUN**.

**hbaport** [\<n >]

Establece el foco en el puerto HBA local especificado. Si no se especifica ningún puerto HBA, el comando muestra el puerto HBA seleccionado actualmente (si existe). Si se especifica un índice de puerto HBA no válido, se producirá un puerto HBA en el foco. Al seleccionar un puerto HBA, se anula la selección de los adaptadores de iniciador seleccionados y los portales de iniciador.

**iadapter** [\<n >]

Establece el foco en el adaptador de iniciador iSCSI local especificado. Si no se especifica ningún adaptador de iniciador, el comando muestra el adaptador de iniciador seleccionado actualmente (si existe). Si se especifica un índice de adaptador de iniciador no válido, se producirá un adaptador de iniciador no enfocado. Al seleccionar un adaptador de iniciador, se anula la selección de los puertos HBA seleccionados y los portales de iniciador.

**IPORTAL** [\<n >]

Establece el foco en el portal del iniciador iSCSI local especificado en el adaptador de iniciador iSCSI seleccionado. Si no se especifica ningún portal de iniciador, el comando muestra el portal de iniciador seleccionado actualmente (si existe). Si se especifica un índice de portal iniciador no válido, no se selecciona ningún portal de iniciador.

**proveedor** de [\<n >]

Establece el foco en el proveedor especificado. Si no se especifica ningún proveedor, el comando muestra el proveedor seleccionado actualmente (si existe). Si se especifica un índice de proveedor no válido, el proveedor no tiene foco.

**subsistema** [\<n >]

Establece el foco en el subsistema especificado. Si no se especifica ningún subsistema, el comando muestra el subsistema con el foco (si existe). Si se especifica un índice de subsistema no válido, el subsistema no tiene el foco. Al seleccionar un subsistema, se selecciona implícitamente su proveedor asociado.

**controlador** de [\<n >]

Establece el foco en el controlador especificado en el subsistema seleccionado actualmente. Si no se especifica ningún controlador, el comando muestra el controlador seleccionado actualmente (si existe). Si se especifica un índice de controlador no válido, no se produce ningún controlador en el foco. Al seleccionar un controlador, se anula la selección de los puertos de controlador, las unidades, los LUN, los portales de destino, los destinos y los grupos de portal de destino seleccionados.

**Puerto** de [\<n >]

Establece el foco en el puerto del controlador especificado en el controlador seleccionado actualmente. Si no se especifica ningún puerto, el comando muestra el puerto seleccionado actualmente (si existe). Si se especifica un índice de puerto no válido, no se selecciona ningún puerto.

**unidad** de [\<n >]

Establece el foco en la unidad especificada, o eje físico, dentro del subsistema seleccionado actualmente. Si no se especifica ninguna unidad, el comando muestra la unidad seleccionada actualmente (si existe). Si se especifica un índice de unidad no válido, se producirá una unidad sin foco. Al seleccionar una unidad, se anula la selección de los controladores, los puertos de controlador, los LUN, los portales de destino, los destinos y los grupos de portal de destino seleccionados.

**LUN** de [\<n >]

Establece el foco en el LUN especificado en el subsistema seleccionado actualmente. Si no se especifica ningún LUN, el comando muestra el LUN seleccionado actualmente (si existe). Si se especifica un índice de LUN no válido, no se produce ningún LUN seleccionado. Al seleccionar un LUN, se anula la selección de los controladores, los puertos de controlador, las unidades, los portales de destino, los destinos y los grupos de portales de destino seleccionados.

**TPORTAL** [\<n >]

Establece el foco en el portal de destino iSCSI especificado dentro del subsistema seleccionado actualmente. Si no se especifica ningún portal de destino, el comando muestra el portal de destino seleccionado actualmente (si existe). Si se especifica un índice de portal de destino no válido, no se selecciona ningún portal de destino. Al seleccionar un portal de destino, se anula la selección de los controladores, los puertos de controlador, las unidades, los LUN, los destinos y los grupos de portal de destino.

**destino** de [\<n >]

Establece el foco en el destino iSCSI especificado en el subsistema seleccionado actualmente. Si no se especifica ningún destino, el comando muestra el destino seleccionado actualmente (si existe). Si se especifica un índice de destino no válido, no se selecciona ningún destino. Al seleccionar un destino, se anula la selección de los controladores, los puertos del controlador, las unidades, los LUN, los portales de destino y los grupos del portal de destino.

**tpgroup** [\<n >]

Establece el foco en el grupo de portal de destino iSCSI especificado dentro del destino iSCSI seleccionado actualmente. Si no se especifica ningún grupo de portal de destino, el comando muestra el grupo de portal de destino seleccionado actualmente (si existe). Si se especifica un índice de grupo de portal de destino no válido, se producirá un grupo de portal de destino sin foco.

[\<n >]

Especifica el \<número de objeto > que se va a seleccionar. Si el <object number> objeto especificado no es válido, se borran las selecciones existentes para los objetos del tipo especificado. Si no <object number> se especifica, se muestra el objeto actual.

### <a name="BKMK_34"></a>setflag

Establece la unidad seleccionada actualmente como reserva activa.

#### <a name="syntax"></a>Sintaxis

```
setflag drive hotspare={true | false}
```

#### <a name="parameters"></a>Parámetros

**true**

Selecciona la unidad seleccionada actualmente como reserva activa.

**false**

Anula la selección de la unidad seleccionada actualmente como reserva activa.

#### <a name="remarks"></a>Comentarios

Las reservas activas no se pueden usar para operaciones de enlace de LUN normales. Solo se reservan para el control de errores. La unidad no debe estar enlazada actualmente a ningún LUN existente.

### <a name="BKMK_shrink"></a>Prime

Reduce el tamaño del LUN seleccionado.

#### <a name="syntax"></a>Sintaxis

```
shrink lun size=<n> [noerr]
```

#### <a name="parameters"></a>Parámetros

**tamaño =**

Especifica la cantidad deseada de espacio en megabytes (MB) para reducir el tamaño del LUN. Para especificar el tamaño con otras unidades, use uno de los sufijos reconocidos (B, KB, MB, GB, TB y PB) inmediatamente después del tamaño.

**Noerr**

Especifica que se omitirán los errores que se produzcan al realizar esta operación. Esto resulta útil en el modo de script.

### <a name="BKMK_35"></a>secundarios

Cambia el estado de las rutas de acceso al puerto del adaptador de bus host (HBA) seleccionado actualmente en modo de espera.

#### <a name="syntax"></a>Sintaxis

```
standby hbaport
```

#### <a name="parameters"></a>Parámetros

**hbaport**

Cambia el estado de las rutas de acceso al puerto del adaptador de bus host (HBA) seleccionado actualmente en modo de espera.

### <a name="BKMK_36"></a>Mask

Hace que los LUN seleccionados actualmente sean accesibles desde los hosts especificados.

#### <a name="syntax"></a>Sintaxis

```
unmask LUN {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

#### <a name="parameters"></a>Parámetros

**all**

Especifica que el LUN debe ser accesible desde todos los hosts. Sin embargo, no se puede quitar la máscara del LUN a todos los destinos de un subsistema iSCSI.

> [!IMPORTANT]
> Debe cerrar sesión en el destino antes de ejecutar el comando unmask ALL.

**ninguna**

Especifica que el LUN no debe ser accesible para ningún host.

> [!IMPORTANT]
> Debe cerrar sesión en el destino antes de ejecutar el comando unmask LUN NONE.

**add**

Especifica que los hosts especificados se deben agregar a la lista de hosts existente desde la que se puede tener acceso a este LUN. Si no se especifica este parámetro, la lista de hosts proporcionada reemplaza la lista existente de hosts de los que se puede tener acceso a este LUN.

**WWN =**

Especifica una lista de números hexadecimales que representan los nombres de todo el mundo a partir del cual se debe hacer accesible el LUN o los hosts. Para enmascarar o desenmascarar un conjunto específico de hosts en un subsistema Canal de fibra, puede escribir una lista de WWN separada por punto y coma para los puertos de los equipos host de interés.

**iniciador =**

Especifica una lista de iniciadores iSCSI a los que se debe hacer accesible el LUN seleccionado actualmente. Para enmascarar o desenmascarar un conjunto específico de hosts en un subsistema iSCSI, puede escribir una lista separada por punto y coma de nombres de iniciador iSCSI para los iniciadores en los equipos host de interés.

**desinstalar**

Si se especifica, desinstala el disco asociado con el LUN en el sistema local antes de enmascarar el LUN.

## <a name="scripting-diskraid"></a>Scripting de Diskraid

Diskraid se puede incluir en un script en cualquier equipo que ejecute Windows Server 2008 o Windows Server 2003 con un proveedor de hardware de VDS asociado. Para invocar un script de Diskraid, en el símbolo del sistema, escriba:
```
diskraid /s <script.txt>
```
De forma predeterminada, Diskraid detiene el procesamiento de comandos y devuelve un código de error si hay un problema en el script. Para seguir ejecutando el script y omitir los errores, incluya el parámetro NOERR en el comando. Esto permite procedimientos útiles como el uso de un único script para eliminar todos los LUN de un subsistema, independientemente del número total de LUN. No todos los comandos admiten el parámetro NOERR. Los errores se devuelven siempre en los errores de sintaxis de comandos, independientemente de si se incluyó el parámetro NOERR,

### <a name="diskraid-error-codes"></a>Códigos de error de Diskraid

|Código de error|Descripción del error|
|----------|-----------------|
|0|No se ha producido ningún error. El script completo se ejecutó sin errores.|
|1|Se produjo una excepción grave.|
|2|Los argumentos especificados en una línea de comandos de Diskraid no eran correctos.|
|3|Diskraid no pudo abrir el script o el archivo de salida especificado.|
|4|Uno de los servicios que usa Diskraid devolvió un error.|
|5|Se produjo un error de sintaxis de comando. No se pudo realizar el script porque un objeto se seleccionó incorrectamente o no era válido para su uso con ese comando.|

## <a name="example-interactively-view-status-of-subsystem"></a>Ejemplo: Ver el estado de subsistema de forma interactiva

Si desea ver el estado del subsistema 0 en el equipo, escriba lo siguiente en la línea de comandos:
```
diskraid
```
Presione ENTRAR. Se muestra lo siguiente:
```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```
Para seleccionar Subsystem 0, escriba lo siguiente en el símbolo del sistema de Diskraid:
```
select subsystem 0
```
Presione ENTRAR. Se muestra una salida similar a la siguiente:
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
Para salir de Diskraid, escriba lo siguiente en el símbolo del sistema de Diskraid:
```
exit
```