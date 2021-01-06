---
title: Permiso de acceso
description: En este tema se proporciona información general sobre el permiso de acceso a la Directiva de red para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: edfdb534843b7b9b81fffb2d0712c65542e6ed3a
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949331"
---
# <a name="access-permission"></a>Permiso de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El permiso de acceso se configura en la pestaña **información general** de cada directiva de red en el servidor de directivas de redes (NPS).

Esta configuración permite configurar la Directiva para conceder o denegar el acceso a los usuarios si las condiciones y restricciones de la Directiva de red coinciden con la solicitud de conexión.

La configuración del permiso de acceso tiene el efecto siguiente:

- **Conceder acceso**. El acceso se concede si la solicitud de conexión cumple las condiciones y restricciones configuradas en la directiva.
- **Denegar el acceso**. El acceso se deniega si la solicitud de conexión cumple las condiciones y restricciones configuradas en la directiva.

También se concede o se deniega el permiso de acceso en función de la configuración de las propiedades de acceso telefónico de cada cuenta de usuario.

>[!NOTE]
>Las cuentas de usuario y sus propiedades, como las propiedades de acceso telefónico, se configuran en el Active Directory usuarios y equipos o en el complemento MMC de Microsoft Management Console usuarios y grupos locales \( \) , en función de si tiene &reg; instalado Active Directory servicios de dominio (AD DS).

El **permiso de acceso a la red** configuración de la cuenta de usuario, que se configura en las propiedades de acceso telefónico de las cuentas de usuario, invalida la configuración de permisos de acceso a la Directiva de red. Cuando el permiso de acceso a la red en una cuenta de usuario se establece en la opción **Controlar acceso a través de la Directiva de red NPS** , el valor de permiso de acceso a la Directiva de red determina si se concede o se deniega el acceso al usuario.

>[!NOTE]
>En Windows Server 2016, el valor predeterminado de **permiso de acceso a la red** en AD DS propiedades de acceso telefónico de la cuenta de usuario es **controlar el acceso a través de la Directiva de red NPS**.

Cuando NPS evalúa las solicitudes de conexión con las directivas de red configuradas, realiza las siguientes acciones:

- Si las condiciones de la primera directiva no se cumplen, NPS evalúa la siguiente directiva y continúa con este procedimiento hasta que encuentra una coincidencia o hasta que se evalúan todas las directivas para comprobar las coincidencias.
- Si las condiciones y restricciones de una directiva coinciden, NPS concede o deniega el acceso, en función del valor de la configuración de permisos de acceso en la Directiva.
- Si las condiciones de una directiva se cumplen, pero las restricciones no, NPS rechaza la solicitud de conexión.
- Si no se cumple ninguna condición de ninguna directiva, NPS rechaza la solicitud de conexión.

## <a name="ignore-user-account-dial-in-properties"></a>Omitir las propiedades de acceso telefónico de las cuentas de usuario

Puede configurar la Directiva de red NPS para que omita las propiedades de acceso telefónico de las cuentas de usuario; para ello, Active o desactive la casilla **omitir las propiedades de acceso telefónico** de la cuenta de usuario en la pestaña **información general** de una directiva de red.

Normalmente, cuando NPS autoriza una solicitud de conexión, comprueba las propiedades de acceso telefónico de la cuenta de usuario, donde el valor de la opción de permiso de acceso a redes puede determinar si el usuario está autorizado para conectarse a la red. Cuando configure NPS para que omita las propiedades de acceso telefónico de cuentas de usuario durante la autorización, la configuración de la directiva de red determinará si se concede al usuario acceso a la red.

Las propiedades de acceso telefónico de las cuentas de usuario incluyen lo siguiente:

- Permiso de acceso a redes
- Id. del autor de la llamada
- Opciones de devolución de llamada
- Dirección IP estática
- Rutas estáticas

Para permitir que NPS autentique y autorice varios tipos de conexiones, puede ser necesario deshabilitar el procesamiento de propiedades de acceso telefónico de las cuentas de usuario. Esto se puede hacer en aquellos casos en los que no se requieran propiedades específicas de acceso telefónico.

Por ejemplo, las propiedades ID. de llamador, devolución de llamada, dirección IP estática y rutas estáticas están diseñadas para un cliente que marca a un servidor NAS de acceso a la red \( \) , no a los clientes que se conectan a puntos de acceso inalámbricos. Es posible que un punto de acceso inalámbrico que recibe esta configuración en un mensaje RADIUS de NPS no pueda procesarlos, lo que puede hacer que el cliente inalámbrico se desconecte.

Cuando NPS proporciona autenticación y autorización para los usuarios que se conectan a la red de la organización y acceden a ella a través de puntos de acceso inalámbricos, debe configurar las propiedades de acceso telefónico para admitir conexiones de acceso telefónico mediante el establecimiento de las propiedades de \( acceso telefónico \) o las conexiones inalámbricas \( sin establecer las propiedades de acceso telefónico \) .

Puede usar NPS para habilitar el procesamiento de propiedades de acceso telefónico para la cuenta de usuario en algunos escenarios, como el \( acceso telefónico \) y para deshabilitar el procesamiento de propiedades de acceso telefónico en otros escenarios, como el \( conmutador de autenticación y la red de 802.1 x \) .

También puede usar **omitir las propiedades de acceso telefónico de las cuentas de usuario** para administrar el control de acceso a la red a través de grupos y la configuración de permisos de acceso en la Directiva de red. Al activar la casilla **omitir las propiedades de acceso telefónico de la cuenta de usuario** , se omite el permiso de acceso a la red en la cuenta de usuario.

La única desventaja para esta configuración es que no podrá usar las propiedades adicionales de acceso telefónico de las cuentas de usuario, como el Id. del autor de la llamada, la devolución de llamadas, la dirección IP estática y las rutas estáticas.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
