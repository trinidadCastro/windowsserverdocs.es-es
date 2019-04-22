---
title: Dnscmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e7f31cb5-a426-4e25-b714-88712b8defd5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 39478e9b7dd8e8c69ed07f5d431486a7ed96b9cb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815506"
---
# <a name="dnscmd"></a>Dnscmd

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una interfaz de línea de comandos para administrar servidores DNS. Esta utilidad resulta útil en los archivos por lotes para ayudar a automatizar las tareas de administración DNS rutinarias o para realizar una instalación desatendida sencilla y la configuración de nuevos servidores DNS en la red de scripting.  
## <a name="syntax"></a>Sintaxis  
```  
dnscmd <ServerName> <command> [<command parameters>]  
```  
## <a name="parameters"></a>Parámetros  
|Parámetro|Descripción|  
|-------|--------|  
|<ServerName>|La dirección IP o host nombre de un servidor DNS local o remoto.|  
## <a name="commands"></a>Comandos  
|Comando|Descripción|  
|------|--------|  
|[dnscmd /ageallrecords](#BKMK_1)|Establece la hora actual en todas las marcas de tiempo en una zona o el nodo.|  
|[dnscmd /clearcache](#BKMK_2)|Borra la caché del servidor DNS.|  
|[dnscmd /config](#BKMK_3)|Restablece la configuración del servidor o la zona DNS.|  
|[dnscmd /createbuiltindirectorypartitions](#BKMK_4)|crea las particiones de directorio de aplicación DNS integradas.|  
|[dnscmd /createdirectorypartition](#BKMK_5)|crea una partición de directorio de aplicaciones DNS.|  
|[dnscmd /deletedirectorypartition](#BKMK_6)|elimina una partición de directorio de aplicaciones DNS.|  
|[dnscmd /directorypartitioninfo](#BKMK_7)|Muestra información acerca de una partición de directorio de aplicaciones DNS.|  
|[dnscmd /enlistdirectorypartition](#BKMK_8)|Agrega un servidor DNS para el conjunto de replicación de una partición de directorio de aplicaciones DNS.|  
|[dnscmd /enumdirectorypartitions](#BKMK_9)|Enumera las particiones de directorio de aplicación DNS para un servidor.|  
|[dnscmd /enumrecords](#BKMK_10)|Enumera los registros de recursos en una zona.|  
|[dnscmd /enumzones](#BKMK_11)|se enumeran las zonas hospedadas por el servidor especificado.|  
|[dnscmd /exportsettings](#BKMK_25a)|Escribe información de configuración de servidor en un archivo de texto.|  
|[dnscmd /info](#BKMK_12)|Obtiene la información del servidor.|  
|[dnscmd /ipvalidate](#BKMK_29a)|Valida los servidores DNS remotos.|  
|[dnscmd /nodedelete](#BKMK_13)|Elimina todos los registros para un nodo en una zona.|  
|[dnscmd /recordadd](#BKMK_14)|Agrega un registro de recursos a una zona.|  
|[dnscmd /recorddelete](#BKMK_15)|Quita un registro de recursos de una zona.|  
|[dnscmd /resetforwarders](#BKMK_16)|Establece los servidores DNS para reenviar consultas recursivas.|  
|[dnscmd /resetlistenaddresses](#BKMK_17)|Establece las direcciones IP del servidor para atender las solicitudes DNS.|  
|[dnscmd /startscavenging](#BKMK_18)|Inicia la compactación de servidor.|  
|[dnscmd /statistics](#BKMK_19)|Las consultas o borra los datos de estadísticas del servidor.|  
|[dnscmd /unenlistdirectorypartition](#BKMK_20)|Quita un servidor DNS desde el conjunto de replicación de una partición de directorio de aplicaciones DNS.|  
|[dnscmd /writebackfiles](#BKMK_21)|Guarda todos los datos de zona o una sugerencia de raíz en un archivo.|  
|[dnscmd /zoneadd](#BKMK_22)|crea una nueva zona en el servidor DNS.|  
|[dnscmd /zonechangedirectorypartition](#BKMK_23)|cambia la partición de directorio en el que reside una zona.|  
|[dnscmd /zonedelete](#BKMK_24)|elimina una zona desde el servidor DNS.|  
|[dnscmd /zoneexport](#BKMK_25)|Escribe los registros de recursos de una zona en un archivo de texto.|  
|[dnscmd /zoneinfo](#BKMK_26)|Muestra información de zona.|  
|[dnscmd /zonepause](#BKMK_27)|pausa una zona.|  
|[dnscmd /zoneprint](#BKMK_28)|Muestra todos los registros de la zona.|  
|[dnscmd /zonerefresh](#BKMK_30)|fuerza una actualización de la zona secundaria desde la zona principal.|  
|[dnscmd /zonereload](#BKMK_31)|Vuelve a cargar una zona de su base de datos.|  
|[dnscmd /zoneresetmasters](#BKMK_32)|cambia los servidores maestros que proporcionan información de transferencia de zona para una zona secundaria.|  
|[dnscmd /zoneresetscavengeservers](#BKMK_33)|cambia los servidores que se pueden compactar una zona.|  
|[dnscmd /zoneresetsecondaries](#BKMK_34)|Restablece la información para una zona secundaria.|  
|[dnscmd /zoneresettype](#BKMK_29)|cambia el tipo de zona.|  
|[dnscmd /zoneresume](#BKMK_35)|Reanuda una zona.|  
|[dnscmd /zoneupdatefromds](#BKMK_36)|Actualiza una zona integrada de active directory con los datos de servicios de dominio de active directory (AD DS).|  
|[dnscmd /zonewriteback](#BKMK_37)|Guarda los datos de la zona en un archivo.|  
### <a name="BKMK_1"></a>dnscmd /ageallrecords  
Establece la hora actual en una marca de tiempo en registros de recursos en un nodo en un servidor DNS o la zona especificada.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /ageallrecords <ZoneName>[<NodeName>] | [/tree]|[/f]  
```  
#### <a name="parameters"></a>Parámetros  
<ServerName>  
Especifica el servidor DNS que el administrador va a administrar, representado por la dirección IP, nombre de dominio completo (FQDN) o nombre de Host. Si se omite este parámetro, se utiliza el servidor local.  
<ZoneName>  
Especifica el FQDN de la zona.  
<NodeName>  
Especifica un nodo específico o un subárbol en la zona. *NodeName* especifica el nodo o el subárbol en la zona utilizando lo siguiente:  
-   @ para la zona raíz o FQDN  
-   El FQDN de un nodo (el nombre con un punto (.) al final)  
-   Una sola etiqueta para el nombre relativo a la raíz de la zona  
/tree  
Especifica que todos los nodos secundarios también reciben la marca de tiempo.  
/f  
Ejecuta el comando sin pedir confirmación.  
#### <a name="remarks"></a>Comentarios  
-   El **ageallrecords** comando es por motivos de compatibilidad entre la versión actual de DNS y las versiones anteriores de DNS en el que se admitían no de detección y eliminación de registros obsoletos. Agrega una marca de tiempo con la hora actual para los registros de recursos que no tienen una marca de tiempo y se establece la hora actual en los registros de recursos que tienen una marca de tiempo.  
-   Limpieza de registros no se produce a menos que sean de los registros con marca de tiempo. Registros de recursos, inicio de autoridad (SOA) de registros de recursos, el nombre del servidor (NS) y registros de recursos de servicio de nombres Internet de Windows (WINS) no se incluyen en el proceso de eliminación de registros obsoletos y no están con marca de tiempo incluso cuando el **ageallrecords**comando se ejecuta.  
-   Este comando produce un error a menos que la eliminación de registros obsoletos está habilitada para el servidor DNS y la zona. Para obtener información sobre cómo habilitar la eliminación de registros obsoletos de la zona, consulte el **antigüedad** parámetro en la sintaxis de nivel de zona en el [config](#BKMK_3) comando.  
-   La adición de una marca de tiempo en los registros de recursos DNS hace que sean incompatibles con los servidores DNS que se ejecutan en sistemas operativos distintos de Windows 2000, Windows XP o Windows Server 2003. Una marca de tiempo que agrega mediante el uso de la **ageallrecords** comando no se puede deshacer.  
-   Si se especifica ninguno de los parámetros opcionales, el comando devuelve todos los registros de recursos en el nodo especificado. Si se especifica un valor para al menos uno de los parámetros opcionales, **dnscmd** enumera solo los registros de recursos que se corresponden con el valor o valores que se especifican en el parámetro opcional o parámetros.  
#### <a name="example"></a>Ejemplo  
Consulte [ejemplo 1: Establezca la hora actual en una marca de tiempo para los registros de recursos](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_2"></a>dnscmd /clearcache  
Borra la memoria caché DNS de los registros de recursos en el servidor DNS especificado.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /clearcache  
```  
#### <a name="parameters"></a>Parámetros  
<ServerName>  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
#### <a name="sample-usage"></a>Ejemplo de uso  
`dnscmd dnssvr1.contoso.com /clearcache`  
### <a name="BKMK_3"></a>dnscmd /config  
cambia los valores del registro para el servidor DNS y zonas individuales. Acepta la configuración de nivel de servidor y la configuración de nivel de zona.  
> [!CAUTION]  
> No modifique el registro directamente a menos que no haya ninguna alternativa. El editor del registro omite la seguridad estándar, lo que permite la configuración que puede degradar el rendimiento, dañar el sistema o incluso obligar a reinstalar Windows. Mayoría de las configuraciones del registro se puede modificar de forma segura mediante el uso de los programas en el Panel de Control o Microsoft Management Console (mmc). Si se debe modificar el registro directamente, copia de seguridad. Lea la Ayuda del editor del registro para obtener más información.  
#### <a name="server-level-syntax"></a>Sintaxis de nivel de servidor  
```  
dnscmd [<ServerName>] /config <Parameter>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifica la configuración del servidor especificado.  
#### <a name="parameters"></a>Parámetros  
<ServerName>  
Especifica el servidor DNS que se va a administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
<Parameter>  
Especifique un valor y, opcionalmente, un valor. Los valores de parámetro use esta sintaxis: *Parámetro* [*valor*]  
En el resto de esta sección se describen los siguientes valores de parámetro:  
-   **/addressanswerlimit**  
-   **/bindsecondaries**  
-   **/bootmethod**  
-   **/defaultagingstate**  
-   **/defaultnorefreshinterval**  
-   **/defaultrefreshinterval**  
-   **/disableautoreversezones**  
-   **/disablensrecordsautocreation**  
-   **/dspollinginterval**  
-   **/dstombstoneinterval**  
-   **/ednscachetimeout**  
-   **/enablednsprobes**  
-   **/enablednssec**  
-   **/enableglobalnamessupport**  
-   **/enableglobalqueryblocklist**  
-   **/eventloglevel**  
-   **/forwarddelegations**  
-   **/forwardingtimeout**  
-   **/globalnamesqueryorder**  
-   **/globalqueryblocklist**  
-   **/isslave**  
-   **/localnetpriority**  
-   **/logfilemaxsize**  
-   **/logfilepath**  
-   **/logipfilterlist**  
-   **/loglevel**  
-   **/maxcachesize**  
-   **/maxcachettl**  
-   **/namecheckflag**  
-   **/notcp**  
-   **/norecursion**  
-   **/recursionretry**  
-   **/recursiontimeout**  
-   **/roundrobin**  
-   **/rpcprotocol**  
-   **/scavenginginterval**  
-   **/secureresponses**  
-   **/sendport**  
-   **/strictfileparsing**  
-   **/updateoptions**  
-   **/writeauthorityns**  
-   **/xfrconnecttimeout**  
**/addressanswerlimit [0|5-28]**  
Especifica el número máximo de registros de host que un servidor DNS puede enviar en respuesta a una consulta. El valor puede ser cero (0), o puede estar en el intervalo de 5 a 28 registros. El valor predeterminado es cero (0).  
**/bindsecondaries[0|1]**  
cambia el formato de la transferencia de zona para que pueden lograr la eficiencia y la compresión máxima. Sin embargo, este formato no es compatible con versiones anteriores de Berkeley Internet Name Domain (BIND).  
**0**  
Utiliza compresión máxima. Este formato es compatible con versiones de BIND 4.9.4 y versiones posteriores únicamente.  
**1**  
Envía solo un registro de recurso por cada mensaje a los servidores DNS que no son de Microsoft. Este formato es compatible con versiones anteriores de enlace a la 4.9.4. Esta es la configuración predeterminada.  
**/bootmethod[0|1|2|3]**  
Determina el origen desde el que el servidor DNS obtiene su información de configuración.  
**0**  
Borra la fuente de información de configuración.  
**1**  
Cargas desde el archivo de enlace que se encuentra en el directorio DNS, que es %systemroot%\System32\DNS de forma predeterminada.  
**2**  
Cargas desde el registro.  
**3**  
Cargas desde AD DS y el registro. Esta es la configuración predeterminada.  
**/defaultagingstate[0|1]**  
Determina si la característica de eliminación de registros obsoletos de DNS está habilitada de forma predeterminada en las zonas recién creadas.  
**0**  
Deshabilita la eliminación de registros obsoletos. Esta es la configuración predeterminada.  
**1**  
Habilita la eliminación de registros obsoletos.  
**/defaultnorefreshinterval[0x1-0xFFFFFFFF|0xA8]**  
Establece un período de tiempo en el que no se aceptan ninguna de las actualizaciones de registros actualizados dinámicamente. Las zonas en el servidor heredan automáticamente este valor. Para cambiar el valor predeterminado, escriba un valor en el intervalo de **0 x 1 0xFFFFFFFF**. El valor predeterminado del servidor es **0xA8**.  
**/defaultrefreshinterval [0x1-0xFFFFFFFF|0xA8]**  
Establece un período de tiempo permitido para las actualizaciones dinámicas para registros de DNS. Las zonas en el servidor heredan automáticamente este valor. Para cambiar el valor predeterminado, escriba un valor en el intervalo de **0 x 1 0xFFFFFFFF**. El valor predeterminado del servidor es **0xA8**.  
**/disableautoreversezones [0|1]**  
Habilita o deshabilita la creación automática de zonas de búsqueda inversa. Zonas de búsqueda inversa proporcionan la resolución de direcciones de protocolo de Internet (IP) a los nombres de dominio DNS.  
**0**  
Permite la creación automática de zonas de búsqueda inversa. Esta es la configuración predeterminada.  
**1**  
Deshabilita la creación automática de zonas de búsqueda inversa.  
**/DisableNSRecordsAutoCreation {0 | 1}**  
Especifica si el servidor DNS crea automáticamente el servidor de nombres (NS) para las zonas que hospeda los registros de recursos.  
**0**  
Crea automáticamente registros de recursos de servidor (NS) de nombres para las zonas de los hosts de servidor DNS.  
**1**  
No crea automáticamente registros de recursos de servidor (NS) de nombres para las zonas que los hosts de servidor DNS.  
**/dspollinginterval 0-30**  
Especifica la frecuencia con zonas integradas en los sondeos del servidor AD DS DNS para los cambios en active directory.  
**/dstombstoneinterval [1-30]**  
La cantidad de tiempo en segundos para conservar los registros eliminados en AD DS.  
**/ednscachetimeout [<seconds>]**  
Especifica el número de segundos que se almacena en caché información de DNS (EDNS) extendida. El valor mínimo es 3600, y el valor máximo es 15,724,800. El valor predeterminado es de 604.800 segundos (una semana).  
**/enableednsprobes {0|1}**  
Habilita o deshabilita el servidor para que otros servidores para determinar si son compatibles con EDNS de sondeo.  
**0**  
Deshabilita la compatibilidad con active EDNS sondeos.  
**1**  
Habilita la compatibilidad con active EDNS sondeos.  
**/enablednssec {0|1}**  
Habilita o deshabilita la compatibilidad para extensiones de seguridad DNS (DNSSEC).  
**0**  
Deshabilita DNSSEC.  
**1**  
Habilita DNSSEC.  
**/enableglobalnamessupport {0|1}**  
Habilita o deshabilita la compatibilidad con la zona GlobalNames. La zona GlobalNames admite la resolución de nombres DNS de etiqueta única en un bosque.  
**0**  
Deshabilita la compatibilidad con la zona GlobalNames. Al establecer el valor de este comando a **0**, el servicio servidor DNS no resuelve nombres de etiqueta única en la zona GlobalNames.  
**1**  
Permite la compatibilidad con la zona GlobalNames. Al establecer el valor de este comando a **1**, el servicio servidor DNS resuelve los nombres de etiqueta única en la zona GlobalNames.  
**/enableglobalqueryblocklist {0|1}**  
Habilita o deshabilita la compatibilidad con la lista de bloqueo de consulta global que bloquea la resolución de nombres en la lista. El servicio servidor DNS se crea y habilita la lista global de consultas bloqueadas de forma predeterminada cuando el servicio inicia por primera vez. Para ver la lista de bloqueo de consulta global actual, use el **dnscmd /info /globalqueryblocklist** comando.  
**0**  
Deshabilita la compatibilidad con la lista global de consultas bloqueadas. Al establecer el valor de este comando a **0**, el servicio servidor DNS responde a consultas de nombres en la lista de bloques.  
**1**  
Permite la compatibilidad con la lista global de consultas bloqueadas. Al establecer el valor de este comando a **1**, el servicio servidor DNS no responde a consultas de nombres en la lista de bloques.  
**/eventloglevel [0|1|2|4]**  
Determina qué eventos se registran en el registro de servidor DNS en el Visor de eventos.  
**0**  
No registra ningún evento.  
**1**  
Solo registra errores.  
**2**  
Registra solo los errores y advertencias.  
**4**  
Registra errores, advertencias y eventos informativos. Esta es la configuración predeterminada.  
**/forwarddelegations [0|1]**  
Determina cómo el servidor DNS controla una consulta en una subzona delegada. Estas consultas pueden enviarse a la subzona que se hace referencia en la consulta o a la lista de reenviadores con el nombre del servidor DNS. Entradas de la configuración se utilizan solo cuando se habilita el reenvío.  
**0**  
Envía automáticamente las consultas que hacen referencia a subzonas delegados a la subzona adecuada. Esta es la configuración predeterminada.  
**1**  
reenvíe las consultas que hacen referencia a la subzona delegada a los reenviadores existentes.  
**/forwardingtimeout [<seconds>]**  
Determina el número de segundos (0 x 1-0xFFFFFFFF) un servidor DNS espera un reenviador responder antes de intentar otra reenviador. El valor predeterminado es **0 x 5**, que es de 5 segundos.  
**/globalneamesqueryorder** {**0|1**}  
Especifica si el servicio servidor DNS busca primero en la zona GlobalNames o zonas locales cuando resuelve nombres.  
**0**  
El servicio servidor DNS intenta resolver los nombres consultando la zona GlobalNames antes de consultar las zonas para el que está autorizado.  
**1**  
El servicio servidor DNS intenta resolver los nombres consultando las zonas para el que está autorizado antes de consultar la zona GlobalNames.  
**/globalqueryblocklist[[<name> [<name>]...]**  
reemplaza la lista de bloqueo de consulta global actual con una lista de los nombres que especifique. Si no especifica ningún nombre, este comando borra la lista de bloques. De forma predeterminada, la lista de bloqueo de consulta global contiene los siguientes elementos:  
-   ISATAP  
-   WPAD  
El servicio servidor DNS quitarlos uno o ambos de estos nombres cuando se inicia la primera vez, si encuentra estos nombres en una zona existente.  
**/isslave [0|1]**  
Determina cómo el servidor DNS responde cuando las consultas que reenvía recibirán ninguna respuesta.  
**0**  
Especifica que el servidor DNS no es un subordinado (también conocido como un *esclavo*). Si el reenviador no responde, el servidor DNS intenta resolver la propia consulta. Esta es la configuración predeterminada.  
**1**  
Especifica que el servidor DNS es un subordinado. Si el reenviador no responde, el servidor DNS finaliza la búsqueda y envía un mensaje de error a la resolución.  
**/localnetpriority [0|1]**  
Determina el orden en que se devuelven los registros de host cuando el servidor DNS tiene varios registros de host para el mismo nombre.  
**0**  
Devuelve los registros en el orden en que se enumeran en la base de datos DNS.  
**1**  
Devuelve los registros que tienen similar IP direcciones de red en primer lugar. Esta es la configuración predeterminada.  
**/logfilemaxsize [<size>]**  
Especifica el tamaño máximo en bytes (0 x 10000-0xFFFFFFFF) del archivo Dns.log. Cuando el archivo alcanza su tamaño máximo, DNS sobrescribe los eventos más antiguos. El tamaño predeterminado es 0 x 400000, que es de 4 megabytes (MB).  
**/logfilepath [<path+LogFileName>]**  
Especifica la ruta de acceso del archivo Dns.log. La ruta de acceso predeterminada es % systemroot%\System32\Dns\Dns.log. Puede especificar otra ruta de acceso con el formato *ruta de acceso + LogFileName*.  
**/logipfilterlist <IPaddress> [,<IPaddress>...]**  
Especifica qué paquetes se registran en el archivo de registro de depuración. Las entradas son una lista de direcciones IP. Se registran sólo paquetes hacia y desde las direcciones IP en la lista.  
**/loglevel [<Eventtype>]**  
Determina qué tipos de eventos se registran en el archivo Dns.log. Cada tipo de evento se representa mediante un número hexadecimal. Si desea más de un evento en el registro, utilice hexadecimal suma para agregar los valores y, a continuación, especifique la suma.  
**0x0**  
El servidor DNS no crea un registro. Se trata de la entrada predeterminada.  
**0x10**  
Registra aquellas consultas.  
**0x10**  
Registra las notificaciones.  
**0x20**  
Registra las actualizaciones.  
**0xFE**  
Registra las transacciones nonquery.  
**0x100**  
Pregunta de los registros de transacciones.  
**0x200**  
Registra las respuestas.  
**0x1000**  
Los registros de envían de paquetes.  
**0x2000**  
Los registros de reciben paquetes.  
**0x4000**  
Registra paquetes de protocolo de datagramas de usuario (UDP).  
**0x8000**  
Registra paquetes de protocolo de Control de transmisión (TCP).  
**0xFFFF**  
Registra todos los paquetes.  
**0x10000**  
Los registros de transacciones de escritura de active directory.  
**0x20000**  
Los registros de transacciones de actualización de active directory.  
**0x1000000**  
Paquetes completos de los registros.  
**0x80000000**  
Transacciones de los registros de escritura simultánea.  
**/maxcachesize**  
Especifica el tamaño máximo, en kilobytes (KB), de la memoria caché DNS server s.  
**/maxcachettl [<seconds>]**  
Determina el número de segundos (0 x 0-0xFFFFFFFF) que se guarda un registro en la memoria caché. Si el **0 x 0** es usar la configuración, el servidor DNS no almacena en caché los registros. El valor predeterminado es **0 x 15180** (86.400 segundos o 1 día).  
**/maxnegativecachettl [<seconds>]**  
Especifica el número de segundos (0 x 1-0xFFFFFFFF) una entrada que registra una respuesta negativa a una consulta permanece almacenada en la caché DNS. El valor predeterminado es **0x384** (900 segundos).  
**/namecheckflag [0|1|2|3]**  
Especifica qué estándar de caracteres que se usa durante la comprobación de nombres DNS.  
**0**  
Forzar la caracteres ANSI de usos que cumplen con las tareas de ingeniería de Internet (IETF) de solicitud de comentarios (RFC).  
**1**  
Usa caracteres ANSI que no son necesariamente compatibles con RFC de IETF.  
**2**  
Transformación de UCS multibyte usa dar formato a 8 caracteres (UTF-8). Esta es la configuración predeterminada.  
**3**  
Utiliza todos los caracteres.  
**/NoRecursion [0 | 1]**  
Determina si un servidor DNS realiza la resolución recursiva de nombres.  
**0**  
El servidor DNS realiza la resolución recursiva de nombres si se solicita en una consulta. Esta es la configuración predeterminada.  
**1**  
El servidor DNS no realiza la resolución recursiva de nombres.  
**/notcp**  
Este parámetro está obsoleto y no tiene ningún efecto en las versiones actuales de Windows Server.  
**/recursionretry [<seconds>]**  
Determina el número de segundos (0 x 1-0xFFFFFFFF) que un servidor DNS espera antes de volver a intentar ponerse en contacto con un servidor remoto. El valor predeterminado es 0 x 3 (tres segundos). Este valor debe aumentarse cuando recursividad se produce a través de un vínculo de área extensa (WAN) de la red.  
**/recursiontimeout [<seconds>]**  
Determina el número de segundos (0 x 1-0xFFFFFFFF) que un servidor DNS espera antes de dejar de intentos de ponerse en contacto con un servidor remoto. El intervalo de valores de **0 x 1** a través de **0xFFFFFFFF**. El valor predeterminado es **0xF** (15 segundos). Este valor debe aumentarse cuando recursividad se produce a través de un vínculo WAN lento.  
**/roundrobin [0|1]**  
Determina el orden en que se devuelven los registros de host cuando un servidor tiene varios registros de host para el mismo nombre.  
0  
El servidor DNS no utiliza round robin. En su lugar, devuelve el primer registro para todas las consultas.  
**1**  
El servidor DNS se rota entre los registros que devuelve desde la parte superior a la parte inferior de la lista de registros coincidentes. Esta es la configuración predeterminada.  
**/rpcprotocol [0x0|0x1|0x2|0x4|0xFFFFFFFF]**  
Especifica el protocolo de llamada a procedimiento remoto (RPC) se usa cuando crea una conexión desde el servidor DNS.  
**0x0**  
Deshabilita la RPC de DNS.  
**0x1**  
Usa TCP/IP.  
**0x2**  
Utiliza canalizaciones con nombre.  
**0x4**  
Usa la llamada a procedimiento local (LPC).  
**0xFFFFFFFF**  
Todos los protocolos. Esta es la configuración predeterminada.  
**/scavenginginterval [<hours>]**  
Determina si está habilitada la característica de eliminación de registros obsoletos para el servidor DNS y establece el número de horas (0 x 0-0xFFFFFFFF) entre los ciclos de eliminación de registros obsoletos. El valor predeterminado es **0 x 0**, lo que deshabilita la eliminación de registros obsoletos para el servidor DNS. Un valor mayor que **0 x 0** habilita la eliminación de registros obsoletos para el servidor y establece el número de horas entre los ciclos de eliminación de registros obsoletos.  
**/secureresponses [0 | 1]**  
Determina si DNS filtra los registros que se guardan en una memoria caché.  
**0**  
Guarda todas las respuestas a las consultas de nombres en una memoria caché. Esta es la configuración predeterminada.  
**1**  
Guarda solo los registros que pertenecen al mismo subárbol DNS a una memoria caché.  
**/sendport [<port>]**  
Especifica el número de puerto (0 x 0-0xFFFFFFFF) que usa el DNS para enviar las consultas recursivas a otros servidores DNS. El valor predeterminado es **0 x 0**, lo que significa que el número de puerto se selecciona aleatoriamente.  
**/serverlevelplugindll[<Dllpath>]**  
Especifica la ruta de acceso de un complemento personalizado. Cuando *Dllpath* especifica el nombre de ruta de acceso completa de un complemento, el servidor DNS se llama a funciones en el complemento para resolver las consultas de nombres que están fuera del ámbito local de todas las zonas hospedan de servidor DNS válido. Si el nombre consultado queda fuera del ámbito del complemento, el servidor DNS realiza la resolución mediante reenvío o recursividad, como se ha configurado. Si *Dllpath* no se especifica, el servidor DNS deja de usar un complemento personalizado si se configuró previamente un complemento personalizado.  
**/strictfileparsing [0|1]**  
Determina el comportamiento de un servidor DNS cuando encuentra un registro erróneo al cargar una zona.  
**0**  
El servidor DNS continúa para cargar la zona incluso si el servidor encuentra un registro erróneo. El error se registra en el registro de DNS. Esta es la configuración predeterminada.  
**1**  
El servidor DNS detiene al cargar la zona y registra el error en el registro DNS.  
**/UpdateOptions <RecordValue>**  
Prohíbe que las actualizaciones dinámicas de determinados tipos de registros. Si desea más de un tipo de registro a prohibirse en el registro, utilice hexadecimal suma para agregar los valores y, a continuación, especifique la suma.  
**0x0**  
No restringe los tipos de registro.  
**0x1**  
Excluye el inicio de autoridad (SOA) de registros de recursos.  
**0x2**  
Excluye los registros de recursos de nombres (NS) de servidor.  
**0x4**  
Excluye la delegación de registros de recursos de servidor (NS) de nombre.  
**0x8**  
Excluye los registros de host de servidor.  
**0x100**  
Durante la actualización dinámica segura, excluye el inicio de autoridad (SOA) de registros de recursos.  
**0x200**  
Durante la actualización dinámica segura, excluye los registros de recursos de servidor (NS) de nombres raíz.  
**0x30F**  
Durante la actualización dinámica estándar, excluye los registros de recursos de nombres (NS) de servidor, inicio de los registros de recursos de autoridad (SOA) y registros de servidor de host. Durante la actualización dinámica segura, excluye los registros de recursos de servidor (NS) de nombre de raíz y el inicio de autoridad (SOA) de registros de recursos. Permite las delegaciones y servidor de actualizaciones de host.  
**0x400**  
Durante la actualización dinámica segura, excluye los registros de recursos de servidor (NS) de nombre de la delegación.  
**0x800**  
Durante la actualización dinámica segura, excluye los registros de host de servidor.  
**0x1000000**  
Excluye los registros de delegación signer (DS).  
**0x80000000**  
Deshabilita la actualización dinámica de DNS.  
**/writeauthorityns [0|1]**  
Determina si el servidor DNS escribe registros de recursos de servidor (NS) de nombre en la sección de la entidad de certificación de una respuesta.  
**0**  
Escribe registros de recursos de servidor (NS) de nombre en la sección de autoridad de las referencias solo. Esta configuración cumple con Rfc 1034, conceptos de los nombres de dominio y las instalaciones y Rfc 2181 aclaraciones de la especificación de DNS. Esta es la configuración predeterminada.  
**1**  
Escribe registros de recursos de servidor (NS) de nombre en la sección de la entidad de todas las respuestas autoritativas correcta.  
**/xfrconnecttimeout [<seconds>]**  
Determina el número de segundos (0 x 0-0xFFFFFFFF) en que un servidor DNS principal espera una respuesta de transferencia de su servidor secundario. El valor predeterminado es **0x1E** (30 segundos). Después de que el valor de tiempo de espera expire, se termina la conexión.  
#### <a name="zone-level-syntax"></a>Sintaxis de nivel de zona  
```  
dnscmd /config <Parameters>  
```  
#### <a name="dnscmd-config"></a>dnscmd /config  
Modifica la configuración de la zona especificada.  
#### <a name="parameters"></a>Parámetros  
**<Parameters>**  
Especifique una configuración de una zona de nombre y, opcionalmente, un valor. Los valores de parámetro use esta sintaxis: *Parámetro nombredezona* [*valor*]  
Los siguientes valores de parámetro se documentan en el resto de esta sección:  
-   **/aging**  
-   **/allownsrecordsautocreation**  
-   **/allowupdate**  
-   **/forwarderslave**  
-   **/forwardertimeout**  
-   **/norefreshinterval**  
-   **/refreshinterval**  
-   **/securesecondaries**  
**/Aging <ZoneName>**  
Habilita o deshabilita la eliminación de registros obsoletos en una zona específica.  
**/allownsrecordsautocreation <ZoneName> [<Value>]**  
Invalidaciones nombre del servidor (NS) recursos registros creación automática del servidor DNS de configuración. Registros de recursos de servidor (NS) de nombres que se registraron anteriormente de la zona no se ven afectados. Por lo tanto, debe quitarlas manualmente si no desea.  
**/AllowUpdate <ZoneName>**  
Determina si la zona especificada acepta las actualizaciones dinámicas.  
**/forwarderslave <ZoneName>**  
Reemplaza el servidor DNS **/isslave** configuración.  
**/forwardertimeout <ZoneName>**  
Determina cuántos segundos una zona DNS espera un reenviador responder antes de intentar otra reenviador. Este valor invalida el valor que se establece en el nivel de servidor.  
**/NoRefreshInterval <ZoneName>**  
Establece un intervalo de tiempo para una zona durante el cual las actualizaciones no pueden actualizar dinámicamente registros DNS en una zona especificada.  
**/refreshinterval <ZoneName>**  
Establece un intervalo de tiempo para una zona durante el cual las actualizaciones pueden actualizar dinámicamente registros DNS en una zona especificada.  
**/securesecondaries <ZoneName>**  
Determina qué servidores secundaria pueden recibir las actualizaciones de zona desde el servidor maestro para esta zona.  
#### <a name="remarks"></a>Comentarios  
-   Debe especificarse el nombre de zona sólo para los parámetros de nivel de zona.  
### <a name="BKMK_4"></a>dnscmd /createbuiltindirectorypartitions  
crea una partición de directorio de aplicaciones DNS. Cuando DNS se instala, se crea una partición de directorio de aplicación para el servicio en los niveles de dominios y bosques. Utilice este comando para crear particiones de directorio de aplicación DNS que se han eliminado o que nunca se ha creado. Sin ningún parámetro, este comando crea una partición de directorio DNS integrada para el dominio.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /createbuiltindirectorypartitions [/forest] [/alldomains]   
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**/forest**  
crea una partición de directorio DNS para el bosque.  
**/AllDomains**  
crea particiones DNS para todos los dominios del bosque.  
### <a name="BKMK_5"></a>dnscmd /createdirectorypartition  
crea una partición de directorio de aplicaciones DNS. Cuando DNS se instala, se crea una partición de directorio de aplicación para el servicio en los niveles de dominios y bosques. Esta operación crea particiones de directorio de aplicación DNS adicionales.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /createdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<PartitionFQDN>**  
El FQDN de la partición de directorio de aplicación DNS que va a crear.  
### <a name="BKMK_6"></a>dnscmd /deletedirectorypartition  
Quita una partición de directorio de aplicación DNS existente.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /deletedirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<PartitionFQDN>**  
El FQDN de la partición de directorio de aplicación DNS que se quitará.  
### <a name="BKMK_7"></a>dnscmd /directorypartitioninfo  
Muestra información acerca de una partición de directorio de aplicación DNS especificada.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /directorypartitioninfo <PartitionFQDN> [/detail]   
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<PartitionFQDN>**  
El FQDN de la partición de directorio de aplicaciones DNS.  
**/detail**  
muestra toda la información acerca de la partición de directorio de aplicación.  
### <a name="BKMK_8"></a>dnscmd /enlistdirectorypartition  
Agrega el servidor DNS para el conjunto de réplicas de la partición de directorio especificado.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /enlistdirectorypartition <PartitionFQDN>  
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<PartitionFQDN>**  
El FQDN de la partición de directorio de aplicaciones DNS.  
### <a name="BKMK_9"></a>dnscmd /enumdirectorypartitions  
Enumera las particiones de directorio de aplicación DNS para el servidor especificado.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /enumdirectorypartitions [/custom]   
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**/custom**  
Muestra solo las particiones de directorio creados por el usuario.  
### <a name="BKMK_10"></a>dnscmd /enumrecords  
Enumera los registros de recursos de un nodo especificado en una zona DNS.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /enumrecords <ZoneName> <NodeName> [/type <RRtype> <Rrdata>] [/authority] [/glue] [/additional] [/node | /child | /startchild<ChildName>] [/continue | /detail]   
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS que va a administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**/enumrecords**  
Enumera los registros de recursos en la zona especificada.  
**<ZoneName>**  
Especifica el nombre de la zona a la que pertenecen los registros de recursos.  
**<NodeName>**  
Especifica el nombre del nodo de los registros de recursos.  
**tipo / <RRtype> <Rrdata>**  
Especifica el tipo de registros de recursos que se mostrarán y el tipo de datos que se espera:  
**<RRtype>**  
Especifica el tipo de registros de recursos que se mostrarán.  
**<Rrdata>**  
Especifica el tipo de datos de registro esperado.  
**/authority**  
Incluye datos autorizados.  
**/glue**  
Incluye datos de adherencia.  
**/additional**  
Incluye toda la información adicional acerca de los registros de recursos enumerados.  
**{/node | /child | /startchild <ChildName>}**  
Filtra o agrega información a la visualización de registros de recursos:  
**/node**  
Enumera los registros de recursos del nodo especificado.  
**/child**  
Enumera los registros de recursos de un dominio secundario especificado.  
**/StartChild <ChildName>**  
Comienza la lista en el dominio secundario especificado.  
**/continue | /detail**  
Especifica cómo se muestran los datos devueltos.  
**/continue**  
Muestra solo los registros de recursos con su tipo y sus datos.  
**/detail**  
muestra toda la información acerca de los registros de recursos.  
#### <a name="sample-usage"></a>Ejemplo de uso  
`dnscmd /enumrecords test.contoso.com test /additional`  
### <a name="BKMK_11"></a>dnscmd /enumzones  
se enumeran las zonas que existen en el servidor DNS especificado.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /enumzones [/primary | /secondary | /forwarder | /stub | /cache | /auto-created] [/forward | /reverse | /ds | /file] [/domaindirectorypartition | /forestdirectorypartition | /customdirectorypartition | /legacydirectorypartition | /directorypartition <PartitionFQDN>]  
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**/primary | /secondary | /forwarder | /stub | /cache | /auto-created**  
Los filtros de los tipos de zonas para mostrar:  
**/primary**  
Enumera todas las zonas que son las zonas principales estándar o zonas integradas en active directory.  
**/secondary**  
Enumera todas las zonas secundarias estándares.  
**/forwarder**  
se enumeran las zonas que reenvían consultas sin resolver a otro servidor DNS.  
**/stub**  
Enumera todas las zonas de rutas internas.  
**/cache**  
Muestra solo las zonas que se cargan en la memoria caché.  
**/auto-created**  
se enumeran las zonas que se crearon automáticamente durante la instalación del servidor DNS.  
**/forward | /reverse | /ds | /file**  
Especifica los filtros adicionales de los tipos de zonas para mostrar:  
**/forward**  
zonas de búsqueda directa de listas.  
**/reverse**  
las listas de zonas de búsqueda inversa.  
**/ds**  
se enumeran las zonas integradas en active directory.  
**/file**  
se enumeran las zonas que están respaldadas por los archivos.  
**/domaindirectorypartition**  
se enumeran las zonas que se almacenan en la partición de directorio de dominio.  
**/forestdirectorypartition**  
se enumeran las zonas que se almacenan en la partición de directorio de aplicaciones DNS de bosque.  
**/customdirectorypartition**  
Enumera todas las zonas que se almacenan en una partición de directorio de aplicación definido por el usuario.  
**/legacydirectorypartition**  
Enumera todas las zonas que se almacenan en la partición de directorio de dominio.  
**/DirectoryPartition <PartitionFQDN>**  
Enumera todas las zonas que se almacenan en la partición de directorio especificado.  
#### <a name="remarks"></a>Comentarios  
-   El **enumzones** parámetros actúen como filtros en la lista de zonas. Si se especifica ningún filtro, se devuelve una lista completa de las zonas. Cuando se especifica un filtro, solo las zonas que cumplen los criterios del filtro se incluyen en la lista de zonas.  
#### <a name="example"></a>Ejemplo  
Consulte [ejemplo 2: Mostrar una lista completa de las zonas en un servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [ejemplo 3: Mostrar una lista de las zonas creado automáticamente en un servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25a"></a>dnscmd /exportsettings  
crea un archivo de texto que muestra los detalles de configuración de un servidor DNS. El archivo de texto se denomina DnsSettings.txt. Se encuentra en el directorio %systemroot%\system32\dns del servidor.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /exportsettings   
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
#### <a name="remarks"></a>Comentarios  
-   Puede usar la información en el archivo que **/exportsettings dnscmd** crea para solucionar problemas de configuración o para asegurarse de que ha configurado varios servidores de forma idéntica.  
### <a name="BKMK_12"></a>dnscmd /info  
Muestra la configuración de la sección DNS del registro del servidor especificado: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters**  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /info [<Setting>]  
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<Setting>**  
Establecer que la **información** comando devuelve se puede especificar individualmente. Si no se especifica un valor, se devuelve un informe de configuración comunes.  
#### <a name="remarks"></a>Comentarios  
-   Este comando muestra la configuración del registro que se encuentran en el nivel de servidor DNS. Para mostrar la configuración del registro de nivel de zona, use el [ha caducado](#BKMK_26) comando. Para ver una lista de configuraciones que se pueden mostrar con este comando, consulte el [config](#BKMK_3) descripción.  
#### <a name="example"></a>Ejemplo  
Consulte [ejemplo 4: Muestra la configuración de un servidor DNS de la IsSlave](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [ejemplo 5: Muestra la configuración de un servidor DNS de la Recursiontimeout](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_29a"></a>dnscmd /ipvalidate  
Comprueba si una dirección IP identifica un servidor DNS operativo o si el servidor DNS puede actuar como un reenviador, un servidor de sugerencia de raíz o un servidor maestro para una zona específica.  
#### <a name="syntax"></a>Sintaxis  
```  
dnscmd [<ServerName>] /ipvalidate <Context> [<ZoneName>] [[<IPaddress>]]  
```  
#### <a name="parameters"></a>Parámetros  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<Context>**  
Especifica el tipo de prueba para realizar. Puede especificar cualquiera de las siguientes pruebas:  
-   **/dnsservers** comprueba los equipos con las direcciones que especifique el funcionamiento de los servidores DNS.  
-   **/FORWARDERS** comprueba que las direcciones que especifique identifican los servidores DNS que pueden actuar como reenviadores.  
-   **/roothints** comprueba que las direcciones que especifique identifican los servidores DNS que pueden actuar como servidores de nombres de sugerencia de raíz.  
-   **/zonemasters** comprueba que las direcciones que especifique identifican servidores DNS que son los servidores maestros de *nombredezona*.  
**<ZoneName>**  
Identifica la zona. Use este parámetro con el **/zonemasters** parámetro.  
**<IPaddress>**  
Especifica las direcciones IP que el comando de prueba.  
#### <a name="sample-usage"></a>Ejemplo de uso  
<pre>dnscmd dnssvr1.contoso.com /ipvalidate /dnsservers 10.0.0.1 10.0.0.2  
dnscmd dnssvr1.contoso.com /ipvalidate /zonemasters corp.contoso.com 10.0.0.2</pre>  
### <a name="BKMK_13"></a>dnscmd /nodedelete  
Elimina todos los registros para un host especificado. ### Sintaxis ```  
dnscmd [<ServerName>] /nodedelete <ZoneName> <NodeName> [/tree] [/ f] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona.  
**<NodeName>**  
Especifica el nombre de host del nodo que se va a eliminar.  
**/tree**  
Elimina todos los registros secundarios.  
**/f**  
ejecuta el comando sin pedir confirmación. ### Vea ejemplo [ejemplo 6: eliminar los registros de un nodo](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_14"></a>dnscmd /recordadd  
Agrega un registro a una zona especificada en un servidor DNS. ### Sintaxis ```  
dnscmd [<ServerName>] /recordadd <ZoneName> <NodeName> <RRtype> <Rrdata> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica la zona en el que reside el registro.  
**<NodeName>**  
Especifica un nodo específico en la zona.  
**<RRtype>**  
Especifica el tipo de registro que se va a agregar.  
**<Rrdata>**  
Especifica el tipo de datos que se espera.  
> [!NOTE]  
Cuando se agrega un registro, asegúrese de que use el formato de datos y el tipo de datos correcto. Para obtener una lista de tipos de registros y los tipos de datos adecuado, vea [registros de recursos de referencia](https://technet.microsoft.com/library/cc758321(v=ws.10).aspx). ### Ejemplo de uso
<pre>dnscmd dnssvr1.contoso.com /recordadd test A 10.0.0.5  
dnscmd /recordadd test.contoso.com test MX 10 mailserver.test.contoso.com</pre>  
### <a name="BKMK_15"></a>dnscmd /recorddelete  
elimina un registro de recursos de una zona especificada. ### Sintaxis ```  
dnscmd <ServerName> /recorddelete <ZoneName> <NodeName> <RRtype> <Rrdata>[/ f] ```  
#### Parameters  
**<ServerName>**  
> Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica la zona en el que reside el registro de recursos.  
**<NodeName>**  
Especifica el nombre del host.  
**<RRtype>**  
Especifica el tipo de registro de recursos que se puede eliminar.  
**<Rrdata>**  
Especifica el tipo de datos que se espera.  
**/f**  
ejecuta el comando sin pedir confirmación:-dado que los nodos pueden tener más de un registro de recursos, este comando requiere que sea muy específico sobre el tipo de registro de recursos que desea eliminar. -Si especifica un tipo de datos y no se especifica un tipo de datos de registro de recursos, se eliminan todos los registros con ese tipo de datos específico para el nodo especificado. ### Ejemplo de uso `dnscmd /recorddelete test.contoso.com test MX 10 mailserver.test.contoso.com`  
### <a name="BKMK_16"></a>dnscmd /resetforwarders  
selecciona o restablece las direcciones IP a la que el servidor DNS reenvía consultas DNS al que no puede resolver localmente. ### Sintaxis ```  
dnscmd [<ServerName>] /resetforwarders [<IPaddress> [,<IPaddress>]...] [/ tiempo de espera <timeOut>] [/ subordinado | / /NoSlave] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<IPaddress>**  
Enumera las direcciones IP a la que el servidor DNS reenvía consultas sin resolver.  
** / tiempo de espera <timeOut>**  
Establece el número de segundos que el servidor DNS espera una respuesta del reenviador. De forma predeterminada, este valor es de cinco segundos.  
**/slave|/noslave**  
Determina si el servidor DNS realiza sus propias consultas iterativos si no se puede resolver una consulta el reenviador:  
**/slave**  
Evita que el servidor DNS realice sus propias consultas iterativas si el reenviador no puede resolver una consulta.  
**/noslave**  
Permite que el servidor DNS realizar sus propias consultas iterativas si el reenviador no puede resolver una consulta. Esta es la configuración predeterminada. ### Comentarios: de forma predeterminada, un servidor DNS realiza consultas iterativas cuando no puede resolver una consulta. -Establecer direcciones IP mediante el uso de la **resetforwarders** comando hace que el servidor DNS realice consultas recursivas a los servidores DNS en las direcciones IP especificadas. Si los reenviadores no resuelven la consulta, el servidor DNS, a continuación, puede realizar sus propias consultas iterativas. -Si el **/subordinado** se usa el parámetro, el servidor DNS no lleva a cabo sus propias consultas iterativas. Esto significa que el servidor DNS reenvía consultas sin resolver sólo a los servidores DNS en la lista, y no realiza consultas iterativas si los reenviadores no resolverlos. Resulta más eficaz para configurar una dirección IP del servidor DNS como reenviador. Puede usar el **resetforwarders** comando para servidores internos en una red para reenviar las consultas sin resolver a un servidor DNS que tiene una conexión externa. -enumerar dos veces una dirección IP del reenviador hace que el servidor DNS intenta reenviar a ese servidor dos veces. ### Ejemplo de uso
<pre>dnscmd dnssvr1.contoso.com /resetforwarders 10.0.0.1 /timeout 7 /slave  
dnscmd dnssvr1.contoso.com /resetforwarders /noslave</pre>  
### <a name="BKMK_17"></a>dnscmd /resetlistenaddresses  
Especifica las direcciones IP en un servidor que escucha las solicitudes de cliente DNS. #### Syntax ```  
dnscmd [<ServerName>] /resetlistenaddresses [<listenaddress>] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<listenaddress>**  
Especifica una dirección IP en el servidor DNS que escucha las solicitudes de cliente DNS. Si no se especifica ninguna dirección de escucha, todas las direcciones IP en el servidor escuchan las solicitudes de cliente. ### Comentarios: de forma predeterminada, todas las direcciones IP en un servidor DNS escuchan las solicitudes de cliente DNS. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /resetlistenaddresses 10.0.0.1`  
### <a name="BKMK_18"></a>dnscmd /startscavenging  
Indica a un servidor DNS para intentar una búsqueda inmediata de registros de recursos obsoletos en un servidor DNS especificado. ### Sintaxis ```  
dnscmd [<ServerName>] /startscavenging ```  
#### Parameter  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local. ### Comentarios - finalización correcta de este comando inicia un barrido inmediatamente. -Aunque el comando para iniciar el barrido parece completarse correctamente, el barrido no se inicia a menos que se cumplen las siguientes condiciones previas:-eliminación de registros obsoletos está habilitada para el servidor y la zona. -La zona se inicia. -Los registros de recursos tienen una marca de tiempo. -Para obtener información sobre cómo habilitar la eliminación de registros obsoletos para el servidor, consulte el **/ScavengingInterval** parámetro en la sintaxis de nivel de servidor en el [config](#BKMK_3) sección. -Para obtener información sobre cómo habilitar la eliminación de registros obsoletos de la zona, consulte el **antigüedad** parámetro en la sintaxis de nivel de zona en el [config](#BKMK_3) sección. -Para obtener información sobre cómo iniciar una zona que está en pausa, consulte el [zoneresume](#BKMK_35) sección. -Para obtener información sobre cómo comprobar los registros de recursos para una marca de tiempo, consulte el [ageallrecords](#BKMK_1) sección. -Si se produce un error de compactar, ningún mensaje de advertencia aparece. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /startscavenging`  
### <a name="BKMK_19"></a>dnscmd /statistics  
Muestra o borra los datos para un servidor DNS especificado. ### Sintaxis ```  
dnscmd [<ServerName>] /statistics [<StatID>] [/ clear] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<StatID>**  
Especifica qué estadística o combinación de estadísticas que mostrar. Un número de identificación se utiliza para identificar una estadística. Si se especifica ningún número de Id. de estadística, se muestran todas las estadísticas.  
La siguiente es una lista de números que se pueden especificar y la estadística correspondiente que muestra:  
**00000001**  
time  
**00000002**  
query  
**00000004**  
query2  
**00000008**  
Recurse  
**00000010**  
Maestro  
**00000020**  
Secundario  
**00000040**  
WINS  
**00000100**  
Actualización  
**00000200**  
SkwanSec  
**00000400**  
DS  
**00010000**  
Memoria  
**00100000**  
PacketMem  
**00040000**  
DBASE  
**00080000**  
Registros  
**00200000**  
NbstatMem  
**/clear**  
Restablece el contador de estadísticas especificado en cero. ### Comentarios - la **estadísticas** comando muestra los contadores que comienzan en el servidor DNS cuando se inicia o se reanuda. ### Vea ejemplos [ejemplo 7: Mostrar las estadísticas de tiempo para un servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [ejemplo 8: Mostrar las estadísticas de NbstatMem para un servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_20"></a>dnscmd /unenlistdirectorypartition  
Quita el servidor DNS de conjunto de réplicas de la partición de directorio especificado. ### Sintaxis ```  
dnscmd [<ServerName>] /unenlistdirectorypartition <PartitionFQDN> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<PartitionFQDN>**  
El FQDN de la partición de directorio de aplicación DNS que se quitará.  
### <a name="BKMK_21"></a>dnscmd /writebackfiles  
Comprueba la memoria del servidor DNS para los cambios y los escribe en un almacenamiento persistente. ### Sintaxis ```  
dnscmd [<ServerName>] /writebackfiles [<ZoneName>] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona que se puede actualizar. ### Comentarios - la **writebackfiles** comando actualiza todas las zonas de datos sucias o una zona especificada. Una zona está desfasada cuando hay cambios en la memoria que aún no se han escrito en el almacenamiento persistente. Se trata de una operación de nivel de servidor que comprueba todas las zonas. Puede especificar una zona en esta operación, o puede usar el [zonewriteback](#BKMK_37) operación. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /writebackfiles`  
### <a name="BKMK_22"></a>dnscmd /zoneadd  
Agrega una zona al servidor DNS. #### Syntax ```  
dnscmd [<ServerName>] /zoneadd <ZoneName> <Zonetype> [/dp <FQDN>| {/domain|/enterprise|/legacy}] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona.  
**<Zonetype>**  
Especifica el tipo de zona que desea crear. Cada tipo de zona tiene diferentes parámetros necesarios:  
**/dsprimary**  
crea una zona integrada de active directory.  
** / /file principal <FileName>**  
crea una zona principal estándar y especifica el nombre del archivo que almacenará la información de zona.  
** / secondary <MasterIPaddress> [<MasterIPaddress>...] **  
crea una zona secundaria estándar.  
** / stub <MasterIPaddress> [<MasterIPaddress>...] / archivo <FileName>**  
crea una zona de código auxiliar correspondiente a un archivo.  
**/dsstub <MasterIPaddress> [<MasterIPaddress>...]**  
crea una zona de rutas internas integrada de active directory.  
** / reenviador <MasterIPaddress> [<MasterIPaddress>]... / archivo <FileName>**  
Especifica que la zona creada reenvía consultas sin resolver a otro servidor DNS.  
**/dsforwarder**  
Especifica que la zona integrada de Active Directory creado reenvía consultas sin resolver a otro servidor DNS.  
**/dp <FQDN> {/domain | /enterprise | /legacy}**  
Especifica la partición de directorio en el que se va a almacenar la zona.  
**<FQDN>**  
Especifica el FQDN de la partición de directorio.  
**/domain**  
La zona se almacena en la partición de directorio de dominio.  
**/enterprise**  
La zona se almacena en la partición de directorio de empresa.  
**/legacy**  
La zona se almacena en una partición de directorio heredado. ### Comentarios - especifica un tipo de zona de **/forwarder** o **/dsforwarder** crea una zona que realiza el reenvío condicional. ### Ejemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneadd test.contoso.com /dsprimary  
dnscmd dnssvr1.contoso.com /zoneadd secondtest.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_23"></a>dnscmd /zonechangedirectorypartition  
cambia la partición de directorio en el que reside la zona especificada. ### Sintaxis ```  
dnscmd [<ServerName>] /zonechangedirectorypartition <ZoneName>] {[<NewPartitionName>] | [<Zonetype>] } ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
El FQDN de la partición del directorio actual en el que reside la zona.  
**<NewPartitionName>**  
El FQDN de la partición de directorio que se va a mover la zona.  
**<Zonetype>**  
Especifica el tipo de partición de directorio que se va a mover la zona.  
**/domain**  
Mueve la zona a la partición de directorio de dominio integrado.  
**/forest**  
Mueve la zona a la partición de directorio de bosque integrados.  
**/legacy**  
Mueve la zona a la partición de directorio que se crea para los controladores de dominio de Active Directory pre. Estas particiones de directorio no son necesarias para el modo nativo.  
### <a name="BKMK_24"></a>dnscmd /zonedelete  
elimina una zona especificada. ### Sintaxis ```  
dnscmd [<ServerName>] /zonedelete <ZoneName> [/dsdel] [/ f] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona que se va a eliminar.  
**/dsdel**  
elimina la zona de AD DS.  
**/f**  
Ejecuta el comando sin pedir confirmación. ### Vea ejemplo [ejemplo 9: eliminar una zona de un servidor DNS](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_25"></a>dnscmd /zoneexport  
crea un archivo de texto que enumera los registros de recursos de una zona especificada. #### Syntax `dnscmd [<ServerName>] /zoneexport <ZoneName> <ZoneExportFile>` #### Parameters **<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona.  
**<ZoneExportFile>**  
Especifica el nombre de archivo que se creará. ### Comentarios - la **zoneexport** operación crea un archivo de registros de recursos para una zona integrada de active directory para solucionar problemas. De forma predeterminada, el archivo que este comando crea se coloca en el directorio DNS, que es, de forma predeterminada, el directorio %systemroot%/System32/Dns. ### Vea ejemplo [ejemplo 10: Exportar lista de registros de recursos de zona a un archivo](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_26"></a>dnscmd /zoneinfo  
Muestra la configuración de la sección del registro de la zona especificada: ** HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>** ### sintaxis ```  
dnscmd [<ServerName>] /zoneinfo <ZoneName> [<Setting>] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona.  
**<Setting>**  
Puede especificar individualmente cualquier configuración que el **ha caducado** comando. Si no especifica un valor, se devuelven todas las configuraciones. ### Comentarios - la **ha caducado** comando muestra la configuración del registro que está en la zona DNS de nivel en ** HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNS\Parameters\Zones\\<ZoneName>**. -Para mostrar la configuración del registro de nivel de servidor, utilice el [información](#BKMK_12) comando. -Para ver una lista de configuraciones que se pueden mostrar con este comando, consulte el [config](#BKMK_3) comando. ### Vea ejemplo [ejemplo 11: Muestra la configuración del registro de RefreshInterval](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx) o [12 de ejemplo: Mostrar configuración del registro de antigüedad](https://technet.microsoft.com/library/cc784399(v=ws.10).aspx).  
### <a name="BKMK_27"></a>dnscmd /zonepause  
pone en pausa la zona especificada, que, a continuación, omite las solicitudes de consulta. ### Sintaxis ```  
dnscmd [<ServerName>] /zonepause <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona se pongan en pausa. ### Comentarios - reanudar una zona y que esté disponible una vez se ha detenido, use el [zoneresume](#BKMK_35) comando. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zonepause test.contoso.com`  
### <a name="BKMK_28"></a>dnscmd /zoneprint  
Enumera los registros en una zona. ### Sintaxis ```  
dnscmd [<ServerName>] /zoneprint <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Identifica la zona se publique.  
### <a name="BKMK_30"></a>dnscmd /zonerefresh  
fuerza una zona DNS secundaria para la actualización de la zona principal. ### Sintaxis ```  
dnscmd <ServerName> /zonerefresh <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona que se va a actualizar. ### Comentarios - la **zonerefresh** comando fuerza una comprobación del número de versión en el inicio del servidor maestro s del registro de recurso de autoridad (SOA). Si el número de versión en el servidor principal es mayor que el número de versión del servidor secundario, se inicia una transferencia de zona que actualiza el servidor secundario. Si el número de versión es el mismo, se produce ninguna transferencia de zona. -La comprobación forzada se produce de forma predeterminada, cada 15 minutos. Para cambiar el valor predeterminado, use el **dnscmd config refreshinterval** comando. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zonerefresh test.contoso.com`  
### <a name="BKMK_31"></a>dnscmd /zonereload  
Copias de la zona información desde su origen. ### Sintaxis ```  
dnscmd <ServerName> /zonereload <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona se vuelva a cargar. ### Comentarios - si la zona está integrada en active directory, vuelve a cargar desde AD DS. -Si la zona es un estándar basado en archivos, vuelve a cargar desde un archivo. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zonereload test.contoso.com`  
### <a name="BKMK_32"></a>dnscmd /zoneresetmasters  
Restablece las direcciones IP del servidor maestro que proporciona información de transferencia de zona para una zona secundaria. #### Syntax ```  
dnscmd <ServerName> /zoneresetmasters <ZoneName> [/local] [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona se vuelva a cargar.  
**/local**  
Establece una lista maestra local. Este parámetro se usa para las zonas integradas en active directory.  
**<IPaddress>**  
Las direcciones IP de los servidores maestros de la zona secundaria. ### Comentarios: este valor se establece inicialmente cuando se crea la zona secundaria. Use la **zoneresetmasters** comando en el servidor secundario. Este valor no tiene ningún efecto si se establece en el servidor DNS principal. ### Ejemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com 10.0.0.1  
dnscmd dnssvr1.contoso.com /zoneresetmasters test.contoso.com /local</pre>  
### <a name="BKMK_33"></a>dnscmd /zoneresetscavengeservers  
cambia las direcciones IP de los servidores que se pueden compactar la zona especificada. ### Sintaxis ```  
dnscmd [<ServerName>] /zoneresetscavengeservers <ZoneName> [<IPaddress> [<IPaddress>]...] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Identifica la zona a compactar.  
**<IPaddress>**  
Enumera las direcciones IP de los servidores que pueden realizar el barrido. Si se omite este parámetro, pueden borrarlos todos los servidores que hospedan esta zona se. ### Comentarios: de forma predeterminada, todos los servidores que hospedan una zona pueden compactar esa zona. -Si una zona se hospeda en más de un servidor DNS, puede usar este comando para reducir el número de veces que se eliminen sus una zona. -Eliminación de registros obsoletos debe habilitarse en el servidor DNS y la zona que se ve afectado por este comando. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zoneresetscavengeservers test.contoso.com 10.0.0.1 10.0.0.2`  
### <a name="BKMK_34"></a>dnscmd /zoneresetsecondaries  
Especifica una lista de direcciones IP de los servidores secundarios a los que responde un servidor maestro cuando se solicita una transferencia de zona. #### Syntax ```  
dnscmd [<ServerName>] /zoneresetsecondaries <ZoneName> {/noxfr | /nonsecure | /securens | /securelist <SecurityIPaddresses>} {/nonotify | /notify | /notifylist <NotifyIPaddresses>} ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si la es el parámetro se omite, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona que tendrá sus servidores secundarios restablecer.  
**/noxfr | /nonsecure | /securens | /securelist <SecurityIPaddresses>**  
Especifica si todos o solo algunos de los servidores secundarios que solicita una actualización de obtención de una actualización.  
**/noxfr**  
Especifica que las transferencias de zona no se permiten.  
**/nonsecure**  
Especifica que todas las solicitudes de transferencia de zona se conceden.  
**/securens**  
Especifica que sólo el servidor que aparece en el registro de recursos de servidor (NS) de nombre de la zona se ha concedido a una transferencia.  
**/securelist**  
Especifica que se conceden las transferencias de zona sólo a la lista de servidores. Este parámetro debe ir seguido por una dirección o direcciones IP que usa el servidor maestro.  
**<SecurityIPaddresses>**  
Enumera las direcciones IP que reciben las transferencias de zona desde el servidor maestro. Este parámetro solo se usa con el **/securelist** parámetro.  
**/nonotify | /notify | /notifylist <NotifyIPaddresses>**  
Especifica que se envía una notificación de cambio sólo a determinados servidores secundarios:  
**/nonotify**  
Especifica que no hay notificaciones de cambio se envían a servidores secundarios.  
**/notify**  
Especifica que se envían las notificaciones de cambio a todos los servidores secundarios.  
**/notifylist**  
Especifica que se envían las notificaciones de cambio a sólo la lista de servidores. Este comando debe ir seguido por una dirección o direcciones IP que usa el servidor maestro.  
**<NotifyIPaddresses>**  
Especifica la dirección o direcciones IP del servidor secundario o a qué cambios se envían las notificaciones de servidores. Esta lista sólo se utiliza con el **/notifylist** parámetro. ### Comentarios - Use el **zoneresetsecondaries** comando en el servidor maestro para especificar cómo responde a las solicitudes de transferencia de zona desde los servidores secundarios. ### Ejemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /noxfr /nonotify  
dnscmd dnssvr1.contoso.com /zoneresetsecondaries test.contoso.com /securelist 11.0.0.2</pre>  
### <a name="BKMK_29"></a>dnscmd /zoneresettype  
cambia el tipo de la zona. #### Syntax ```  
dnscmd [<ServerName>] /zoneresettype <ZoneName> <Zonetype> [/overwrite_mem | /overwrite_ds] ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la sintaxis del equipo local, dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Identifica la zona en el que se cambiará el tipo.  
**<Zonetype>**  
Especifica el tipo de zona que desea crear. Cada tipo tiene diferentes parámetros necesarios:  
**/dsprimary**  
crea una zona integrada de active directory.  
** / /file principal <FileName>**  
crea una zona principal estándar.  
** / secondary <MasterIPaddress> [,<MasterIPaddress>...] **  
crea una zona secundaria estándar.  
**/stub <MasterIPaddress>[,<MasterIPaddress>...] /file <FileName>**  
crea una zona de código auxiliar correspondiente a un archivo.  
**/dsstub <MasterIPaddress>[,<MasterIPaddress>...]**  
crea una zona de rutas internas integrada de active directory.  
** / reenviador <MasterIPaddress[,<MasterIPaddress>]... / archivo<FileName>**  
Especifica que la zona creada reenvía consultas sin resolver a otro servidor DNS.  
**/dsforwarder**  
Especifica que la zona integrada de Active Directory creado reenvía consultas sin resolver a otro servidor DNS.  
**/overwrite_mem | /overwrite_ds**  
Especifica cómo sobrescribir los datos existentes:  
**/overwrite_mem**  
Sobrescribe los datos DNS de los datos de AD DS.  
**/overwrite_ds**  
Sobrescribe los datos existentes en AD DS. ### Comentarios - establecer la zona se escriba como **/dsforwarder** crea una zona que realiza el reenvío condicional. ### Ejemplo de uso
<pre>dnscmd dnssvr1.contoso.com /zoneresettype test.contoso.com /primary /file test.contoso.com.dns  
dnscmd dnssvr1.contoso.com /zoneresettype second.contoso.com /secondary 10.0.0.2</pre>  
### <a name="BKMK_35"></a>dnscmd /zoneresume  
inicia una zona especificada que se pausó previamente. ### Sintaxis ```  
dnscmd <ServerName> /zoneresume <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona a reanudar. ### Comentarios: puede usar esta operación para invertir el [zonepause](#BKMK_27) operación. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zoneresume test.contoso.com`  
### <a name="BKMK_36"></a>dnscmd /zoneupdatefromds  
Actualiza la zona integrada de active directory especificada de AD DS. ### Sintaxis ```  
dnscmd <ServerName> /zoneupdatefromds <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona para la actualización. ### Comentarios - active directory de las zonas integradas realizan esta actualización de forma predeterminada cada cinco minutos. Para cambiar este parámetro, use el **dnscmd config dspollinginterval** comando. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zoneupdatefromds`  
### <a name="BKMK_37"></a>dnscmd /zonewriteback  
Comprueba la memoria del servidor DNS para los cambios que son relevantes para una zona especificada y los escribe en un almacenamiento persistente. ### Sintaxis ```  
dnscmd <ServerName> /zonewriteback <ZoneName> ```  
#### Parameters  
**<ServerName>**  
Especifica el servidor DNS para administrar, representado por la dirección IP, FQDN o nombre de host. Si se omite este parámetro, se utiliza el servidor local.  
**<ZoneName>**  
Especifica el nombre de la zona para la actualización. ### Comentarios: se trata de una operación de nivel de zona. Puede actualizar todas las zonas en un servidor DNS con el [writebackfiles](#BKMK_21) operación. ### Ejemplo de uso `dnscmd dnssvr1.contoso.com /zonewriteback test.contoso.com`  
