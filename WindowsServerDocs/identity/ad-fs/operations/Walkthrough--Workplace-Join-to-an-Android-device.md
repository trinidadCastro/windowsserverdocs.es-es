---
ms.assetid: a33bd54c-e6db-4b58-8264-c0f34bd8ba39
title: "Tutorial - unirse a un área de trabajo a un dispositivo Android"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cfe26947b6b0de28ea50367f82d52815fff8f323
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="walkthrough-workplace-join-to-an-android-device"></a>Tutorial: Unión a un área de trabajo a un dispositivo Android

>Se aplica a: Windows Server 2016, Windows Server 2012 R2


## <a name="join-your-device-with-workplace-join"></a>Une el dispositivo al área de trabajo

> [!NOTE]
> Unirse a un área de trabajo Android requiere el servicio de registro de Azure Active Directory dispositivo. Para aplicar dispositivo condicional directivas locales, la herramienta de sincronización (DirSync) se deben implementar con la opción de reescritura del objeto de dispositivo habilitada. En este momento, dispositivo reescritura a Active Directory de Azure Active Directory puede tardar hasta 3 horas. Como tal, los usuarios deben esperar a 3 horas acceder a las aplicaciones web local y después de crear una cuenta de trabajo. Para obtener más información acerca de la implementación de registro del dispositivo de Azure Active Directory de servicio, consulte la sección [Introducción a Azure Active Directory dispositivo registro de servicio](https://msdn.microsoft.com/library/azure/dn788908.aspx)

#### <a name="create-a-work-account-that-joins-your-device-with-workplace-join"></a>Crear una cuenta de trabajo que se une a tu dispositivo con la combinación de empresa

1.  Tendrás que instalar la aplicación de Azure Authenticator en el dispositivo para crear una cuenta de trabajo que se une a tu dispositivo con unirse a un área de trabajo. La siguiente dirección URL tiene instrucciones sobre cómo instalar la aplicación autenticador de Azure en tu dispositivo Android y agregar una cuenta de trabajo. La cuenta de trabajo hace que tu dispositivo de Android en un dispositivo de confianza y proporciona el inicio de sesión único (SSO) para las aplicaciones en el dispositivo. Puedes usar el dispositivo de confianza para el acceso a las aplicaciones web y aplicaciones de línea de negocio modernas recomendada por el Administrador de TI. Para obtener más información, consulta [Azure Authenticator para Android](https://docs.microsoft.com/azure/multi-factor-authentication/end-user/microsoft-authenticator-app-how-to).

## <a name="see-also"></a>Consulta también
[Únete a área de trabajo desde cualquier dispositivo para SSO y transparente segundo Factor de autenticación a través de aplicaciones de la compañía](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
[configurar acceso condicional local mediante el servicio de registro de Azure Active Directory dispositivo](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)


