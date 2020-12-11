---
description: Más información acerca de cómo solucionar problemas de máquinas virtuales blindadas
title: Solución de problemas de máquinas virtuales blindadas
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 10/3/2018
ms.openlocfilehash: 3f4c0eb81169f9b66170d3332d220233d476ba5f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049213"
---
# <a name="troubleshoot-shielded-vms"></a>Solución de problemas de máquinas virtuales blindadas

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

A partir de Windows Server versión 1803, el modo de sesión mejorada de conexión a máquina virtual (VMConnect) y PS Direct se vuelven a habilitar para las máquinas virtuales protegidas completamente. El administrador de virtualización todavía requiere credenciales de invitado de máquina virtual para obtener acceso a la máquina virtual, pero esto facilita a un anfitrión la solución de problemas de una máquina virtual blindada cuando se interrumpe su configuración de red.

Para habilitar VMConnect y PS Direct para las máquinas virtuales blindadas, simplemente muévalos a un host de Hyper-V que ejecute Windows Server versión 1803 o posterior. Los dispositivos virtuales que permiten estas características se volverán a habilitar automáticamente. Si una máquina virtual blindada se mueve a un host que ejecuta y una versión anterior de Windows Server, VMConnect y PS Direct se deshabilitarán de nuevo.

En el caso de los clientes con seguridad que se preocupan si los anfitriones tienen acceso a la máquina virtual y desean volver al comportamiento original, deben deshabilitarse las siguientes características en el SO invitado:

- Deshabilite el servicio directo de PowerShell en la máquina virtual:

  ```powershell
  Stop-Service vmicvmsession
  Set-Service vmicvmsession -StartupType Disabled
  ```

- El modo de sesión mejorada de VMConnect solo se puede deshabilitar si el SO invitado es como mínimo Windows Server 2019 o Windows 10, versión 1809. Agregue la siguiente clave del registro en la máquina virtual para deshabilitar las conexiones de la consola de sesión mejorada de VMConnect:

  ```
  reg add "HKLM\Software\Microsoft\Virtual Machine\Guest" /v DisableEnhancedSessionConsoleConnection /t REG_DWORD /d 1
  ```
