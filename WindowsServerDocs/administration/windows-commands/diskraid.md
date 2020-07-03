---
title: Diskraid
description: Artículo de referencia de la herramienta de línea de comandos Diskraid, que permite configurar y administrar matrices redundantes de subsistemas de almacenamiento de discos independientes (o económicos) (RAID).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20aef1e5-7641-47cf-b4eb-cda117f65b6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0745c708878fa9da6571666b5702b4408976164
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924762"
---
# <a name="diskraid"></a>Diskraid

**Diskraid** es una herramienta de línea de comandos que permite configurar y administrar matrices redundantes de subsistemas de almacenamiento de discos independientes (o económicos).

RAID se usa normalmente en servidores para estandarizar y clasificar los sistemas de disco tolerantes a errores. Los niveles RAID ofrecen varias combinaciones de rendimiento, confiabilidad y costo. Algunos servidores proporcionan tres niveles de RAID: nivel 0 (seccionado), nivel 1 (creación de reflejo) y nivel 5 (seccionamiento con paridad).

Un subsistema RAID de hardware distingue las unidades de almacenamiento físicamente direccionable entre sí mediante un número de unidad lógica (LUN). Un objeto LUN debe tener al menos un Plex y puede tener cualquier número de Plex adicionales. Cada Plex contiene una copia de los datos en el objeto LUN. Los Plex se pueden agregar y quitar de un objeto LUN.

La mayoría de los comandos de Diskraid funcionan en un puerto de adaptador de bus host (HBA) específico, un adaptador de iniciador, un portal de iniciador, un proveedor, un subsistema, un controlador, un puerto, una unidad, un LUN, un portal de destino, un destino o un grupo de portal de destino. Use el comando **seleccionar** para seleccionar un objeto. Se dice que el objeto seleccionado tiene el foco. El enfoque simplifica las tareas de configuración comunes, como la creación de varios LUN en el mismo subsistema.

> [!NOTE]
> La herramienta de línea de comandos Diskraid solo funciona con subsistemas de almacenamiento compatibles con el servicio de disco virtual (VDS).

## <a name="diskraid-commands"></a>Comandos de Diskraid

Los siguientes comandos están disponibles en la herramienta Diskraid.

### <a name="add"></a>add

Agrega un LUN existente al LUN seleccionado actualmente o agrega un portal de destino iSCSI al grupo del portal de destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
add plex lun=n [noerr]
add tpgroup tportal=n [noerr]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| LUN de Plex =`<n>` | Especifica el número de LUN que se va a agregar como complejo al LUN seleccionado actualmente. PRECAUCIÓN: se eliminarán todos los datos del LUN que se va a agregar como complejo. |
| tpgroup TPORTAL =`<n>` | Especifica el número de portal de destino iSCSI que se va a agregar al grupo de portal de destino iSCSI seleccionado actualmente. |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="associate"></a>asócielos

Establece la lista especificada de puertos de controlador como activo para el LUN seleccionado actualmente (otros puertos de controlador se establecen como inactivos), o agrega los puertos de controlador especificados a la lista de puertos de controlador activos existentes para el LUN seleccionado actualmente, o asocia el destino iSCSI especificado para el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
associate controllers [add] <n>[,<n> [,…]]
associate ports [add] <n-m>[,<n-m>[,…]]
associate targets [add] <n>[,<n> [,…]]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| controlador | Agrega o reemplaza la lista de controladores asociados al LUN seleccionado actualmente. Use solo con proveedores de VDS 1,0. |
| ports | Agrega o reemplaza la lista de puertos de controlador asociados al LUN seleccionado actualmente. Use solo con proveedores de VDS 1,1. |
| destinos | Agrega o reemplaza la lista de destinos iSCSI asociados al LUN seleccionado actualmente. Use solo con proveedores de VDS 1,1. |
| add | **Si usa proveedores de VDS 1,0:** Agrega los controladores especificados a la lista existente de controladores asociados con el LUN. Si no se especifica este parámetro, la lista de controladores reemplazará a la lista existente de controladores asociados a este LUN.<p>**Si usa proveedores de VDS 1,1:** Agrega los puertos de controlador especificados a la lista existente de puertos de controlador asociados con el LUN. Si no se especifica este parámetro, la lista de puertos de controlador reemplaza la lista existente de puertos de controlador asociados a este LUN. |
| `<n>[,<n> [, ...]]` | Use con el parámetro **Controllers** o **targets** . Especifica los números de los controladores o destinos iSCSI que se van a establecer en activo o asociado. |
| `<n-m>[,<n-m>[,…]]` | Use con el parámetro **ports** . Especifica los puertos del controlador para establecer activos mediante un par de número de controlador (*n*) y número de puerto (*m*). |

#### <a name="example"></a>Ejemplo

Para asociar y agregar puertos a un LUN que usa un proveedor de VDS 1,1:

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

