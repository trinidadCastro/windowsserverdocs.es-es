---
title: Administrar estaciones de usuario
description: Obtenga información sobre cómo administrar las estaciones de usuario de MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b418578d-3a4c-49b0-90db-8389b320b2f6
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8c5351f3e8ec9890ef72905b646c37e9b049745e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823726"
---
# <a name="manage-user-stations"></a>Administrar estaciones de usuario
En esta sección se describe cómo administrar las *estaciones* que forman el sistema MultiPoint Services. Administración de un sistema MultiPoint Services incluye tanto la administración de los componentes de hardware y software de MultiPoint Manager. En un sistema MultiPoint Services, un equipo de escritorio es la interfaz de usuario de software presentada en el monitor para cada estación de usuario.  
  
## <a name="station-status"></a>Estado de la estación  
Puede ver los siguientes tipos de estado para cada escritorio en la pestaña **Estaciones**. El estado incluye lo siguiente:  
  
-   Los usuarios que han iniciado sesión  
  
-   Las sesiones de usuario que están suspendidas, pero que siguen activas en el equipo  
  
-   Las estaciones que se están usando y por qué usuario  
  
Para más información sobre cómo ver el estado del escritorio, vea el tema [Ver estado de la conexión de usuario](View-User-Connection-Status.md).  

>[!TIP] 
> Puede asignar nombres descriptivos a cada estación, lo que le ayudará a identificarlas más fácilmente. Use **Identify station** (Identificar estación), que muestra el nombre de estación en la pantalla asignada.
  
## <a name="different-ways-to-log-standard-users-off-of-the-multipoint-services-system"></a>Distintas formas de cerrar la sesión de usuarios estándar del sistema MultiPoint Services  
Como *usuario administrativo*, puede cerrar la sesión de Windows en cualquier momento, sin que afecte a los usuarios activos del sistema MultiPoint Services. Los *usuarios estándar* también pueden *desconectar* su sesión o *cerrar sesión* en el sistema MultiPoint Services. En caso de que esté habilitada la protección de disco, si un usuario está terminando su jornada laboral, debe asegurarse de guardar el trabajo en el equipo o en un dispositivo de almacenamiento externo para que cuando se apague el sistema MultiPoint Services, pueda recuperar su trabajo guardado al día siguiente.  
  
Como usuario administrativo, es posible que tenga que finalizar la *sesión* de un usuario estándar, en lugar de que el usuario cierre la sesión. Hay dos formas de finalizar la sesión de un usuario estándar:  
  
-   Finalice la sesión y cierre la sesión del usuario. Para más información sobre cómo finalizar una sesión de usuario, vea el tema [Finalizar una sesión de usuario](End-a-User-Session.md).  
  
-   Suspenda al usuario para finalizar temporalmente la sesión de usuario, pero mantenga la sesión activa en la memoria del equipo del sistema MultiPoint Services. El usuario suspendido se puede volver a conectar a la sesión desde la misma estación o desde otra diferente y continuar con su trabajo. Para más información sobre cómo suspender una sesión de usuario, vea el tema [Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md).  
  
## <a name="set-a-station-to-automatically-log-on"></a>Configurar una estación para que inicie sesión automáticamente  
Como usuario administrativo, puede configurar una o varias estaciones para que inicien sesión automáticamente cuando que se inicie el equipo que está ejecutando MultiPoint Services. Para más información sobre cómo iniciar sesión automáticamente, vea el tema [Configurar una estación para el inicio de sesión automático](Set-up-a-Station-for-Automatic-Logon.md).  
  
## <a name="split-a-station"></a>Dividir una estación  
Cualquier monitor de estación que tenga una resolución mayor de 1024x768 se puede dividir en dos estaciones. Para más información sobre cómo dividir una estación, vea el tema [Dividir una estación de usuario](Split-a-User-Station.md).  
  
## <a name="see-also"></a>Vea también  
[Ver el estado de conexión de usuario](View-User-Connection-Status.md)  
[Cierre la sesión o desconectar sesiones de usuario](Log-off-or-Disconnect-User-Sessions.md)  
[Suspender y dejar activa la sesión de usuario](Suspend-and-Leave-User-Session-Active.md)  
[Configurar una estación de inicio de sesión automático](Set-up-a-Station-for-Automatic-Logon.md)  
[Finalizar una sesión de usuario](End-a-User-Session.md)  
[Dividir una estación de usuario](Split-a-User-Station.md)