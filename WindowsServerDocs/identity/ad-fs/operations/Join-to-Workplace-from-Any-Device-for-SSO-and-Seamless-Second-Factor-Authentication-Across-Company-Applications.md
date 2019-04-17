---
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: Unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación entre las aplicaciones de empresa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4926eb32a0bbffb092ec02ca2508fe97d52d1466
ms.sourcegitcommit: 46439194e5deb0fa5f338b428f95dd6b5b799337
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2018
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>Unir al área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación entre las aplicaciones de empresa

>Se aplica a: Windows Server 2012 R2

El rápido incremento en el número de dispositivos de consumidor y acceso a la información ubicuas está cambiando la forma que personas perciben su tecnología. El uso de constante de tecnología de la información durante todo el día junto con acceso fácil de obtener información, borrosa tradicionales límites entre trabajo y la duración de la casa. Estos límites de desplazamiento van acompañados de una creencia ese personal seleccionado a la tecnología y personalizada para ajustarse a los usuarios personalidades, actividades y planes-debes ampliar en el área de trabajo. Para adaptarse al requisito creciente de dispositivos personales estar conectado a redes de empresa, presentamos las propuestas de valor siguientes:

-   Los administradores pueden controlar quién tiene acceso a recursos de la compañía que se basan en la aplicación, el usuario, el dispositivo y ubicación.

-   Los empleados pueden acceder a aplicaciones y los datos en todas partes, en cualquier dispositivo. Los empleados pueden usar el inicio de sesión único en aplicaciones de explorador o de las aplicaciones de empresa.

## <a name="key-concepts-introduced-in-the-solution"></a>Conceptos clave que se introdujo en la solución

### <a name="workplace-join"></a>Unirse a un área de trabajo
Mediante el uso de unión a un área de trabajo, los trabajadores de información pueden unirse a sus dispositivos personales con los equipos de área de trabajo de la empresa para obtener acceso a servicios y recursos de la compañía. Cuando te unes a su dispositivo personal para tu empresa, se convierte en un dispositivo conocido y proporciona sin problemas segundo factor de autenticación e inicio de sesión único para acceder a los recursos y las aplicaciones. Cuando un dispositivo se une al área de trabajo, se pueden recuperar atributos del dispositivo desde el directorio a acceso condicional a la unidad con el fin de autorizar la emisión de tokens de seguridad para las aplicaciones. Windows 8.1 y dispositivos iOS 6.0 + y Android 4.0 + pueden estar Unidos usando al área de trabajo.

### <a name="BKMK_DRS"></a>Servicio de registro de dispositivo de Active Directory de Azure
Unirse a un área de trabajo se realiza mediante el servicio de registro del dispositivo de Azure Active Directory. Cuando un dispositivo se une al unirse a la empresa, el servicio aprovisiona un objeto de dispositivo en Azure Active Directory y, a continuación, Establece una clave en el dispositivo local que se usa para representar la identidad del dispositivo. La identidad del dispositivo puede usarse con las reglas de control de acceso para las aplicaciones que están hospedadas en la nube y local.

Para obtener más información acerca de cómo habilitar el servicio de registro de dispositivo de Azure Active Directory, consulte [Introducción a Azure Active Directory dispositivo registro servicio](https://msdn.microsoft.com/6a14cb1f-a058-4453-8ede-d9f4a66a7073.aspx).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>Unirse a un área de trabajo como un segundo sin problemas autenticación multifactor
Las empresas pueden administrar el riesgo de que está relacionada con acceso a la información y control de la unidad y cumplimiento al conceder acceso a los dispositivos a los recursos corporativos de consumidor. Unirse a un área de trabajo en dispositivos proporciona las siguientes capacidades a los administradores:

-   Identifica los dispositivos conocidos con la autenticación de dispositivo. Los administradores pueden usar esta información para controlar el acceso condicional y controlar el acceso a los recursos.

-   Proporciona una experiencia de inicio de sesión más transparente para que los usuarios acceder a recursos de empresa desde dispositivos de confianza.

### <a name="single-sign-on"></a>Inicio de sesión único
Único inicio de sesión único (SSO) en el contexto de este escenario es la funcionalidad que reduce el número de solicitudes de contraseña que el usuario final tiene que escribir para acceder a los recursos de empresa desde dispositivos conocidos. Esta funcionalidad implica que se pide a los usuarios solo una vez durante la vigencia de SSO para aplicaciones de la compañía de acceso y el recurso de este dispositivo. Si usa un dispositivo al área de trabajo, el usuario que esté registrado para usar este dispositivo obtiene SSO persistente, de manera predeterminada para siete días. Este usuario tiene una inicio de sesión experiencia en la misma sesión o en las sesiones de nuevo.

## <a name="solution-overview"></a>Información general de la solución
Como parte de esta solución, aprende a usar al área de trabajo en un dispositivo compatible y experiencia de inicio de sesión único a un recurso de empresa.

> [!NOTE]
> Compatibilidad con Windows 8.1, los dispositivos iOS 6.0 + y Android 4.0 +, debes configurar el registro de dispositivo de Azure Active Directory junto con el objeto de dispositivo reescritura, consulta [guía paso a paso para acceso condicional local mediante el servicio de registro de Azure Active Directory dispositivo](https://msdn.microsoft.com/library/azure/dn788908.aspx)

Esta solución te guía toma te guiará por los siguientes pasos Tutorial:

1.  [Tutorial: Unión a un área de trabajo con un dispositivo de Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Tutorial: Unirse un área de trabajo con un dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Tutorial: Unión a un área de trabajo con un dispositivo Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Consulta también
[Configurar un servidor de federación con el servicio de registro de dispositivo](../deployment/configure-a-federation-server-with-device-registration-service.md)



