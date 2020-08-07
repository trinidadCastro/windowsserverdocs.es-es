---
title: Directivas de red
description: En este tema se proporciona información general sobre las directivas de red para el servidor de directivas de redes en Windows Server 2016 y se incluyen vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f499de643a2460696305ef1ab35f695236849035
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952050"
---
# <a name="network-policies"></a>Directivas de red

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para obtener información general sobre las directivas de red en NPS.

>[!NOTE]
>Además de este tema, está disponible la siguiente documentación sobre directivas de red.
> - [Permiso de acceso](nps-np-access.md)
> - [Configurar directivas de red](nps-np-configure.md)

Las directivas de red son conjuntos de condiciones, restricciones y valores de configuración que le permiten designar quién está autorizado para conectarse a la red y bajo qué condiciones podrá o no conectarse.

Al procesar las solicitudes de conexión como un servidor Servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación y autorización para la solicitud de conexión. Durante el proceso de autenticación, NPS comprueba la identidad del usuario o equipo que se está conectando a la red. Durante el proceso de autorización, NPS determina si el usuario o equipo tiene permitido el acceso a la red.

Para efectuar estas determinaciones, NPS usa directivas de red que se configuran en la consola de NPS. NPS también examina las propiedades de acceso telefónico de la cuenta de usuario en Active Directory &reg; AD DS de servicios de dominio \( \) para realizar la autorización.

## <a name="network-policies---an-ordered-set-of-rules"></a>Directivas de red: un conjunto ordenado de reglas

Las directivas de red se pueden ver como unas reglas. Cada regla tiene un conjunto de condiciones y valores de configuración. NPS compara las condiciones de la regla con las propiedades de las solicitudes de conexión. Si se produce una coincidencia entre la regla y la solicitud de conexión, los valores de configuración definidos en la regla se aplican a la conexión.

Cuando hay varias directivas de red configuradas en NPS, son un conjunto ordenado de reglas. NPS comprueba cada solicitud de conexión con la primera regla de la lista, después con la segunda y así sucesivamente hasta que se encuentra una coincidencia.

Cada directiva de red tiene una configuración de **Estado de directiva** que le permite habilitar o deshabilitar la Directiva. Si se deshabilita una directiva de red, NPS no evaluará la directiva durante la autorización de solicitudes de conexión.

>[!NOTE]
>Si desea que NPS evalúe una directiva de red al realizar la autorización de las solicitudes de conexión, debe establecer la configuración de estado de la **Directiva** activando la casilla Directiva habilitada.

## <a name="network-policy-properties"></a>Propiedades de las directivas de red

Existen cuatro categorías de propiedades para cada directiva de red:

### <a name="overview"></a>Introducción

 Estas propiedades permiten especificar si la Directiva está habilitada, si la Directiva concede o deniega el acceso y si se requiere un método de conexión de red específico, o un tipo de servidor de acceso a la red (NAS), para las solicitudes de conexión. Las propiedades de Introducción también le permiten especificar si se ignorarán las propiedades de marcado de cuentas de usuario en AD DS. Si selecciona esta opción, NPS sólo usará los valores de configuración de la directiva de red para determinar si la conexión está autorizada.


### <a name="conditions"></a>Condiciones

 Estas propiedades permiten especificar las condiciones que debe cumplir la solicitud de conexión para que coincida con la directiva de red; si las condiciones configuradas en la directiva coinciden con la solicitud de conexión, NPS aplicará a la conexión los valores de configuración designados en la directiva de red. Por ejemplo, si especifica la dirección IPv4 de NAS como una condición de la Directiva de red y NPS recibe una solicitud de conexión de un servidor NAS que tiene la dirección IP especificada, la condición de la Directiva coincide con la solicitud de conexión.


### <a name="constraints"></a>Restricciones

 Las restricciones son parámetros adicionales de la directiva de red que deben coincidir con la solicitud de conexión. Si la solicitud de conexión no cumple una de las restricciones, NPS rechazará automáticamente la solicitud. A diferencia de la respuesta NPS a condiciones no coincidentes en la Directiva de red, si una restricción no coincide, NPS deniega la solicitud de conexión sin evaluar las directivas de red adicionales.

### <a name="settings"></a>Configuración

 Estas propiedades permiten especificar la configuración que NPS aplicará a la solicitud de conexión si se cumplen todas las condiciones de la directiva de red.

Al agregar una nueva Directiva de red mediante la consola de NPS, debe usar el Asistente para nueva Directiva de red. Después de crear una directiva de red mediante el asistente, puede personalizar la Directiva haciendo doble clic en la Directiva en la consola de NPS para obtener las propiedades de la Directiva.

Para obtener ejemplos de sintaxis de coincidencia de patrones para especificar atributos de directiva de red, consulte [usar expresiones regulares en NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