### <a name="automagic"></a>automagic

Establece o borra las marcas que proporcionan sugerencias a los proveedores sobre cómo configurar un LUN. Cuando se usa sin parámetros, la operación **automagic** muestra una lista de marcas.

#### <a name="syntax"></a>Sintaxis

```
automagic {set | clear | apply} all <flag=value> [<flag=value> [...]]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| set | Establece las marcas especificadas en los valores especificados. |
| clear | Borra las marcas especificadas. La palabra clave **All** borra todas las marcas automágicas. |
| apply | Aplica las marcas actuales al LUN seleccionado. |
| `<flag>` | Las marcas se identifican mediante acrónimos de tres letras, entre los que se incluyen:<ul><li>**FCR** : recuperación de bloqueos rápida requerida</li><li>**FTL** : tolerante a errores</li><li>Lecturas principalmente de **MSR**</li><li>**MXD** : número máximo de unidades</li><li>**MXS** : tamaño máximo esperado</li><li>**Ora** : alineación de lectura óptima</li><li>**Ors** : tamaño de lectura óptimo</li><li>**OSR** -Optimize para lecturas secuenciales</li><li>**OSW** : optimizar para escrituras secuenciales</li><li> **OWA** : alineación de escritura óptima</li><li>**OWS** : tamaño de escritura óptimo</li><li>**RBP** : prioridad de recompilación</li><li>**RBV** : comprobación de la prueba de lectura habilitada</li><li>**RMP** : reasignación habilitada</li><li>**STS** -tamaño de franja</li><li>**WTC** : almacenamiento en caché de escritura a través habilitado</li><li>**YNK** : extraíble</li></ul> |

### <a name="break"></a>break

Quita el Plex del LUN seleccionado actualmente. El Plex y los datos que contiene no se conservan y se pueden recuperar las extensiones de unidad.

> [!CAUTION]
> Primero debe seleccionar un LUN reflejado antes de usar este comando. Se eliminarán todos los datos del complejo. No se garantiza que todos los datos contenidos en el LUN original sean coherentes.

#### <a name="syntax"></a>Sintaxis

```
break plex=<plex_number> [noerr]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| complejos | Especifica el número del complejo que se va a quitar. El Plex y los datos que contiene no se conservarán y se recuperarán los recursos usados por este complejo. No se garantiza que los datos contenidos en el LUN sean coherentes. Si desea conservar este complejo, use el Servicio de instantáneas de volumen (VSS). |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="chap"></a>CHAPv2

Establece el secreto compartido del Protocolo de autenticación por desafío mutuo (CHAP) para que los iniciadores iSCSI y los destinos iSCSI puedan comunicarse entre sí.

#### <a name="syntax"></a>Sintaxis

