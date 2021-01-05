---
description: 'Más información sobre: Advanced Active Directory la administración de la replicación y la topología con Windows PowerShell (nivel 200)'
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: Advanced Active Directory Replication and Topology Management Using Windows PowerShell (Level 200)
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4c233be8905c61e66d6b1b3226d149acab96ef23
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711830"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Advanced Active Directory Replication and Topology Management Using Windows PowerShell (Level 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explican más detalladamente los nuevos cmdlets de administración de topología y replicación de AD DS y se incluyen más ejemplos relacionados. Para obtener una introducción, consulte [Introducción a la administración de la replicación y la topología de Active Directory con el nivel 100 de Windows PowerShell &#40;&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).

1. [Introducción](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)

2. [Replicación y metadatos](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)

3. [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)

4. [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)

5. [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)

6. [Get-ADReplicationQueueOperation y Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)

7. [Sync-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)

8. [Topología](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)

## <a name="introduction"></a><a name="BKMK_Intro"></a>Introducción
Windows Server 2012 amplía el módulo de Active Directory para Windows PowerShell con 25 nuevos cmdlets con los que se administra la replicación y la topología de bosques. Antes de esto, se forzó el uso de los nombres genéricos **\* -AdObject** o la llamada a funciones de .net.

Al igual que sucede con todos los cmdlets de Windows PowerShell para Active Directory, esta nueva funcionalidad requiere que se instale el [Servicio de administración de la puerta de enlace de Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) en un controlador de dominio como mínimo (aunque lo ideal sería hacerlo en todos los controladores de dominio).

En la siguiente tabla se recogen los nuevos cmdlets de topología y replicación que se han agregado al módulo de Windows PowerShell para Active Directory.

| Cmdlet | Explicación |
|--|--|
| Get-ADReplicationAttributeMetadata | Devuelve los metadatos de replicación de atributo de un objeto. |
| Get-ADReplicationConnection | Devuelve los detalles de objeto de conexión del controlador de dominio. |
| Get-ADReplicationFailure | Devuelve el error de replicación más reciente de un controlador de dominio. |
| Get-ADReplicationPartnerMetadata | Devuelve la configuración de replicación de un controlador de dominio. |
| Get-ADReplicationQueueOperation | Devuelve el trabajo pendiente de cola de replicación actual. |
| Get-ADReplicationSite | Devuelve información del sitio. |
| Get-ADReplicationSiteLink | Devuelve información del vínculo del sitio. |
| Get-ADReplicationSiteLinkBridge | Devuelve información del puente de vínculos a sitios. |
| Get-ADReplicationSubnet | Devuelve información de subred de AD. |
| Get-ADReplicationUpToDatenessVectorTable | Devuelve el vector UTD de un controlador de dominio. |
| Get-ADTrust | Devuelve información sobre una confianza entre dominios o entre bosques. |
| New-ADReplicationSite | Crea un sitio. |
| New-ADReplicationSiteLink | Crea un vínculo de sitio. |
| New-ADReplicationSiteLinkBridge | Crea un puente de vínculos a sitios. |
| New-ADReplicationSubnet | Crea una subred de AD. |
| Remove-ADReplicationSite | Elimina un sitio. |
| Remove-ADReplicationSiteLink | Elimina un vínculo de sitio. |
| Remove-ADReplicationSiteLinkBridge | Elimina un puente de vínculos a sitios. |
| Remove-ADReplicationSubnet | Elimina una subred de AD. |
| Set-ADReplicationConnection | Modifica una conexión. |
| Set-ADReplicationSite | Modifica un sitio. |
| Set-ADReplicationSiteLink | Modifica un vínculo de sitio. |
| Set-ADReplicationSiteLinkBridge | Modifica un puente de vínculos a sitios. |
| Set-ADReplicationSubnet | Modifica una subred de AD. |
| Sync-ADObject | Fuerza la replicación de un único objeto. |

