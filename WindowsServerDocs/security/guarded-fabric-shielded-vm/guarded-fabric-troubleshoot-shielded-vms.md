---
title: Solución de problemas de las máquinas virtuales blindadas
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 10/3/2018
ms.openlocfilehash: 13ff0dad1519d394ce74a91efbfcc9e2f237e4a5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850036"
---
# <a name="troubleshoot-shielded-vms"></a>Solución de problemas de las máquinas virtuales blindadas

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

A partir de Windows Server versión 1803, modo de sesión mejorada de la conexión a máquina Virtual (VMConnect) y PS directo se vuelven a habilitar para las máquinas virtuales blindadas completamente. El Administrador de virtualización todavía requiere credenciales de Invitado VM para obtener acceso a la máquina virtual, pero esto facilita un proveedor de hospedaje solucionar problemas de una máquina virtual blindada cuando se infringe su configuración de red.

Para habilitar VMConnect y PS directo para las máquinas virtuales blindadas, simplemente moverlos a un host de Hyper-V que ejecuta Windows Server versión 1803 o posterior. Los dispositivos virtuales que se permite para estas características se volverá a habilitar, automáticamente. Si se mueve una máquina virtual blindada para un host que ejecuta y una versión anterior de Windows Server, se deshabilitarán nuevo VMConnect y directa de PS.

Para los clientes de seguridad que se preocupe si los proveedores de hospedaje tienen acceso a la máquina virtual y deseen volver al comportamiento original, se deben deshabilitar las siguientes características en el sistema operativo invitado:

- Deshabilitar el servicio de PowerShell Direct en la máquina virtual:

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- Solo se puede deshabilitar el modo de sesión mejorada de VMConnect si el sistema operativo invitado es al menos Windows Server 2019 o Windows 10, versión 1809. Agregue la siguiente clave del registro en la máquina virtual para deshabilitar las conexiones de consola de sesión mejorada de VMConnect:

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
