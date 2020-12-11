---
description: 'Más información sobre: Apéndice de administración simplificada'
ms.assetid: c911d6c6-98c6-4532-b1db-5724e1ceb96c
title: Anexo de administración simplificada
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: d54ed836b60c4b1551b3a87ff92a50f2b10bda23
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046003"
---
# <a name="simplified-administration-appendix"></a>Anexo de administración simplificada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

-   [Administrador del servidor cuadro de diálogo Agregar servidores (Active Directory)](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_AddServers)

-   [Administrador del servidor el estado del servidor remoto](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_ServerMgrStatus)

-   [Carga del módulo de Windows PowerShell](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_PSLoadModule)

-   [Revisiones de emisión de RID para sistemas operativos anteriores](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_Rid)

-   [Ntdsutil.exe instalar desde cambios de medios](../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)

## <a name="server-manager-add-servers-dialog-active-directory"></a><a name="BKMK_AddServers"></a>Administrador del servidor cuadro de diálogo Agregar servidores (Active Directory)

El cuadro de diálogo **agregar servidores** permite buscar Active Directory servidores, por sistema operativo, mediante caracteres comodín y por ubicación. El cuadro de diálogo también permite el uso de consultas DNS por nombre de dominio completo o por nombre de prefijo. Estas búsquedas usan protocolos DNS y LDAP nativos implementados mediante .NET, no AD Windows PowerShell en la puerta de enlace de administración de AD a través de SOAP, lo que significa que los controladores de dominio a los que se ha puesto en contacto con Administrador del servidor pueden incluso ejecutar Windows Server 2003. También puede importar un archivo con nombres de servidor para fines de aprovisionamiento.

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

## <a name="server-manager-remote-server-status"></a><a name="BKMK_ServerMgrStatus"></a>Administrador del servidor el estado del servidor remoto
Administrador del servidor comprueba la accesibilidad del servidor remoto mediante el protocolo de enrutamiento de direcciones. Los servidores que no responden a las solicitudes ARP no aparecen en la lista, aunque estén en el grupo.

Si ARP responde, las conexiones DCOM y WMI se realizan en el servidor para devolver información de estado. Si no se puede tener acceso a RPC, DCOM y WMI, el administrador del servidor no podrá administrar el servidor por completo.

## <a name="windows-powershell-module-loading"></a><a name="BKMK_PSLoadModule"></a>Carga del módulo de Windows PowerShell
Windows PowerShell 3,0 implementa la carga dinámica de módulos. Normalmente ya no se requiere el uso del cmdlet **Import-Module** ; en su lugar, basta con invocar el cmdlet, el alias o la función para cargar automáticamente el módulo.

Para ver los módulos cargados, use el cmdlet **Get-Module** .

```
Get-Module

```

![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSGetModule.gif)

Para ver todos los módulos instalados con sus funciones y cmdlets exportados, use:

```
Get-Module -ListAvailable

```

El caso principal de usar el comando **Import-Module** es cuando se necesita acceso a la unidad virtual de Windows POWERSHELL "ad:" y ningún otro elemento ya ha cargado el módulo. Por ejemplo, con los siguientes comandos:

```
import-module activedirectory
cd ad:
dir

```

## <a name="rid-issuance-hotfixes-for-previous-operating-systems"></a><a name="BKMK_Rid"></a>Revisiones de emisión de RID para sistemas operativos anteriores
Vea [que hay disponible una actualización para detectar y evitar demasiado consumo del grupo de RID global en un controlador de dominio que ejecuta Windows Server 2008 R2](https://support.microsoft.com/kb/2618669).

## <a name="ntdsutilexe-install-from-media-changes"></a><a name="BKMK_IFM"></a>Ntdsutil.exe instalar desde cambios de medios
Windows Server 2012 agrega dos opciones adicionales a la herramienta de línea de comandos Ntdsutil.exe para el menú **IFM (creación de medios IFM)** . Estos permiten crear almacenes IFM sin realizar primero una desfragmentación sin conexión del NTDS exportado. Archivo de base de datos DIT. Cuando el espacio en disco no es un nivel Premium, se ahorra tiempo al crear el IFM.

En la tabla siguiente se describen los dos nuevos elementos de menú:

|Elemento de menú|Explicación|
|--|--|
|Crear la nodefrag% s completa|Crear medios IFMs sin desfragmentación para un DC de AD completo o una instancia de AD/LDS en la carpeta% s|
|Crear SYSVOL total nodefrag% s|Crear medios IFMs con SYSVOL y sin desfragmentar un DC de AD completo en la carpeta% s|

![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFM.png)

![Administración simplificada](media/Simplified-Administration-Appendix/ADDS_PSIFMComplete.gif)
