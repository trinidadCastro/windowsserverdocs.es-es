---
title: Cuentas de usuario de multiPoint Services
description: Obtenga información sobre las cuentas de usuario de MultiPoint Services, especialmente el tipo que se usará para distintos escenarios
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 31279f81d5af597b0b1f1729c953fefaf24a214f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855366"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Situaciones que sirven de ejemplo: Cuentas de usuario de multiPoint Services
¿Qué necesita hacer para implementar el escenario de la cuenta de usuario que eligió para el entorno de MultiPoint Services? Las tablas siguientes describen cada tarea que desea realizar para configurar las cuentas de usuario y preparar las estaciones compartida o individual las cuentas de usuario en un equipo de MultiPoint independiente o en servidores en red en un grupo de trabajo o un dominio de Active Directory. Elija el escenario que se aplica a su entorno. A continuación, siga los vínculos de la tabla para completar cada tarea de configuración necesarias.  
  
> [!NOTE]  
> Si aún no ha decidido cómo configurar las cuentas de usuario, consulte [Plan de cuentas de usuario para el entorno de MultiPoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md) para obtener más información acerca de cómo afecta cada opción a los usuarios.  
  
## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Equipo de servicios de MultiPoint solo en un entorno independiente (ninguna red)  
  
|||  
|-|-|  
|**Mis usuarios no es necesario iniciar sesión.** Las estaciones pueden estar disponibles para cualquier persona que asciende a ellos. No necesitan una experiencia de escritorio de Windows individual que incluye las carpetas privadas para almacenar datos o los equipos de escritorio personalizadas.|1.  Crear una cuenta de usuario local único (para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br />2.  [Permitir que una cuenta para tener varias sesiones](Allow-one-account-to-have-multiple-sessions.md)<br />3.  [Configurar estaciones de inicio de sesión automático](Configure-stations-for-automatic-logon.md)|  
|**Mis usuarios pueden compartir el mismo inicio de sesión de usuario.** No necesitan una experiencia de escritorio de Windows individual que incluye las carpetas privadas para almacenar datos o los equipos de escritorio personalizadas.|1.  Crear una cuenta de usuario local único (para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br />2.  [Permitir que una cuenta para tener varias sesiones](Allow-one-account-to-have-multiple-sessions.md)|  
|**Mis usuarios deben tener su propia experiencia de escritorio de Windows individual.**|Cree una cuenta de usuario local para cada usuario (para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)|  
  
## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Varios equipos de MultiPoint Services en una red, pero sin dominio  
  
|||  
|-|-|  
|**Mis usuarios no es necesario iniciar sesión.** Las estaciones pueden estar disponibles para cualquier persona que asciende a ellos. No necesitan una experiencia de escritorio de Windows individual que incluye las carpetas privadas para almacenar datos o los equipos de escritorio personalizadas.|1.  Crear una cuenta de usuario local única en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br />2.  [Permitir que una cuenta para tener varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor<br />3.  [Configurar estaciones de inicio de sesión automático](Configure-stations-for-automatic-logon.md) en cada servidor|  
|**Mis usuarios pueden compartir el mismo inicio de sesión de usuario.** No necesitan una experiencia de escritorio de Windows individual que incluye las carpetas privadas para almacenar datos o los equipos de escritorio personalizadas.|1.  Crear una cuenta de usuario local única en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br />2.  [Permitir que una cuenta para tener varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor.|  
|**Mis usuarios deben tener su propia experiencia de escritorio de Windows individual.**<br /><br />-   **Opción A** -Mis usuarios siempre usará las estaciones locales conectadas al mismo equipo MultiPoint Services.<br />-   **Opción B** -Mis usuarios van a usar las estaciones locales en más de un equipo de MultiPoint Services.<br />-   **Opción C** -Mis usuarios van a usar los clientes remotos de la LAN.|-   **Opción A** -crear una cuenta de usuario local única en cada servidor para los usuarios de ese servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br />-   **Opción B** -crear cuentas de usuario local para todos los usuarios en todos los servidores. **Nota:** Esto significa que cada usuario tenga un perfil en cada servidor. En otras palabras, si guarda un archivo en Mis documentos mientras ha iniciado sesión en la estación del servidor, no verán el archivo cuando inician sesión en la estación del servidor B. (Para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br />-   **Opción C** -asignar a cada usuario a un determinado equipo de MultiPoint Services. Crear cuentas de usuario local para los usuarios asignados en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)|  
  
## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Uno o más equipos de MultiPoint Services en un entorno de red de dominio  
  
|||  
|-|-|  
|**Mis usuarios no es necesario iniciar sesión.** Las estaciones pueden estar disponibles para cualquier persona que asciende a ellos. No necesitan una experiencia de escritorio de Windows individual que incluye las carpetas privadas para almacenar datos o los equipos de escritorio personalizadas.|1.  Cree una cuenta de dominio para iniciar sesión en los servidores.<br />2.  [Permitir que una cuenta para tener varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor.<br />3.  [Configurar estaciones de inicio de sesión automático](Configure-stations-for-automatic-logon.md) en cada servidor.|  
|**Mis usuarios pueden compartir el mismo inicio de sesión de usuario.** No necesitan una experiencia de escritorio de Windows individual que incluye las carpetas privadas para almacenar datos o los equipos de escritorio personalizadas.|1.  Cree una cuenta de dominio para un grupo o para cada usuario.<br />2.  [Permitir que una cuenta para tener varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor.|  
|**Mis usuarios deben tener su propia experiencia de escritorio de Windows individual.**<br /><br />-   **Opción A** : cualquier usuario con una cuenta de dominio puede utilizar el equipo de MultiPoint Services.<br />-   **Opción B** -quiero limitar qué cuentas de dominio pueden tener acceso al servidor.|-   **Opción A** -se requiere ninguna configuración. De forma predeterminada, todos los usuarios de dominio tienen acceso a cualquier equipo de MultiPoint Services en la red.<br />-   **Opción B** -limitar el acceso de cuentas de usuario de dominio en el equipo de MultiPoint Services. Para obtener instrucciones, consulte [limitar el acceso a los usuarios al servidor](limit-users--access-to-the-server-in-multipoint-services.md).|  
|**Me gustaría usar cuentas de usuario local y administrarlas por separado desde Mis cuentas de dominio.** Por ejemplo, desea que otra persona para administrar MultiPoint Services, pero no en el dominio o no desea permitir que las cuentas de dominio a todos los usuarios de MultiPoint Services.|Crear una o varias cuentas de usuario local en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario local](Create-local-user-accounts.md).)<br /><br />**Nota:** Esto significa que cada cuenta de usuario tendrá un perfil en cada servidor. En otras palabras, si guarda un archivo en Mis documentos mientras ha iniciado sesión en la estación del servidor, no verán el archivo cuando inician sesión en la estación del servidor B.|  