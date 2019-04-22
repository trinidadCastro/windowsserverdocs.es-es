---
title: Paso 4 comprobar DirectAccess con OTP
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ed49a0a3-1c45-42e5-8f13-cad20c1c1d68
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcc152723ee339c8652c9480647bfb8f438c87d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813246"
---
# <a name="step-4-verify-directaccess-with-otp"></a>Paso 4 comprobar DirectAccess con OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo comprobar que ha configurado correctamente su DirectAccess con la implementación de OTP.
  
### <a name="to-verify-otp-health-on-the-remote-access-server"></a>Para comprobar el estado OTP en el servidor de acceso remoto

1. En el servidor de acceso remoto abierto el **administración de acceso remoto** consola.  

2. En **servidores de acceso remoto** haga clic en el servidor de acceso remoto que se ha configurado para admitir la OTP.  

3. Haga clic en **estado de las operaciones**.  

4. Compruebe que el estado de OTP muestra el icono verde y trabajo.  
  
    > [!NOTE]  
    > El intervalo de actualización de estado de mantenimiento será un máximo de la suma de los valores de la clave del registro HKLM\SYSTEM\CCS\Services\Ramgmtsvc\parameters\HealthRefreshTimeout y **intervalo de tiempo de actividad del servidor** que se estableció el Configuración de acceso remoto.  
  
### <a name="to-verify-access-to-internal-resources-using-otp-authentication"></a>Para comprobar el acceso a recursos internos con autenticación OTP  
  
1.  Conectar un equipo cliente de DirectAccess a la red corporativa y ejecute **gpupdate /force** desde el símbolo del sistema para obtener la directiva de grupo.  
  
2.  Desconectar el equipo cliente de la red corporativa, conectarse a la red externa e intentan tener acceso a los recursos internos. No debe tener acceso a los recursos internos.  
  
3.  En el caso de un token de software, acceder al token de cliente OTP con las instrucciones del proveedor y tenga en cuenta el código del token actual. Cuando se usa un token de hardware, siga las instrucciones del proveedor para la autenticación.  
  
4.  Haga clic en el icono **Conexiones de red** del área de notificación para tener acceso al Administrador de medios de DA.  
  
5.  Haga clic en el **conexión de DirectAccess**y haga clic en **continuar**.  
  
6.  Escriba el código del token se ha indicado anteriormente y haga clic en **Aceptar**. Espere completar la autenticación. El estado de conexión de DirectAccess al área de trabajo ahora estará **conectado**.  
  
7.  Intento de acceder a los recursos internos. Debe poder tener acceso a todos los recursos corporativos.  
  


