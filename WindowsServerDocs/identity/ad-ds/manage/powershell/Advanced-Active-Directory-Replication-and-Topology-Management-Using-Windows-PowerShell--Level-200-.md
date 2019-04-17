---
ms.assetid: fe05e52c-cbf8-428b-8176-63407991042f
title: "Avanzadas de réplica de Active Directory y administración de la topología mediante Windows PowerShell (nivel 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1e05616b4b594ae54fcaa3ec6496c0917ecde38b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-active-directory-replication-and-topology-management-using-windows-powershell-level-200"></a>Avanzadas de réplica de Active Directory y administración de la topología mediante Windows PowerShell (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica la nueva replicación de AD DS y los cmdlets de administración de la topología con más detalle y proporciona ejemplos adicionales. Para obtener una introducción, consulta [Introducción a la replicación de Active Directory y usando Windows PowerShell de administración de topología & #40; Nivel 100 & #41; ](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md).  
  
1.  [Introducción](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Intro)  
  
2.  [Replicación y metadatos](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Repl)  
  
3.  [Get-ADReplicationAttributeMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplAttrMD)  
  
4.  [Get-ADReplicationPartnerMetadata](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_PartnerMD)  
  
5.  [Get-ADReplicationFailure](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplFail)  
  
6.  [Get-ADReplicationQueueOperation y Get ADReplicationUpToDatenessVectorTable](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_ReplQueue)  
  
7.  [ID de sincronización](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Sync)  
  
8.  [Topología](../../../ad-ds/manage/powershell/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-.md#BKMK_Topo)  
  
## <a name="BKMK_Intro"></a>Introducción  
Windows Server 2012, se amplía el módulo de Active Directory para Windows PowerShell con 25 nuevos cmdlets para administrar la topología de bosque y de replicación. Antes de esto, se vio obligado a usar la clase genérica **\*-AdObject** sustantivos o funciones de llamada de. NET.  
  
Como todos los cmdlets de Windows PowerShell de Active Directory, esta nueva funcionalidad requiere la instalación de la [el servicio de puerta de enlace de administración de Active Directory](https://www.microsoft.com/download/details.aspx?displaylang=en&id=2852) al menos un controlador de dominio (y preferiblemente, todos los controladores de dominio).  
  
La siguiente tabla enumera la replicación nuevo y los cmdlets de topología agregado al módulo de Active Directory de Windows PowerShell.  
  
|||  
|-|-|  
|Cmdlet|Explicación|  
|Get-ADReplicationAttributeMetadata|Devuelve metadatos de replicación de un objeto de atributo|  
|Get-ADReplicationConnection|Devuelve detalles del objeto de conexión del controlador de dominio|  
|Get-ADReplicationFailure|Devuelve el más reciente error de replicación de un controlador de dominio|  
|Get-ADReplicationPartnerMetadata|Devuelve la configuración de replicación de un controlador de dominio|  
|Get-ADReplicationQueueOperation|Devuelve el registro de cola de replicación actual|  
|Get-ADReplicationSite|Devuelve la información del sitio|  
|Get-ADReplicationSiteLink|Devuelve la información de vínculo del sitio|  
|Get-ADReplicationSiteLinkBridge|Devuelve la información del sitio vínculo puente|  
|Get-ADReplicationSubnet|Devuelve información de subred de anuncios|  
|Get-ADReplicationUpToDatenessVectorTable|Devuelve el vector UTD para un controlador de dominio|  
|Get-ADTrust|Devuelve información acerca de una relación de confianza entre dominio o entre bosque|  
|Nueva ADReplicationSite|Crea un nuevo sitio|  
|Nueva ADReplicationSiteLink|Crea un nuevo vínculo de sitio|  
|Nueva ADReplicationSiteLinkBridge|Crea un nuevo puente|  
|Nueva ADReplicationSubnet|Crea una nueva subred de anuncios|  
|Quitar ADReplicationSite|Elimina un sitio|  
|Quitar ADReplicationSiteLink|Elimina un vínculo de sitio|  
|Quitar ADReplicationSiteLinkBridge|Elimina un puente|  
|Quitar ADReplicationSubnet|Elimina una subred de anuncios|  
|Conjunto ADReplicationConnection|Modifica una conexión|  
|Conjunto ADReplicationSite|Modifica un sitio|  
|Conjunto ADReplicationSiteLink|Modifica un vínculo de sitio|  
|Conjunto ADReplicationSiteLinkBridge|Modifica un puente|  
|Conjunto ADReplicationSubnet|Modifica una subred de anuncios|  
|ID de sincronización|Fuerza la duplicación de un solo objeto|  
  
La mayoría de estos cmdlets tienen su base en Repadmin.exe. Otros cmdlets (no aparece) administran las características como Control de acceso dinámico y cuentas de servicio administradas de grupo.  
  
Para obtener una lista completa de todos los cmdlets de Active Directory de Windows PowerShell, ejecuta:  
  
```  
Get-command -module ActiveDirectory  
```  
  
Para obtener una lista completa de todos los argumentos de cmdlet de Windows PowerShell de Active Directory, hacer referencia a la Ayuda. Por ejemplo:  
  
```  
Get-help New-ADReplicationSite  
  
```  
  
Usa el `Update-Help` cmdlet para descargar e instalar los archivos de ayuda  
  
### <a name="BKMK_Repl"></a>Replicación y metadatos  
Repadmin.exe valida el estado y la coherencia de réplica de Active Directory. Repadmin.exe ofrece opciones de manipulación de datos simple: algunos argumentos admiten salidas CSV, por ejemplo - pero automatización requerido por lo general, análisis de los resultados del archivo de texto. El módulo de Active Directory para Windows PowerShell es el primer intento de ofrecer una opción que permite el control real sobre los datos devueltos; antes de esto, tenías que crear scripts o usar herramientas de terceros.  
  
Además, los siguientes cmdlets implementar un nuevo conjunto de parámetro de **destino**, **ámbito**, y **EnumerationServer**:  
  
-   **Get-ADReplicationFailure**  
  
-   **Get-ADReplicationPartnerMetadata**  
  
-   **Get-ADReplicationUpToDatenessVectorTable**  
  
La **destino** argumento acepta una lista separada por comas de cadenas que identifican los servidores de destino, sitios, dominios o bosques especificados por el **ámbito** argumento. Un asterisco (\ *) también es aceptable y significa todos los servidores dentro del ámbito especificado. Si no se especifica ningún ámbito, implica todos los servidores en el bosque del usuario actual. La **ámbito** argumento especifica la latitud de la búsqueda. Los valores aceptables son **Server**, **sitio**, **dominio**, y **bosque**. La **EnumerationServer** especifica el servidor que enumera la lista de controladores de dominio especificado en **destino** y **ámbito**. Funciona igual que la **Server** argumento y requiere que el servidor especificado que se ejecuta el servicio Web de Active Directory.  
  
Para presentar los cmdlets de nuevo, estos son algunos escenarios de ejemplo que muestra las funcionalidades imposibles a repadmin.exe; Gracias a estas ilustraciones, las posibilidades administrativas están visible. Revisa la Ayuda de cmdlet de requisitos de uso específicos.  
  
### <a name="BKMK_ReplAttrMD"></a>Get-ADReplicationAttributeMetadata  
Es similar a este cmdlet **repadmin.exe /showobjmeta**. Permite devolver metadatos de replicación, como cuando cambia un atributo, el controlador de dominio de origen, la versión e información USN y datos del atributo. Este cmdlet es útil para la auditoría donde y cuando se ha producido un cambio.  
  
A diferencia de Repadmin, búsqueda de Windows PowerShell ofrece flexible y salida de control. Por ejemplo, puedes trasladar los metadatos del objeto administradores de dominio, ordenado como una lista de lectura:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-list  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMd.png)  
  
Como alternativa, puedes organizar los datos de aspecto repadmin, en una tabla:  
  
```  
Get-ADReplicationAttributeMetadata -object "cn=domain admins,cn=users,dc=corp,dc=contoso,dc=com" -server dc1.corp.contoso.com -showalllinkedvalues | format-table -wrap  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdTable.png)  
  
Como alternativa, puedes obtener metadatos para toda una clase de objetos, por la canalización la **Get-ID** cmdlet con un filtro, por ejemplo, todos los grupos - combinar que con una fecha específica. La canalización es un canal utilizado entre varios cmdlets para pasar datos. Para ver todos los grupos de modificación de alguna manera el 13 de enero de 2012:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.lastoriginatingchangetime -like "*1/13/2012*" -and $_.attributename -eq "name"} | format-table object  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdClass.png)  
  
Para obtener más información acerca de las operaciones de Windows PowerShell más con canalizaciones, consulta [diseño y la canalización de Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Como alternativa, para obtener información sobre cada grupo que tiene Tony Wang como miembro, y cuando se modificó por última vez el grupo:  
  
```  
get-adobject -filter 'objectclass -eq "group"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | where-object {$_.attributevalue -like "*tony wang*"} | format-table object,LastOriginatingChangeTime,version -auto  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter.png)  
  
Como alternativa buscar todos los objetos de forma autorizada restaura mediante una copia de seguridad de estado del sistema en el dominio, en función de su versión artificialmente alto:  
  
```  
get-adobject -filter 'objectclass -like "*"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com | where-object {$_.version -gt "100000" -and $_.attributename -eq "name"} | format-table object,LastOriginatingChangeTime  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplAttrMdFilter2.png)  
  
Como alternativa, enviar todos los metadatos de usuario en un archivo CSV para su análisis posterior en Microsoft Excel:  
  
```  
get-adobject -filter 'objectclass -eq "user"' | Get-ADReplicationAttributeMetadata -server dc1.corp.contoso.com -showalllinkedvalues | export-csv allgroupmetadata.csv  
```  
  
### <a name="BKMK_PartnerMD"></a>Get-ADReplicationPartnerMetadata  
Este cmdlet devuelve información acerca de la configuración y el estado de replicación de un controlador de dominio, lo que permite supervisar, realizar un inventario o solucionar problemas. A diferencia de Repadmin.exe, mediante Windows PowerShell significa que ver solamente los datos que son importantes para TI, en el formato que quieras.  
  
Por ejemplo, el estado de replicación legible de un controlador de dominio:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMd.png)  
  
Como alternativa, la última vez duplicó un controlador de dominio de entrada y sus asociados, en una tabla de formato:  
  
```  
Get-ADReplicationPartnerMetadata -target dc1.corp.contoso.com | format-table lastreplicationattempt,lastreplicationresult,partner -auto  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdTable.png)  
  
Como alternativa, ponte en contacto con todos los controladores de dominio en el bosque y mostrar cualquier error cuya última replicación intento por algún motivo:  
  
```  
Get-ADReplicationPartnerMetadata -target * -scope server | where {$_.lastreplicationresult -ne "0"} | ft server,lastreplicationattempt,lastreplicationresult,partner -auto  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplPartnerMdFail.png)  
  
### <a name="BKMK_ReplFail"></a>Get-ADReplicationFailure  
Este cmdlet puede usarse para devuelve información acerca de los errores recientes de replicación. Es análoga al **Repadmin.exe /showreplsum**, pero de nuevo, con mucha más control gracias a Windows PowerShell.  
  
Por ejemplo, puede devolver errores más recientes de un controlador de dominio y los socios que no pudo ponerse en contacto con:  
  
```  
Get-ADReplicationFailure dc1.corp.contoso.com  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFail.png)  
  
Como alternativa, puede devolver una vista de tabla para todos los servidores en un sitio lógico de anuncios específico, ordenado para sea más fácil de ver y que contenga solo los datos más importantes:  
  
```  
Get-ADReplicationFailure -scope site -target default-first-site-name | format-table server,firstfailuretime,failurecount,lasterror,partner -auto  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSGetADReplFailScoped.png)  
  
### <a name="BKMK_ReplQueue"></a>Get-ADReplicationQueueOperation y Get ADReplicationUpToDatenessVectorTable  
Dos de estos cmdlets devuelve más aspectos de controlador de dominio "hasta dateness", que incluye pendientes replicación e información de vector de versión.  
  
### <a name="BKMK_Sync"></a>ID de sincronización  
Es similar a que se ejecuta este cmdlet **Repadmin.exe /replsingleobject**. Es muy útil cuando se realizan cambios que requieren de replicación de banda, especialmente para solucionar un problema.  
  
Por ejemplo, si alguien elimina la cuenta de usuario del director general y, a continuación, restaura con la Papelera de reciclaje de Active Directory, es recomendable que se replique inmediatamente a todos los controladores de dominio. Probablemente también quieras hacerlo sin necesidad de duplicación de todos los otros objetos cambios realizados; Después de todo, que es el motivo de tener una programación de replicación - para evitar la sobrecarga de los vínculos WAN.  
  
```  
Get-ADDomainController -filter * | foreach {Sync-ADObject -object "cn=tony wang,cn=users,dc=corp,dc=contoso,dc=com" -source dc1 -destination $_.hostname}  
  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSSyncAD.png)  
  
