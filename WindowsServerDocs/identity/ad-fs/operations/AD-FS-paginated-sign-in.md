---
title: AD FS inicio de sesión paginado
description: En este documento se describe la nueva experiencia de inicio de sesión para AD FS 2019.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.openlocfilehash: 27b0232b65a3003dde9a5702ec45063781abd813
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947451"
---
# <a name="ad-fs-paginated-sign-in"></a>AD FS inicio de sesión paginado


Por AD FS en Windows Server 2019, hemos rediseñado la interfaz de usuario de inicio de sesión.  Ahora, el inicio de sesión AD FS tendrá la misma apariencia y funcionamiento de Azure AD.  Esto proporcionará a los usuarios una experiencia de inicio de sesión más coherente, incorporando un flujo de usuario centrado y paginado.

## <a name="whats-changing"></a>Qué está cambiando
En AD FS en Windows Server 2012 R2 y 2016, la pantalla de inicio de sesión tiene un aspecto similar al siguiente:

![oldsignin](media/AD-FS-paginated-sign-in/signin1.png)

Estamos pasando de mostrar un único formulario situado en el lado derecho de la pantalla.

En AD FS en Windows Server 2019, estos son los principales cambios de diseño que verá:


- **Interfaz de usuario centrada**. Anteriormente, la interfaz de usuario de inicio de sesión existía en el lado derecho de la pantalla, como se mostró anteriormente. Hemos mudado la interfaz de usuario y el centro para modernizar la experiencia.
- **Paginación**. En lugar de proporcionar una forma larga de rellenar, hemos incorporado un nuevo flujo que le guiará a través de la experiencia de inicio de sesión paso a paso. Nuestra telemetría muestra que, con este enfoque, nuestros clientes tienen inicios de sesión más correctos. También nos proporciona más flexibilidad para incorporar varios métodos de autenticación, como la autenticación de factor de teléfono de EE. UU.

![newsignin](media/AD-FS-paginated-sign-in/signin2.png)

En la primera página, se le pedirá que escriba su nombre de usuario. También puede seleccionar la opción "mantener la sesión iniciada" para reducir la frecuencia de los mensajes de inicio de sesión y permanecer con la sesión iniciada cuando sea seguro hacerlo. (Esta opción está deshabilitada de forma predeterminada).

![newsignin](media/AD-FS-paginated-sign-in/signin3.png)

En la segunda página, se mostrarán las opciones de autenticación, configuradas por el administrador. Si permitir la autenticación externa como principal está habilitada, también se incluirá.

![newsignin](media/AD-FS-paginated-sign-in/signin4.png)

En la tercera página, se le pedirá que escriba su contraseña (suponiendo que ha seleccionado "contraseña" como opción de autenticación).

## <a name="how-to-get-the-new-experience"></a>Cómo obtener la nueva experiencia

### <a name="new-installation-of-ad-fs"></a>Nueva instalación de AD FS
Si es un cliente nuevo para AD FS, recibirá el nuevo diseño de forma predeterminada.

### <a name="upgrading-a-farm"></a>Actualización de una granja de servidores
Si es un cliente existente AD FS 2012 R2 o 2016, hay dos maneras de recibir el nuevo diseño después de actualizar los servidores a AD FS 2019 y habilitar FBL en 2019.

- Permita el nuevo inicio de sesión a través de PowerShell. Ejecute el siguiente comando para habilitar la paginación:``Set-AdfsGlobalAuthenticationPolicy -EnablePaginatedAuthenticationPages $true``

 - Habilite la autenticación externa como principal, ya sea a través de PowerShell o a través del Administrador del servidor de AD FS. La nueva página de inicio de sesión paginada se habilitará cuando se habilite esta característica.
Si es un cliente nuevo para AD FS, recibirá el nuevo diseño de forma predeterminada. Sin embargo, si es un cliente existente con AD FS 2012 R2 o 2016, debe realizar varios pasos para recibir el nuevo diseño:``Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true``

## <a name="customization"></a>Personalización
Las opciones de personalización seguirán siendo aplicables en AD FS 2019.
A continuación se muestran algunos vínculos a otros documentos para su referencia.

• Para aquellos que no tienen previsto actualizar sus servidores a AD FS 2019 pero desean el nuevo diseño: [usar un tema Web de Azure ad UX en servicios de Federación de Active Directory (AD FS)](azure-ux-web-theme-in-ad-fs.md)

• Una ubicación central para la personalización: [AD FS la personalización del inicio de sesión del usuario](ad-fs-user-sign-in-customization.md)
