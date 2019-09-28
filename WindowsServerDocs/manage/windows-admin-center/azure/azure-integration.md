---
title: Configuración de la integración de Azure
description: Configuración del centro de administración de Windows de integración de Azure (proyecto Honolulu). Conexión de la puerta de enlace del centro de administración de Windows a Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 448a8fb3e4340752b673b06f86d5d49211b6b147
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357358"
---
# <a name="configuring-azure-integration"></a>Configuración de la integración de Azure

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

El centro de administración de Windows admite varias características opcionales que se integran con los servicios de Azure. [Obtenga información sobre las opciones de integración de Azure disponibles con el centro de administración de Windows.](../plan/azure-integration-options.md)

Para permitir que la puerta de enlace del centro de administración de Windows se comunique con Azure para aprovechar la autenticación Azure Active Directory para el acceso a la puerta de enlace o para crear recursos de Azure en su nombre (por ejemplo, para proteger las máquinas virtuales administradas en el centro de administración de Windows mediante Azure site Recuperación), tendrá que registrar primero la puerta de enlace del centro de administración de Windows con Azure. Solo tiene que hacerlo una vez para la puerta de enlace del centro de administración de Windows: la configuración se conserva al actualizar la puerta de enlace a una versión más reciente.

## <a name="register-your-gateway-with-azure"></a>Registro de la puerta de enlace con Azure

La primera vez que intente usar una característica de integración de Azure en el centro de administración de Windows, se le pedirá que registre la puerta de enlace en Azure. También puede registrar la puerta de enlace yendo a la pestaña **Azure** en la configuración del centro de administración de Windows.

Los pasos guiados en el producto crearán una aplicación Azure AD en el directorio, lo que permite que el centro de administración de Windows se comunique con Azure. Para ver la aplicación Azure AD que se crea automáticamente, vaya a la pestaña **Azure** de configuración del centro de administración de Windows. La **vista de hipervínculo de Azure** le permite ver la aplicación Azure ad en el Azure portal. 

La aplicación Azure AD creada se usa para todos los puntos de integración de Azure en el centro de administración de Windows, incluida [la autenticación de Azure ad en la puerta de enlace](../configure/user-access-control.md#azure-active-directory).