```
chap initiator set secret=[<secret>] [target=<target>]
chap initiator remember secret=[<secret>] target=<target>
chap target set secret=[<secret>] [initiator=<initiatorname>]
chap target remember secret=[<secret>] initiator=<initiatorname>
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| conjunto de iniciadores | Establece el secreto compartido en el servicio del iniciador iSCSI local que se usa para la autenticación CHAP mutua cuando el iniciador autentica el destino. |
| se recuerda al iniciador | Comunica el secreto CHAP de un destino iSCSI con el servicio del iniciador iSCSI local para que el servicio iniciador pueda usar el secreto con el fin de autenticarse en el destino durante la autenticación CHAP. |
| conjunto de destinos | Establece el secreto compartido en el destino iSCSI seleccionado actualmente que se usa para la autenticación CHAP cuando el destino autentica al iniciador. |
| objetivo recordar | Comunica el secreto CHAP de un iniciador iSCSI con el destino iSCSI actual enfocado para que el destino pueda usar el secreto con el fin de autenticarse en el iniciador durante la autenticación CHAP mutua. |
| secret | Especifica el secreto que se va a usar. Si está vacío, se borrará el secreto. |
| Destino | Especifica un destino en el subsistema seleccionado actualmente para asociarlo con el secreto. Esto es opcional cuando se establece un secreto en el iniciador y se deja indica que el secreto se usará para todos los destinos que aún no tienen un secreto asociado. |
| initiatorname | Especifica un nombre iSCSI del iniciador que se va a asociar con el secreto. Esto es opcional cuando se establece un secreto en un destino y se deja indica que el secreto se usará para todos los iniciadores que aún no tienen un secreto asociado. |

### <a name="create"></a>create

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

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| simple | Crea un LUN simple. |
| franja | Crea un LUN seccionado. |
| RAID | Crea un LUN seccionado con paridad. |
| mirror | Crea un LUN reflejado. |
| automagic | Crea un LUN con las sugerencias de *automagic* que están actualmente en vigor. Para obtener más información, consulte el subcomando **automagic** de este artículo. |
| size= | Especifica el tamaño total del LUN en megabytes. Se debe especificar el parámetro **size**= o **Drives**=. También se pueden usar juntos. Si no se especifica el parámetro **size =** , el LUN creado será el tamaño más grande posible permitido por todas las unidades especificadas.<p>Normalmente, un proveedor crea un LUN al menos tan grande como el tamaño solicitado, pero es posible que el proveedor tenga que redondear al siguiente tamaño más grande en algunos casos. Por ejemplo, si el tamaño se especifica como. 99 GB y el proveedor solo puede asignar GB de extensiones de disco, el LUN resultante sería 1 GB. Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:<ul><li>Byte **B**</li><li>**KB** -kilobytes</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte.</li></ul> |
| unidades = | Especifica el *drive_number* de las unidades que se van a usar para crear un LUN. Se debe especificar el parámetro **size**= o **Drives**=. También se pueden usar juntos. Si no se especifica el parámetro **size =** , el LUN creado es el tamaño más grande posible permitido por todas las unidades especificadas. Si se especifica el parámetro **size =** , los proveedores seleccionarán las unidades de la lista de unidades especificada para crear el LUN. Los proveedores intentarán usar las unidades en el orden especificado cuando sea posible. |
| StripeSize = | Especifica el tamaño en megabytes para un LUN *secciona* o *RAID* . La StripeSize no se puede cambiar una vez creado el LUN. Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:<ul><li>Byte **B**</li><li>**KB** -kilobytes</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte.</li></ul> |
| Destino | Crea un nuevo destino iSCSI en el subsistema seleccionado actualmente. |
| name | Proporciona el nombre descriptivo para el destino. |
| iscsiname | Proporciona el nombre iSCSI del destino y se puede omitir para que el proveedor genere un nombre. |
| tpgroup | Crea un nuevo grupo de portales de destino iSCSI en el destino seleccionado actualmente. |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="delete"></a>delete

Elimina el LUN seleccionado actualmente, el destino iSCSI (siempre y cuando no haya ningún LUN asociado con el destino iSCSI) o el grupo del portal de destino iSCSI.

#### <a name="syntax"></a>Sintaxis

```
delete lun [uninstall] [noerr]
delete target [noerr]
delete tpgroup [noerr]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| lun | Elimina el LUN seleccionado actualmente y todos los datos que hay en él. |
| uninstall | Especifica que se limpiará el disco del sistema local asociado con el LUN antes de que se elimine el LUN. |
| Destino | Elimina el destino iSCSI seleccionado actualmente si no hay ningún LUN asociado al destino. |
| tpgroup | Elimina el grupo de portal de destino iSCSI seleccionado actualmente. |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="detail"></a>detalles

Muestra información detallada sobre el objeto seleccionado actualmente del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
detail {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup} [verbose]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| hbaport | Muestra información detallada acerca del puerto del adaptador de bus host (HBA) seleccionado actualmente. |
| iadapter | Muestra información detallada acerca del adaptador de iniciador iSCSI seleccionado actualmente. |
| IPORTAL | Muestra información detallada acerca del portal del iniciador iSCSI seleccionado actualmente. |
| provider | Muestra información detallada acerca del proveedor seleccionado actualmente. |
| subsistema | Muestra información detallada acerca del subsistema seleccionado actualmente. |
| controlador | Muestra información detallada acerca del controlador seleccionado actualmente. |
| port | Muestra información detallada acerca del puerto del controlador seleccionado actualmente. |
| unidad | Muestra información detallada acerca de la unidad seleccionada actualmente, incluidos los LUN de ocupante. |
| lun | Muestra información detallada acerca del LUN seleccionado actualmente, incluidas las unidades contribuyentes. El resultado varía ligeramente en función de si el LUN forma parte de un subsistema de Canal de fibra o iSCSI. Si la lista de hosts sin máscara solo contiene un asterisco, significa que el LUN no se enmascara en todos los hosts. |
| Portal | Muestra información detallada acerca del portal de destino iSCSI seleccionado actualmente. |
| Destino | Muestra información detallada acerca del destino iSCSI seleccionado actualmente. |
| tpgroup | Muestra información detallada acerca del grupo de portal de destino iSCSI seleccionado actualmente. |
| verbose | Solo se usa con el parámetro LUN. Muestra información adicional, incluidos sus Plex. |

### <a name="dissociate"></a>Desasociar

