---
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: "Apéndice de administración simplificada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5de7431d0f3fe9a078432b11a63ce996d3abe447
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="simplified-administration-appendix"></a>Apéndice de administración simplificada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
-   [El administrador del servidor agregar servidores diálogo (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)  
  
-   [Estado del servidor de administrador del servidor remoto](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)  
  
-   [Carga del módulo de PowerShell de Windows](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)  
  
-   [ELIMINAR las revisiones de emisión de sistemas operativos anteriores](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)  
  
-   [Cambia la instalación Ntdsutil.exe desde medios](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)  
  
## <a name="BKMK_AddServers"></a>El administrador del servidor agregar servidores diálogo (Active Directory)  

La **agregar servidores** cuadro de diálogo permite buscar servidores de Active Directory por el sistema operativo, usar caracteres comodín y por ubicación. El cuadro de diálogo también permite usar las consultas DNS por nombre de dominio completo o el nombre del prefijo. Estas búsquedas usan protocolos DNS y LDAP nativos implementados a través. NET, no AD Windows PowerShell contra la puerta de enlace de administración de anuncios a través de SOAP - lo que significa que los controladores de dominio con ponerse en contacto con el administrador del servidor pueden ejecutar incluso Windows Server 2003. También puedes importar un archivo con los nombres de servidor para fines de aprovisionamiento.  
  
La búsqueda de Active Directory usa los siguientes filtros LDAP:  
  
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
El administrador del servidor pruebas de accesibilidad de servidor remoto mediante el protocolo de enrutamiento de dirección. No se enumeran los servidores que no responde a las solicitudes ARP, incluso si están en el grupo.  
  
Si responde ARP, conexiones DCOM y WMI se realizan en el servidor para obtener información sobre el estado. Si hay RPC, DCOM y WMI inaccesible, el administrador del servidor no puede administrar completamente el servidor.  
  
## <a name="BKMK_PSLoadModule"></a>Carga del módulo de PowerShell de Windows  
Windows PowerShell 3.0 implementa la carga dinámica de módulos. Con la **Importar módulo** cmdlet normalmente ya no es necesario; en su lugar, carga el módulo simplemente invocar el cmdlet, alias o función automáticamente.  
  
Para ver los módulos cargados, usa el **Get módulo** cmdlet.  
  
```  
Get-Module  
  
```  
  
![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)  
  
Para ver todos los módulos instalados con sus funciones exportadas y cmdlets, usa:  
  
```  
Get-Module -ListAvailable  
  
```  
  
El principal caso para usar la **módulo de importación** comando es cuando se necesita acceso a la "AD:" ya cargado el módulo de unidad virtual de Windows PowerShell y nada más. Por ejemplo, con los siguientes comandos:  
  
```  
import-module activedirectory  
cd ad:  
dir  
  
```  
  
## <a name="BKMK_Rid"></a>ELIMINAR las revisiones de emisión de sistemas operativos anteriores  
Consulta [una actualización está disponible para detectar y prevenir mucho el consumo del conjunto global de RID en un controlador de dominio que ejecute Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).  
  
## <a name="BKMK_IFM"></a>Cambia la instalación Ntdsutil.exe desde medios  
Windows Server 2012 agrega dos opciones adicionales a la herramienta de línea de comandos Ntdsutil.exe para la **IFM (creación de medios IFM)** menú. Estos te permiten crear almacenes IFM sin tener que realizar primero una desfragmentación del archivo exportado NTDS sin conexión. Archivo de base de datos DIT. Cuando el espacio de disco no es primordial, esto ahorra tiempo a crear el IFM.  
  
La siguiente tabla describe los dos elementos de menú nuevos:  
  
|||  
|-|-|  
|Elemento de menú|Explicación|  
|Crear NoDefrag completa %s|Crear medios IFM sin desfragmentar para un controlador de dominio de AD completa o una instancia de AD/LDS en la carpeta %s|  
|Crear Sysvol completa NoDefrag %s|Crear medios IFM con SYSVOL y sin la desfragmentación de un controlador de dominio de AD completa en la carpeta %s|  
  
![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)  
  
![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)  
  


