---
title: Permiso de acceso
description: En este tema se proporciona información general de permiso de acceso de directiva de red para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cdec41fb7925061bb8c8402634e1d9b1625bf301
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849496"
---
# <a name="access-permission"></a>Permiso de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Permiso de acceso se configura en el **Introducción** pestaña de cada directiva de red en el servidor de directivas de redes (NPS). 

Esta configuración le permite configurar la directiva para conceder o denegar el acceso a los usuarios si se cumplen las condiciones y restricciones de la directiva de red por la solicitud de conexión. 

Configuración de permisos de acceso tiene el siguiente efecto:

- **Conceder acceso**. Se concede acceso si la solicitud de conexión cumple las condiciones y restricciones configuradas en la directiva.
- **Denegar el acceso**. Se denegó el acceso si la solicitud de conexión coincide con las condiciones y restricciones configuradas en la directiva.

Permiso de acceso también se concede o deniega según la configuración de las propiedades de marcado de cada cuenta de usuario.

>[!NOTE]
>Las cuentas de usuario y sus propiedades, como propiedades de acceso telefónico, se configuran en los usuarios de Active Directory y equipos, o bien los usuarios locales y grupos de Microsoft Management Console \(MMC\) complemento, dependiendo de si tiene Active Directory&reg; Domain Services (AD DS) instalado.

La configuración de la cuenta de usuario **permiso de acceso de red**, que se configura en las propiedades de marcado de cuentas de usuario, invalidaciones de configuración de permiso de acceso de la directiva de red. Cuando se establece el permiso de acceso de red en una cuenta de usuario en el **controlar el acceso a través de la directiva de red NPS** opción, la configuración de permisos de acceso de red directiva determina si el usuario se concede o deniega el acceso.

>[!NOTE]
>En Windows Server 2016, el valor predeterminado de **permiso de acceso de red** en AD DS propiedades cuentas de usuario de acceso telefónico es **controlar el acceso a través de la directiva de red NPS**.

Cuando NPS evalúa las solicitudes de conexión con directivas de red configuradas, realiza las siguientes acciones:

- Si no se cumplen las condiciones de la primera directiva, NPS evalúa la directiva siguiente y este proceso continúa hasta que se encuentra una coincidencia o que se hayan evaluado todas las directivas para una coincidencia.
- Si se cumplen las condiciones y restricciones de una directiva, NPS concede o deniega el acceso, dependiendo del valor de la configuración de permisos de acceso en la directiva.
- Si no coinciden con las condiciones de una coincidencia de directiva, pero las restricciones en la directiva, NPS rechaza la solicitud de conexión.
- Si no coinciden con las condiciones de todas las directivas, NPS rechaza la solicitud de conexión.

## <a name="ignore-user-account-dial-in-properties"></a>Omitir propiedades de marcado de cuentas de usuario

Puede configurar la directiva de red NPS para que omita las propiedades de marcado de cuentas de usuario activando o desactivando la **Omitir propiedades de marcado de cuentas de usuario** casilla de verificación en la **Introducción** pestaña de una red Directiva. 

Normalmente cuando NPS autoriza una solicitud de conexión, comprueba las propiedades de acceso telefónico de la cuenta de usuario, donde puede afectar al valor el permiso de acceso de red si el usuario está autorizado para conectarse a la red. Al configurar NPS para que omita las propiedades de marcado de cuentas de usuario durante la autorización, configuración de directiva de red determina si se concede al usuario acceso a la red.

Las propiedades de marcado de cuentas de usuario contengan lo siguiente:

- Permiso de acceso de red
- Caller-ID
- Opciones de devolución de llamada
- Dirección IP estática
- Rutas estáticas

Para admitir varios tipos de conexiones para que NPS proporciona autenticación y autorización, podría ser necesario deshabilitar el procesamiento de propiedades de acceso telefónico de la cuenta de usuario. Esto puede hacerse para admitir escenarios en que las propiedades de marcado específicas no son necesarias.

Por ejemplo, el identificador de llamada, devolución de llamada, dirección IP estática y las propiedades de las rutas estáticas están diseñadas para un cliente que marca a un servidor de acceso de red \(NAS\), pero no para los clientes que se conectan a puntos de acceso inalámbrico. Un punto de acceso inalámbrico que reciba esta configuración en un mensaje RADIUS en NPS podría no pueda procesarlos, lo que puede provocar la desconexión del cliente inalámbrico.

Cuando NPS proporciona autenticación y autorización para los usuarios marcar y tener acceso a la red de su organización a través de puntos de acceso inalámbrico, debe configurar las propiedades de marcado para admitir cualquier conexiones de acceso telefónico \(por establecer propiedades de acceso telefónico\) o conexiones inalámbricas \(estableciendo propiedades de acceso telefónico de no\).

Puede usar NPS para habilitar las propiedades de marcado para la cuenta de usuario en algunos escenarios de procesamiento \(como el acceso telefónico\) y deshabilitar las propiedades de acceso telefónico en otros escenarios de procesamiento \(como 802.1X inalámbrico y conmutador de autenticación\).

También puede usar **Omitir propiedades de marcado de cuentas de usuario** para administrar el control de acceso de red a través de grupos y la configuración de permiso de acceso de la directiva de red. Cuando se selecciona el **Omitir propiedades de marcado de cuentas de usuario** se omite la casilla de verificación, permiso de acceso de red en la cuenta de usuario.

La única desventaja de esta configuración es que no se puede usar las propiedades de marcado del cuenta de usuario adicional de devolución de llamada, el identificador de llamada, dirección IP estática y rutas estáticas.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
