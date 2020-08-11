---
title: Settings
description: Obtén información sobre la configuración en Windows Admin Center (Proyecto Honolulu). La configuración de usuario permite a los usuarios cambiar su idioma o región y otras preferencias. La configuración de la puerta de enlace permite a los administradores configurar la puerta de enlace.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.openlocfilehash: ff06a19d85858b8332412a51c029c9aeeba2af50
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997437"
---
# <a name="windows-admin-center-settings"></a>Configuración de Windows Admin Center

> Se aplica a: Windows Admin Center

La configuración de Windows Admin Center consta de valores de configuración de nivel de usuario y de nivel de puerta de enlace. Un cambio en la configuración de nivel de usuario solo afecta al perfil del usuario actual, mientras que un cambio en la configuración de nivel de puerta de enlace afecta a todos los usuarios de esa puerta de enlace de Windows Admin Center.

## <a name="user-settings"></a>Configuración de usuario

La configuración de nivel de usuario se compone de las secciones siguientes:

- Cuenta
- Personalization
- Idioma o región
- Sugerencias
- Avanzadas

En la pestaña **Cuenta**, los usuarios pueden revisar las credenciales que han usado para autenticarse en Windows Admin Center. Si Azure AD está configurado para ser el proveedor de identidades, el usuario puede cerrar la sesión de su cuenta de Azure AD desde esta pestaña.

En la pestaña **Personalización**, los usuarios pueden cambiar a un tema oscuro de la interfaz de usuario.

En la pestaña **Idioma o región**, los usuarios pueden cambiar los formatos de idioma y región que muestra Windows Admin Center.

En la pestaña **Sugerencias**, los usuarios pueden activar o desactivar las sugerencias sobre los servicios y las nuevas características de Azure.

La pestaña **Opciones avanzadas** proporciona funcionalidades adicionales a los desarrolladores de extensiones de Windows Admin Center.

## <a name="gateway-settings"></a>Configuración de puerta de enlace

La configuración de nivel de puerta de enlace se compone de las secciones siguientes:

- Extensions
- Acceso
- Azure
- Conexiones compartidas

Solo los administradores de puerta de enlace pueden ver y cambiar esta configuración. Los cambios en esta sección cambian la configuración de la puerta de enlace y afectan a todos los usuarios de la puerta de enlace de Windows Admin Center.

En la pestaña **Extensiones**, los administradores pueden instalar, desinstalar o actualizar las extensiones de puerta de enlace. [Obtén más información acerca de las extensiones.](using-extensions.md)

La pestaña **Acceso** permite a los administradores configurar quién puede acceder a la puerta de enlace de Windows Admin Center, así como el proveedor de identidades que se usa para autenticar a los usuarios. [Obtén más información sobre el control del acceso a la puerta de enlace.](user-access-control.md)

En la pestaña **Azure**, los administradores pueden registrar la puerta de enlace con Azure para habilitar las [características de integración de Azure](../azure/azure-integration.md) en Windows Admin Center.

Con la pestaña **Conexiones compartidas**, los administradores pueden configurar una lista única de conexiones que se compartirán entre todos los usuarios de la puerta de enlace de Windows Admin Center. [Obtén más información sobre la configuración de conexiones una vez para todos los usuarios de una puerta de enlace.](shared-connections.md)