---
title: Configuración
description: Obtener información sobre la configuración del centro de administración de Windows (proyecto Honolulu). La configuración de usuario permite a los usuarios cambiar su idioma o región y otras preferencias. La configuración de la puerta de enlace permite a los administradores configurar la puerta de enlace.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e184064aa913bf4fb18cadd8ddbb08b5b97c59c6
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865320"
---
# <a name="windows-admin-center-settings"></a>Configuración del centro de administración de Windows

> Se aplica a: Windows Admin Center

La configuración del centro de administración de Windows consta de los valores de nivel de usuario y de nivel de puerta de enlace. Un cambio en la configuración de nivel de usuario solo afecta al perfil del usuario actual, mientras que un cambio en la configuración de nivel de puerta de enlace afecta a todos los usuarios de esa puerta de enlace del centro de administración de Windows.

## <a name="user-settings"></a>Configuración de usuario

La configuración de nivel de usuario consta de las siguientes secciones:

- Cuenta
- Personalization
- Idioma o región
- Sugerencias
- Avanzado

En la pestaña **cuenta** , los usuarios pueden revisar las credenciales que han usado para autenticarse en el centro de administración de Windows. Si Azure AD está configurado para ser el proveedor de identidades, el usuario puede cerrar la sesión de su cuenta de Azure AD desde esta pestaña.

En la pestaña **Personalización** , los usuarios pueden alternar a un tema oscuro de la interfaz de usuario.

En la pestaña **idioma o región** , los usuarios pueden cambiar los formatos de idioma y región mostrados por el centro de administración de Windows.

En la pestaña **sugerencias** , los usuarios pueden alternar sugerencias sobre los servicios de Azure y las nuevas características.

La pestaña **Opciones avanzadas** proporciona a los desarrolladores de extensiones del centro de administración de Windows funciones adicionales.

## <a name="gateway-settings"></a>Configuración de puerta de enlace

La configuración de nivel de puerta de enlace consta de las siguientes secciones:

- Extensiones
- Acceso
- Azure
- Conexiones compartidas

Solo los administradores de puerta de enlace pueden ver y cambiar esta configuración. Los cambios en esta configuración cambian la configuración de la puerta de enlace y afectan a todos los usuarios de la puerta de enlace del centro de administración de Windows.

En la pestaña **extensiones** , los administradores pueden instalar, desinstalar o actualizar las extensiones de puerta de enlace. [Más información acerca de las extensiones.](using-extensions.md)

La pestaña **acceso** permite a los administradores configurar quién puede tener acceso a la puerta de enlace del centro de administración de Windows, así como el proveedor de identidades que se usa para autenticar a los usuarios. [Obtenga más información sobre cómo controlar el acceso a la puerta de enlace.](user-access-control.md)

En la pestaña **Azure** , los administradores pueden registrar la puerta de enlace con Azure para habilitar [las características de integración de Azure](azure-integration.md) en el centro de administración de Windows.

Mediante la pestaña **conexiones compartidas** , los administradores pueden configurar una sola lista de conexiones que se compartirán entre todos los usuarios de la puerta de enlace del centro de administración de Windows. [Obtenga más información sobre la configuración de conexiones una vez para todos los usuarios de una puerta de enlace.](shared-connections.md)