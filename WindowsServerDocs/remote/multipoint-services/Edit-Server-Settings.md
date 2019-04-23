---
title: Editar la configuración del servidor
description: Obtenga información sobre la configuración de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 105b10428835d11a0ea0661fe2fa7d57f80a1aba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885186"
---
# <a name="edit-server-settings"></a>Editar la configuración del servidor
Al instalar MultiPoint Services, estableció la configuración para su sistema, incluyendo las opciones de ciertos programas. En este tema se describe la configuración que puede establecer para su sistema MultiPoint Services y se explica cómo editar la configuración.  
  
## <a name="about-multipoint-services-settings"></a>Sobre la configuración de MultiPoint Services  
En la siguiente tabla se describe la diferente configuración que puede cambiar para su sistema MultiPoint Services.  
  
|Opción de MultiPoint Services|Descripción|  
|-----------------------------------------------------------------------------------------|---------------|  
|Permitir que una cuenta tenga varias sesiones|Permite a una cuenta de un solo usuario iniciar sesión de forma simultánea en varias estaciones. Esto puede ser útil en casos como una clase en la que cada estudiante usa una única cuenta compartida. Al usar esta opción, cualquier cambio en los recursos de la cuenta, como las carpetas de documentos o el escritorio, están disponibles para todos los usuarios que hayan iniciado sesión en la misma cuenta.|  
|Allow this computer to be managed remotely (Permitir que este equipo se administre remotamente)|Permite que el equipo que está ejecutando MultiPoint Services para administrarlos con otros sistemas MultiPoint de la red. Si esta opción se selecciona y el equipo que administra está en la misma subred, este equipo aparece en la lista de servidores disponibles para administrar. Si esta opción se selecciona y el equipo que administra está en una subred diferente, el equipo administrador puede seguir administrando este equipo, pero debe especificar la dirección IP del equipo.|
|Allow monitoring of this computer’s desktops (Permitir supervisar los escritorios de este equipo)|Le permite controlar si los escritorios se pueden supervisar en el sistema MultiPoint Services. Si esta opción está desactivada (no seleccionados), escritorios de las estaciones (locales y remotas) que están conectadas al equipo que se está ejecutando MultiPoint Services no se mostrarán en la pestaña Inicio de administrador de MultiPoint (incluido en un equipo diferente si el equipo se va a administración remota).|  
|Always start in console mode (Iniciar siempre en el modo de consola)|Habilita la tecnología RemoteFX, que se ha diseñado para permitir que las sesiones de Escritorio remoto se ejecuten de forma más rápida y eficiente al descargar procesamientos a la CPU y la GPU. Si se conecta a MultiPoint Services mediante un cliente compatible con RemoteFX, puede lograr un mejor rendimiento con esta opción. Las ventajas dependen de las capacidades del servidor y la red. Por ejemplo, esto depende en parte de si el tiempo empleado en realizar procesamientos adicionales para comprimir el flujo de datos es inferior al tiempo que se ahorra al transmitir menos datos.|  
|Do not show privacy notification at first user logon (No mostrar la notificación de privacidad en el primer inicio de sesión de usuario)|Cuando un usuario inicia sesión en una estación MultiPoint por primera vez, se muestra una notificación para informar al usuario de que sus actividades en la estación pueden supervisarse.|  
|Assign a unique IP to each station (Asignar una IP única a cada estación)|Asigna una dirección IP única a cada estación. De manera predeterminada, MultiPoint Services tiene una dirección IP, que se comparte con todas las sesiones que se ejecutan en el sistema. En cambio, esta opción puede provocar algunos problemas de compatibilidad de aplicaciones. Por ejemplo, si una aplicación requiere una dirección IP única, puede que no se ejecute correctamente en MultiPoint Services. Al seleccionar esta opción, también conocida como virtualización de IP, puede resolver este problema.<br /><br />La virtualización de IP también es útil para supervisar las sesiones activas en MultiPoint Services. Algunas herramientas de supervisión informan del uso según la dirección IP. Para habilitar la supervisión de sesión, puede usar la virtualización de IP para asignar una dirección IP única a cada sesión. Tenga en cuenta que, si selecciona esta opción, cada nueva sesión recibe una dirección IP única. Cualquier sesión existente continúa usando la dirección IP compartida hasta que cierra sesión y vuelve a iniciarla.|  
|Allow IM between MultiPoint Dashboard and a user session on this computer (Permitir mensajería instantánea entre MultiPoint Dashboard y una sesión de usuario en este equipo)|Permite el chat entre MultiPoint Manager y una sesión de usuario en este equipo. Para más información, vea [Use IM](Use-IM.md) (Usar la mensajería instantánea).|  
|Allow orchestration of administrator and MultiPoint Dashboard user sessions (Permitir la orquestación de administrador y de sesiones de usuario de MultiPoint Dashboard)|Cuando está habilitada, permite a los administradores usar MultiPoint Dashboard para la orquestación de la sesión. Estas sesiones se muestran como miniaturas.|  
|Allow stations to use GPU hardware rendering (Permitir a las estaciones usar la representación de hardware de GPU)|Controla si las estaciones pueden usar la unidad de procesamiento gráfico (GPU) del sistema.|   
  
## <a name="editing-the-computer-settings"></a>Editar la configuración del equipo  
  
1.  Abra MultiPoint Manager en [modo de estación](Switch-Between-Modes.md)y, a continuación, haga clic en el **inicio** ficha.  
  
2.  En el **equipo** columna, haga clic en el nombre del equipo y, a continuación, en el *nombre_equipo* **tareas**, haga clic en **editar la configuración del equipo**.  
  
3.  Active o desactive los elementos que desea cambiar y, a continuación, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Vea también  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
