---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: 'Tutorial: unión a un dispositivo Android'
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73dbe4d62d460f9487467c7d4198d62b3b6af539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188930"
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Tutorial: Unión a un dispositivo Android



## <a name="join-your-device-with-workplace-join"></a>Unir un dispositivo al área de trabajo

> [!NOTE]
> El área de trabajo Android requiere Azure Active Directory Device Registration Service. Para aplicar las directivas condicionales de dispositivo local, se debe implementar la herramienta de sincronización de directorios (DirSync) con la opción de escritura diferida de objetos de dispositivo habilitada. En este momento, escritura diferida de dispositivos a Active Directory de Azure Active Directory puede tardar hasta 3 horas. Por lo tanto, los usuarios deben esperar durante 3 horas tener acceso a aplicaciones web locales, después de crear una cuenta profesional. Para obtener más información sobre cómo implementar el registro de dispositivos de Azure Active Directory service, consulte la sección [Azure Active Directory registro servicio de información general sobre dispositivos](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Crear una cuenta profesional que se une el dispositivo con workplace Join

1.  Deberá instalar la aplicación Azure Authenticator en su dispositivo para crear una cuenta de trabajo que se une el dispositivo al área de trabajo. La siguiente dirección URL contiene instrucciones sobre cómo instalar Azure Authenticator en su dispositivo Android y agregar una cuenta profesional. La cuenta de trabajo hace que el dispositivo Android en un dispositivo de confianza y proporciona inicio de sesión único (SSO) para las aplicaciones en el dispositivo. Puede usar el dispositivo de confianza para acceder a aplicaciones web y aplicaciones de línea de negocio modernas recomendada por el Administrador de TI. Para obtener más información, consulte [Azure Authenticator para Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Vea también
[Unirse a un área de trabajo desde cualquier dispositivo para SSO y sin problemas segundo Factor Authentication Across Company Applications](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configuración del acceso condicional local mediante Azure Active Directory Device Registration Service](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


