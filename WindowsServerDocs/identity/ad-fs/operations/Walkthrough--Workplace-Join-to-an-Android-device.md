---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 'Tutorial: Workplace Join a un dispositivo Android'
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a52c5b6e808094e1ef31c800e8a724dd0a37f3d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816088"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Tutorial: Workplace Join a un dispositivo Android



## <a name="join-your-device-with-workplace-join"></a>Unir un dispositivo al área de trabajo

> [!NOTE]
> La Unión al área de trabajo de Android requiere Registro de dispositivos de Azure Active Directory servicio. Para aplicar las directivas de dispositivo condicional en local, la herramienta de sincronización de directorios (DirSync) debe implementarse con la opción de reescritura de objetos de dispositivo habilitada. En este momento, la reescritura de dispositivos en Active Directory de Azure Active Directory puede tardar hasta 3 horas. Por lo tanto, los usuarios deben esperar 3 horas para acceder a las aplicaciones web locales, después de crear una cuenta profesional. Para obtener más información acerca de cómo implementar Registro de dispositivos de Azure Active Directory servicio, consulte [registro de dispositivos de Azure Active Directory información general sobre el servicio](https://msdn.microsoft.com/library/azure/dn788908.aspx) .

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Creación de una cuenta profesional que se une al dispositivo con Workplace join

1.  Tendrá que instalar Azure Authenticator aplicación en el dispositivo para crear una cuenta profesional que se una al dispositivo con la Unión al área de trabajo. La siguiente dirección URL contiene instrucciones sobre cómo instalar la aplicación Azure Authenticator en el dispositivo Android y agregar una cuenta profesional. La cuenta profesional convierte el dispositivo Android en un dispositivo de confianza y proporciona el inicio de sesión único (SSO) a las aplicaciones del dispositivo. Puede usar el dispositivo de confianza para acceder a aplicaciones web y aplicaciones de línea de negocio modernas como recomienda el administrador de ti. Para obtener más información, vea [Azure Authenticator para Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Consulta también
[Unirse al área de trabajo desde cualquier dispositivo para SSO y autenticación de segundo factor sin problemas en todas las aplicaciones](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md) de la empresa
[configurar el acceso condicional local mediante registro de dispositivos de Azure Active Directory servicio](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