### <a name="BKMK_Topo"></a>Topología  
Mientras Repadmin.exe es apropiado para devolver información acerca de la topología de replicación como sitios, vínculos a sitios, puentes y conexiones, no tiene un completo conjunto de argumentos para realizar cambios. De hecho, hay nunca ha diseñada específicamente para que los administradores crear y modificar la topología de AD DS que permite ejecutar script, incluidos utilidad de Windows. Como Active Directory ha evolucionado en millones de entornos de cliente, la necesidad de forma masiva modificar Active Directory información lógica se hace evidente.  
  
Por ejemplo, después de una rápida expansión de nuevo las sucursales, combinada con la consolidación de otros, podría tener cien cambios en el sitio que basándose en ubicaciones físicas, los cambios de red y los nuevos requisitos de capacidad. En lugar de usar Dssites.msc y Adsiedit.exe para realizar cambios, puedes automatizar. Esto es especialmente atractiva cuando se inicia con una hoja de cálculo de los datos proporcionados por los equipos de red y las instalaciones.  
  
La **obtener-Adreplication\ *** cmdlets devuelven información acerca de la topología de replicación y son útiles para la canalización a la **establecer-Adreplication\ *** cmdlets en masa. **Obtener** cmdlets no modifican los datos, solo muestren los datos o para crear Windows PowerShell sesión objetos que pueden canalización a **establecer-Adreplication\ *** cmdlets. La **nueva** y **quitar** cmdlets son útiles para la creación o eliminación de objetos de la topología de Active Directory.  
  
