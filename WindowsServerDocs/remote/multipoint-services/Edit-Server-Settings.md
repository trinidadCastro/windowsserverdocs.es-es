---
title: Editar la configuración del servidor
description: Más información sobre la configuración de Multipoint Services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: afb64b94-9055-4703-b8ce-a8839b2718da
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1812db96c4acf2e3e820f44660c91d2a67b4348f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859278"
---
# <a name="edit-server-settings"></a>Editar la configuración del servidor
Al instalar MultiPoint Services, estableció la configuración para su sistema, incluyendo las opciones de ciertos programas. En este tema se describe la configuración que puede establecer para su sistema MultiPoint Services y se explica cómo editar la configuración.  
  
## <a name="about-multipoint-services-settings"></a>Sobre la configuración de MultiPoint Services  
En la siguiente tabla se describe la diferente configuración que puede cambiar para su sistema MultiPoint Services.  
  
|Opción de MultiPoint Services|Descripción|  
|-----------------------------------------------------------------------------------------|---------------|  
|Permitir que una cuenta tenga varias sesiones|Permite a una cuenta de un solo usuario iniciar sesión de forma simultánea en varias estaciones. Esto puede ser útil en casos como una clase en la que cada estudiante usa una única cuenta compartida. Al usar esta opción, cualquier cambio en los recursos de la cuenta, como las carpetas de documentos o el escritorio, están disponibles para todos los usuarios que hayan iniciado sesión en la misma cuenta.|  
|Allow this computer to be managed remotely (Permitir que este equipo se administre remotamente)|Permite que el equipo que ejecuta Multipoint Services sea administrado por otros sistemas Multipoint en la red. Si esta opción se selecciona y el equipo que administra está en la misma subred, este equipo aparece en la lista de servidores disponibles para administrar. Si esta opción se selecciona y el equipo que administra está en una subred diferente, el equipo administrador puede seguir administrando este equipo, pero debe especificar la dirección IP del equipo.|
|Permitir la supervisión de escritorios de este equipo|Le permite controlar si los escritorios se pueden supervisar en el sistema MultiPoint Services. Si esta opción está desactivada (no seleccionada), los escritorios de estaciones (tanto locales como remotos) que están conectados al equipo que está ejecutando Multipoint Services no se mostrarán en la pestaña Inicio de Multipoint Manager (incluido en un equipo diferente si el equipo se está administrando de forma remota).|  
|Always start in console mode (Iniciar siempre en el modo de consola)|Habilita la tecnología RemoteFX, que se ha diseñado para permitir que las sesiones de Escritorio remoto se ejecuten de forma más rápida y eficiente al descargar procesamientos a la CPU y la GPU. Si se va a conectar a multipoint Services mediante un cliente compatible con RemoteFX, es posible que pueda mejorar el rendimiento con esta opción. Las ventajas dependen de las capacidades del servidor y la red. Por ejemplo, esto depende en parte de si el tiempo empleado en realizar procesamientos adicionales para comprimir el flujo de datos es inferior al tiempo que se ahorra al transmitir menos datos.|  
|Do not show privacy notification at first user logon (No mostrar la notificación de privacidad en el primer inicio de sesión de usuario)|Cuando un usuario inicia sesión en una estación MultiPoint por primera vez, se muestra una notificación para informar al usuario de que sus actividades en la estación pueden supervisarse.|  
|Assign a unique IP to each station (Asignar una IP única a cada estación)|Asigna una dirección IP única a cada estación. De manera predeterminada, MultiPoint Services tiene una dirección IP, que se comparte con todas las sesiones que se ejecutan en el sistema. En cambio, esta opción puede provocar algunos problemas de compatibilidad de aplicaciones. Por ejemplo, si una aplicación requiere una dirección IP única, puede que no se ejecute correctamente en MultiPoint Services. Al seleccionar esta opción, también conocida como virtualización de IP, puede resolver este problema.<p>La virtualización de IP también es útil para supervisar las sesiones activas en MultiPoint Services. Algunas herramientas de supervisión informan del uso según la dirección IP. Para habilitar la supervisión de sesión, puede usar la virtualización de IP para asignar una dirección IP única a cada sesión. Tenga en cuenta que, si selecciona esta opción, cada nueva sesión recibe una dirección IP única. Cualquier sesión existente continúa usando la dirección IP compartida hasta que cierra sesión y vuelve a iniciarla.|  
|Allow IM between MultiPoint Dashboard and a user session on this computer (Permitir mensajería instantánea entre MultiPoint Dashboard y una sesión de usuario en este equipo)|Permite el chat entre MultiPoint Manager y una sesión de usuario en este equipo. Para más información, vea [Use IM](Use-IM.md) (Usar la mensajería instantánea).|  
|Allow orchestration of administrator and MultiPoint Dashboard user sessions (Permitir la orquestación de administrador y de sesiones de usuario de MultiPoint Dashboard)|Cuando está habilitada, permite a los administradores usar MultiPoint Dashboard para la orquestación de la sesión. Estas sesiones se muestran como miniaturas.|  
|Allow stations to use GPU hardware rendering (Permitir a las estaciones usar la representación de hardware de GPU)|Controla si las estaciones pueden usar la unidad de procesamiento gráfico (GPU) del sistema.|   
  
## <a name="editing-the-computer-settings"></a>Editar la configuración del equipo  
  
1.  Abra Multipoint Manager en [modo de estación](Switch-Between-Modes.md)y, después, haga clic en la pestaña **Inicio** .  
  
2.  En la columna **equipo** , haga clic en el nombre del equipo y, a continuación, en las **tareas** *nombre de equipo* , haga clic en **Editar configuración del equipo**.  
  
3.  Active o desactive los elementos que desee cambiar y, a continuación, haga clic en **Aceptar**.  
  
## <a name="see-also"></a>Consulta también  
[Administrar tareas del sistema mediante MultiPoint Manager](Manage-System-Tasks-Using-MultiPoint-Manager.md)  
  
