---
title: Directivas de red
description: Este tema proporciona una visión general de las directivas de red para el servidor de directivas de red en Windows Server 2016 e incluye vínculos a instrucciones adicionales sobre NPS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: e4a9b134-6d1d-40d7-a49c-5f46d5fdb419
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7e1604f4839bd955e5ea10d9eafea5ef0c978977
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="network-policies"></a>Directivas de red

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para obtener información general de las directivas de red en NPS.

>[!NOTE]
>Además de este tema, la siguiente documentación de la directiva de red está disponible.
> - [Permiso de acceso](nps-np-access.md)
> - [Configurar las directivas de red](nps-np-configure.md)

Las directivas de red son conjuntos de condiciones, restricciones y opciones de configuración que le permiten designar quién está autorizado para conectarse a la red y las circunstancias en las que pueden o no se puede conectar.

Al procesar las solicitudes de conexión como un servidor de servicio de autenticación remota telefónica de usuario (RADIUS), NPS realiza la autenticación y autorización para la solicitud de conexión. Durante el proceso de autenticación, NPS comprueba la identidad del usuario o equipo que se conecta a la red. Durante el proceso de autorización, NPS determina si el usuario o equipo puede tener acceso a la red.

Para determinarlo, NPS usa las directivas de red que están configuradas en la consola NPS. NPS también examina las propiedades de marcado de la cuenta de usuario en Active Directory&reg; \(AD DS\) los servicios de dominio para realizar la autorización.

## <a name="network-policies---an-ordered-set-of-rules"></a>Directivas de red, un conjunto de reglas ordenado

Las directivas de red pueden verse como reglas. Cada regla tiene un conjunto de condiciones y configuración. NPS compara las condiciones de la regla a las propiedades de las solicitudes de conexión. Si se produce una coincidencia entre la regla y la solicitud de conexión, la configuración definida en la regla se aplica a la conexión.

Cuando hay varias directivas de red configuradas en NPS, son un conjunto de reglas ordenado. NPS comprueba cada solicitud de conexión con la primera regla en la lista, a continuación, el segundo y así sucesivamente, hasta que se encuentra una coincidencia.

Cada directiva de red tiene un **estado de la directiva** configuración que te permite habilitar o deshabilitar la directiva. Al deshabilitar una directiva de red, NPS no evalúa la directiva cuando autorizar las solicitudes de conexión.

>[!NOTE]
>Si quieres que NPS evalúe una directiva de red al realizar la autorización para las solicitudes de conexión, debes configurar el **estado de la directiva** habilitada seleccionando la directiva de casilla de verificación.

## <a name="network-policy-properties"></a>Propiedades de la directiva de red

Hay cuatro categorías de propiedades para cada directiva de red:

### <a name="overview"></a>Introducción

 Estas propiedades te permiten especificar si la directiva está habilitada, si la directiva concede o deniega el acceso y si un método de conexión de red específica, o el tipo de servidor de acceso de red (NAS), es necesario para las solicitudes de conexión. Las propiedades de Introducción también permiten especificar si se omiten las propiedades de marcado de cuentas de usuario en AD DS. Si seleccionas esta opción, la configuración en la directiva de red se usa por NPS para determinar si la conexión está autorizada.


### <a name="conditions"></a>Condiciones

 Estas propiedades te permiten especificar las condiciones que debe tener la solicitud de conexión para coincidir con la directiva de red. Si las condiciones configuradas en la directiva coincide con la solicitud de conexión, NPS aplica la configuración que se indican en la directiva de red a la conexión. Por ejemplo, si especifica la dirección IPv4 NAS como condición de la directiva de red y NPS recibe una solicitud de conexión de un NAS que tiene la dirección IP especificada, la condición de la directiva coincide con la solicitud de conexión. 


### <a name="constraints"></a>Restricciones

 Las restricciones son parámetros adicionales de la directiva de red necesarios para que coincida con la solicitud de conexión. Si una restricción no coincide con la solicitud de conexión, NPS rechazará automáticamente la solicitud. A diferencia de la respuesta NPS inigualable condiciones en la directiva de red, si no se asocia una restricción, NPS deniega la solicitud de conexión sin evaluar las directivas de red adicionales.

### <a name="settings"></a>Configuración

 Estas propiedades te permiten especificar la configuración de NPS se aplica a la solicitud de conexión si se cumplen todas las condiciones de la directiva de red para la directiva.

Cuando agregas una nueva directiva de red mediante la consola NPS, debes usar al Asistente para nueva directiva de red. Después de crear una directiva de red mediante el asistente, puedes personalizar la directiva haciendo doble clic en la directiva en la consola NPS para obtener las propiedades de la directiva.

Para los ejemplos de la sintaxis de coincidencia para especificar los atributos de directiva de red, consulta [usa las expresiones regulares de NPS](nps-crp-reg-expressions.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
