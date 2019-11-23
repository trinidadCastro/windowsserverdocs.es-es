---
title: Permiso de acceso
description: En este tema se proporciona información general sobre el permiso de acceso a la Directiva de red para el servidor de directivas de redes en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d6d1ca5e-bde0-4509-9e14-dc3fa9ff447e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a63bf7d277108a528a3ab1c20263e31b59c1547
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405361"
---
# <a name="access-permission"></a>Permiso de acceso

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El permiso de acceso se configura en la pestaña **información general** de cada directiva de red en el servidor de directivas de redes (NPS). 

Esta configuración permite configurar la Directiva para conceder o denegar el acceso a los usuarios si las condiciones y restricciones de la Directiva de red coinciden con la solicitud de conexión. 

La configuración de permisos de acceso tiene el siguiente efecto:

- **Conceder acceso**. Se concede acceso si la solicitud de conexión cumple las condiciones y restricciones configuradas en la Directiva.
- **Denegar el acceso**. Se deniega el acceso si la solicitud de conexión cumple las condiciones y restricciones configuradas en la Directiva.

También se concede o se deniega el permiso de acceso en función de la configuración de las propiedades de acceso telefónico de cada cuenta de usuario.

>[!NOTE]
>Las cuentas de usuario y sus propiedades, como las propiedades de acceso telefónico, se configuran en el Active Directory usuarios y equipos o en el complemento usuarios y grupos locales de Microsoft Management Console \(MMC\), en función de si tiene instalado Active Directory&reg; servicios de dominio (AD DS).

El **permiso de acceso a la red**configuración de la cuenta de usuario, que se configura en las propiedades de acceso telefónico de las cuentas de usuario, invalida la configuración de permisos de acceso a la Directiva de red. Cuando el permiso de acceso a la red en una cuenta de usuario se establece en la opción **Controlar acceso a través de la Directiva de red NPS** , el valor de permiso de acceso a la Directiva de red determina si se concede o se deniega el acceso al usuario.

>[!NOTE]
>En Windows Server 2016, el valor predeterminado de **permiso de acceso a la red** en AD DS propiedades de acceso telefónico de la cuenta de usuario es **controlar el acceso a través de la Directiva de red NPS**.

Cuando NPS evalúa las solicitudes de conexión con las directivas de red configuradas, realiza las siguientes acciones:

- Si no coinciden las condiciones de la primera Directiva, NPS evalúa la Directiva siguiente y continúa con este proceso hasta que se encuentra una coincidencia o se evalúan todas las directivas para buscar coincidencias.
- Si las condiciones y restricciones de una directiva coinciden, NPS concede o deniega el acceso, en función del valor de la configuración de permisos de acceso en la Directiva.
- Si las condiciones de una directiva coinciden pero las restricciones de la Directiva no coinciden, NPS rechaza la solicitud de conexión.
- Si las condiciones de todas las directivas no coinciden, NPS rechaza la solicitud de conexión.

## <a name="ignore-user-account-dial-in-properties"></a>Omitir las propiedades de acceso telefónico de cuentas de usuario

Puede configurar la Directiva de red NPS para que omita las propiedades de acceso telefónico de las cuentas de usuario; para ello, Active o desactive la casilla **omitir las propiedades de acceso telefónico** de la cuenta de usuario en la pestaña **información general** de una directiva de red. 

Normalmente, cuando NPS realiza la autorización de una solicitud de conexión, comprueba las propiedades de acceso telefónico de la cuenta de usuario, donde el valor de configuración del permiso de acceso a la red puede afectar a si el usuario está autorizado para conectarse a la red. Cuando configure NPS para que omita las propiedades de acceso telefónico de las cuentas de usuario durante la autorización, la configuración de la Directiva de red determinará si se concede al usuario acceso a la red.

Las propiedades de acceso telefónico de las cuentas de usuario contienen lo siguiente:

- Permiso de acceso a la red
- IDENTIFICADOR del autor de la llamada
- Opciones de devolución de llamada
- Dirección IP estática
- Rutas estáticas

Para admitir varios tipos de conexiones para las que NPS proporciona autenticación y autorización, puede que sea necesario deshabilitar el procesamiento de las propiedades de acceso telefónico de la cuenta de usuario. Esto se puede hacer para admitir escenarios en los que no se necesitan propiedades de acceso telefónico específicas.

Por ejemplo, las propiedades ID. de llamador, devolución de llamada, dirección IP estática y rutas estáticas están diseñadas para un cliente que marca a un servidor de acceso a la red \(\)NAS, no para los clientes que se conectan a puntos de acceso inalámbricos. Es posible que un punto de acceso inalámbrico que recibe esta configuración en un mensaje RADIUS de NPS no pueda procesarlos, lo que puede hacer que el cliente inalámbrico se desconecte.

Cuando NPS proporciona autenticación y autorización para los usuarios que se conectan a la red de la organización y acceden a ella a través de puntos de acceso inalámbricos, debe configurar las propiedades de acceso telefónico para admitir conexiones de acceso telefónico \(estableciendo las propiedades de acceso telefónico\) o las conexiones inalámbricas \(sin establecer las propiedades de acceso telefónico\).

Puede usar NPS para habilitar el procesamiento de propiedades de acceso telefónico para la cuenta de usuario en algunos escenarios \(como el\) de acceso telefónico y para deshabilitar el procesamiento de las propiedades de acceso telefónico en otros escenarios \(como el conmutador de autenticación e inalámbricos 802.1 X\).

También puede usar **omitir las propiedades de acceso telefónico de las cuentas de usuario** para administrar el control de acceso a la red a través de grupos y la configuración de permisos de acceso en la Directiva de red. Al activar la casilla **omitir las propiedades de acceso telefónico de la cuenta de usuario** , se omite el permiso de acceso a la red en la cuenta de usuario.

El único inconveniente de esta configuración es que no se pueden usar las propiedades de acceso telefónico de la cuenta de usuario adicionales del ID. de llamada, la devolución de llamada, la dirección IP estática y las rutas estáticas.

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
