---
title: AD FS paginado inicio de sesión
description: Este documento describe la nueva experiencia de inicio de sesión de AD FS de 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b11454427a65e37604b430a63b5ed745f4a2bb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864456"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS paginado inicio de sesión

>Se aplica a: Windows Server 2019

Para AD FS 2019, hemos rediseñado la interfaz de inicio de sesión de usuario.  Ahora, el inicio de sesión de AD FS tendrán la misma apariencia y comportamiento de Azure AD.  Esto proporcionará a los usuarios una experiencia más coherente de inicio de sesión, que incorpora un flujo de usuario paginados y centrado. 

## <a name="whats-changing"></a>¿Qué está cambiando
En AD FS 2012 R2 y 2016, la pantalla de inicio de sesión tenía algo parecido a esto:

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Nos estamos alejándose de mostrar un único formulario ubicado en el lado derecho de la pantalla.

En AD FS 2019, éstos son los cambios de diseño importante que verá:


- **Centrado de una interfaz de usuario**. Anteriormente, la interfaz de inicio de sesión de usuario existía en el lado derecho de la pantalla, como se indicó anteriormente. Nos hemos movido la interfaz de usuario protagonista modernizar la experiencia.
- **Paginación**. En lugar de proporcionar un formato largo a rellenar, hemos incorporado un nuevo flujo que le llevarán a través de la experiencia de inicio de sesión de paso a paso. La telemetría muestra que, con este enfoque, nuestros clientes tienen más éxito inicios de sesión. Lo también nos proporciona mayor flexibilidad para incorporar varios métodos de autenticación, tales como autenticación de factor de teléfono. 

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

En la primera página, le pedirá que escriba su nombre de usuario. También puede seleccionar la opción "Mantener la sesión iniciada" para reducir la frecuencia de solicitudes de inicio de sesión y seguir conectado cuándo es seguro hacerlo. (Esta opción está deshabilitada de forma predeterminada).

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

En la segunda página, se mostrarán con las opciones de autenticación configuradas por el administrador. Si está habilitado el permiso de autenticación externa como principal, esto se incluirán también.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

En la tercera página, le pedirá que escriba su contraseña (suponiendo que ha seleccionado "Password" como opción de autenticación). 

## <a name="how-to-get-the-new-experience"></a>Cómo obtener la nueva experiencia
Si es un nuevo cliente a AD FS, recibirá el nuevo diseño de forma predeterminada. Sin embargo, si es un cliente existente con AD FS 2012 R2 o 2016, hay varios pasos que deberá tomar para recibir el nuevo diseño: 

1. Actualizar los servidores de AD FS de 2019. 
2.  Habilite su FBL para 2019.
3.  Habilitar la nueva experiencia de inicio de sesión.
- Permitir el nuevo inicio de sesión a través de PowerShell. En PowerShell, ejecute el siguiente comando para habilitar la paginación: ``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``
- Permitir la autenticación externa como principal, ya sea a través de PowerShell o mediante el administrador del servidor de AD FS. En PowerShell, ejecute el siguiente comando para permitir la autenticación externa como principal: ``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personalización
Las opciones de personalización todavía serán aplicables para AD FS de 2019. A continuación se muestran algunos vínculos a otros documentos para su referencia. 

• Para quienes no previsto actualizar sus servidores a AD FS 2019 pero todavía desea que el nuevo diseño: [Uso de un tema de sitio Web de la experiencia de usuario de Azure AD en servicios de federación de Active Directory](azure-ux-web-theme-in-ad-fs.md)

• Una ubicación central para la personalización: [Personalización de inicio de sesión del usuario de FS AD](ad-fs-user-sign-in-customization.md)