Por ejemplo, puedes crear nuevos sitios con un archivo CSV:  
  
```  
import-csv -path C:\newsites.csv | new-adreplicationsite  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewSitesCSV.png)  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSImportCSV.png)  
  
Como alternativa, crea un nuevo vínculo entre dos sitios existentes con un coste de intervalo y el sitio de duplicación personalizada:  
  
```  
new-adreplicationsitelink -name "chicago<-->waukegan" -sitesincluded chicago,waukegan -cost 50 -replicationfrequencyinminutes 15  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSite.png)  
  
Como alternativa, encontrar todos los sitios en el bosque y reemplaza su **opciones** atributos con el indicador para habilitar entre sitios notificación de cambios, para poder replicar a máxima velocidad con compresión:  
  
```  
get-adreplicationsitelink -filter * | set-adobject -replace @{options=$($_.options -bor 1)}  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteLink.gif)  
  
> [!IMPORTANT]  
> Establecer **- BO 5** para deshabilitar la compresión en los vínculos del sitio.  
  
Como alternativa, buscar todos los sitios que faltan las asignaciones de subred, para conciliar la lista con las subredes reales de las ubicaciones:  
  
```  
get-adreplicationsite -filter * -property subnets | where-object {!$_.subnets -eq "*"} | format-table name  
```  
  
![Administración avanzada con powershell](media/Advanced-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-200-/ADDS_PSNewADReplSiteFiltrer.png)  
  
## <a name="see-also"></a>Consulta también  
[Introducción a la réplica de Active Directory y administración de la topología mediante Windows PowerShell & #40; Nivel 100 & #41;](../../../ad-ds/manage/powershell/Introduction-to-Active-Directory-Replication-and-Topology-Management-Using-Windows-PowerShell--Level-100-.md)  
  

