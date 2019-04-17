---
title: Configuración
description: Obtén información sobre la configuración de Windows Admin Center (proyecto Honolulu). Configuración de usuario permite a los usuarios cambiar su idioma o la región y otras preferencias. Configuración de puerta de enlace permite a los administradores configurar la puerta de enlace.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 04/12/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d67a2c743900792353141186112cd09dbf780309
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296627"
---
# Configuración de Windows Admin Center

> Se aplica a: Windows Admin Center

Opciones de configuración de Windows Admin Center constan de opciones de configuración de nivel de usuario de puerta de enlace. Un cambio en una configuración de nivel de usuario solo afecta a perfil del usuario actual, mientras que un cambio en una configuración de nivel de puerta de enlace afecta a todos los usuarios de dicha puerta de enlace de Windows Admin Center.

## Configuración de usuario

Configuración de nivel de usuario consta de las siguientes secciones:

- Cuenta
- Personalización
- Idioma o la región
- Sugerencias
- Avanzado

En la pestaña de la **cuenta** , los usuarios pueden revisar las credenciales que han usado para autenticar al centro de administración de Windows. Si Azure AD está configurado para que sea el proveedor de identidad, el usuario puede cerrar sesión su cuenta de Azure AD desde ella.

En la pestaña de la **personalización** , los usuarios pueden alternar a un tema oscuro de la interfaz de usuario.

En la pestaña de **Idioma y región** , los usuarios pueden cambiar los formatos de idioma y región que se muestren con Windows Admin Center.

En la pestaña de **sugerencias** , los usuarios pueden alternar sugerencias sobre nuevas características y servicios de Azure.

La pestaña **Opciones avanzadas** ofrece a los desarrolladores de extensiones de Windows Admin Center funcionalidades adicionales.

## Configuración de puerta de enlace

Configuración de nivel de puerta de enlace consta de las siguientes secciones:

- Extensiones
- Access
- Azure
- Conexiones compartidas

Solo los administradores de puerta de enlace son podrán ver y cambiar esta configuración. Los cambios en estas opciones de configuración cambiar la configuración de la puerta de enlace y afecta a todos los usuarios de la puerta de enlace de Windows Admin Center.

En la pestaña **extensiones** , los administradores pueden instalar, desinstalar o actualizar las extensiones de puerta de enlace. [Más información sobre las extensiones.](using-extensions.md)

La pestaña de **acceso** permite a los administradores configurar quién tiene acceso a la puerta de enlace de Windows Admin Center, así como el proveedor de identidad que se usa para autenticar a los usuarios. [Más información acerca de cómo controlar el acceso a la puerta de enlace.](user-access-control.md)

En la pestaña de **Azure** , los administradores pueden registrar la puerta de enlace con Azure para habilitar [las características de integración de Azure](azure-integration.md) en Windows Admin Center.

En la ficha **Conexiones compartidas** , los administradores pueden configurar una sola lista de conexiones que se comparten entre todos los usuarios de la puerta de enlace de Windows Admin Center. [Más información sobre cómo configurar conexiones una vez para todos los usuarios de una puerta de enlace.](shared-connections.md)