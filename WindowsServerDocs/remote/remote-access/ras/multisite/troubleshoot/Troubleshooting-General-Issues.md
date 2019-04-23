---
title: Solucionar problemas generales
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 354ae5e3-bae1-44f9-afd7-7eaba70f2346
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe1fdc4fb5aff2e34555b08d3b2c4347e643085e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831086"
---
# <a name="troubleshooting-general-issues"></a>Solucionar problemas generales

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema contiene información para solucionar problemas generales relacionados con el acceso remoto.  
  
## <a name="gpo-retrieval-error"></a>Error al recuperar el GPO  
**Recibidas el error**. No se puede recuperar la configuración de GPO de servidor de DirectAccess. Asegúrese de que tiene permisos de edición para el GPO.  
  
La consola de administración de acceso remoto no responde después de recibir este error.  
  
**Causa**  
  
DirectAccess no puede acceder a lo GPO de uno de los puntos de entrada en la implementación y como resultado la configuración no se puede cargar.  
  
**Solución**  
  
Asegúrese de que cada punto de entrada en la implementación tiene un GPO correspondiente en su controlador de dominio y compruebe que el usuario ha iniciado la sesión ha leído y permisos de escritura para todos los GPO configurados en la implementación de acceso remoto.  
  
Como alternativa, use los cmdlets de configuración en lugar de usar la consola de administración de acceso remoto; Por ejemplo, mediante `Get-RemoteAccess` y `Get-DAEntryPoint`.  
  
> [!NOTE]  
> Este escenario no se produce cuando el GPO de servidor del punto de entrada actual no está disponible.  
  
Puede usar el `Get-DAEntryPointDC` cmdlet para enumerar todos los controladores de dominio que almacenan los GPO de servidor y `Get-DAMultiSite` junto con `Get-RemoteAccess` para recuperar una lista completa de los GPO de servidor en la implementación. Por ejemplo:  
  
```  
$ServerGpos = Get-DAEntryPointDC | ForEach-Object {   
   @{   
       GpoName = (Get-RemoteAccess -EntryPoint $_.EntryPointName).ServerGpoName;   
        DC = $_.DomainControllerName }   
}  
$ServerGpos | ForEach-Object { $GpoName = $_['GpoName'] ; $DC = $_['DC'] ; Write-Host "Server GPO '$GpoName' on DC '$DC'" }  
```  
  
## <a name="windows-7-to-windows-8-or-10-client-upgrade"></a>Windows 7 a Windows 8 o 10 la actualización de cliente  
**Síntoma**. Después de actualiza un cliente de Windows 7 a Windows 10 o Windows 8 en una implementación multisitio, la conexión de DirectAccess no es visible en la lista de redes.  
  
**Causa**  
  
Los GPO de Windows 7 en una implementación multisitio no contienen la configuración del Asistente de conectividad de red de Windows 8.  
  
 Los clientes de Windows 7 deben usar el Asistente de conectividad de DirectAccess para supervisar su estado de conectividad de DirectAccess que requiere una configuración manual independiente en el GPO de cliente de Windows 7. Cuando se actualizan los clientes de Windows 7 a Windows 10 o Windows 8, el Asistente de conectividad de red no funcionará si todavía se aplica el GPO de cliente de Windows 7.  
  
**Solución**  
  
Si la configuración del Asistente para la conectividad de DirectAccess está configurados en los GPO de Windows 7, puede resolver este problema antes de actualizar los equipos cliente mediante la modificación de los GPO de 7 de Windows con los siguientes cmdlets de PowerShell:  
  
```  
Set-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant" -ValueName "TemporaryValue" -Type Dword -Value 1  
Remove-GPRegistryValue -Name <Windows7GpoName> -Domain <DomainName> -Key "HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\NetworkConnectivityAssistant"  
```  
  
Si ya se ha actualizado un cliente o no está configurado el DCA, mueva el equipo cliente al grupo de seguridad de Windows 10 o Windows 8.  
  
## <a name="general-cmdlet-errors"></a>Errores de general cmdlet  
  
-   **Problema 1**  
  
    **Recibidas el error**. No se puede alcanzar el controlador de dominio < controlador_dominio > para < nombre_servidor o nombre_punto_entrada >.  
  
    **Causa**  
  
    Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. Cuando el controlador de dominio que administra el GPO de servidor de un punto de entrada no está disponible, no se lean o modifiquen los valores de configuración de acceso remoto.  
  
    **Solución**  
  
    Siga el procedimiento "para cambiar el controlador de dominio que administra los GPO de servidor" se describe en [2.4. Configurar GPO](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Issue 2**  
  
    **Recibidas el error**. No se puede alcanzar el controlador de dominio principal en el dominio < nombreDeDominio >.  
  
    **Causa**  
  
    Para lograr que la configuración sea coherente en unas implementación multisitio, es importante asegurarse de que cada GPO se administra mediante un único controlador de dominio. Los GPO de cliente se administran en el controlador de dominio principal. En caso de que el controlador de dominio principal no esté disponible, la configuración de Acceso remoto no se puede leer ni alterar.  
  
    **Solución**  
  
    Siga el procedimiento "para transferir el rol de emulador PDC" descrito en [2.4. Configurar GPO](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  


