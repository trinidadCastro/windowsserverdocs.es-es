---
title: Permiso de acceso
description: Este tema proporciona una visión general de permiso de acceso de directiva de red para el servidor de directivas de red en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aeacfaeb159598d2e53bac29fb09ffc3e7739476
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="access-permission"></a>Permiso de acceso

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

El permiso de acceso está configurado en el **Introducción** ficha de cada directiva de red en el servidor de directivas de redes (NPS). 

Esta configuración permite configurar la directiva para conceder o denegar el acceso a los usuarios si se cumplen las condiciones y las restricciones de la directiva de red por la solicitud de conexión. 

Configuración de permisos de acceso tiene el siguiente efecto:

- **Conceder acceso**. Se concedió acceso si la solicitud de conexión que coincida con las condiciones y restricciones que están configuradas en la directiva.
- **Denegar el acceso**. Si la solicitud de conexión coincide con las condiciones y restricciones que están configuradas en la directiva, se deniega el acceso.

También se concede o se deniega el permiso de acceso basado en la configuración de las propiedades de marcado de cada cuenta de usuario.

>[!NOTE]
>Cuentas de usuario y sus propiedades, como las propiedades de marcado, se configuran en cualquier los usuarios de Active Directory y equipos o los usuarios locales y grupos de Microsoft Management Console \(MMC\) complemento, dependiendo de si tienes Active Directory&reg; instalado los servicios de dominio (AD DS).

La configuración de la cuenta de usuario **permiso de acceso de red**, que está configurado en las propiedades de marcado de cuentas de usuario, invalidaciones de configuración de permiso de acceso de la directiva de red. Cuando se establece el permiso de acceso de red de una cuenta de usuario en el **controlar el acceso a través de la directiva de red NPS** opción, la configuración de permisos de acceso de red directiva determina si el usuario se concede o deniega el acceso.

>[!NOTE]
>En Windows Server 2016, el valor predeterminado de **permiso de acceso de red** en AD DS propiedades cuentas de usuario en el marcado es **controlar el acceso a través de la directiva de red NPS**.

Cuando NPS evalúa las solicitudes de conexión con directivas de red configurados, realiza las siguientes acciones:

- Si no se cumplen las condiciones de la primera directiva, evalúa la siguiente directiva NPS y este proceso continúa hasta que se encuentra una coincidencia o todas las directivas que se hayan evaluado para una coincidencia.
- Si se cumplen las condiciones y las restricciones de una directiva, NPS concede o deniega el acceso, según el valor de la configuración de permisos de acceso en la directiva.
- Si no coinciden con las condiciones de una coincidencia de directiva, pero las restricciones en la directiva, NPS rechaza la solicitud de conexión.
- Si las condiciones de todas las directivas no coinciden, NPS rechaza la solicitud de conexión.

## <a name="ignore-user-account-dial-in-properties"></a>Omitir propiedades de marcado de la cuenta de usuario

Puedes configurar la directiva de red NPS para omitir las propiedades de marcado de cuentas de usuario activando o desactivando el **Omitir propiedades de marcado de la cuenta de usuario** casilla de verificación de la **Introducción** ficha de una directiva de red. 

Normalmente cuando NPS realiza la autorización de una solicitud de conexión, comprueba las propiedades de marcado de la cuenta de usuario, donde puede afectar el permiso de acceso de red valor si el usuario está autorizado para conectarse a la red. Cuando se configura NPS pasar por alto las propiedades de marcado de cuentas de usuario durante la autorización, configuración de directiva de red determina si el usuario se concede acceso a la red.

Las propiedades de marcado de cuentas de usuario contienen lo siguiente:

- Permisos de acceso de red
- Identificador del autor de llamada
- Opciones de devolución de llamada
- Dirección IP estática
- Rutas estáticas

Para admitir varios tipos de conexiones que NPS proporciona autenticación y autorización, sería necesario deshabilitar el procesamiento de propiedades de marcado de la cuenta de usuario. Esto puede hacerse para admitir escenarios en los que no se requieren propiedades específicas de acceso telefónico.

Por ejemplo, el identificador, devolución de llamada, dirección IP estática y rutas estáticas propiedades están diseñadas para un cliente que marca a un servidor de acceso de red \(NAS\), no para los clientes que se conectan al acceso inalámbrico puntos. Un punto de acceso inalámbrico que recibe estas opciones de configuración en un mensaje RADIUS de NPS posible que no pueda procesarlos, lo que puede provocar la desconexión del cliente inalámbrico.

Cuando NPS proporciona autenticación y autorización para los usuarios que son marcado y el acceso a la red de tu organización a través de puntos de acceso inalámbrico, debes configurar las propiedades de marcado para admitir cualquier conexiones de acceso telefónico \ (mediante la configuración de acceso telefónico properties\) o conexiones inalámbricas \ (estableciendo no properties\ marcado).

Puedes usar para habilitar las propiedades de marcado de la cuenta de usuario en algunos escenarios de procesamiento NPS \ (como marcado in\) y deshabilitar las propiedades de acceso telefónico en otros escenarios de procesamiento \ (por ejemplo, 802.1X switch\ autenticación e inalámbrico).

También puedes usar **Omitir propiedades de marcado de la cuenta de usuario** para administrar el control de acceso de red a través de grupos y la configuración de permisos de acceso de la directiva de red. Al seleccionar el **Omitir propiedades de marcado de la cuenta de usuario** se omite la casilla de verificación, los permisos de acceso de red de la cuenta de usuario.

La desventaja solo a esta configuración es que no puedes usar las propiedades de marcado del cuenta de usuario adicionales de identificador, devolución de llamada, dirección IP estática y rutas estáticas.

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
