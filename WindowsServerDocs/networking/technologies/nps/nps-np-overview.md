---
title: Directivas de red
description: En este tema se proporciona información general sobre las directivas de red para el servidor de directivas de redes en Windows Server 2016 y se incluyen vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c241191f54080ed92a1f1a274c0b2aff0b8c564c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405345"
---
# <a name="network-policies"></a>Directivas de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información general sobre las directivas de red en NPS.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación sobre directivas de red.
> - [Permiso de acceso](nps-np-access.md)
> - [Configurar las directivas de red](nps-np-configure.md)

Las directivas de red son conjuntos de condiciones, restricciones y valores de configuración que le permiten designar quién está autorizado para conectarse a la red y las circunstancias en las que pueden o no pueden conectarse.

Al procesar las solicitudes de conexión como un servidor Servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación y autorización para la solicitud de conexión. Durante el proceso de autenticación, NPS comprueba la identidad del usuario o equipo que se está conectando a la red. Durante el proceso de autorización, NPS determina si el usuario o el equipo pueden tener acceso a la red.

Para efectuar estas determinaciones, NPS usa directivas de red que se configuran en la consola de NPS. NPS también examina las propiedades de acceso telefónico de la cuenta de usuario en Active Directory @ no__t-0 Domain Services \(AD DS @ no__t-2 para realizar la autorización.

## <a name="network-policies---an-ordered-set-of-rules"></a>Directivas de red: un conjunto ordenado de reglas

Las directivas de red se pueden ver como reglas. Cada regla tiene un conjunto de condiciones y valores de configuración. NPS compara las condiciones de la regla con las propiedades de las solicitudes de conexión. Si se produce una coincidencia entre la regla y la solicitud de conexión, la configuración definida en la regla se aplica a la conexión.

Cuando se configuran varias directivas de red en NPS, son un conjunto ordenado de reglas. NPS comprueba cada solicitud de conexión con la primera regla de la lista, el segundo, etc. hasta que se encuentra una coincidencia.

Cada directiva de red tiene una configuración de **Estado de directiva** que le permite habilitar o deshabilitar la Directiva. Cuando se deshabilita una directiva de red, NPS no evalúa la Directiva al autorizar solicitudes de conexión.

>[!NOTE]
>Si desea que NPS evalúe una directiva de red al realizar la autorización de las solicitudes de conexión, debe establecer la configuración de estado de la **Directiva** activando la casilla Directiva habilitada.

## <a name="network-policy-properties"></a>Propiedades de directiva de red

Hay cuatro categorías de propiedades para cada directiva de red:

### <a name="overview"></a>Información general

 Estas propiedades permiten especificar si la Directiva está habilitada, si la Directiva concede o deniega el acceso y si se requiere un método de conexión de red específico, o un tipo de servidor de acceso a la red (NAS), para las solicitudes de conexión. Las propiedades de información general también permiten especificar si se omiten las propiedades de acceso telefónico de las cuentas de usuario de AD DS. Si selecciona esta opción, NPS solo usará la configuración de la Directiva de red para determinar si la conexión está autorizada.


### <a name="conditions"></a>Condiciones

 Estas propiedades permiten especificar las condiciones que debe tener la solicitud de conexión para que coincida con la Directiva de red. Si las condiciones configuradas en la Directiva coinciden con la solicitud de conexión, NPS aplica la configuración designada en la Directiva de red a la conexión. Por ejemplo, si especifica la dirección IPv4 de NAS como una condición de la Directiva de red y NPS recibe una solicitud de conexión de un servidor NAS que tiene la dirección IP especificada, la condición de la Directiva coincide con la solicitud de conexión. 


### <a name="constraints"></a>Restricciones

 Las restricciones son parámetros adicionales de la Directiva de red que deben coincidir con la solicitud de conexión. Si la solicitud de conexión no coincide con una restricción, NPS rechaza automáticamente la solicitud. A diferencia de la respuesta NPS a condiciones no coincidentes en la Directiva de red, si una restricción no coincide, NPS deniega la solicitud de conexión sin evaluar las directivas de red adicionales.

### <a name="settings"></a>Configuración

 Estas propiedades permiten especificar la configuración que NPS aplica a la solicitud de conexión si se cumplen todas las condiciones de la Directiva de red de la Directiva.

Al agregar una nueva Directiva de red mediante la consola de NPS, debe usar el Asistente para nueva Directiva de red. Después de crear una directiva de red mediante el asistente, puede personalizar la Directiva haciendo doble clic en la Directiva en la consola de NPS para obtener las propiedades de la Directiva.

Para obtener ejemplos de sintaxis de coincidencia de patrones para especificar atributos de directiva de red, consulte [usar expresiones regulares en NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
