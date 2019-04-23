---
title: Configuración
description: Obtenga información sobre la configuración en Windows Admin Center (proyecto Honolulu). Configuración de usuario permite a los usuarios cambiar su idioma y región y otras preferencias. Configuración de puerta de enlace permite a los administradores configurar la puerta de enlace.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/19/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1e1231500733f70ddfcbd4f8a847047b73f24a00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882386"
---
# <a name="settings"></a>Configuración

> Se aplica a: Windows Admin Center

Configuración de Windows Admin Center consta de configuración de nivel de usuario y nivel de puerta de enlace. Un cambio en una configuración de nivel de usuario solo afecta al perfil del usuario actual, mientras que un cambio en una configuración de puerta de enlace afecta a todos los usuarios de esa puerta de enlace de Windows Admin Center.

## <a name="user-settings"></a>Configuración de usuario

Configuración de nivel de usuario consta de las secciones siguientes:

- Cuenta
- Idioma y región
- Sugerencias

En el **cuenta** ficha, los usuarios pueden revisar las credenciales que han usado para autenticar a Windows Admin Center. Si Azure AD está configurada para ser el proveedor de identidades, el usuario puede iniciar fuera de su cuenta de Azure AD desde esta ficha.

En el **idioma y región** ficha, los usuarios pueden cambiar los formatos de idioma y región mostrados por Windows Admin Center.

En el **sugerencias** ficha, los usuarios pueden alternar sugerencias acerca de nuevas características y servicios de Azure.

### <a name="dark-theme"></a>Tema oscuro

> Se aplica a: Versión preliminar de Windows Admin Center

En la versión preliminar de Windows Admin Center, puede desbloquear adicional **personalización** sección, que contiene la opción para activar o desactivar para un tema oscuro de la interfaz de usuario. Para habilitar el **personalización** sección, escriba ```msft.sme.shell.personalization``` como una clave de experimento.

>[!IMPORTANT]
> El tema oscuro es un trabajo en curso, hacer, no informe de errores en este momento.

## <a name="gateway-settings"></a>Configuración de puerta de enlace

Configuración del nivel de puerta de enlace consisten en las secciones siguientes:

- Extensions
- Acceso
- Azure

Solo los administradores de la puerta de enlace pueden ver y cambiar esta configuración. Los cambios realizados en esta configuración de cambiar la configuración de la puerta de enlace y afectan a todos los usuarios de la puerta de enlace de Windows Admin Center.

En el **extensiones** ficha, los administradores pueden instalar, desinstalar o actualizar las extensiones de la puerta de enlace. [Más información acerca de las extensiones.](using-extensions.md)

El **acceso** pestaña permite a los administradores configurar quién puede tener acceso a la puerta de enlace de Windows Admin Center, así como el proveedor de identidades que se usa para autenticar a los usuarios. [Más información acerca de cómo controlar el acceso a la puerta de enlace.](user-access-control.md)

Desde el **Azure** ficha, los administradores pueden registrar la puerta de enlace con Azure para habilitar [características de integración de Azure](azure-integration.md) en Windows Admin Center.