Establece la lista especificada de puertos de controlador como inactivo para el LUN seleccionado actualmente (otros puertos de controlador no se ven afectados) o desasocia la lista especificada de destinos iSCSI para el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
dissociate controllers <n> [,<n> [,...]]
dissociate ports <n-m>[,<n-m>[,…]]
dissociate targets <n> [,<n> [,…]]
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| controllers | Quita los controladores de la lista de controladores asociados al LUN seleccionado actualmente. Use solo con proveedores de VDS 1,0. |
| ports | Quita los puertos del controlador de la lista de puertos de controlador asociados al LUN seleccionado actualmente. Use solo con proveedores de VDS 1,1. |
| destinos | Quita los destinos de la lista de destinos iSCSI asociados al LUN seleccionado actualmente. Use solo con proveedores de VDS 1,1. |
| `<n> [,<n> [,…]]` | Para su uso con el parámetro **Controllers** o **targets** . Especifica los números de los controladores o destinos iSCSI que se van a establecer como inactivos o desasociar. |
| `<n-m>[,<n-m>[,…]]` | Para su uso con el parámetro **ports** . Especifica los puertos de controlador que se van a establecer como inactivos mediante un par de número de controlador (*n*) y número de puerto (*m*). |

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

### <a name="exit"></a>exit

Sale de Diskraid.

#### <a name="syntax"></a>Sintaxis

```
exit
```

### <a name="extend"></a>extend

Extiende el LUN seleccionado actualmente agregando sectores al final del LUN. No todos los proveedores admiten la ampliación de LUN. No extiende ningún volumen ni sistema de archivos contenidos en el LUN. Después de extender el LUN, debe extender las estructuras en disco asociadas mediante el comando de **extensión DiskPart** .

#### <a name="syntax"></a>Sintaxis

```
extend lun [size=<LUN_size>] [drives=<drive_number>, [<drive_number>, ...]] [noerr]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño | Especifica el tamaño en megabytes para extender el LUN. Se debe especificar el *tamaño* o el `<drive>` parámetro. También se pueden usar juntos. Si no se especifica el parámetro **size =** , el LUN se extiende por el tamaño más grande posible permitido por todas las unidades especificadas. Si se especifica el parámetro **size =** , los proveedores seleccionan las unidades de la lista especificada por el parámetro **Drives =** para crear el LUN. Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:<ul><li>Byte **B**</li><li>**KB** -kilobytes</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte.</li></ul> |
| unidades = | Especifica el `<drive_number>` de las unidades que se van a utilizar al crear un LUN. Se debe especificar el *tamaño* o el `<drive>` parámetro. También se pueden usar juntos. Si no se especifica el parámetro **size =** , el LUN creado es el tamaño más grande posible permitido por todas las unidades especificadas. Los proveedores usan las unidades en el orden especificado cuando sea posible. |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="flushcache"></a>flushcache

Borra la memoria caché del controlador seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
flushcache controller
```

### <a name="help"></a>ayuda

Muestra una lista de todos los comandos de Diskraid.

#### <a name="syntax"></a>Sintaxis

```
help
```

### <a name="importtarget"></a>importtarget

Recupera o establece el destino de importación de Servicio de instantáneas de volumen (VSS) actual que se establece para el subsistema seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
importtarget subsystem [set target]
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| establecer destino | Si se especifica, establece el destino seleccionado actualmente en el destino de importación de VSS para el subsistema seleccionado actualmente. Si no se especifica, el comando recupera el destino de importación de VSS actual que se establece para el subsistema seleccionado actualmente. |

### <a name="initiator"></a>initiator

Recupera información sobre el iniciador iSCSI local.

#### <a name="syntax"></a>Sintaxis

```
initiator
```

### <a name="invalidatecache"></a>invalidatecache

