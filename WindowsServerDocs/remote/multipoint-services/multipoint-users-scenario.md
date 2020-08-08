---
title: cuentas de usuario de MultiPoint Services
description: Obtenga información acerca de las cuentas de usuario en Multipoint Services, especialmente qué tipo usar para distintos escenarios.
ms.topic: article
ms.assetid: 7f3c6ce5-9b7c-45a0-83c5-3f9b9f5f48d4
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: a14d7da2d633c659be9ea949e913857534e244a2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992758"
---
# <a name="example-scenarios-multipoint-services-user-accounts"></a>Escenarios de ejemplo: cuentas de usuario de MultiPoint Services
¿Qué debe hacer para implementar el escenario de cuenta de usuario que eligió para el entorno de Multipoint Services? En las tablas siguientes se describen las tareas que se deben realizar para configurar cuentas de usuario y preparar estaciones para cuentas de usuario compartidas o individuales en un equipo Multipoint independiente o en servidores en red de un grupo de trabajo o un dominio de Active Directory. Elija el escenario que se aplica a su entorno. A continuación, siga los vínculos de la tabla para completar cada tarea de configuración necesaria.

> [!NOTE]
> Si aún no ha decidido cómo configurar las cuentas de usuario, consulte [planeación de cuentas de usuario para el entorno de Multipoint Services](Plan-user-accounts-for-your-MultiPoint-services-environment.md) para obtener más información sobre cómo afecta cada opción a los usuarios.

## <a name="single-multipoint-services-computer-in-a-stand-alone-environment-no-network"></a>Un único equipo de Multipoint Services en un entorno independiente (sin red)

|||
|-|-|
|**No es necesario que mis usuarios inicien sesión.** Las estaciones pueden estar disponibles para cualquier persona que se ponga en marcha. No necesitan una experiencia de escritorio de Windows individual que incluya carpetas privadas para almacenar datos o escritorios personalizados.|1. cree una sola cuenta de usuario local (para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<br />2. [permitir que una cuenta tenga varias sesiones](Allow-one-account-to-have-multiple-sessions.md)<br />3. [configurar estaciones para el inicio de sesión automático](Configure-stations-for-automatic-logon.md)|
|**Mis usuarios pueden compartir el mismo inicio de sesión de usuario.** No necesitan una experiencia de escritorio de Windows individual que incluya carpetas privadas para almacenar datos o escritorios personalizados.|1. cree una sola cuenta de usuario local (para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<br />2. [permitir que una cuenta tenga varias sesiones](Allow-one-account-to-have-multiple-sessions.md)|
|**Mis usuarios deben tener su propia experiencia de escritorio de Windows individual.**|Cree una cuenta de usuario local para cada usuario (para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).|

## <a name="multiple-multipoint-services-computers-on-a-network-but-with-no-domain"></a>Varios equipos Multipoint Services en una red, pero sin dominio

|||
|-|-|
|**No es necesario que mis usuarios inicien sesión.** Las estaciones pueden estar disponibles para cualquier persona que se ponga en marcha. No necesitan una experiencia de escritorio de Windows individual que incluya carpetas privadas para almacenar datos o escritorios personalizados.|1. cree una sola cuenta de usuario local en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<br />2. [permitir que una cuenta tenga varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor<br />3. [configurar estaciones para el inicio de sesión automático](Configure-stations-for-automatic-logon.md) en cada servidor|
|**Mis usuarios pueden compartir el mismo inicio de sesión de usuario.** No necesitan una experiencia de escritorio de Windows individual que incluya carpetas privadas para almacenar datos o escritorios personalizados.|1. cree una sola cuenta de usuario local en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<br />2. [permitir que una cuenta tenga varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor.|
|**Mis usuarios deben tener su propia experiencia de escritorio de Windows individual.**<p>-   **Opción a** : mis usuarios siempre usarán estaciones locales conectadas al mismo equipo Multipoint Services.<br />-   **Opción B** : mis usuarios usarán estaciones locales en más de un equipo Multipoint Services.<br />-   **Opción C** : mis usuarios usarán clientes remotos en la LAN.|-   **Opción a** : cree una sola cuenta de usuario local en cada servidor para los usuarios de ese servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<br />-   **Opción B** : crear cuentas de usuario locales para cada usuario en cada servidor. **Nota:** Esto significa que cada usuario tendrá un perfil en cada servidor. En otras palabras, si guarda un archivo en mis documentos mientras está conectado a la estación de servidor A, no verá el archivo al iniciar sesión en la estación del servidor B. (Para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<br />-   **Opción C** : asignación de cada usuario a un equipo de Multipoint Services específico. Cree cuentas de usuario locales para los usuarios asignados en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).|

## <a name="one-or-more-multipoint-services-computers-in-a-domain-network-environment"></a>Uno o varios equipos Multipoint Services en un entorno de red de dominio

|||
|-|-|
|**No es necesario que mis usuarios inicien sesión.** Las estaciones pueden estar disponibles para cualquier persona que se ponga en marcha. No necesitan una experiencia de escritorio de Windows individual que incluya carpetas privadas para almacenar datos o escritorios personalizados.|1. cree una cuenta de dominio para iniciar sesión en los servidores.<br />2. [permitir que una cuenta tenga varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor.<br />3. [configurar estaciones para el inicio de sesión automático](Configure-stations-for-automatic-logon.md) en cada servidor.|
|**Mis usuarios pueden compartir el mismo inicio de sesión de usuario.** No necesitan una experiencia de escritorio de Windows individual que incluya carpetas privadas para almacenar datos o escritorios personalizados.|1. cree una cuenta de dominio para un grupo o para cada usuario.<br />2. [permitir que una cuenta tenga varias sesiones](Allow-one-account-to-have-multiple-sessions.md) en cada servidor.|
|**Mis usuarios deben tener su propia experiencia de escritorio de Windows individual.**<p>-   **Opción a** : cualquier usuario con una cuenta de dominio puede usar el equipo Multipoint Services.<br />-   **Opción B** : deseo limitar las cuentas de dominio que pueden tener acceso al servidor.|-   **Opción a** : no se requiere ninguna instalación. De forma predeterminada, todos los usuarios del dominio tienen acceso a cualquier equipo Multipoint Services de la red.<br />-   **Opción B** : limitar el acceso de las cuentas de usuario de dominio al equipo de Multipoint Services. Para obtener instrucciones, consulte [limitar el acceso de los usuarios al servidor](./limit-user-access-to-multipoint.md).|
|**Deseo usar cuentas de usuario locales y administrarlas por separado de las cuentas de dominio.** Por ejemplo, desea que alguien administre Multipoint Services pero no el dominio o que no desee proporcionar cuentas de dominio a todos los usuarios de Multipoint Services.|Cree una o varias cuentas de usuario locales en cada servidor. (Para obtener instrucciones, consulte [crear cuentas de usuario locales](Create-local-user-accounts.md)).<p>**Nota:** Esto significa que cada cuenta de usuario tendrá un perfil en cada servidor. En otras palabras, si guarda un archivo en mis documentos mientras está conectado a la estación de servidor A, no verá el archivo al iniciar sesión en la estación del servidor B.|