---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: Anexo de administración simplificada
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 36cdacec27e64586c359146b858a9d68750e5026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858266"
---
# <a name="simplified-administration-appendix"></a>Anexo de administración simplificada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [El administrador del servidor agregar servidores de cuadro de diálogo (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Estado del servidor de administrador del servidor remoto](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Carga de módulos de PowerShell de Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [Las revisiones de emisión de RID para sistemas operativos anteriores](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Ntdsutil.exe Install from Media Changes](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>El administrador del servidor agregar servidores de cuadro de diálogo (Active Directory)  

El **agregar servidores** cuadro de diálogo permite búsquedas en Active Directory para los servidores, por el sistema operativo, con caracteres comodín y por ubicación. El cuadro de diálogo también permite mediante consultas DNS por nombre de dominio completo o el nombre de prefijo. Estas búsquedas usar protocolos DNS y LDAP nativos implementados a través de. NET, no AD Windows PowerShell con la puerta de enlace de administración de AD a través de SOAP - lo que significa que los controladores de dominio puesto en contacto con el administrador del servidor incluso pueden ejecutar Windows Server 2003. También puede importar un archivo con los nombres de servidor para fines de aprovisionamiento.  
  
La búsqueda de Active Directory utiliza los siguientes filtros LDAP:  
  
```  
(&(ObjectCategory=computer)  
  
(&(ObjectCategory=computer)(cn=dc*)(OperatingSystemVersion=6.2*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.1*))  
  
(&(ObjectCategory=computer)(OperatingSystemVersion=6.0*))  
  
(&(ObjectCategory=computer)(|(OperatingSystemVersion=5.2*)(OperatingSystemVersion=5.1*)))  
  
```  
  
La búsqueda de Active Directory devuelve los siguientes atributos:  
  
```  
( dnsHostName )( operatingSystem )( cn )  
  
```  
  
## <a name="BKMK_ServerMgrStatus"></a>Estado del servidor de administrador del servidor remoto  
El administrador del servidor comprueba la accesibilidad del servidor remoto mediante el protocolo de enrutamiento de direcciones. No se muestran todos los servidores no responde a las solicitudes ARP, incluso si se encuentran en el grupo.  
  
Si responde ARP, las conexiones DCOM y WMI se realizan en el servidor para devolver información de estado. Si RPC, DCOM y WMI se encuentran accesibles, el administrador del servidor no puede administrar completamente el servidor.  
  
## <a name="BKMK_PSLoadModule"></a>Carga de módulos de PowerShell de Windows  
Windows PowerShell 3.0 implementa la carga del módulo dinámico. Mediante el **Import-Module** cmdlet normalmente ya no es necesario; en su lugar, basta con invocar el cmdlet, alias o función automáticamente carga el módulo.  
  
Para ver los módulos cargados, use el **Get-Module** cmdlet.  
  
```  
Get-Module  
  
```  
  
![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Para ver todos los módulos instalados con sus funciones exportadas y los cmdlets, use:  
  
```  
Get-Module -ListAvailable  
  
```  
  
El caso principal para usar el **import-module** comando es cuando se necesita acceso a la "AD:" Unidad virtual de Windows PowerShell y nada más, ya ha cargado el módulo. Por ejemplo, mediante los siguientes comandos:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>Las revisiones de emisión de RID para sistemas operativos anteriores  
Consulte [una actualización está disponible para detectar y evitar demasiada consumo del grupo RID global en un controlador de dominio que ejecuta Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Ntdsutil.exe Install from Media Changes  
Windows Server 2012 agrega dos opciones adicionales a la herramienta de línea de comandos de Ntdsutil.exe para el **IFM (creación de medios IFM)** menú. Estos permiten crear almacenes de IFM sin realizar antes una desfragmentación sin conexión del archivo exportado NTDS. Archivo de base de datos DIT. Cuando el espacio en disco no es obligatorio, esto ahorra tiempo a crear el IFM.  
  
En la tabla siguiente se describe los dos nuevos elementos de menú:  
  
|||  
|-|-|  
|Elemento de menú|Explicación|  
|Creación completa NoDefrag %s|Crear medios de IFM prolongado sin desfragmentar un DC de AD completo o una instancia de AD/LDS en la carpeta %s|  
|Creación de Sysvol completo NoDefrag %s|Crear medios de IFM con SYSVOL y sin desfragmentar un DC de AD completa en la carpeta %s|  
  
![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