Invalida la caché en el controlador seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
invalidatecache controller
```

### <a name="lbpolicy"></a>lbpolicy

Establece la Directiva de equilibrio de carga en el LUN seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
lbpolicy set lun type=<type> [paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]]
lbpolicy set lun paths=<path>-{primary | <weight>}[,<path>-{primary | <weight>}[,…]]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| type | Especifica la Directiva de equilibrio de carga. Si no se especifica el tipo, se debe especificar el parámetro **path** . El elemento Type puede ser uno de los siguientes:<ul><li>**Conmutación por error** : usa una ruta de acceso principal con otras rutas de acceso de copia de seguridad.</li><li>**ROUNDROBIN** : usa todas las rutas de acceso en modo Round Robin, que prueba cada ruta de forma secuencial.</li><li>**SUBSETROUNDROBIN** : usa todas las rutas de acceso principales en modo Round Robin; las rutas de acceso de copia de seguridad se usan solo si se producen errores en todas las rutas principales</li><li>**DYNLQD** : usa la ruta de acceso con el menor número de solicitudes activas.<li><li>**Ponderado** : usa la ruta de acceso con el menor peso (cada ruta de acceso debe tener asignada una ponderación).</li><li>**LEASTBLOCKS** : usa la ruta de acceso con los bloques mínimos.</li><li>**VENDORSPECIFIC** : usa una directiva específica del proveedor.</li></ul> |
| path | Especifica si una ruta de acceso es **principal** o tiene un determinado `<weight>` . Las rutas de acceso no especificadas se establecen implícitamente como copia de seguridad. Cualquier ruta de acceso enumerada debe ser una de las rutas de acceso de LUN seleccionadas actualmente. |

### <a name="list"></a>list

Muestra una lista de objetos del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
list {hbaports | iadapters | iportals | providers | subsystems | controllers | ports | drives | LUNs | tportals | targets | tpgroups}
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| hbaports | Muestra información de resumen sobre todos los puertos HBA conocidos para VDS. El puerto HBA seleccionado actualmente se marca con un asterisco (*). |
| iadapters | Muestra información de resumen sobre todos los adaptadores de iniciador iSCSI conocidos para VDS. El adaptador de iniciador seleccionado actualmente se marca con un asterisco (*). |
| iportals | Muestra información de resumen sobre todos los portales del iniciador iSCSI en el adaptador de iniciador seleccionado actualmente. El portal iniciador seleccionado actualmente se marca con un asterisco (*). |
| providers | Muestra información de resumen acerca de cada proveedor conocido para VDS. El proveedor seleccionado actualmente se marca con un asterisco (*). |
| subsistemas | Muestra información de resumen acerca de cada subsistema del sistema. El subsistema seleccionado actualmente está marcado con un asterisco (*). |
| controllers | Muestra información de resumen sobre cada controlador del subsistema seleccionado actualmente. El controlador seleccionado actualmente se marca con un asterisco (*). |
| ports | Muestra información de resumen sobre cada puerto del controlador en el controlador seleccionado actualmente. El puerto seleccionado actualmente se marca con un asterisco (*). |
| unidades | Muestra información de resumen sobre cada unidad del subsistema seleccionado actualmente. La unidad seleccionada actualmente se marca con un asterisco (*). |
| soporta | Muestra información de resumen sobre cada LUN del subsistema seleccionado actualmente. El LUN seleccionado actualmente se marca con un asterisco (*). |
| tportals | Muestra información de resumen sobre todos los portales de destino iSCSI en el subsistema seleccionado actualmente. El portal de destino seleccionado actualmente se marca con un asterisco (*). |
| destinos | Muestra información de resumen sobre todos los destinos iSCSI en el subsistema seleccionado actualmente. El destino seleccionado actualmente se marca con un asterisco (*). |
| tpgroups | Muestra información de resumen sobre todos los grupos del portal de destino iSCSI del destino seleccionado actualmente. El grupo de portal seleccionado está marcado con un asterisco (*). |

### <a name="login"></a>login

Registra el adaptador de iniciador iSCSI especificado en el destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
login target iadapter=<iadapter> [type={manual | persistent | boot}] [chap={none | oneway | mutual}] [iportal=<iportal>] [tportal=<tportal>] [<flag> [<flag> […]]]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| type | Especifica el tipo de inicio de sesión que se va a realizar: **manual** o **persistente**. Si no se especifica, se realizará un inicio de sesión manual. |
| manual | Inicio de sesión manual. También hay una opción de **arranque** pensada para el desarrollo futuro y no se usa actualmente. |
| permanentes | Usar automáticamente el mismo inicio de sesión cuando se reinicie el equipo. |
| CHAPv2 | Especifica el tipo de autenticación CHAP que se va a usar: **ninguno**, el CHAP **unidireccional** o el CHAP **mutuo** ; Si no se especifica, no se utilizará ninguna autenticación. |
| Portal | Especifica un portal de destino opcional en el subsistema seleccionado actualmente que se va a usar para el inicio de sesión. |
| IPORTAL | Especifica un portal de iniciador opcional en el adaptador de iniciador especificado que se usará para el inicio de sesión. |
| `<flag>` | Identificado por acrónimos de tres letras:<ul><li>**IP** : requerir IPSec</li><li>**EMP** -habilitar múltiples rutas</li><li>**EHD** : habilitar síntesis de encabezado</li><li>**EDD** : habilitar síntesis de datos</li></ul> |

### <a name="logout"></a>logout

Registra el adaptador de iniciador iSCSI especificado fuera del destino iSCSI seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
logout target iadapter= <iadapter>
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| iadapter | Especifica el adaptador de iniciador con una sesión de inicio de sesión de. |

### <a name="maintenance"></a>mantenimiento

Realiza operaciones de mantenimiento en el objeto seleccionado actualmente del tipo especificado.

#### <a name="syntax"></a>Sintaxis

```
maintenance <object operation> [count=<iteration>]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<object>` | Especifica el tipo de objeto en el que se va a realizar la operación. El tipo de *objeto* puede ser un **subsistema**, un **controlador**, un **Puerto, una unidad** o un **LUN**. |
| `<operation>` | Especifica la operación de mantenimiento que se va a realizar. El tipo de *operación* puede ser **Spinup**, **SpinDown**, **Blink**, **beep** o **ping**. Se debe especificar una *operación* . |
| recuento = | Especifica el número de veces que se va a repetir la *operación*. Normalmente se usa con **parpadeo**, **bip**o **ping**. |

### <a name="name"></a>name

Establece el nombre descriptivo del subsistema, LUN o destino iSCSI seleccionado actualmente en el nombre especificado.

#### <a name="syntax"></a>Sintaxis

```
name {subsystem | lun | target} [<name>]
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| `<name>` | Especifica un nombre para el subsistema, LUN o destino. El nombre debe tener menos de 64 caracteres de longitud. Si no se proporciona ningún nombre, se elimina el nombre existente, si existe. |

