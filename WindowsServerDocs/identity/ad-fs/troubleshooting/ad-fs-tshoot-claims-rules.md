---
title: 'Reglas de notificaciones de AD FS solucionar:'
description: Este documento describe cómo solucionar problemas de sintaxis de regla de notificaciones con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 027b2afc9e580253ec820e7e5be14419387ddd44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837926"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>Sintaxis de las reglas de notificaciones de AD FS solucionar:
Una notificación es una instrucción que hace un individuo sobre sí mismo o a otro asunto.  Notificaciones emitidas por un usuario de confianza, y se proporciona uno o más valores y, a continuación, empaqueta en tokens de seguridad emitidos por el servidor de AD FS.  Este artículo trata sobre la creación y la sintaxis de las notificaciones.  Para obtener información sobre notificaciones de emisión vea [AD FS solución de problemas: emisión de notificaciones](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>Puede usar [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) en el [ADFS ayuda](https://adfshelp.microsoft.com) sitio para ayudar a solucionar problemas de notificaciones.   

## <a name="how-claim-rules-are-processed"></a>Cómo se procesan las reglas de notificación
Reglas de notificación se procesan a través de la [canalización de notificaciones](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) utilizando el [motor de notificaciones](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). El motor de notificaciones es un componente lógico del Servicio de federación que examina el conjunto de notificaciones entrantes que presenta un usuario y, después, según la lógica de cada regla, generará un conjunto de resultados de notificaciones.

## <a name="how-to-create-a-claim-rule"></a>Cómo crear una regla de notificación
Las reglas de notificación se crean por separado para cada relación de confianza federada dentro del Servicio de federación y no se comparten entre varias relaciones de confianza. Puede crear una regla de un [plantilla de regla de notificación](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), empezar desde cero mediante la creación de la regla mediante la [lenguaje de reglas de notificación](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) o usar Windows PowerShell para personalizar una regla.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Descripción de los componentes del lenguaje de reglas de notificaciones
El lenguaje de reglas de notificación consta de los siguientes componentes, separados por el "= >" operador:

- Una condición - utilizada para comprobar las notificaciones de entrada y determinar si se debe ejecutar la instrucción de emisión de la regla.  Representa una expresión lógica que debe evaluarse en true para ejecutar la parte del cuerpo de la regla.

- Una declaración de emisión

Por ejemplo:

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

La siguiente notificación tiene las siguientes:
- condición - `c:[type == "Name", value == "domain user"] ` -se evalúa como la notificación de entrada si el nombre de cuenta de windows es un usuario de dominio
- emisión - `issue(type = "Role", value = "employee")` : si la condición es true, agrega una nueva notificación a la notificación de entrada con el rol del empleado.

Para obtener más información sobre las notificaciones y la sintaxis, vea [el papel de lenguaje de reglas de notificaciones](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Editor de reglas de notificaciones
Comprobación de sintaxis se realiza mediante el editor de reglas notificaciones una vez que haya completado la notificación y haga clic en **Aceptar**.  Por lo que si tiene una sintaxis incorrecta, a continuación, el editor le permitirá saber.

![notificaciones](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Registros de eventos
Al buscar intentando solucionar problemas de una notificación mediante los registros del mejor enfoque consiste en Buscar resultados de notificaciones.  Puede buscar los eventos 1000 y 1001 en el registro de eventos.

![notificaciones](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Creación de una aplicación de ejemplo
También puede crear una aplicación de ejemplo los ecos sus notificaciones.  Por ejemplo puede usar una aplicación de ejemplo y crear un usuario de confianza que tiene la misma notificación que está intentando solucionar problemas y vea si la aplicación tiene problemas con la notificación.

![notificaciones](media/ad-fs-tshoot-claims/claim4.png)

Una buena aplicación web está disponible aquí.  Esta aplicación es una aplicación web simple que reenvía las notificaciones que recibe de usuario de confianza.  Para usar esta opción, deberá editar la aplicación de web.config por:
- cambiar https://app1.contoso.com/sampapp a la dirección URL la va a usar para hospedar el sampapp
- cambiar todas las instancias de sts.contoso.com para que señale el servidor de federación de AD FS
- Reemplazando la huella digital con la huella digital

![notificaciones](media/ad-fs-tshoot-claims/claims3.png)

La siguiente [artículo del blog](https://blogs.technet.microsoft.com/tangent_thoughts/2015/02/20/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp/) tiene instrucciones excelente profundidad, para la configuración.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)