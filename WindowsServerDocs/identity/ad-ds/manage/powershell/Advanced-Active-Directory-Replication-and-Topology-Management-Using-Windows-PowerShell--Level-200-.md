---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: Administración de la replicación y la topología de Active Directory con Windows PowerShell (nivel 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d56bc89189c3b17367549aeb076633a6ea0e1007
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442741"
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Administración de la replicación y la topología de Active Directory con Windows PowerShell (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se explican más detalladamente los nuevos cmdlets de administración de topología y replicación de AD DS y se incluyen más ejemplos relacionados. Para obtener una introducción, consulte [Introducción a la replicación de Active Directory y topología de administración mediante Windows PowerShell &#40;nivel 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introducción](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Replicación y metadatos](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation y Get-ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [Sync-ADObject](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topología](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Introducción  
Windows Server 2012 amplía el módulo de Active Directory para Windows PowerShell con 25 nuevos cmdlets con los que se administra la replicación y la topología de bosques. Antes de esto, se veían obligados a usar el tipo genérico  **\*- AdObject** sustantivos o llaman a funciones. NET.  
  
Al igual que sucede con todos los cmdlets de Windows PowerShell para Active Directory, esta nueva funcionalidad requiere que se instale el [Servicio de administración de la puerta de enlace de Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) en un controlador de dominio como mínimo (aunque lo ideal sería hacerlo en todos los controladores de dominio).  
  
En la siguiente tabla se recogen los nuevos cmdlets de topología y replicación que se han agregado al módulo de Windows PowerShell para Active Directory.  
  
|||  
|-|-|  
|Cmdlet|Explicación|  
|Get-ADReplicationAttributeMetadata|Devuelve los metadatos de replicación de atributo de un objeto.|  
|Get-ADReplicationConnection|Devuelve los detalles de objeto de conexión del controlador de dominio.|  
|Get-ADReplicationFailure|Devuelve el error de replicación más reciente de un controlador de dominio.|  
|Get-ADReplicationPartnerMetadata|Devuelve la configuración de replicación de un controlador de dominio.|  
|Get-ADReplicationQueueOperation|Devuelve el trabajo pendiente de cola de replicación actual.|  
|Get-ADReplicationSite|Devuelve información del sitio.|  
|Get-ADReplicationSiteLink|Devuelve información del vínculo del sitio.|  
|Get-ADReplicationSiteLinkBridge|Devuelve información del puente de vínculos a sitios.|  
|Get-ADReplicationSubnet|Devuelve información de subred de AD.|  
|Get-ADReplicationUpToDatenessVectorTable|Devuelve el vector UTD de un controlador de dominio.|  
|Get-ADTrust|Devuelve información sobre una confianza entre dominios o entre bosques.|  
|New-ADReplicationSite|Crea un sitio.|  
|New-ADReplicationSiteLink|Crea un vínculo de sitio.|  
|New-ADReplicationSiteLinkBridge|Crea un puente de vínculos a sitios.|  
|New-ADReplicationSubnet|Crea una subred de AD.|  
|Remove-ADReplicationSite|Elimina un sitio.|  
|Remove-ADReplicationSiteLink|Elimina un vínculo de sitio.|  
|Remove-ADReplicationSiteLinkBridge|Elimina un puente de vínculos a sitios.|  
|Remove-ADReplicationSubnet|Elimina una subred de AD.|  
|Set-ADReplicationConnection|Modifica una conexión.|  
|Set-ADReplicationSite|Modifica un sitio.|  
|Set-ADReplicationSiteLink|Modifica un vínculo de sitio.|  
|Set-ADReplicationSiteLinkBridge|Modifica un puente de vínculos a sitios.|  
|Set-ADReplicationSubnet|Modifica una subred de AD.|  
|Sync-ADObject|Fuerza la replicación de un único objeto.|  
  
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
  
### <a name="BKMK_Repl"></a>Replicación y metadatos  
Repadmin.exe valida el estado y la coherencia de la replicación de Active Directory. Repadmin.exe ofrece opciones de manipulación de datos muy sencillas (algunos argumentos, por ejemplo, admiten salidas CSV), cuando la automatización normalmente requería el análisis mediante salidas de archivo de texto. El módulo de Active Directory para Windows PowerShell es el primer intento por ofrecer una opción que permita ejercer un control real de los datos devueltos; antes de esto, había que crear scripts o usar herramientas de terceros.  
  
Además, los siguientes cmdlets implementan un nuevo conjunto de parámetros de **Target**, **Scope** y **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
El argumento **Target** acepta una lista separada por comas de cadenas con las que se distinguen los servidores, sitios, dominios o bosques de destino especificados mediante el argumento **Scope** . Un asterisco (\*) también es admisible y significa que todos los servidores dentro del ámbito especificado. Si no se especifica ningún ámbito, implica todos los servidores en el bosque del usuario actual. El argumento **Scope** especifica la latitud de la búsqueda. Los valores que se aceptan son **Server**, **Site**, **Domain** y **Forest**. El argumento **EnumerationServer** especifica el servidor que enumera la lista de controladores de dominio señalados por los argumentos **Target** y **Scope**. Funciona igual que el argumento **Server** y requiere que el servidor especificado ejecute el Servicio web de Active Directory.  
  
Para presentarte los nuevos cmdlets, incluimos aquí algunos escenarios de ejemplo en los que se muestran funciones que son imposibles de conseguir con repadmin.exe (estas ilustraciones dejan claras las posibilidades administrativas). Repasa la ayuda de los cmdlets para conocer los requisitos de uso específicos.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Este cmdlet es similar a **repadmin.exe /showobjmeta**. Permite obtener metadatos de replicación, como cuándo ha cambiado un atributo, el controlador de dominio de origen, la versión e información de USN y los datos de atributo. Este cmdlet resulta útil a la hora de auditar dónde y cuándo se ha producido un cambio.  
  
Al contrario que Repadmin, Windows PowerShell proporciona búsquedas flexibles y control de la salida. Por ejemplo, puedes obtener una salida de los metadatos del objeto Admins. del dominio ordenados como una lista legible:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
También puedes ordenar los datos de forma que tengan la apariencia de repadmin, en una tabla:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
O, si quieres, puedes obtener los metadatos de una clase de objetos entera; para ello, debes canalizar el cmdlet **Get-Adobject** con un filtro (como, por ejemplo, todos los grupos) y, luego, combinarlo con una fecha concreta. La canalización es un canal que se utiliza entre varios cmdlets para pasar datos. Para ver todos los grupos que se han modificado de algún modo el 13 de enero de 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Para obtener más información sobre las operaciones de Windows PowerShell con canalizaciones, consulta [Canalizar y la canalización de Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
También puedes averiguar de qué grupos es miembro Tony Wang y cuándo se modificó el grupo por última vez:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Asimismo, para encontrar todos los objetos que se han restaurado autoritariamente mediante una copia de seguridad de estado del sistema en el dominio, según su versión posterior artificial:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
O, también, puedes enviar todos los metadatos de usuario a un archivo CSV para examinarlo más adelante en Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Este cmdlet devuelve información sobre la configuración y el estado de la replicación de un controlador de dominio, lo que permite realizar tareas de supervisión, inventario o solución de problemas. Al contrario que Repadmin.exe, el uso de Windows PowerShell hace que solamente se vean los datos que sean importantes y en el formato deseado.  
  
Por ejemplo, el estado de replicación legible de un solo controlador de dominio:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
O, también, la última vez que un controlador de dominio realizó una replicación de entrada y de sus asociados, en formato tabular:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
O, asimismo, puedes ponerte en contacto con todos los controladores de dominio del bosque y mostrar aquellos en los que el último intento de replicación no se realizó por algún motivo:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Este cmdlet sirve para devolver información sobre los errores de replicación más recientes. Se parece a **Repadmin.exe /showreplsum**, si bien, de nuevo, el control es mucho mayor gracias a Windows PowerShell.  
  
Así, por ejemplo, puedes obtener los errores más recientes del controlador de dominio y los asociados con los que no se pudo establecer el contacto:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
También puedes obtener una vista de tabla con todos los servidores de un sitio lógico de AD específico, ordenados de forma que se vean con mayor facilidad y con únicamente los datos más importantes:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation y Get-ADReplicationUpToDatenessVectorTable  
Estos dos cmdlets devuelven más aspectos del nivel de actualización del controlador de dominio, lo que engloba información sobre la replicación pendiente y el vector de versión.  
  
### <a name="BKMK_Sync"></a>Sync-ADObject  
Este cmdlet es similar a ejecutar **Repadmin.exe /replsingleobject**. Es muy práctico cuando se realizan cambios que requieren replicación fuera de banda, especialmente cuando se quiere solucionar un problema.  
  
Si, por ejemplo, alguien elimina la cuenta de usuario del consejero delegado y la recupera después de la Papelera de reciclaje de Active Directory, seguramente quieras que se replique de inmediato en todos los controladores de dominio. Asimismo, lo más probable es que prefieras que esto suceda sin forzar la replicación de todos los demás cambios de objeto que hayan tenido lugar. Al fin y al cabo, para eso están las programaciones de replicaciones: para evitar una sobrecarga de los vínculos WAN.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topología  
Si bien Repadmin.exe es adecuado para obtener determinados datos de la topología de replicación (como los sitios, los vínculos de sitio, los puentes de vínculos a sitios y las conexiones), carece de un conjunto completo de argumentos para realizar cambios. De hecho, nunca ha habido una utilidad de Windows incluida de forma predeterminada y que admita scripts que se haya diseñado expresamente para que los administradores puedan crear y modificar la topología de AD DS. A medida que Active Directory ha ido evolucionando en millones de entornos de clientes, ha surgido la clara necesidad de modificar la información lógica de Active Directory de arriba a abajo.  
  
Por ejemplo, tras una rápida expansión de nuevas sucursales, a lo que se suma la consolidación de las ya existentes, probablemente tengas un centenar de cambios de sitios que realizar según las ubicaciones físicas, los cambios de red y los nuevos requisitos de capacidad. En lugar de usar Dssites.msc y Adsiedit.msc para efectuar tales cambios, puedes automatizarlos. Esto resulta especialmente interesante cuando comienzas con una hoja de cálculo de datos que te han proporcionado los equipos de red y de instalaciones.  
  
El **Get-Adreplication\\** * cmdlets devuelven información sobre la topología de replicación y son útiles para la canalización en el **Set-Adreplication\\** * cmdlets de forma masiva. **Obtener** cmdlets no modifican los datos, solo muestra los datos o para la creación de Windows PowerShell sesión los objetos que se pueden canalizar a **Set-Adreplication\\** * cmdlets. Los cmdlets **New** y **Remove** son de utilidad para crear o quitar objetos de topología de Active Directory.  
  
Por ejemplo, se pueden crear sitios mediante un archivo CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
También puedes crear un vínculo entre dos sitios con un intervalo de replicación y costo de sitio personalizados:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
O, si lo deseas, puedes buscar todos los sitios del bosque y reemplazar sus atributos **Options** por la marca que permite la notificación de cambios entre sitios y, así, poder replicar a la máxima velocidad con compresión:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Establece **-bor 5** para deshabilitar la compresión también en esos vínculos de sitio.  
  
Opcionalmente, puedes buscar todos los sitios en los que faltan asignaciones de subred y, de este modo, poder conciliar la lista con las subredes reales de esas ubicaciones:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Vea también  
[Introducción a la replicación de Active Directory y administración de la topología mediante Windows PowerShell &#40;nivel 100&#41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