La mayoría de estos cmdlets se basa en Repadmin.exe. Otros cmdlets (que no figuran aquí) controlan características como el control de acceso dinámico y las cuentas de servicio administradas de grupo.

Ejecuta lo siguiente para ver una lista con todos los cmdlets de Windows PowerShell para Active Directory:

```
Get-command -module ActiveDirectory
```

Consulta la ayuda para ver una lista con todos los argumentos de cmdlet de Windows PowerShell para Active Directory. Por ejemplo:

```
Get-help New-ADReplicationSite

```

Usa el cmdlet `Update-Help` para descargar e instalar archivos de ayuda.

### <a name="replication-and-metadata"></a><a name="BKMK_Repl"></a>Replicación y metadatos
Repadmin.exe valida el estado y la coherencia de la replicación de Active Directory. Repadmin.exe ofrece opciones de manipulación de datos muy sencillas (algunos argumentos, por ejemplo, admiten salidas CSV), cuando la automatización normalmente requería el análisis mediante salidas de archivo de texto. El módulo de Active Directory para Windows PowerShell es el primer intento por ofrecer una opción que permita ejercer un control real de los datos devueltos; antes de esto, había que crear scripts o usar herramientas de terceros.

Además, los siguientes cmdlets implementan un nuevo conjunto de parámetros de **Target**, **Scope** y **EnumerationServer**:

- **Get-ADReplicationFailure**

- **Get-ADReplicationPartnerMetadata**

- **Get-ADReplicationUpToDatenessVectorTable**

El argumento **Target** acepta una lista separada por comas de cadenas con las que se distinguen los servidores, sitios, dominios o bosques de destino especificados mediante el argumento **Scope**. \*También se permite un asterisco () y se refiere a todos los servidores del ámbito especificado. Si no se especifica ningún ámbito, significa que todos los servidores del bosque del usuario actual. El argumento **Scope** especifica la latitud de la búsqueda. Los valores que se aceptan son **Server**, **Site**, **Domain** y **Forest**. El argumento **EnumerationServer** especifica el servidor que enumera la lista de controladores de dominio señalados por los argumentos **Target** y **Scope**. Funciona igual que el argumento **Server** y requiere que el servidor especificado ejecute el Servicio web de Active Directory.

Para presentarte los nuevos cmdlets, incluimos aquí algunos escenarios de ejemplo en los que se muestran funciones que son imposibles de conseguir con repadmin.exe (estas ilustraciones dejan claras las posibilidades administrativas). Repasa la ayuda de los cmdlets para conocer los requisitos de uso específicos.

### <a name="get-adreplicationattributemetadata"></a><a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata
Este cmdlet es similar a **repadmin.exe /showobjmeta**. Permite obtener metadatos de replicación, como cuándo ha cambiado un atributo, el controlador de dominio de origen, la versión e información de USN y los datos de atributo. Este cmdlet resulta útil a la hora de auditar dónde y cuándo se ha producido un cambio.

Al contrario que Repadmin, Windows PowerShell proporciona búsquedas flexibles y control de la salida. Por ejemplo, puedes obtener una salida de los metadatos del objeto Admins. del dominio ordenados como una lista legible:

```
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list

```

![Captura de pantalla que muestra la salida de metadatos del objeto Admins. del dominio ordenados como una lista legible.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)

También puedes ordenar los datos de forma que tengan la apariencia de repadmin, en una tabla:

```
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap

```

![Captura de pantalla que muestra los datos organizados para que se parezca a repadmin en una tabla.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)

O, si quieres, puedes obtener los metadatos de una clase de objetos entera; para ello, debes canalizar el cmdlet **Get-Adobject** con un filtro (como, por ejemplo, todos los grupos) y, luego, combinarlo con una fecha concreta. La canalización es un canal que se utiliza entre varios cmdlets para pasar datos. Para ver todos los grupos que se han modificado de algún modo el 13 de enero de 2012:

```
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object
```

![Captura de pantalla que muestra cómo ver todos los grupos modificados de alguna manera el 13 de enero de 2012.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)