### <a name="offline"></a>sin conexión

Establece el estado del objeto actualmente seleccionado del tipo especificado en **offline**.

#### <a name="syntax"></a>Sintaxis

```
offline <object>
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| `<object>` | Especifica el tipo de objeto en el que se va a realizar esta operación. El tipo puede ser: **Subsystem**, **Controller**, **Drive**, **LUN**o **TPORTAL**. |

### <a name="online"></a>online (en línea)

Establece el estado del objeto seleccionado del tipo especificado en **online**. Si el objeto es **hbaport**, cambia el estado de las rutas de acceso al puerto HBA seleccionado actualmente a **en línea**.

#### <a name="syntax"></a>Sintaxis

```
online <object>
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| `<object>` | Especifica el tipo de objeto en el que se va a realizar esta operación. El tipo puede ser: **hbaport**, **Subsystem**, **Controller**, **Drive**, **LUN**o **TPORTAL**. |

### <a name="recover"></a>recover

Realiza las operaciones necesarias, como la resincronización o el reemplazo activo, para reparar el LUN tolerante a errores seleccionado actualmente. Por ejemplo, la recuperación puede hacer que una reserva activa se enlace a un conjunto RAID que tenga un disco con errores u otra reasignación de la extensión de disco.

#### <a name="syntax"></a>Sintaxis

```
recover <lun>
```

### <a name="reenumerate"></a>reenumerar

Reenumera los objetos del tipo especificado. Si usa el comando Extend LUN, debe usar el comando Refresh para actualizar el tamaño del disco antes de usar el comando reenumerate.

#### <a name="syntax"></a>Sintaxis

```
reenumerate {subsystems | drives}
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| subsistemas | Consulta el proveedor para detectar los nuevos subsistemas que se agregaron en el proveedor seleccionado actualmente. |
| unidades | Consulta los buses de e/s internos para detectar las unidades nuevas que se agregaron en el subsistema seleccionado actualmente. |

### <a name="refresh"></a>actualizar

Actualiza los datos internos para el proveedor seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
refresh provider
```

### <a name="rem"></a>rem

Se usa para comentar scripts.

#### <a name="syntax"></a>Sintaxis

```
Rem <comment>
```

### <a name="remove"></a>remove

Quita el portal de destino iSCSI especificado del grupo de portal de destino seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
remove tpgroup tportal=<tportal> [noerr]
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| tpgroup TPORTAL =`<tportal>` | Especifica el portal de destino iSCSI que se va a quitar. |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="replace"></a>replace

Reemplaza la unidad especificada por la unidad seleccionada actualmente. Es posible que la unidad especificada no sea la unidad seleccionada actualmente.

#### <a name="syntax"></a>Sintaxis

```
replace drive=<drive_number>
```

##### <a name="parameter"></a>Parámetro

| Parámetro | Descripción |
| --------- | ----------- |
| unidad = | Especifica el `<drive_number>` de la unidad que se va a reemplazar. |

### <a name="reset"></a>reset

Restablece el controlador o el puerto seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
reset {controller | port}
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| controlador | Restablece el controlador. |
| port | Restablece el puerto. |

### <a name="select"></a>select

Muestra o cambia el objeto seleccionado actualmente.

#### <a name="syntax"></a>Sintaxis

