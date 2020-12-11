---
description: 'Más información acerca de: unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en las aplicaciones de la empresa'
ms.assetid: e22d84a5-113d-4bec-b484-036ed29f0c28
title: unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/05/2017
ms.topic: article
ms.openlocfilehash: 3f681ba75ea55245948b1ed9acfd84830b3f3909
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97039803"
---
# <a name="join-to-workplace-from-any-device-for-sso-and-seamless-second-factor-authentication-across-company-applications"></a>unirse a un área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones de la compañía



El rápido aumento en el número de dispositivos de consumo y el acceso a la información desde cualquier lugar está cambiando la manera en que las personas perciben su tecnología. El uso constante de tecnologías de la información durante todo el día, junto con el fácil acceso a la información, está difuminando los límites tradicionales entre trabajo y vida privada. Estos límites de cambio van acompañados de la creencia de que la tecnología personal (seleccionada y personalizada para adaptarse a las personalidades, actividades y programaciones de los usuarios) debe extenderse al lugar de trabajo. Para satisfacer la creciente demanda de que los dispositivos de consumo personal se conecten a las redes empresariales, estamos incorporando las siguientes propuestas:

-   Los administradores pueden controlar quién tiene acceso a los recursos de la compañía según la aplicación, el usuario, el dispositivo y la ubicación.

-   Los empleados pueden acceder a aplicaciones y datos en cualquier lugar, en cualquier dispositivo. Los empleados pueden usar el inicio de sesión único en aplicaciones de explorador o en aplicaciones empresariales.

## <a name="key-concepts-introduced-in-the-solution"></a>Principales conceptos que incorpora la solución

### <a name="workplace-join"></a>Unión al área de trabajo
Con la unión al área de trabajo, los trabajadores de la información pueden unir sus dispositivos personales con sus equipos de trabajo de la compañía para acceder a los recursos y servicios de la compañía. Cuando unas tu dispositivo personal al área de trabajo, se convertirá en un dispositivo conocido y proporciona autenticación de segundo factor sin problemas e inicio de sesión único para las aplicaciones y los recursos del área de trabajo. Cuando un dispositivo se une mediante unión al área de trabajo, los atributos del dispositivo se pueden recuperar del directorio para controlar el acceso condicional con el fin de autorizar la emisión de tokens de seguridad para las aplicaciones. Los dispositivos Windows 8.1, iOS 6.0+ y Android 4.0+ se pueden unir mediante unión al área de trabajo.

### <a name="azure-active-directory-device-registration-service"></a><a name="BKMK_DRS"></a>Servicio de registro de dispositivo de Azure Active Directory
Unión al área de trabajo se realiza mediante el servicio de registro de dispositivo de Azure Active Directory. Cuando un dispositivo se une mediante unión al área de trabajo, el servicio aprovisiona un objeto de dispositivo en Active Directory y establece una clave en el dispositivo local que se usa para representar la identidad del dispositivo. Esta identidad de dispositivo puede usarse con las reglas de control de acceso para las aplicaciones que se hospedan en la nube y localmente.

Para obtener más información, consulte [Introducción a la administración de dispositivos en Azure Active Directory](/azure/active-directory/device-management-introduction).

### <a name="workplace-join-as-a-seamless-second-factor-authentication"></a>La unión al área de trabajo como autenticación de segundo factor sin problemas
Las empresas pueden controlar el riesgo que supone el acceso a la información, así como la gobernanza y el cumplimiento normativo, al conceder a los dispositivos de consumo acceso a los recursos corporativos. La unión de dispositivos al área de trabajo ofrece las siguientes funcionalidades a los administradores:

-   Identifica los dispositivos conocidos mediante la identificación de dispositivos. Los administradores pueden usar esta información para controlar el acceso condicional así como el acceso a los recursos.

-   Proporciona una experiencia de inicio de sesión sin problemas para que los usuarios accedan a los recursos de la compañía desde dispositivos de confianza.

### <a name="single-sign-on"></a>Inicio de sesión único
En el contexto de este escenario, el inicio de sesión único (SSO) es la funcionalidad que reduce el número de peticiones de contraseña que el usuario tiene que escribir para acceder a los recursos de la compañía desde dispositivos conocidos. El resultado de esta funcionalidad es que solo se pregunta una vez a los usuarios durante todo el inicio de sesión único para acceder a las aplicaciones y recursos de la compañía desde ese dispositivo. Si un dispositivo usa la unión al área de trabajo, el usuario que está registrado para este dispositivo obtiene un inicio de sesión único persistente para siete días de forma predeterminada. Este usuario tiene una experiencia de inicio de sesión sin problemas en la misma sesión o en sesiones nuevas.

## <a name="solution-overview"></a>Información general de la solución
Como parte de esta solución, aprenderá a usar la unión al área de trabajo en un dispositivo compatible y experimentará un inicio de sesión único en un recurso de la compañía.

> [!NOTE]
> Para disponer de compatibilidad con dispositivos Windows 8.1, iOS 6.0+ y Android 4.0+, debe configurar el registro de dispositivo de Azure Active Directory junto con la reescritura de objetos de dispositivo. Consulte [Guía paso a paso para el acceso condicional local con el servicio de registro de dispositivo de Azure Active Directory](/previous-versions/azure/dn788908(v=azure.100))

Esta solución te lleva por los siguientes pasos del tutorial:

1.  [Tutorial: Workplace Join con un dispositivo Windows](../../ad-fs/operations/Walkthrough--Workplace-Join-with-a-Windows-Device.md)

2.  [Tutorial: Workplace Join con un dispositivo iOS](../../ad-fs/operations/Walkthrough--Workplace-Join-with-an-iOS-Device.md)

3.  [Tutorial: Workplace Join con un dispositivo Android](../../ad-fs/operations/walkthrough--workplace-join-to-an-android-device.md)

## <a name="see-also"></a>Consulte también
[Configuración de un servidor de Federación con el servicio de registro de dispositivos](../deployment/configure-a-federation-server-with-device-registration-service.md)
