---
title: Configurar la integración de Azure
description: Configurar Azure integración de Windows Admin Center (proyecto Honolulu). Conectar la puerta de enlace de Windows Admin Center a Azure.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 38b973680463cdebc1b3168e447abfcf1caba6b5
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297099"
---
# Configurar la integración de Azure

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Windows Admin Center es compatible con varias características opcionales que se integran con los servicios de Azure. [Obtén información sobre las opciones de integración de Azure disponibles con Windows Admin Center.](../plan/azure-integration-options.md)

Para permitir que la puerta de enlace de Windows Admin Center para comunicarse con Azure para sacar provecho de autenticación de Azure Active Directory para el acceso de puerta de enlace, o para crear recursos de Azure en su nombre (por ejemplo, para proteger las máquinas virtuales administradas en Windows Admin Center con Azure Site Recuperación), debes registrar primero la puerta de enlace de Windows Admin Center con Azure. Solo debes hacer esto una vez para la puerta de enlace de Windows Admin Center - la configuración se conserva cuando se actualiza la puerta de enlace a una versión más reciente.

## Registrar la puerta de enlace con Azure

La primera vez que intentas usar una característica de integración de Azure en Windows Admin Center, se pedirá al registrar la puerta de enlace a Azure. También puedes registrar la puerta de enlace, ve a la pestaña de **Azure** en la configuración de Windows Admin Center.

Los pasos en el producto guiados creará una aplicación de Azure AD en el directorio, lo que permite que Windows Admin Center para comunicarse con Azure. Para ver la aplicación de Azure AD que se crea automáticamente, ve a la pestaña de **Azure** de opciones de configuración de Windows Admin Center. El hipervínculo de **vista en Azure** te permite ver la aplicación de Azure AD en el portal de Azure. 

La aplicación de Azure AD creada se usa para todos los puntos de integración de Azure en Windows Admin Center, incluida la [autenticación de Azure AD a la puerta de enlace](../configure/user-access-control.md#azure-active-directory).