```
select {hbaport | iadapter | iportal | provider | subsystem | controller | port | drive | lun | tportal | target | tpgroup } [<n>]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| object | Especifica el tipo de objeto que se va a seleccionar, incluidos: **proveedor**, **subsistema**, **controlador**, **unidad**o **LUN**. |
| hbaport`[<n>]` | Establece el foco en el puerto HBA local especificado. Si no se especifica ningún puerto HBA, el comando muestra el puerto HBA seleccionado actualmente (si existe). Si se especifica un índice de puerto HBA no válido, se producirá un puerto HBA en el foco. Al seleccionar un puerto HBA, se anula la selección de los adaptadores de iniciador seleccionados y los portales de iniciador. |
| iadapter`[<n>]` | Establece el foco en el adaptador de iniciador iSCSI local especificado. Si no se especifica ningún adaptador de iniciador, el comando muestra el adaptador de iniciador seleccionado actualmente (si existe). Si se especifica un índice de adaptador de iniciador no válido, se producirá un adaptador de iniciador no enfocado. Al seleccionar un adaptador de iniciador, se anula la selección de los puertos HBA seleccionados y los portales de iniciador. |
| IPORTAL`[<n>]` | Establece el foco en el portal del iniciador iSCSI local especificado en el adaptador de iniciador iSCSI seleccionado. Si no se especifica ningún portal de iniciador, el comando muestra el portal de iniciador seleccionado actualmente (si existe). Si se especifica un índice de portal iniciador no válido, no se selecciona ningún portal de iniciador. |
| presta`[<n>]` | Establece el foco en el proveedor especificado. Si no se especifica ningún proveedor, el comando muestra el proveedor seleccionado actualmente (si existe). Si se especifica un índice de proveedor no válido, el proveedor no tiene foco. |
| subsistema`[<n>]` | Establece el foco en el subsistema especificado. Si no se especifica ningún subsistema, el comando muestra el subsistema con el foco (si existe). Si se especifica un índice de subsistema no válido, el subsistema no tiene el foco. Al seleccionar un subsistema, se selecciona implícitamente su proveedor asociado. |
| módem`[<n>]` | Establece el foco en el controlador especificado en el subsistema seleccionado actualmente. Si no se especifica ningún controlador, el comando muestra el controlador seleccionado actualmente (si existe). Si se especifica un índice de controlador no válido, no se produce ningún controlador en el foco. Al seleccionar un controlador, se anula la selección de los puertos de controlador, las unidades, los LUN, los portales de destino, los destinos y los grupos de portal de destino seleccionados. |
| casilla`[<n>]` | Establece el foco en el puerto del controlador especificado en el controlador seleccionado actualmente. Si no se especifica ningún puerto, el comando muestra el puerto seleccionado actualmente (si existe). Si se especifica un índice de puerto no válido, no se selecciona ningún puerto. |
| dispositivo`[<n>]` | Establece el foco en la unidad especificada, o eje físico, dentro del subsistema seleccionado actualmente. Si no se especifica ninguna unidad, el comando muestra la unidad seleccionada actualmente (si existe). Si se especifica un índice de unidad no válido, se producirá una unidad sin foco. Al seleccionar una unidad, se anula la selección de los controladores, los puertos de controlador, los LUN, los portales de destino, los destinos y los grupos de portal de destino seleccionados. |
| LUN`[<n>]` | Establece el foco en el LUN especificado en el subsistema seleccionado actualmente. Si no se especifica ningún LUN, el comando muestra el LUN seleccionado actualmente (si existe). Si se especifica un índice de LUN no válido, no se produce ningún LUN seleccionado. Al seleccionar un LUN, se anula la selección de los controladores, los puertos de controlador, las unidades, los portales de destino, los destinos y los grupos de portales de destino seleccionados. |
| Portal`[<n>]` | Establece el foco en el portal de destino iSCSI especificado dentro del subsistema seleccionado actualmente. Si no se especifica ningún portal de destino, el comando muestra el portal de destino seleccionado actualmente (si existe). Si se especifica un índice de portal de destino no válido, no se selecciona ningún portal de destino. Al seleccionar un portal de destino, se anula la selección de los controladores, los puertos de controlador, las unidades, los LUN, los destinos y los grupos de portal de destino. |
| dirigir`[<n>]` | Establece el foco en el destino iSCSI especificado en el subsistema seleccionado actualmente. Si no se especifica ningún destino, el comando muestra el destino seleccionado actualmente (si existe). Si se especifica un índice de destino no válido, no se selecciona ningún destino. Al seleccionar un destino, se anula la selección de los controladores, los puertos del controlador, las unidades, los LUN, los portales de destino y los grupos del portal de destino. |
| tpgroup`[<n>]` | Establece el foco en el grupo de portal de destino iSCSI especificado dentro del destino iSCSI seleccionado actualmente. Si no se especifica ningún grupo de portal de destino, el comando muestra el grupo de portal de destino seleccionado actualmente (si existe). Si se especifica un índice de grupo de portal de destino no válido, se producirá un grupo de portal de destino sin foco. |
|`[<n>]` | Especifica el `<object number>` que se va a seleccionar. Si el objeto `<object number>` especificado no es válido, se borran las selecciones existentes para los objetos del tipo especificado. Si no `<object number>` se especifica, se muestra el objeto actual.

### <a name="setflag"></a>setflag

Establece la unidad seleccionada actualmente como reserva activa. Las reservas activas no se pueden usar para operaciones de enlace de LUN normales. Solo se reservan para el control de errores. La unidad no debe estar enlazada actualmente a ningún LUN existente.

#### <a name="syntax"></a>Sintaxis

```
setflag drive hotspare={true | false}
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| true | Selecciona la unidad seleccionada actualmente como reserva activa. |
| false | Anula la selección de la unidad seleccionada actualmente como reserva activa. |

### <a name="shrink"></a>shrink

Reduce el tamaño del LUN seleccionado.

#### <a name="syntax"></a>Sintaxis

