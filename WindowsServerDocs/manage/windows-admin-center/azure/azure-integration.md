---
title: Configuración de la integración de Azure
description: Configuración del centro de administración de Windows de integración de Azure (proyecto Honolulu). Conexión de la puerta de enlace del centro de administración de Windows a Azure.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: b56960a531c8d7d8cf42cb0462d2fe4d422dfba7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970902"
---
# <a name="configuring-azure-integration"></a>Configuración de la integración de Azure

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

El centro de administración de Windows admite varias características opcionales que se integran con los servicios de Azure. [Obtenga información sobre las opciones de integración de Azure disponibles con el centro de administración de Windows.](../plan/azure-integration-options.md)

Para permitir que la puerta de enlace del centro de administración de Windows se comunique con Azure para aprovechar la autenticación Azure Active Directory para el acceso a la puerta de enlace o para crear recursos de Azure en su nombre (por ejemplo, para proteger las máquinas virtuales administradas en el centro de administración de Windows con Azure Site Recovery), deberá registrar primero la puerta de enlace del centro de administración de Windows con Azure. Solo tiene que hacerlo una vez para la puerta de enlace del centro de administración de Windows: la configuración se conserva al actualizar la puerta de enlace a una versión más reciente.

## <a name="register-your-gateway-with-azure"></a>Registro de la puerta de enlace con Azure

La primera vez que intente usar una característica de integración de Azure en el centro de administración de Windows, se le pedirá que registre la puerta de enlace en Azure. También puede registrar la puerta de enlace yendo a la pestaña **Azure** en la configuración del centro de administración de Windows. Tenga en cuenta que solo los administradores de puerta de enlace del centro de administración de Windows pueden registrar la puerta de enlace del centro de administración de Windows con Azure. [Obtenga más información sobre los permisos de usuario y administrador del centro de administración de Windows](../configure/user-access-control.md#gateway-access-role-definitions).

Los pasos guiados en el producto crearán una aplicación Azure AD en el directorio, lo que permite que el centro de administración de Windows se comunique con Azure. Para ver la aplicación Azure AD que se crea automáticamente, vaya a la pestaña **Azure** de configuración del centro de administración de Windows. La **vista de hipervínculo de Azure** le permite ver la aplicación Azure ad en el Azure portal.

La aplicación Azure AD creada se usa para todos los puntos de integración de Azure en el centro de administración de Windows, incluida [la autenticación de Azure ad en la puerta de enlace](../configure/user-access-control.md#azure-active-directory). El centro de administración de Windows configura automáticamente los permisos necesarios para crear y administrar recursos de Azure en su nombre:

- Azure Active Directory Graph
    - Directory.AccessAsUser.All
    - User.Read
- Azure Service Management
    - user_impersonation

### <a name="manual-azure-ad-app-configuration"></a>Configuración manual de la aplicación Azure AD

Si desea configurar una aplicación Azure AD manualmente, en lugar de usar la aplicación Azure AD creada automáticamente por el centro de administración de Windows durante el proceso de registro de la puerta de enlace, debe hacer lo siguiente.

1. Conceda a la aplicación Azure AD los permisos de API necesarios enumerados anteriormente. Puede hacerlo si navega a la aplicación Azure AD en el Azure Portal. Vaya al > de Azure portal **Azure Active Directory**  >  **registros de aplicaciones** > seleccione la aplicación de Azure ad que quiere usar. A continuación, a la pestaña permisos de la **API** y agregue los permisos de API enumerados anteriormente.
2. Agregue la dirección URL de la puerta de enlace del centro de administración de Windows a las direcciones URL de respuesta (también conocidas como URI de redirección). Vaya a la aplicación de Azure AD y, a continuación, vaya a **manifiesto**. Busque la clave "replyUrlsWithType" en el manifiesto. Dentro de la clave, agregue un objeto que contenga dos claves: "URL" y "type". La clave "URL" debe tener un valor de la dirección URL de la puerta de enlace del centro de administración de Windows, anexando un carácter comodín al final. La clave "type" de la clave debe tener un valor de "Web". Por ejemplo:

    ```json
    "replyUrlsWithType": [
            {
                    "url": "http://localhost:6516/*",
                    "type": "Web"
            }
    ],
    ```
