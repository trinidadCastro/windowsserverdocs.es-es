---
title: dnscmd
description: Artículo de referencia para el comando DNSCmd, que es una interfaz de línea de comandos para administrar servidores DNS.
ms.topic: reference
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a804965efaa135d366b62fe2379ad7d9c3d17e5c
ms.sourcegitcommit: 8e330f9066097451cd40e840d5f5c3317cbc16c2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2020
ms.locfileid: "97696924"
---
# <a name="dnscmd"></a>Dnscmd

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Interfaz de línea de comandos para administrar servidores DNS. Esta utilidad es útil en los archivos por lotes de scripting para ayudar a automatizar las tareas de administración de DNS rutinarias, o para realizar una instalación desatendida sencilla y una configuración de nuevos servidores DNS en la red.

## <a name="syntax"></a>Sintaxis

```
dnscmd <servername> <command> [<command parameters>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Description |
| --------- | ----------- |
| `<servername>` | La dirección IP o el nombre de host de un servidor DNS local o remoto. |

## <a name="dnscmd-ageallrecords-command"></a>comando DNSCmd/ageallrecords

Establece la hora actual de una marca de tiempo en los registros de recursos de una zona o nodo especificado en un servidor DNS.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /ageallrecords <zonename>[<nodename>] | [/tree]|[/f]
```

#### <a name="parameters"></a>Parámetros

| Parámetro | Description |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que el administrador tiene previsto administrar, representado por la dirección IP, el nombre de dominio completo (FQDN) o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el FQDN de la zona. |
| `<nodename>` | Especifica un nodo o subárbol específico de la zona, mediante lo siguiente:<ul><li>**@** para la zona raíz o el FQDN</li><li>El FQDN de un nodo (el nombre con un punto (.) al final)</li><li>Una sola etiqueta para el nombre relativo a la raíz de la zona.</li></ul> |
| /tree | Especifica que todos los nodos secundarios también reciben la marca de tiempo. |
| /f | Ejecuta el comando sin solicitar confirmación. |

##### <a name="remarks"></a>Comentarios

- El comando **ageallrecords** es para la compatibilidad con versiones anteriores de DNS y versiones anteriores de DNS en las que no se admitía la detección y eliminación de registros obsoletos. Agrega una marca de tiempo con la hora actual a los registros de recursos que no tienen una marca de tiempo y establece la hora actual en los registros de recursos que tienen una marca de tiempo.

- La limpieza de registros no se realiza a menos que los registros tengan marca de tiempo. Los registros de recursos del servidor de nombres (NS), los registros de recursos de inicio de autoridad (SOA) y los registros de recursos del servicio de nombres Internet de Windows (WINS) no se incluyen en el proceso de eliminación de registros obsoletos, y no se marcan de tiempo incluso cuando se ejecuta el comando **ageallrecords** .

- Este comando produce un error a menos que esté habilitada la eliminación de registros obsoletos para el servidor DNS y la zona. Para obtener información sobre cómo habilitar la eliminación de registros obsoletos de la zona, vea el parámetro **Aging** en la sintaxis del `dnscmd /config` comando de este artículo.

- La adición de una marca de tiempo a los registros de recursos DNS hace que sean incompatibles con los servidores DNS que se ejecutan en sistemas operativos distintos de Windows Server. No se puede revertir una marca de tiempo agregada mediante el comando **ageallrecords** .

- Si no se especifica ninguno de los parámetros opcionales, el comando devuelve todos los registros de recursos en el nodo especificado. Si se especifica un valor para al menos uno de los parámetros opcionales, **DNSCmd** solo enumera los registros de recursos correspondientes al valor o los valores que se especifican en el parámetro o los parámetros opcionales.

### <a name="examples"></a>Ejemplos

[Ejemplo 1: establecer la hora actual de una marca de tiempo en registros de recursos](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-1-set-the-current-time-on-a-time-stamp-to-resource-records)

## <a name="dnscmd-clearcache-command"></a>comando DNSCmd/clearcache

Borra la memoria caché de DNS de los registros de recursos en el servidor DNS especificado.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /clearcache
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |

### <a name="example"></a>Ejemplo

```
dnscmd dnssvr1.contoso.com /clearcache
```

## <a name="dnscmd-config-command"></a>comando DNSCmd/config

Cambia los valores del registro para el servidor DNS y las zonas individuales. Este comando también modifica la configuración del servidor especificado. Acepta valores de nivel de servidor y de nivel de zona.

> [!CAUTION]
> No edite el registro directamente a menos que no tenga ninguna alternativa. El editor del registro omite las medidas de seguridad estándar, lo que permite valores que pueden degradar el rendimiento, dañar el sistema o incluso requerir que se vuelva a instalar Windows. Puede modificar la mayoría de la configuración del registro de forma segura mediante los programas del panel de control o Microsoft Management Console (MMC). Si debe modificar el registro directamente, haga una copia de seguridad en primer lugar. Para obtener más información, consulte la ayuda del editor del registro.

### <a name="server-level-syntax"></a>Sintaxis de nivel de servidor

```
dnscmd [<servername>] /config <parameter>
```

#### <a name="parameters"></a>Parámetros

