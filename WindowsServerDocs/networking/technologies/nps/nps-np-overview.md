---
title: Directivas de red
description: En este tema proporciona información general de directivas de red para el servidor de directivas de redes en Windows Server 2016 e incluye vínculos a guías adicionales acerca de NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60ab80bb6cf26578430b76806405a65a0f596ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848956"
---
# <a name="network-policies"></a>Directivas de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información general de directivas de redes en NPS.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación de la directiva de red.
> - [Permiso de acceso](nps-np-access.md)
> - [Configurar directivas de redes](nps-np-configure.md)

Las directivas de red son conjuntos de condiciones, restricciones y configuración que le permite designar quién está autorizado para conectarse a la red y las circunstancias en las que pueden o no se puede conectar.

Al procesar las solicitudes de conexión como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación y autorización para la solicitud de conexión. Durante el proceso de autenticación, NPS comprueba la identidad del usuario o equipo que se está conectando a la red. Durante el proceso de autorización, NPS determina si el usuario o el equipo puede tener acceso a la red.

Para determinarlo, NPS usa las directivas de red que están configuradas en la consola de NPS. Además, NPS examina las propiedades de acceso telefónico de la cuenta de usuario en Active Directory&reg; servicios de dominio \(AD DS\) para realizar la autorización.

## <a name="network-policies---an-ordered-set-of-rules"></a>Directivas de red: un conjunto ordenado de reglas

Las directivas de red pueden verse como las reglas. Cada regla tiene un conjunto de condiciones y configuraciones. NPS compara las condiciones de la regla a las propiedades de las solicitudes de conexión. Si se produce una coincidencia entre la regla y la solicitud de conexión, la configuración definida en la regla se aplica a la conexión.

Cuando se configuran varias directivas de red en NPS, son un conjunto ordenado de reglas. NPS comprueba cada solicitud de conexión con la primera regla en la lista, a continuación, el segundo y así sucesivamente, hasta que se encuentra una coincidencia.

Cada directiva de red tiene un **estado de la directiva** configuración que permite habilitar o deshabilitar la directiva. Cuando se deshabilita una directiva de red, NPS no evaluará la directiva al autorizar las solicitudes de conexión.

>[!NOTE]
>Si desea que NPS evalúe una directiva de red al realizar la autorización para las solicitudes de conexión, debe configurar el **estado de la directiva** habilitada mediante la selección de la directiva de casilla de verificación.

## <a name="network-policy-properties"></a>Propiedades de directiva de red

Hay cuatro categorías de propiedades para cada directiva de red:

### <a name="overview"></a>Información general

 Estas propiedades permiten especificar si la directiva está habilitada, si la directiva concede o deniega el acceso, y si un método de conexión de red específica, o el tipo de servidor de acceso de red (NAS), se requiere para las solicitudes de conexión. Las propiedades de Introducción también le permiten especificar si se omiten las propiedades de marcado de cuentas de usuario en AD DS. Si selecciona esta opción, se usa solo la configuración de la directiva de red NPS para determinar si la conexión está autorizada.


### <a name="conditions"></a>Condiciones

 Estas propiedades permiten especificar las condiciones que debe tener la solicitud de conexión para que coincida con la directiva de red; Si las condiciones configuradas en la directiva coincide con la solicitud de conexión, NPS aplica la configuración designada en la directiva de red para la conexión. Por ejemplo, si especifica la dirección IPv4 de NAS como una condición de la directiva de red y NPS recibe una solicitud de conexión de un NAS que tiene la dirección IP especificada, la condición de la directiva coincide con la solicitud de conexión. 


### <a name="constraints"></a>Restricciones

 Las restricciones son parámetros adicionales de la directiva de red que tienen que coincidir con la solicitud de conexión. Si una restricción no coincide con la solicitud de conexión, NPS rechazará automáticamente la solicitud. A diferencia de la respuesta NPS a las condiciones no coincidentes de la directiva de red, si no se cumple una restricción, NPS rechaza la solicitud de conexión sin evaluar las directivas de red adicional.

### <a name="settings"></a>Configuración

 Estas propiedades permiten especificar la configuración que NPS aplicará a la solicitud de conexión si se cumplen todas las condiciones de directiva de red para la directiva.

Cuando se agrega una nueva directiva de red mediante la consola NPS, debe usar al Asistente para nueva directiva de red. Después de haber creado una directiva de red mediante el asistente, puede personalizar la directiva haciendo doble clic en la directiva en la consola NPS para obtener las propiedades de la directiva.

Para obtener ejemplos de sintaxis de coincidencia de patrón para especificar los atributos de directiva de red, consulte [usar expresiones regulares en NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