Para obtener más información sobre las operaciones de Windows PowerShell con canalizaciones, consulta [Canalizar y la canalización de Windows PowerShell](/previous-versions/windows/it-pro/windows-powershell-1.0/ee176927(v=technet.10)).

También puedes averiguar de qué grupos es miembro Tony Wang y cuándo se modificó el grupo por última vez:

```
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto

```

![Captura de pantalla que muestra cómo averiguar cada grupo que tiene Tony Wang como miembro y cuándo se modificó por última vez.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)

Asimismo, para encontrar todos los objetos que se han restaurado autoritariamente mediante una copia de seguridad de estado del sistema en el dominio, según su versión posterior artificial:

```
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime
```

![Captura de pantalla que muestra cómo buscar todos los objetos que se han restaurado de forma autoritativa mediante una copia de seguridad del estado del sistema en el dominio, según su versión artificialmente alta.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)

O, también, puedes enviar todos los metadatos de usuario a un archivo CSV para examinarlo más adelante en Microsoft Excel:

```
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv
```

### <a name="get-adreplicationpartnermetadata"></a><a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata
Este cmdlet devuelve información sobre la configuración y el estado de la replicación de un controlador de dominio, lo que permite realizar tareas de supervisión, inventario o solución de problemas. Al contrario que Repadmin.exe, el uso de Windows PowerShell hace que solamente se vean los datos que sean importantes y en el formato deseado.

Por ejemplo, el estado de replicación legible de un solo controlador de dominio:

```
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com
```

![Captura de pantalla que muestra cómo obtener el estado de replicación legible de un solo controlador de dominio.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)

O, también, la última vez que un controlador de dominio realizó una replicación de entrada y de sus asociados, en formato tabular:

```
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto
```

![Captura de pantalla que muestra la última vez que un controlador de dominio ha replicado de entrada y sus asociados, en formato de tabla.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)

O, asimismo, puedes ponerte en contacto con todos los controladores de dominio del bosque y mostrar aquellos en los que el último intento de replicación no se realizó por algún motivo:

```
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto

```

![Captura de pantalla que muestra cómo ponerse en contacto con todos los controladores de dominio del bosque y mostrar cualquiera de los que se ha producido un error en el último intento de replicación por cualquier motivo.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)

### <a name="get-adreplicationfailure"></a><a name="BKMK_ReplFail"></a>Get-ADReplicationFailure
Este cmdlet sirve para devolver información sobre los errores de replicación más recientes. Se parece a **Repadmin.exe /showreplsum**, si bien, de nuevo, el control es mucho mayor gracias a Windows PowerShell.

Así, por ejemplo, puedes obtener los errores más recientes del controlador de dominio y los asociados con los que no se pudo establecer el contacto:

```
Get-ADReplicationFailure dc1.corp.contoso.com
```

![Captura de pantalla que muestra cómo puede devolver los errores más recientes de un controlador de dominio y los asociados con los que no se pudo establecer contacto.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)

También puedes obtener una vista de tabla con todos los servidores de un sitio lógico de AD específico, ordenados de forma que se vean con mayor facilidad y con únicamente los datos más importantes:

```
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto

```

![Captura de pantalla que muestra cómo devolver una vista de tabla para todos los servidores de un sitio lógico de AD específico, ordenado para facilitar su visualización y que solo contiene los datos más importantes.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)

### <a name="get-adreplicationqueueoperation-and-get-adreplicationuptodatenessvectortable"></a><a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation y Get-ADReplicationUpToDatenessVectorTable
Estos dos cmdlets devuelven más aspectos del nivel de actualización del controlador de dominio, lo que engloba información sobre la replicación pendiente y el vector de versión.

### <a name="sync-adobject"></a><a name="BKMK_Sync"></a>Sync-ADObject
Este cmdlet es similar a ejecutar **Repadmin.exe /replsingleobject**. Es muy práctico cuando se realizan cambios que requieren replicación fuera de banda, especialmente cuando se quiere solucionar un problema.

