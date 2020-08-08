---
title: Solucionar problemas generales
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 911d12088acc5071e18e7e24000364d9fe539250
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87958483"
---
# <a name="troubleshooting-general-issues"></a>Solucionar problemas generales

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene información para la solución de problemas generales relacionados con el acceso remoto.

## <a name="gpo-retrieval-error"></a>Error de recuperación de GPO
**Error recibido**. No se puede recuperar la configuración de GPO del servidor de DirectAccess. Asegúrese de que tiene permisos de edición para el GPO.

La consola de administración de acceso remoto no responde después de recibir este error.

**Causa**

DirectAccess no puede tener acceso al GPO de uno de los puntos de entrada de la implementación y, como resultado, no se puede cargar la configuración.

**Solución**

Asegúrese de que cada punto de entrada de la implementación tenga un GPO correspondiente en su controlador de dominio y compruebe que el usuario que ha iniciado sesión tiene permisos de lectura y escritura para todos los GPO configurados en la implementación de acceso remoto.

Como solución alternativa, use los cmdlets de configuración en lugar de usar la consola de administración de acceso remoto. por ejemplo, mediante `Get-RemoteAccess` y `Get-DAEntryPoint` .

> [!NOTE]
> Este escenario no se produce cuando no está disponible el GPO de servidor del punto de entrada actual.

Puede usar el `Get-DAEntryPointDC` cmdlet para enumerar todos los controladores de dominio que almacenan los GPO de servidor y `Get-DAMultiSite` junto con `Get-RemoteAccess` para recuperar una lista completa de los GPO de servidor en la implementación. Por ejemplo:

```
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {
   @{
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;
        DC = $_.DomainControllerName }
}
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }
```

## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Actualización de cliente de Windows 7 a Windows 8 o 10
**Síntoma**. Después de que un cliente de Windows 7 se actualice a Windows 10 o Windows 8 en una implementación multisitio, la conexión de DirectAccess no está visible en la lista redes.

**Causa**

Los GPO de Windows 7 en una implementación multisitio no contienen la configuración del Asistente para la conectividad de red de Windows 8.

 Los clientes de Windows 7 deben usar el Asistente de conectividad de DirectAccess para supervisar el estado de conectividad de DirectAccess, que requiere una configuración manual independiente en los GPO de cliente de Windows 7. Cuando los clientes de Windows 7 se actualizan a Windows 10 o Windows 8, el Asistente para la conectividad de red no funcionará si se sigue aplicando el GPO de cliente de Windows 7.

**Solución**

Si la configuración del asistente de conectividad de DirectAccess está configurada en los GPO de Windows 7, puede resolver este problema antes de actualizar los equipos cliente mediante la modificación de los GPO de Windows 7 mediante los siguientes cmdlets de PowerShell:

```
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"
```

Si un cliente ya se ha actualizado o el DCA no está configurado, mueva el equipo cliente al grupo de seguridad de Windows 10 o Windows 8.

## <a name="general-cmdlet-errors"></a>Errores generales de cmdlet

-   **Problema 1**

    **Error recibido**. No se puede tener acceso al> de domain_controller de <del controlador de dominio para <SERVER_NAME o entry_point_name>.

    **Causa**

    Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. Cuando el controlador de dominio que administra el GPO de servidor de un punto de entrada no está disponible, las opciones de configuración de acceso remoto no se pueden leer ni modificar.

    **Solución**

    Siga el procedimiento "para cambiar el controlador de dominio que administra los GPO de servidor" descrito en [2,4. Configurar GPO](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).

-   **Problema 2**

    **Error recibido**. No se puede tener acceso al controlador de dominio principal en el> domain_name de <de dominio.

    **Causa**

    Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. Los GPO de cliente se administran en el controlador de dominio principal. En caso de que el controlador de dominio principal no esté disponible, la configuración de Acceso remoto no se puede leer ni alterar.

    **Solución**

    Siga el procedimiento "para transferir el rol de emulador de PDC" descrito en [2,4. Configurar GPO](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).