```
shrink lun size=<n> [noerr]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| tamaño | Especifica la cantidad deseada de espacio en megabytes (MB) para reducir el tamaño del LUN. Para especificar el tamaño con otras unidades, use uno de los siguientes sufijos reconocidos inmediatamente después del tamaño:<ul><li>Byte **B**</li><li>**KB** -kilobytes</li><li>**MB** -megabyte</li><li>**GB** -Gigabyte</li><li>**TB** -terabyte</li><li>**PB** -petabyte. |
| noerr | Sólo para scripting. Cuando se encuentra un error, Diskraid continúa procesando comandos como si no se hubiera producido el error. |

### <a name="standby"></a>espera

Cambia el estado de las rutas de acceso al puerto del adaptador de bus host (HBA) seleccionado actualmente en modo de espera.

#### <a name="syntax"></a>Sintaxis

```
standby hbaport
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| hbaport | Cambia el estado de las rutas de acceso al puerto del adaptador de bus host (HBA) seleccionado actualmente en modo de espera. |

### <a name="unmask"></a>Mask

Hace que los LUN seleccionados actualmente sean accesibles desde los hosts especificados.

#### <a name="syntax"></a>Sintaxis

```
unmask lun {all | none | [add] wwn=<hexadecimal_number> [;<hexadecimal_number> [;…]] | [add] initiator=<initiator>[;<initiator>[;…]]} [uninstall]
```

##### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| todo | Especifica que el LUN debe ser accesible desde todos los hosts. Sin embargo, no se puede quitar la máscara del LUN a todos los destinos de un subsistema iSCSI.<P>Debe cerrar sesión en el destino antes de ejecutar el `unmask lun all` comando. |
| ninguno | Especifica que el LUN no debe ser accesible para ningún host.<P>Debe cerrar sesión en el destino antes de ejecutar el `unmask lun none` comando. |
| add | Especifica que los hosts especificados se deben agregar a la lista de hosts existente desde la que se puede tener acceso a este LUN. Si no se especifica este parámetro, la lista de hosts proporcionada reemplaza la lista existente de hosts de los que se puede tener acceso a este LUN. |
| WWN = | Especifica una lista de números hexadecimales que representan los nombres de todo el mundo a partir del cual se debe hacer accesible el LUN o los hosts. Para enmascarar o desenmascarar un conjunto específico de hosts en un subsistema Canal de fibra, puede escribir una lista de WWN separada por punto y coma para los puertos de los equipos host de interés. |
| iniciador = | Especifica una lista de iniciadores iSCSI a los que se debe hacer accesible el LUN seleccionado actualmente. Para enmascarar o desenmascarar un conjunto específico de hosts en un subsistema iSCSI, puede escribir una lista separada por punto y coma de nombres de iniciador iSCSI para los iniciadores en los equipos host de interés. |
| uninstall | Si se especifica, desinstala el disco asociado con el LUN en el sistema local antes de enmascarar el LUN. |

## <a name="scripting-diskraid"></a>Scripting de Diskraid

Diskraid se puede incluir en un script en cualquier equipo que ejecute una versión compatible de Windows Server, con un proveedor de hardware de VDS asociado. Para invocar un script de Diskraid, en el símbolo del sistema, escriba:

```
diskraid /s <script.txt>
```

De forma predeterminada, Diskraid detiene el procesamiento de comandos y devuelve un código de error si hay un problema en el script. Para seguir ejecutando el script y omitir los errores, incluya el parámetro **Noerr** en el comando. Esto permite procedimientos útiles como el uso de un único script para eliminar todos los LUN de un subsistema, independientemente del número total de LUN. No todos los comandos admiten el parámetro **Noerr** . Los errores se devuelven siempre en los errores de sintaxis de comandos, independientemente de si se incluyó el parámetro **Noerr** .

## <a name="diskraid-error-codes"></a>Códigos de error de Diskraid

| Código de error | Descripción del error |
| ---------- | ----------------- |
| 0 | No se ha producido ningún error. El script completo se ejecutó sin errores. |
| 1 | Excepción grave. |
| 2 | Los argumentos especificados en una línea de comandos de Diskraid no eran correctos. |
| 3 | Diskraid no pudo abrir el script o el archivo de salida especificado. |
| 4 | Uno de los servicios que usa Diskraid devolvió un error. |
| 5 | Error de sintaxis de comando. El script produjo un error porque un objeto se seleccionó incorrectamente o no era válido para su uso con dicho comando. |

## <a name="example"></a>Ejemplo

Para ver el estado del subsistema 0 en el equipo, escriba:

```
diskraid
```

Presione entrar y se muestra una salida similar a la siguiente:

```
Microsoft Diskraid version 5.2.xxxx
Copyright (©) 2003 Microsoft Corporation
On computer: COMPUTER_NAME
```

Para seleccionar Subsystem 0, escriba lo siguiente en el símbolo del sistema de Diskraid:

```
select subsystem 0
```

Presione entrar y se muestra una salida similar a la siguiente:

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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)