Si, por ejemplo, alguien elimina la cuenta de usuario del consejero delegado y la recupera después de la Papelera de reciclaje de Active Directory, seguramente quieras que se replique de inmediato en todos los controladores de dominio. Asimismo, lo más probable es que prefieras que esto suceda sin forzar la replicación de todos los demás cambios de objeto que hayan tenido lugar. Al fin y al cabo, para eso están las programaciones de replicaciones: para evitar una sobrecarga de los vínculos WAN.

```
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}

```

![Captura de pantalla que muestra cómo replicar una cuenta eliminada de la papelera de reciclaje de Active Directory en todos los controladores de dominio sin forzar la replicación de los demás cambios de objeto realizados.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)

### <a name="topology"></a><a name="BKMK_Topo"></a>Topología
Si bien Repadmin.exe es adecuado para obtener determinados datos de la topología de replicación (como los sitios, los vínculos de sitio, los puentes de vínculos a sitios y las conexiones), carece de un conjunto completo de argumentos para realizar cambios. De hecho, nunca ha habido una utilidad de Windows incluida de forma predeterminada y que admita scripts que se haya diseñado expresamente para que los administradores puedan crear y modificar la topología de AD DS. A medida que Active Directory ha ido evolucionando en millones de entornos de clientes, ha surgido la clara necesidad de modificar la información lógica de Active Directory de arriba a abajo.

Por ejemplo, tras una rápida expansión de nuevas sucursales, a lo que se suma la consolidación de las ya existentes, probablemente tengas un centenar de cambios de sitios que realizar según las ubicaciones físicas, los cambios de red y los nuevos requisitos de capacidad. En lugar de usar Dssites.msc y Adsiedit.msc para efectuar tales cambios, puedes automatizarlos. Esto resulta especialmente interesante cuando comienzas con una hoja de cálculo de datos que te han proporcionado los equipos de red y de instalaciones.

Los cmdlets **Get \\ -Adreplication** _ devuelven información sobre la topología de replicación y son útiles para la canalización en los cmdlets _*set-Adreplication \\* *_ de forma masiva. Los _cmdlets *Get** no cambian los datos, solo muestran datos o crean objetos de sesión de Windows PowerShell que se pueden canalizar a cmdlets **set-Adreplication \\**_ . Los cmdlets _ *New** y **Remove** son útiles para crear o quitar Active Directory objetos de topología.

Por ejemplo, se pueden crear sitios mediante un archivo CSV:

```
import-csv -path C:\newsites.csv | new-adreplicationsite
```

![Captura de pantalla que muestra la interfaz del Bloc de notas.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)

![Captura de pantalla que muestra cómo crear sitios nuevos con un archivo CSV.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)

También puedes crear un vínculo entre dos sitios con un intervalo de replicación y costo de sitio personalizados:

```
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15
```

![Captura de pantalla que muestra cómo crear un nuevo vínculo de sitio entre dos sitios existentes con un intervalo de replicación personalizado y el costo del sitio.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)

O, si lo deseas, puedes buscar todos los sitios del bosque y reemplazar sus atributos **Options** por la marca que permite la notificación de cambios entre sitios y, así, poder replicar a la máxima velocidad con compresión:

```
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}
```

![Administración avanzada con PowerShell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)

> [!IMPORTANT]
> Establece **-bor 5** para deshabilitar la compresión también en esos vínculos de sitio.

Opcionalmente, puedes buscar todos los sitios en los que faltan asignaciones de subred y, de este modo, poder conciliar la lista con las subredes reales de esas ubicaciones:

```
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name
```

![Captura de pantalla que muestra cómo buscar todos los sitios en los que faltan asignaciones de subred, con el fin de conciliar la lista con las subredes reales de esas ubicaciones.](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)

## <a name="see-also"></a>Consulte también
[Introducción a la administración de la replicación y la topología de Active Directory con el nivel 100 de &#40;de Windows PowerShell&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)

