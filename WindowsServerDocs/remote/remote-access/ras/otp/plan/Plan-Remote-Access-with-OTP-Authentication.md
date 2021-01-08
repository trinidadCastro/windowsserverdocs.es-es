---
title: Planear el acceso remoto con autenticación OTP
description: Obtenga información acerca de los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.
manager: brianlic
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: a3da7c4b67180f7b203e19111b3887432043ac9a
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040115"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planear el acceso remoto con autenticación OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y la VPN del servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.


-  Paso 1: [implementar un único servidor de DirectAccess con configuración avanzada](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md). Este paso incluye la planificación de la infraestructura necesaria para implementar un solo servidor. Incluye la planeación de la configuración de red y del servidor, los requisitos de certificados, la configuración de DNS, la implementación del servidor de ubicación de red, los servidores de administración de DirectAccess, la configuración de Active Directory y los objetos de directiva de grupo (GPO).

-   [Paso 2: planear la implementación del servidor RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)

-   [Paso 3: planeamiento de la implementación de certificados OTP](Step-3-Plan-OTP-Certificate-Deployment.md)

-   [Paso 4: planear la OTP en el servidor de acceso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)

Una vez que haya completado estos pasos de planeación, consulte [configurar el acceso remoto con autenticación OTP](../configure/configure-ra-with-otp-authentication.md). Para obtener información sobre cómo configurar una implementación multisitio como una prueba de concepto en un entorno de laboratorio, vea [Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID](../../../directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid.md).