> [!NOTE]
> Este artículo contiene referencias al término esclavo, un término que Microsoft ya no usa. Cuando se quite el término del software, se quitará también del artículo.

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la sintaxis del equipo local, la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<parameter>` | Especifique una configuración y, como opción, un valor. Los valores de parámetro usan esta sintaxis: *parámetro* [*valor*]. |
| /addressanswerlimit`[0|5-28]` | Especifica el número máximo de registros de host que un servidor DNS puede enviar como respuesta a una consulta. El valor puede ser cero (0) o puede estar en el intervalo de 5 a 28 registros. El valor predeterminado es cero (0). |
| /bindsecondaries`[0|1]` | Cambia el formato de la transferencia de zona para que pueda lograr la máxima compresión y eficacia. Acepta los valores:<ul><li>**0** : usa la compresión máxima y es compatible con las versiones de BIND 4.9.4 y versiones posteriores.</li><li>**1** -envía solo un registro de recursos por mensaje a servidores DNS que no son de Microsoft y es compatible con las versiones de BIND anteriores a la 4.9.4. Esta es la configuración predeterminada.</li></ul> |
| /bootmethod`[0|1|2|3]` | Determina el origen desde el que el servidor DNS obtiene la información de configuración. Acepta los valores:<ul><li>**0** : borra el origen de la información de configuración.</li><li>**1** : carga desde el archivo de enlace que se encuentra en el directorio DNS, que es de `%systemroot%\System32\DNS` forma predeterminada.</li><li>**2** : carga desde el registro.</li><li>**3** -carga desde AD DS y el registro. Esta es la configuración predeterminada.</li></ul> |
| /defaultagingstate`[0|1]` | Determina si la característica de eliminación de registros obsoletos de DNS está habilitada de forma predeterminada en las zonas recién creadas. Acepta los valores:<ul><li>**0** : deshabilita la eliminación de registros obsoletos. Esta es la configuración predeterminada.</li><li>**1** : habilita la eliminación de registros obsoletos.</li></ul> |
| /defaultnorefreshinterval`[0x1-0xFFFFFFFF|0xA8]` | Establece un período de tiempo en el que no se aceptan actualizaciones para los registros actualizados dinámicamente. Las zonas del servidor heredan este valor automáticamente.<p>Para cambiar el valor predeterminado, escriba un valor en el intervalo de **0x1-0xFFFFFFFF**. El valor predeterminado del servidor es **0xA8**. |
| /defaultrefreshinterval `[0x1-0xFFFFFFFF|0xA8]` | Establece un período de tiempo que se permite para las actualizaciones dinámicas en registros DNS. Las zonas del servidor heredan este valor automáticamente.<p>Para cambiar el valor predeterminado, escriba un valor en el intervalo de **0x1-0xFFFFFFFF**. El valor predeterminado del servidor es **0xA8**. |
| /disableautoreversezones `[0|1]` | Habilita o deshabilita la creación automática de zonas de búsqueda inversa. Las zonas de búsqueda inversa proporcionan la resolución de direcciones de protocolo de Internet (IP) a nombres de dominio DNS. Acepta los valores:<ul><li>**0** : habilita la creación automática de zonas de búsqueda inversa. Esta es la configuración predeterminada.</li><li>**1** : deshabilita la creación automática de zonas de búsqueda inversa.</li></ul> |
| /disablensrecordsautocreation `[0|1]` | Especifica si el servidor DNS crea automáticamente registros de recursos del servidor de nombres (NS) para las zonas que hospeda. Acepta los valores:<ul><li>**0** : crea automáticamente registros de recursos del servidor de nombres (NS) para las zonas que hospeda el servidor DNS.</li><li>**1** : no crea automáticamente registros de recursos del servidor de nombres (NS) para las zonas que hospeda el servidor DNS.</li></ul> |
| /dspollinginterval `[0-30]` | Especifica la frecuencia con que el servidor DNS sondea AD DS para buscar cambios en zonas integradas de Active Directory. |
| /dstombstoneinterval `[1-30]` |Cantidad de tiempo en segundos que se conservarán los registros eliminados en AD DS. |
| /ednscachetimeout `[3600-15724800]` | Especifica el número de segundos que se almacena en caché la información de DNS extendido (EDNS). El valor mínimo es **3600** y el valor máximo es **15.724.800**. El valor predeterminado es **604.800** segundos (una semana). |
| /enableednsprobes `[0|1]` | Habilita o deshabilita el servidor para sondear a otros servidores a fin de determinar si admiten EDNS. Acepta los valores:<ul><li>**0** : deshabilita la compatibilidad activa para los sondeos de EDNS.</li><li>**1** : habilita la compatibilidad activa para sondeos de EDNS.</li></ul> |
| /enablednssec `[0|1]` | Habilita o deshabilita la compatibilidad con las extensiones de seguridad DNS (DNSSEC). Acepta los valores:<ul><li>**0** : deshabilita DNSSEC.</li><li>**1** : habilita DNSSEC.</li></ul> |
| /enableglobalnamessupport `[0|1]` | Habilita o deshabilita la compatibilidad con la zona GlobalNames. La zona GlobalNames admite la resolución de nombres DNS de etiqueta única en un bosque. Acepta los valores:<ul><li>**0** : deshabilita la compatibilidad con la zona GlobalNames. Cuando el valor de este comando se establece en 0, el servicio servidor DNS no resuelve los nombres de etiqueta única en la zona GlobalNames.</li><li>**1** : habilita la compatibilidad con la zona GlobalNames. Al establecer el valor de este comando en 1, el servicio servidor DNS resuelve los nombres de etiqueta única en la zona GlobalNames.</li></ul> |
| /enableglobalqueryblocklist `[0|1]` | Habilita o deshabilita la compatibilidad con la lista global de consultas bloqueadas que bloquea la resolución de nombres de los nombres de la lista. El servicio servidor DNS crea y habilita la lista global de consultas bloqueadas de forma predeterminada cuando el servicio se inicia por primera vez. Para ver la lista global de consultas bloqueadas actualmente, use el comando DNSCmd/info **/GlobalQueryBlockList** . Acepta los valores:<ul><li>**0** : deshabilita la compatibilidad con la lista global de consultas bloqueadas. Cuando el valor de este comando se establece en 0, el servicio servidor DNS responde a las consultas de nombres en la lista de bloques.</li><li>**1** : habilita la compatibilidad con la lista global de consultas bloqueadas. Al establecer el valor de este comando en 1, el servicio servidor DNS no responde a las consultas de los nombres de la lista de bloques.</li></ul> |
| /eventloglevel `[0|1|2|4]` | Determina qué eventos se registran en el registro del servidor DNS en Visor de eventos. Acepta los valores:<ul><li>**0** : no registra ningún evento.</li><li>**1** : solo registra errores.</li><li>**2** -solo registra errores y advertencias.</li><li>**4** : registra los errores, las advertencias y los eventos informativos. Esta es la configuración predeterminada.</li></ul> |
| /forwarddelegations `[0|1]` | Determina cómo controla el servidor DNS una consulta para una subzona delegada. Estas consultas se pueden enviar a la subzona a la que se hace referencia en la consulta o a la lista de reenviadores que se denomina para el servidor DNS. Las entradas de la configuración solo se usan cuando está habilitado el reenvío. Acepta los valores:<ul><li>**0** : envía automáticamente las consultas que hacen referencia a las subzonas delegadas a la subzona adecuada. Esta es la configuración predeterminada.</li><li>**1** -reenvía las consultas que hacen referencia a la subzona delegada a los reenviadores existentes.</li></ul> |
| /forwardingtimeout `[<seconds>]` | Determina el número de segundos (**0x1-0xFFFFFFFF**) que un servidor DNS espera a que un reenviador responda antes de intentar otro reenviador. El valor predeterminado es **0X5**, que es 5 segundos. |
| /globalneamesqueryorder `[0|1]` | Especifica si el servicio servidor DNS busca primero en la zona GlobalNames o en las zonas locales cuando resuelve los nombres. Acepta los valores:<ul><li>**0** -el servicio servidor DNS intenta resolver nombres consultando la zona GlobalNames antes de consultar las zonas para las que es autoritativo.</li><li>**1** -el servicio servidor DNS intenta resolver nombres mediante la consulta de las zonas para las que es autoritativo antes de consultar la zona GlobalNames.</li></ul> |
| /globalqueryblocklist`[[<name> [<name>]...]` | Reemplaza la lista global de consultas bloqueadas actual por una lista de los nombres que especifique. Si no especifica ningún nombre, este comando borra la lista de bloques. De forma predeterminada, la lista global de consultas bloqueadas contiene los siguientes elementos:<ul><li>ISATAP</li><li>WPAD</li></ul>El servicio servidor DNS puede quitar uno o ambos nombres cuando se inicia por primera vez, si encuentra estos nombres en una zona existente. |
| /isslave `[0|1]` | Determina cómo responde el servidor DNS cuando las consultas que reenvía no reciben ninguna respuesta. Acepta los valores:<ul><li>**0** : especifica que el servidor DNS no es un subordinado. Si el reenviador no responde, el servidor DNS intenta resolver la propia consulta. Esta es la configuración predeterminada.</li><li>**1** : especifica que el servidor DNS es un subordinado. Si el reenviador no responde, el servidor DNS finaliza la búsqueda y envía un mensaje de error al solucionador.</li></ul> |
| /localnetpriority `[0|1]` | Determina el orden en el que se devuelven los registros de host cuando el servidor DNS tiene varios registros de host para el mismo nombre. Acepta los valores:<ul><li>**0** -devuelve los registros en el orden en que aparecen en la base de datos DNS.</li><li>**1** -devuelve los registros que tienen direcciones de red IP similares en primer lugar. Esta es la configuración predeterminada.</li></ul> |
| /logfilemaxsize `[<size>]` | Especifica el tamaño máximo en bytes (**0x10000-0xFFFFFFFF**) del archivo DNS. log. Cuando el archivo alcanza su tamaño máximo, DNS sobrescribe los eventos más antiguos. El tamaño predeterminado es **0x400000**, que es 4 megabytes (MB). |
| /logFilePath `[<path+logfilename>]` | Especifica la ruta de acceso del archivo DNS. log. La ruta de acceso predeterminada es `%systemroot%\System32\Dns\Dns.log`. Puede especificar una ruta de acceso diferente con el formato `path+logfilename` . |
| /logipfilterlist `<IPaddress> [,<IPaddress>...]` | Especifica los paquetes que se registran en el archivo de registro de depuración. Las entradas son una lista de direcciones IP. Solo se registran los paquetes que van hacia y desde las direcciones IP de la lista. |
| /loglevel `[<eventtype>]` | Determina qué tipos de eventos se registran en el archivo DNS. log. Cada tipo de evento se representa mediante un número hexadecimal. Si desea más de un evento en el registro, use la suma hexadecimal para agregar los valores y, a continuación, escriba la suma. Acepta los valores:<ul><li>**0X0** : el servidor DNS no crea un registro. Esta es la entrada predeterminada.</li><li>**0x10** : registra consultas y notificaciones.</li><li>**0x20** : registra las actualizaciones.</li><li>**0xFE** : registra transacciones que no son de consulta.</li><li>**0x100** : registra las transacciones de preguntas.</li><li>**0x200** : registra las respuestas.</li><li>**0x1000** : los registros envían paquetes.</li><li>**0x2000** : los registros reciben paquetes.</li><li>**0x4000** : registra paquetes UDP (Protocolo de datagramas de usuario).</li><li>**0x8000** : registra los paquetes del Protocolo de control de transmisión (TCP).</li><li>**0xFFFF** : registra todos los paquetes.</li><li>**0x10000** : registra las transacciones de escritura de Active Directory.</li><li>**0x20000** : registra las transacciones de actualización de Active Directory.</li><li>**0x1000000** : registra los paquetes completos.</li><li>**0x80000000** : registra transacciones de escritura a través.</li><li></ul> |
| /maxcachesize | Especifica el tamaño máximo, en kilobytes (KB), de la memoria caché del servidor DNS. |
| /maxcachettl `[<seconds>]` | Determina cuántos segundos (**0X0-0xFFFFFFFF**) se guarda un registro en la memoria caché. Si se usa la opción **0X0** , el servidor DNS no almacena en caché los registros. La configuración predeterminada es **0x15180** (86.400 segundos o 1 día). |
| /maxnegativecachettl `[<seconds>]` | Especifica el número de segundos (**0x1-0xFFFFFFFF**) que una entrada que registra una respuesta negativa en una consulta permanece almacenada en la memoria caché de DNS. La configuración predeterminada es **0x384** (900 segundos). |
| /namecheckflag `[0|1|2|3]` | Especifica el estándar de caracteres que se utiliza al comprobar los nombres DNS. Acepta los valores:<ul><li>**0** : usa caracteres ANSI que cumplen con la solicitud de comentarios (RFC) de Internet Engineering Task Force (IETF).</li><li>**1** : usa caracteres ANSI que no cumplen necesariamente las RFC de IETF.</li><li>**2** -usa caracteres de formato de transformación UCS 8 (UTF-8) multibyte. Esta es la configuración predeterminada.</li><li>**3** -usa todos los caracteres.</li></ul> |
| /norecursion `[0|1]` | Determina si un servidor DNS realiza la resolución de nombres recursivos. Acepta los valores:<ul><li>**0** : el servidor DNS realiza la resolución de nombres recursivo si se solicita en una consulta. Esta es la configuración predeterminada.</li><li>**1** : el servidor DNS no realiza la resolución de nombres recursivo.</li></ul> |
| /notcp | Este parámetro está obsoleto y no tiene ningún efecto en las versiones actuales de Windows Server. |
| /recursionretry `[<seconds>]` | Determina el número de segundos (**0x1-0xFFFFFFFF**) que un servidor DNS espera antes de volver a intentar ponerse en contacto con un servidor remoto. El valor predeterminado es **0X3** (tres segundos). Este valor debe aumentarse cuando se produce la recursividad a través de un vínculo de red de área extensa (WAN) lento. |
| /recursiontimeout `[<seconds>]` | Determina el número de segundos (**0x1-0xFFFFFFFF**) que un servidor DNS espera antes de que se insiga intentando ponerse en contacto con un servidor remoto. La configuración oscila entre **0x1** y **0xFFFFFFFF**. La configuración predeterminada es **0xF** (15 segundos). Este valor debe aumentarse cuando se produce la recursividad a través de un vínculo WAN lento. |
| /roundrobin `[0|1]` | Determina el orden en el que se devuelven los registros de host cuando un servidor tiene varios registros de host para el mismo nombre. Acepta los valores:<ul><li>**0** : el servidor DNS no usa Round Robin. En su lugar, devuelve el primer registro en cada consulta.</li><li>**1** -el servidor DNS gira entre los registros que devuelve desde la parte superior a la parte inferior de la lista de registros coincidentes. Esta es la configuración predeterminada.</li></ul> |
| /rpcprotocol `[0x0|0x1|0x2|0x4|0xFFFFFFFF]` | Especifica el protocolo que usa la llamada a procedimiento remoto (RPC) cuando realiza una conexión desde el servidor DNS. Acepta los valores:<ul><li>**0X0** : deshabilita RPC para DNS.</li><li>**0x01** : usa TCP/IP</li><li>**0X2** : usa canalizaciones con nombre.</li><li>**0x4** : usa la llamada a procedimiento local (LPC).</li><li>**0xFFFFFFFF** : todos los protocolos. Esta es la configuración predeterminada.</li></ul> |
| /scavenginginterval `[<hours>]` | Determina si la característica de eliminación de registros obsoletos para el servidor DNS está habilitada y establece el número de horas (**0X0-0xFFFFFFFF**) entre los ciclos de eliminación de registros obsoletos. La configuración predeterminada es **0X0**, que deshabilita la eliminación de registros obsoletos para el servidor DNS. Un valor mayor que **0X0** habilita la eliminación de registros obsoletos para el servidor y establece el número de horas entre los ciclos de eliminación de registros obsoletos. |
| /secureresponses `[0|1]` | Determina si DNS filtra los registros que se guardan en una memoria caché. Acepta los valores:<ul><li>**0** : guarda todas las respuestas a las consultas de nombres en una memoria caché. Esta es la configuración predeterminada.</li><li>**1** : guarda solo los registros que pertenecen al mismo subárbol DNS en una memoria caché.</li></ul> |
| /sendport `[<port>]` | Especifica el número de puerto (**0X0-0xFFFFFFFF**) que DNS usa para enviar consultas recursivas a otros servidores DNS. La configuración predeterminada es **0X0**, lo que significa que el número de puerto se selecciona aleatoriamente. |
| /serverlevelplugindll`[<dllpath>]` | Especifica la ruta de acceso de un complemento personalizado. Cuando Dllpath especifica el nombre completo de la ruta de acceso de un complemento de servidor DNS válido, el servidor DNS llama a las funciones del complemento para resolver las consultas de nombres que están fuera del ámbito de todas las zonas hospedadas localmente. Si un nombre consultado está fuera del ámbito del complemento, el servidor DNS realiza la resolución de nombres mediante reenvío o recursividad, tal y como se ha configurado. Si no se especifica Dllpath, el servidor DNS deja de usar un complemento personalizado si se configuró previamente un complemento personalizado. |
| /strictfileparsing `[0|1]` | Determina el comportamiento de un servidor DNS cuando encuentra un registro erróneo mientras carga una zona. Acepta los valores:<ul><li>**0** : el servidor DNS continúa cargando la zona aunque el servidor encuentre un registro erróneo. El error se registra en el registro de DNS. Esta es la configuración predeterminada.</li><li>**1** : el servidor DNS deja de cargar la zona y registra el error en el registro de DNS.</li></ul> |
| /updateoptions `<RecordValue>` | Prohíbe las actualizaciones dinámicas de los tipos de registros especificados. Si desea que se prohíba más de un tipo de registro en el registro, use la adición hexadecimal para agregar los valores y, a continuación, escriba la suma. Acepta los valores:<ul><li>**0X0** : no restringe ningún tipo de registro.</li><li>**0x1** : excluye los registros de recursos de inicio de autoridad (SOA).</li><li>**0X2** -excluye los registros de recursos del servidor de nombres (NS).</li><li>**0x4** : excluye la delegación de registros de recursos del servidor de nombres (NS).</li><li>**0x8** : excluye los registros de host de servidor.</li><li>**0x100** : durante la actualización dinámica segura, excluye los registros de recursos de inicio de autoridad (SOA).</li><li>**0x200** : durante la actualización dinámica segura, excluye los registros de recursos del servidor de nombres raíz (NS).</li><li>**0x30F** : durante la actualización dinámica estándar, excluye los registros de recursos del servidor de nombres (NS), los registros de recursos de inicio de autoridad (SOA) y los registros de host de servidor. Durante la actualización dinámica segura, excluye los registros de recursos del servidor de nombres raíz (NS) y los registros de recursos de inicio de autoridad (SOA). Permite las delegaciones y las actualizaciones del host de servidor.</li><li>**0x400** : durante la actualización dinámica segura, excluye los registros de recursos del servidor de nombres de delegación (NS).</li><li>**0x800** : durante la actualización dinámica segura, excluye los registros de host de servidor.</li><li>**0x1000000** : excluye los registros del firmante de delegación (DS).</li><li>**0x80000000** : deshabilita la actualización dinámica de DNS.</li></ul> |
| /writeauthorityns `[0|1]` | Determina cuándo el servidor DNS escribe los registros de recursos del servidor de nombres (NS) en la sección de autoridad de una respuesta. Acepta los valores:<ul><li>**0** : escribe los registros de recursos del servidor de nombres (NS) en la sección autoridad de las referencias. Esta configuración cumple con RFC 1034, los conceptos y las instalaciones de los nombres de dominio, y con RFC 2181, aclaraciones de la especificación de DNS. Esta es la configuración predeterminada.</li><li>**1** : escribe los registros de recursos del servidor de nombres (NS) en la sección autoridad de todas las respuestas autoritativas correctas.</li></ul> |
| /xfrconnecttimeout `[<seconds>]` | Determina el número de segundos (**0X0-0xFFFFFFFF**) que un servidor DNS principal espera una respuesta de transferencia de su servidor secundario. El valor predeterminado es **0x1E** (30 segundos). Después de que expire el valor de tiempo de espera, se finaliza la conexión. |

### <a name="zone-level-syntax"></a>Sintaxis de nivel de zona

Modifica la configuración de la zona especificada. El nombre de la zona solo se debe especificar para los parámetros de nivel de zona.

```
dnscmd /config <parameters>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<parameter>` | Especifique una configuración, un nombre de zona y, como opción, un valor. Los valores de parámetro usan esta sintaxis: `zonename parameter [value]` . |
| /Aging `<zonename>`| Habilita o deshabilita la eliminación de registros obsoletos en una zona específica. |
| /allownsrecordsautocreation `<zonename>``[value]` | Invalida la configuración de creación de registros de recursos del servidor de nombres (NS) del servidor DNS. Los registros de recursos del servidor de nombres (NS) que se registraron previamente para esta zona no se ven afectados. Por lo tanto, debe quitarlos manualmente si no los desea. |
| /AllowUpdate `<zonename>` | Determina si la zona especificada acepta actualizaciones dinámicas. |
| /forwarderslave `<zonename>` | Invalida el valor **/isslave** del servidor DNS. |
| /forwardertimeout `<zonename>` | Determina el número de segundos que una zona DNS espera a que un reenviador responda antes de intentar otro reenviador. Este valor invalida el valor que se establece en el nivel de servidor. |
| /norefreshinterval `<zonename>` | Establece un intervalo de tiempo para una zona durante la cual ninguna actualización puede actualizar dinámicamente los registros DNS de una zona especificada. |
| /refreshinterval `<zonename>` | Establece un intervalo de tiempo para una zona durante la cual las actualizaciones pueden actualizar dinámicamente los registros DNS de una zona especificada. |
| /securesecondaries `<zonename>` | Determina qué servidores secundarios pueden recibir actualizaciones de zona del servidor principal para esta zona. |

## <a name="dnscmd-createbuiltindirectorypartitions-command"></a>comando DNSCmd/createbuiltindirectorypartitions

Crea una partición del directorio de aplicaciones DNS. Cuando se instala DNS, se crea una partición de directorio de aplicaciones para el servicio en los niveles de bosque y dominio. Use este comando para crear particiones de directorio de aplicaciones DNS que se han eliminado o que no se han creado nunca. Sin ningún parámetro, este comando crea una partición de directorio DNS integrada para el dominio.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /createbuiltindirectorypartitions [/forest] [/alldomains]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| /Forest | Crea una partición de directorio DNS para el bosque. |
| /alldomains | Crea particiones DNS para todos los dominios del bosque. |

## <a name="dnscmd-createdirectorypartition-command"></a>comando DNSCmd/createdirectorypartition

Crea una partición del directorio de aplicaciones DNS. Cuando se instala DNS, se crea una partición de directorio de aplicaciones para el servicio en los niveles de bosque y dominio. Esta operación crea particiones de directorio de aplicaciones DNS adicionales.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /createdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<partitionFQDN>` | El FQDN de la partición de directorio de aplicaciones DNS que se creará. |

## <a name="dnscmd-deletedirectorypartition-command"></a>comando DNSCmd/deletedirectorypartition

Quita una partición de directorio de aplicaciones DNS existente.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /deletedirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<partitionFQDN>` | El FQDN de la partición de directorio de aplicaciones DNS que se va a quitar. |

## <a name="dnscmd-directorypartitioninfo-command"></a>comando DNSCmd/directorypartitioninfo

Muestra información acerca de una partición de directorio de aplicaciones DNS especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /directorypartitioninfo <partitionFQDN> [/detail]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<partitionFQDN>` | El FQDN de la partición de directorio de aplicaciones DNS. |
| /detail | Muestra toda la información acerca de la partición de directorio de aplicaciones. |

## <a name="dnscmd-enlistdirectorypartition-command"></a>comando DNSCmd/enlistdirectorypartition

Agrega el servidor DNS al conjunto de réplicas de la partición de directorio especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /enlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<partitionFQDN>` | El FQDN de la partición de directorio de aplicaciones DNS. |

## <a name="dnscmd-enumdirectorypartitions-command"></a>comando DNSCmd/enumdirectorypartitions

Enumera las particiones de directorio de aplicaciones DNS para el servidor especificado.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /enumdirectorypartitions [/custom]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| /custom | Muestra solo las particiones de directorio creadas por el usuario. |

## <a name="dnscmd-enumrecords-command"></a>comando DNSCmd/enumrecords

Enumera los registros de recursos de un nodo especificado en una zona DNS.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /enumrecords <zonename> <nodename> [/type <rrtype> <rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<childname>] [/continue | /detail]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| /enumrecords | Enumera los registros de recursos de la zona especificada. |
| `<zonename>` | Especifica el nombre de la zona a la que pertenecen los registros de recursos. |
| `<nodename>` | Especifica el nombre del nodo de los registros de recursos. |
| `[/type <rrtype> <rrdata>]` | Especifica el tipo de registros de recursos que se van a mostrar y el tipo de datos que se espera. Acepta los valores:<ul><li>`<rrtype>` : Especifica el tipo de registros de recursos que se van a mostrar.</li><li>`<rrdata>` : Especifica el tipo de datos que se espera que sea un registro.</li></ul> |
| /authority | Incluye datos autoritativos. |
| /glue | Incluye datos de adherencia. |
| /additional | Incluye toda la información adicional sobre los registros de recursos enumerados. |
| /node | Muestra solo los registros de recursos del nodo especificado. |
| /Child | Muestra solo los registros de recursos de un dominio secundario especificado. |
| /startchild`<childname>` | Inicia la lista en el dominio secundario especificado. |
| /Continue | Muestra solo los registros de recursos con el tipo y los datos. |
| /detail | Muestra toda la información sobre los registros de recursos. |

#### <a name="example"></a>Ejemplo

```
dnscmd /enumrecords test.contoso.com test /additional
```

## <a name="dnscmd-enumzones-command"></a>comando DNSCmd/enumzones

Muestra una lista de las zonas que existen en el servidor DNS especificado. Los parámetros de **enumzones** actúan como filtros en la lista de zonas. Si no se especifica ningún filtro, se devuelve una lista completa de las zonas. Cuando se especifica un filtro, solo se incluyen en la lista de zonas devueltas las zonas que cumplen los criterios del filtro.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <partitionFQDN>]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| /primary | Muestra todas las zonas que son zonas principales estándar o zonas integradas de Active Directory. |
| /secondary | Muestra todas las zonas secundarias estándar. |
| /forwarder | Muestra las zonas que reenvían consultas sin resolver a otro servidor DNS. |
| /stub | Muestra todas las zonas de rutas internas. |
| /cache | Muestra solo las zonas que se cargan en la memoria caché. |
| /auto-created] | Muestra las zonas que se crearon automáticamente durante la instalación del servidor DNS. |
| /zonas | Enumera las zonas de búsqueda directa. |
| /zonas | Muestra las zonas de búsqueda inversa. |
| /ds | Enumera las zonas integradas de Active Directory. |
| /file | Muestra las zonas respaldadas por archivos. |
| /domaindirectorypartition | Muestra las zonas que se almacenan en la partición de directorio de dominio. |
| /forestdirectorypartition | Enumera las zonas que se almacenan en la partición de directorio de aplicaciones DNS de bosque. |
| /customdirectorypartition | Muestra todas las zonas que se almacenan en una partición de directorio de aplicación definida por el usuario. |
| /legacydirectorypartition | Muestra todas las zonas almacenadas en la partición de directorio de dominio. |
| /directorypartition `<partitionFQDN>` | Muestra todas las zonas almacenadas en la partición de directorio especificada. |

#### <a name="examples"></a>Ejemplos

- [Ejemplo 2: mostrar una lista completa de zonas en un servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-2-display-a-complete-list-of-zones-on-a-dns-server))

- [Ejemplo 3: mostrar una lista de zonas creadas en un servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-3-display-a-list-of-autocreated-zones-on-a-dns-server)

## <a name="dnscmd-exportsettings-command"></a>comando DNSCmd/exportsettings

Crea un archivo de texto que enumera los detalles de configuración de un servidor DNS. El archivo de texto se denomina *DnsSettings.txt*. Se encuentra en el `%systemroot%\system32\dns` directorio del servidor. Puede usar la información del archivo que **DNSCmd/exportsettings** crea para solucionar problemas de configuración o para asegurarse de que ha configurado varios servidores de forma idéntica.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /exportsettings
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |

## <a name="dnscmd-info-command"></a>comando DNSCmd/info

Muestra la configuración de la sección DNS del registro del servidor especificado `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters` . Para mostrar la configuración del registro en el nivel de zona, use el `dnscmd zoneinfo` comando.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /info [<settings>]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<settings>` | Cualquier configuración que devuelva el comando **info** se puede especificar individualmente. Si no se especifica un valor, se devuelve un informe de la configuración común. |

#### <a name="example"></a>Ejemplo

- [Ejemplo 4: mostrar el valor de IsSlave desde un servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-4-display-the-isslave-setting-from-a-dns-server)

- [Ejemplo 5: mostrar el valor de RecursionTimeout desde un servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-5-display-the-recursiontimeout-setting-from-a-dns-server)

## <a name="dnscmd-ipvalidate-command"></a>comando DNSCmd/ipvalidate

Comprueba si una dirección IP identifica un servidor DNS en funcionamiento o si el servidor DNS puede actuar como reenviador, un servidor de sugerencias raíz o un servidor principal para una zona específica.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /ipvalidate <context> [<zonename>] [[<IPaddress>]]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<context>` | Especifica el tipo de prueba que se va a realizar. Puede especificar cualquiera de las siguientes pruebas:<ul><li>**/dnsservers** : comprueba que los equipos con las direcciones que especifique son servidores DNS que funcionan.</li><li>**/Forwarders** : comprueba que las direcciones que especifique identifican los servidores DNS que pueden actuar como reenviadores.</li><li>**/roothints** : comprueba que las direcciones que especifique identifican los servidores DNS que pueden actuar como servidores de nombres de sugerencia de raíz.</li><li>**/zonemasters** : comprueba que las direcciones que especifique identifican los servidores DNS que son servidores principales de *zonename*. |
| `<zonename>` | Identifica la zona. Use este parámetro con el parámetro **/zonemasters** . |
| `<IPaddress>` | Especifica las direcciones IP que el comando comprueba. |

#### <a name="examples"></a>Ejemplos

```
nscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2
```

## <a name="dnscmd-nodedelete-command"></a>comando DNSCmd/nodedelete

Elimina todos los registros de un host especificado.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /nodedelete <zonename> <nodename> [/tree] [/f]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona. |
| `<nodename>` | Especifica el nombre de host del nodo que se va a eliminar. |
| /tree | Elimina todos los registros secundarios. |
| /f | Ejecuta el comando sin solicitar confirmación. |

#### <a name="example"></a>Ejemplo

[Ejemplo 6: eliminación de los registros de un nodo](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-6-delete-the-records-from-a-node)

## <a name="dnscmd-recordadd-command"></a>comando DNSCmd/recordadd

Agrega un registro a una zona especificada en un servidor DNS.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /recordadd <zonename> <nodename> <rrtype> <rrdata>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica la zona en la que reside el registro. |
| `<nodename>` | Especifica un nodo específico de la zona. |
| `<rrtype>` | Especifica el tipo de registro que se va a agregar. |
| `<rrdata>` | Especifica el tipo de datos que se espera. |

> [!NOTE]
> Después de agregar un registro, asegúrese de usar el tipo de datos y el formato de datos correctos. Para obtener una lista de tipos de registro de recursos y los tipos de datos adecuados, vea [ejemplos de DNSCmd](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)).

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-recorddelete-command"></a>comando DNSCmd/recorddelete

Elimina un registro de recursos en una zona especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /recorddelete <zonename> <nodename> <rrtype> <rrdata> [/f]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica la zona en la que reside el registro de recursos. |
| `<nodename>` | Especifica el nombre del host. |
| `<rrtype>` | Especifica el tipo de registro de recursos que se va a eliminar. |
| `<rrdata>` | Especifica el tipo de datos que se espera. |
| /f | Ejecuta el comando sin solicitar confirmación. Dado que los nodos pueden tener más de un registro de recursos, este comando requiere que sea muy específico sobre el tipo de registro de recursos que desea eliminar. Si especifica un tipo de datos y no especifica un tipo de datos de registro de recursos, se eliminarán todos los registros con ese tipo de datos específico para el nodo especificado. |

#### <a name="examples"></a>Ejemplos

```
dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com
```

## <a name="dnscmd-resetforwarders-command"></a>comando DNSCmd/resetforwarders

Selecciona o restablece las direcciones IP a las que el servidor DNS reenvía las consultas DNS cuando no puede resolverlas de forma local.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /resetforwarders <IPaddress> [,<IPaddress>]...][/timeout <timeout>] [/slave | /noslave]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<IPaddress>` | Muestra las direcciones IP a las que el servidor DNS reenvía las consultas no resueltas. |
| /timeout `<timeout>` | Establece el número de segundos que el servidor DNS espera una respuesta del reenviador. De forma predeterminada, este valor es de cinco segundos. |
| /Slave | Impide que el servidor DNS realice sus propias consultas iterativas si el reenviador no puede resolver una consulta. |
| /noslave | Permite que el servidor DNS realice sus propias consultas iterativas si el reenviador no puede resolver una consulta. Esta es la configuración predeterminada. |
| /f | Ejecuta el comando sin solicitar confirmación. Dado que los nodos pueden tener más de un registro de recursos, este comando requiere que sea muy específico sobre el tipo de registro de recursos que desea eliminar. Si especifica un tipo de datos y no especifica un tipo de datos de registro de recursos, se eliminarán todos los registros con ese tipo de datos específico para el nodo especificado. |

##### <a name="remarks"></a>Comentarios

- De forma predeterminada, un servidor DNS realiza consultas iterativas cuando no puede resolver una consulta.

- La configuración de direcciones IP mediante el comando **resetforwarders** hace que el servidor DNS realice consultas recursivas en los servidores DNS de las direcciones IP especificadas. Si los reenviadores no resuelven la consulta, el servidor DNS puede realizar sus propias consultas iterativas.

- Si se usa el parámetro **/Slave** , el servidor DNS no realiza sus propias consultas iterativas. Esto significa que el servidor DNS reenvía las consultas no resueltas solo a los servidores DNS de la lista y no intenta consultas iterativas si los reenviadores no las resuelven. Es más eficaz establecer una dirección IP como reenviador para un servidor DNS. Puede usar el comando **resetforwarders** para los servidores internos de una red para reenviar sus consultas sin resolver a un servidor DNS que tenga una conexión externa.

- Si se muestra la dirección IP de un reenviador dos veces, el servidor DNS intentará reenviarse al servidor dos veces.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave
dnscmd dnssvr1.contoso.com /resetforwarders /noslave
```

## <a name="dnscmd-resetlistenaddresses-command"></a>comando DNSCmd/resetlistenaddresses

Especifica las direcciones IP de un servidor que escucha las solicitudes de cliente DNS. De forma predeterminada, todas las direcciones IP de un servidor DNS escuchan las solicitudes DNS del cliente.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /resetlistenaddresses <listenaddress>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<listenaddress>` | Especifica una dirección IP en el servidor DNS que escucha las solicitudes de cliente DNS. Si no se especifica ninguna dirección de escucha, todas las direcciones IP del servidor escuchan las solicitudes de cliente. |

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1
```

## <a name="dnscmd-startscavenging-command"></a>comando DNSCmd/startscavenging

Indica a un servidor DNS que intente realizar una búsqueda inmediata de registros de recursos obsoletos en un servidor DNS especificado.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /startscavenging
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |

##### <a name="remarks"></a>Comentarios

- La finalización correcta de este comando inicia una eliminación de registros obsoletos inmediatamente. Si se produce un error en la eliminación de registros obsoletos, no aparece ningún mensaje de advertencia.

- Aunque el comando para iniciar la eliminación de registros obsoletos parece completarse correctamente, la eliminación de registros obsoletos no se inicia a menos que se cumplan las siguientes condiciones previas:

    - La eliminación de registros obsoletos está habilitada tanto para el servidor como para la zona.

    - La zona se ha iniciado.

    - Los registros de recursos tienen una marca de tiempo.

- Para obtener información sobre cómo habilitar la eliminación de registros obsoletos para el servidor, vea el parámetro **scavenginginterval** en **Sintaxis de nivel de servidor** en la sección **/config** .

- Para obtener información sobre cómo habilitar la eliminación de registros obsoletos para la zona, vea el parámetro **Aging** en **Sintaxis de nivel de zona** en la sección **/config** .

- Para obtener información sobre cómo reiniciar una zona en pausa, consulte el parámetro **zoneresume** en este artículo.

- Para obtener información sobre cómo comprobar los registros de recursos para una marca de tiempo, vea el parámetro **ageallrecords** en este artículo.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /startscavenging
```

## <a name="dnscmd-statistics-command"></a>comando DNSCmd/Statistics

Muestra o borra los datos de un servidor DNS especificado.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /statistics [<statid>] [/clear]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<statid>` | Especifica la estadística o combinación de estadísticas que se va a mostrar. El comando **estadísticas** muestra los contadores que comienzan en el servidor DNS cuando se inicia o se reanuda. Se usa un número de identificación para identificar una estadística. Si no se especifica ningún número de identificación de estadística, se mostrarán todas las estadísticas. Los números que se pueden especificar, junto con la estadística correspondiente que se muestra, pueden incluir:<ul><li>**00000001** -hora</li><li>**00000002** : consulta</li><li>**00000004** -consulta 2</li><li>**00000008** -recurse</li><li>**00000010** : Maestro</li><li>**00000020** -secundario</li><li>**00000040** -WINS</li><li>**00000100** : actualización</li><li>**00000200** -SkwanSec</li><li>**00000400** -DS</li><li>**00010000** -memoria</li><li>**00100000** -PacketMem</li><li>**00040000** : dBase</li><li>**00080000** -registros</li><li>**00200000** -NbstatMem</li><li>**/Clear** : restablece el contador de estadísticas especificado en cero.</li></ul> |

#### <a name="examples"></a>Ejemplos

- [Ejemplo 7: ](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-7-display-time-statistics-for-a-dns-server)

- [Ejemplo 8: Mostrar estadísticas de NbstatMem para un servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-8-display-nbstatmem-statistics-for-a-dns-server)

## <a name="dnscmd-unenlistdirectorypartition-command"></a>comando DNSCmd/unenlistdirectorypartition

Quita el servidor DNS del conjunto de réplicas de la partición de directorio especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /unenlistdirectorypartition <partitionFQDN>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<partitionFQDN>` | El FQDN de la partición de directorio de aplicaciones DNS que se va a quitar. |

## <a name="dnscmd-writebackfiles-command"></a>comando DNSCmd/writebackfiles

Comprueba si hay cambios en la memoria del servidor DNS y los escribe en el almacenamiento persistente. El comando **writebackfiles** actualiza todas las zonas desfasadas o una zona especificada. Una zona se ha modificado cuando hay cambios en la memoria que todavía no se han escrito en el almacenamiento persistente. Se trata de una operación de nivel de servidor que comprueba todas las zonas. Puede especificar una zona en esta operación o puede usar la operación **zonewriteback** .

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /writebackfiles <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a actualizar. |

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /writebackfiles
```

## <a name="dnscmd-zoneadd-command"></a>comando DNSCmd/zoneadd

Agrega una zona al servidor DNS.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneadd <zonename> <zonetype> [/dp <FQDN> | {/domain | enterprise | legacy}]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona. |
| `<zonetype>` | Especifica el tipo de zona que se va a crear. Al especificar un tipo de zona de **/Forwarder** o **/dsforwarder** , se crea una zona que realiza el reenvío condicional. Cada tipo de zona tiene parámetros necesarios diferentes:<ul><li>**/DsPrimary** : crea una zona integrada de Active Directory.</li><li>**/Primary/file `<filename>`** -Crea una zona principal estándar y especifica el nombre del archivo que almacenará la información de la zona.</li><li>**/Secondary `<masterIPaddress> [<masterIPaddress>...]`** : Crea una zona secundaria estándar.</li><li>**/stub `<masterIPaddress> [<masterIPaddress>...]` /file `<filename>`** : crea una zona de rutas internas con copia de seguridad de archivos.</li><li>**/DsStub `<masterIPaddress> [<masterIPaddress>...]`** : Crea una zona de rutas internas integrada de Active Directory.</li><li>**/Forwarder `<masterIPaddress> [<masterIPaddress>]` .../file `<filename>`** : especifica que la zona creada reenvía las consultas sin resolver a otro servidor DNS.</li><li>**/dsforwarder** : especifica que la zona integrada de Active Directory que se ha creado reenvía las consultas no resueltas a otro servidor DNS.</li></ul> |
| `<FQDN>` | Especifica el FQDN de la partición de directorio. |
| /Domain | Almacena la zona en la partición de directorio de dominio. |
| /enterprise | Almacena la zona en la partición de directorio de la empresa. |
| /legacy | Almacena la zona en una partición de directorio heredada. |

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zonechangedirectorypartition-command"></a>comando DNSCmd/zonechangedirectorypartition

Cambia la partición de directorio en la que reside la zona especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zonechangedirectorypartition <zonename> {[<newpartitionname>] | [<zonetype>]}
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | El FQDN de la partición de directorio actual en la que reside la zona. |
| `<newpartitionname>` | El FQDN de la partición de directorio a la que se va a desplace la zona. |
| `<zonetype>` | Especifica el tipo de partición de directorio al que se va a desplace la zona. |
| /Domain | Mueve la zona a la partición de directorio de dominio integrada. |
| /Forest | Mueve la zona a la partición de directorio de bosque integrada. |
| /legacy | Mueve la zona a la partición de directorio que se crea para los controladores de dominio anteriores a Active Directory. Estas particiones de directorio no son necesarias para el modo nativo. |

## <a name="dnscmd-zonedelete-command"></a>comando DNSCmd/zonedelete

Elimina una zona especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zonedelete <zonename> [/dsdel] [/f]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a eliminar. |
| /dsdel | Elimina la zona de los servicios de dominio de directorio de Azure (AD DS). |
| /f | Ejecuta el comando sin solicitar confirmación. |

#### <a name="examples"></a>Ejemplos

- [Ejemplo 9: eliminar una zona de un servidor DNS](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-9-delete-a-zone-from-a-dns-server)

## <a name="dnscmd-zoneexport-command"></a>comando DNSCmd/zoneexport

Crea un archivo de texto que enumera los registros de recursos de una zona especificada. La operación **zoneexport** crea un archivo de registros de recursos para una zona integrada de Active Directory con el fin de solucionar problemas. De forma predeterminada, el archivo que crea este comando se coloca en el directorio DNS, que, de forma predeterminada, es el `%systemroot%/System32/Dns` directorio.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneexport <zonename> <zoneexportfile>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona. |
| `<zoneexportfile>` | Especifica el nombre del archivo que se va a crear. |

#### <a name="examples"></a>Ejemplos

- [Ejemplo 10: exportar la lista de registros de recursos de zona a un archivo](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-10-export-zone-resource-records-list-to-a-file)

## <a name="dnscmd-zoneinfo"></a>DNSCmd/zoneinfo

Muestra la configuración de la sección del registro de la zona especificada: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\<zonename>`

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneinfo <zonename> [<setting>]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona. |
| `<setting>` | Puede especificar individualmente cualquier valor que devuelva el comando **zoneinfo** . Si no especifica un valor, se devolverá toda la configuración. |

##### <a name="remarks"></a>Comentarios

- Para mostrar la configuración del registro en el nivel de servidor, use el comando **/info** .

- Para ver una lista de los valores de configuración que puede mostrar con este comando, consulte el comando **/config** .

#### <a name="examples"></a>Ejemplos

- [Ejemplo 11: Mostrar la configuración de RefreshInterval desde el registro](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-11-display-refreshinterval-setting-from-the-registry)

- [Ejemplo 12: Mostrar la configuración de caducidad del registro](/previous-versions/windows/it-pro/windows-server-2003/cc784399(v=ws.10)#example-12-display-aging-setting-from-the-registry)

## <a name="dnscmd-zonepause-command"></a>comando DNSCmd/zonepause

Pausa la zona especificada, que luego omite las solicitudes de consulta.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zonepause <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a pausar. |

##### <a name="remarks"></a>Comentarios

- Para reanudar una zona y ponerla a disposición una vez que se haya pausado, use el comando **/zoneresume** .

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zonepause test.contoso.com
```

## <a name="dnscmd-zoneprint-command"></a>comando DNSCmd/zoneprint

Enumera los registros de una zona.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneprint <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a mostrar. |







## <a name="dnscmd-zonerefresh-command"></a>comando DNSCmd/zonerefresh

Fuerza la actualización de una zona DNS secundaria desde la zona maestra.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zonerefresh <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a actualizar. |

##### <a name="remarks"></a>Comentarios

- El comando **zonerefresh** fuerza una comprobación del número de versión en el registro de recursos de inicio de autoridad (SOA) del servidor principal. Si el número de versión del servidor principal es mayor que el número de versión del servidor secundario, se inicia una transferencia de zona que actualiza el servidor secundario. Si el número de versión es el mismo, no se produce ninguna transferencia de zona.

- La comprobación forzada se realiza de forma predeterminada cada 15 minutos. Para cambiar el valor predeterminado, use el `dnscmd config refreshinterval` comando.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com
```

## <a name="dnscmd-zonereload-command"></a>comando DNSCmd/zonereload

Copia la información de la zona desde su origen.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zonereload <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a recargar. |

##### <a name="remarks"></a>Comentarios

- Si la zona está integrada en Active Directory, se vuelve a cargar desde Active Directory Domain Services (AD DS).

- Si la zona es una zona con copia de seguridad de archivos estándar, se vuelve a cargar desde un archivo.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zonereload test.contoso.com
```

## <a name="dnscmd-zoneresetmasters-command"></a>comando DNSCmd/zoneresetmasters

Restablece las direcciones IP del servidor principal que proporciona información de transferencia de zona a una zona secundaria.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneresetmasters <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a restablecer. |
| /local | Establece una lista maestra local. Este parámetro se usa para las zonas integradas de Active Directory. |
| `<IPaddress>` | Las direcciones IP de los servidores principales de la zona secundaria. |

##### <a name="remarks"></a>Comentarios

- Este valor se establece originalmente cuando se crea la zona secundaria. Use el comando **zoneresetmasters** en el servidor secundario. Este valor no tiene ningún efecto si se establece en el servidor DNS maestro.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local
```

## <a name="dnscmd-zoneresetscavengeservers-command"></a>comando DNSCmd/zoneresetscavengeservers

Cambia las direcciones IP de los servidores que pueden borrar la zona especificada.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneresetscavengeservers <zonename> [/local] [<IPaddress> [<IPaddress>]...]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica la zona que se va a borrar. |
| /local | Establece una lista maestra local. Este parámetro se usa para las zonas integradas de Active Directory. |
| `<IPaddress>` | Muestra las direcciones IP de los servidores que pueden realizar la eliminación de registros obsoletos. Si se omite este parámetro, todos los servidores que hospedan esta zona podrán borrarlo. |

##### <a name="remarks"></a>Comentarios

- De forma predeterminada, todos los servidores que hospedan una zona pueden borrar dicha zona.

- Si una zona se hospeda en más de un servidor DNS, puede usar este comando para reducir el número de veces que se borra una zona.

- La eliminación de registros obsoletos debe estar habilitada en el servidor DNS y en la zona afectada por este comando.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2
```

## <a name="dnscmd-zoneresetsecondaries-command"></a>comando DNSCmd/zoneresetsecondaries

Especifica una lista de direcciones IP de los servidores secundarios a los que responde un servidor principal cuando se le pide una transferencia de zona.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneresetsecondaries <zonename> {/noxfr | /nonsecure | /securens | /securelist <securityIPaddresses>} {/nonotify | /notify | /notifylist <notifyIPaddresses>}
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona en la que se restablecerán los servidores secundarios. |
| /local | Establece una lista maestra local. Este parámetro se usa para las zonas integradas de Active Directory. |
| /noxfr | Especifica que no se permiten las transferencias de zona. |
| /nonsecure | Especifica que se concedan todas las solicitudes de transferencia de zona. |
| /securens | Especifica que solo se conceda una transferencia al servidor que aparece en el registro de recursos de servidor de nombres (NS) para la zona. |
| /securelist | Especifica que las transferencias de zona solo se conceden a la lista de servidores. Este parámetro debe ir seguido de una dirección IP o de las direcciones que usa el servidor principal. |
| `<securityIPaddresses>` | Muestra las direcciones IP que reciben transferencias de zona del servidor principal. Este parámetro solo se usa con el parámetro **/securelist** . |
| /nonotify | Especifica que no se envían notificaciones de cambios a los servidores secundarios. |
| /notify | Especifica que las notificaciones de cambios se envían a todos los servidores secundarios. |
| /notifylist | Especifica que las notificaciones de cambio solo se envían a la lista de servidores. Este comando debe ir seguido de una dirección IP o de las direcciones que usa el servidor principal. |
| `<notifyIPaddresses>` | Especifica la dirección o direcciones IP del servidor secundario o los servidores a los que se envían las notificaciones de cambios. Esta lista solo se usa con el parámetro **/notifylist** . |

##### <a name="remarks"></a>Comentarios

- Use el comando **zoneresetsecondaries** en el servidor principal para especificar cómo responde a las solicitudes de transferencia de zona de los servidores secundarios.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2
```

## <a name="dnscmd-zoneresettype-command"></a>comando DNSCmd/zoneresettype

Cambia el tipo de la zona.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneresettype <zonename> <zonetype> [/overwrite_mem | /overwrite_ds]
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Identifica la zona en la que se cambiará el tipo. |
| `<zonetype>` | Especifica el tipo de zona que se va a crear. Cada tipo tiene parámetros necesarios diferentes, entre los que se incluyen:<ul><li>**/DsPrimary** : crea una zona integrada de Active Directory.</li><li>**/Primary/file `<filename>`** : Crea una zona principal estándar.</li><li>**/Secondary `<masterIPaddress> [,<masterIPaddress>...]`** : Crea una zona secundaria estándar.</li><li>**/stub `<masterIPaddress>[,<masterIPaddress>...]` /file `<filename>`** : crea una zona de rutas internas con copia de seguridad de archivos.</li><li>**/DsStub `<masterIPaddress>[,<masterIPaddress>...]`** : Crea una zona de rutas internas integrada de Active Directory.</li><li>**/Forwarder `<masterIPaddress[,<masterIPaddress>]` .../file `<filename>`** : especifica que la zona creada reenvía las consultas sin resolver a otro servidor DNS.</li><li>**/dsforwarder** : especifica que la zona integrada de Active Directory que se ha creado reenvía las consultas no resueltas a otro servidor DNS.</li></ul> |
| /overwrite_mem | Sobrescribe los datos DNS de los datos en AD DS. |
| /overwrite_ds | Sobrescribe los datos existentes en AD DS. |

##### <a name="remarks"></a>Comentarios

- Al establecer el tipo de zona como **/dsforwarder** , se crea una zona que realiza el reenvío condicional.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2
```

## <a name="dnscmd-zoneresume-command"></a>comando DNSCmd/zoneresume

Inicia una zona especificada que se pausó previamente.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneresume <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a reanudar. |

##### <a name="remarks"></a>Comentarios

- Puede usar esta operación para reiniciar desde la operación **/zonepause** .

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com
```

## <a name="dnscmd-zoneupdatefromds-command"></a>comando DNSCmd/zoneupdatefromds

Actualiza la zona integrada de Active Directory especificada desde AD DS.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zoneupdatefromds <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a actualizar. |

##### <a name="remarks"></a>Comentarios

- Las zonas integradas de Active Directory realizan esta actualización de forma predeterminada cada cinco minutos. Para cambiar este parámetro, use el `dnscmd config dspollinginterval` comando.

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zoneupdatefromds
```

## <a name="dnscmd-zonewriteback-command"></a>comando DNSCmd/zonewriteback

Comprueba la memoria del servidor DNS en busca de cambios relevantes para una zona especificada y los escribe en el almacenamiento persistente.

### <a name="syntax"></a>Sintaxis

```
dnscmd [<servername>] /zonewriteback <zonename>
```

#### <a name="parameters"></a>Parámetros

| Parámetros | Descripción |
| ---------- | ----------- |
| `<servername>` | Especifica el servidor DNS que se va a administrar, representado por la dirección IP, el FQDN o el nombre de host. Si se omite este parámetro, se utiliza el servidor local. |
| `<zonename>` | Especifica el nombre de la zona que se va a actualizar. |

##### <a name="remarks"></a>Comentarios

- Se trata de una operación de nivel de zona. Puede actualizar todas las zonas de un servidor DNS mediante la operación **/writebackfiles** .

#### <a name="examples"></a>Ejemplos

```
dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
