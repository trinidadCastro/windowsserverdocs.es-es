---
title: 'Solución de problemas de AD FS: reglas de notificaciones'
description: En este documento se describe cómo solucionar problemas de sintaxis de reglas de notificaciones con AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 94e841282963e2b2b6ada552b54c7732d965b6b6
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955977"
---
# <a name="ad-fs-troubleshooting---claims-rules-syntax"></a>Solución de problemas de AD FS: sintaxis de reglas de notificaciones
Una demanda es una instrucción que hace un sujeto sobre sí misma u otro asunto.  Las notificaciones las emite un usuario de confianza, y se les asigna uno o varios valores y, a continuación, se empaquetan en los tokens de seguridad que emite el servidor de AD FS.  En este artículo se trata la sintaxis y la creación de notificaciones.  Para obtener información sobre la emisión de notificaciones [, consulte AD FS solución de problemas: emisión de notificaciones](ad-fs-tshoot-claims-issuance.md).

>[!NOTE]  
>Puede usar [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) en el sitio de [ayuda de ADFS](https://adfshelp.microsoft.com) para ayudar en la solución de problemas de notificaciones.   

## <a name="how-claim-rules-are-processed"></a>Cómo se procesan las reglas de notificación
Las reglas de notificación se procesan a través de la [canalización de notificaciones](../../ad-fs/technical-reference/The-Role-of-the-Claims-Pipeline.md) mediante el [motor de notificaciones](../../ad-fs/technical-reference/The-Role-of-the-Claims-Engine.md). El motor de notificaciones es un componente lógico del Servicio de federación que examina el conjunto de notificaciones entrantes que presenta un usuario y, después, según la lógica de cada regla, generará un conjunto de resultados de notificaciones.

## <a name="how-to-create-a-claim-rule"></a>Cómo crear una regla de notificación
Las reglas de notificación se crean por separado para cada relación de confianza federada dentro del Servicio de federación y no se comparten entre varias relaciones de confianza. Puede crear una regla a partir de una [plantilla de regla de notificaciones](../../ad-fs/technical-reference/determine-the-type-of-claim-rule-template-to-use.md), empezar desde cero mediante la creación de la regla mediante el [lenguaje de reglas de notificaciones](../../ad-fs/technical-reference/when-to-use-a-custom-claim-rule.md) o usar Windows PowerShell para personalizar una regla.

## <a name="understanding-the-components-of-the-claim-rule-language"></a>Descripción de los componentes del lenguaje de reglas de notificaciones
El lenguaje de reglas de notificaciones consta de los siguientes componentes, separados por el operador "=>":

- Condición: se usa para comprobar las notificaciones de entrada y determinar si se debe ejecutar la instrucción de emisión de la regla.  Representa una expresión lógica que debe evaluarse como true para ejecutar la parte del cuerpo de la regla.

- Una declaración de emisión

Ejemplo:

```c:[type == "Name", value == "domain user"] => issue(type = "Role", value = "employee");``` 

La siguiente demanda tiene lo siguiente:
- condición: `c:[type == "Name", value == "domain user"] ` evalúa la petición de entrada de si el nombre de cuenta de Windows es un usuario de dominio.
- emisión: `issue(type = "Role", value = "employee")` si la condición es verdadera, agrega una nueva demanda a la notificaciones de entrada con el rol de empleado.

Para obtener más información sobre las notificaciones y la sintaxis, vea [el rol del lenguaje de reglas de notificaciones](../../ad-fs/technical-reference/the-role-of-the-claim-rule-language.md).

## <a name="claims-rule-editor"></a>Editor de reglas de notificaciones
La comprobación de sintaxis se realiza mediante el editor de reglas de notificaciones una vez completada la notificación y haciendo clic en **Aceptar**.  Por lo tanto, si tiene la sintaxis incorrecta, el editor le informará.

![claims](media/ad-fs-tshoot-claims/claims1.png)

## <a name="event-logs"></a>Registros de eventos
Al intentar solucionar problemas de una notificación con los registros, el mejor enfoque consiste en buscar la salida de las notificaciones.  Puede buscar eventos 1000 y 1001 en el registro de eventos.

![claims](media/ad-fs-tshoot-claims/claims2.png)

## <a name="creating-a-sample-application"></a>Creación de una aplicación de ejemplo
También puede crear una aplicación de ejemplo que repite las notificaciones.  Por ejemplo, puede usar una aplicación de ejemplo y crear un usuario de confianza que tenga la misma demanda en la que está intentando solucionar problemas y ver si la aplicación tiene algún problema con dicha demanda.

![claims](media/ad-fs-tshoot-claims/claim4.png)

Aquí encontrará una buena aplicación Web de ejemplo.  Esta aplicación es una aplicación web simple que devuelve las notificaciones que recibe del usuario de confianza.  Para usarlo, debe editar la aplicación web.config de la siguiente manera:
- cambiar https://app1.contoso.com/sampapp a la dirección URL que utilizará para hospedar el sampapp
- cambiar todas las instancias de sts.contoso.com para que apunten al servidor de Federación AD FS
- Reemplazo de la huella digital con la huella digital

![claims](media/ad-fs-tshoot-claims/claims3.png)

El siguiente [artículo del blog](/archive/blogs/tangent_thoughts/install-and-configure-a-simple-net-4-5-sample-federated-application-samapp) contiene instrucciones detalladas excelentes para configurar esta opción.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de AD FS](ad-fs-tshoot-overview.md)
