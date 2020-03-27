---
title: Planear el acceso remoto con autenticación OTP
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f0b030667b0b10a22b4e90d1ddff87086c04aa0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313605"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planear el acceso remoto con autenticación OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y la VPN del servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.  
  
  
-  Paso 1: [implementar un único servidor de DirectAccess con configuración avanzada](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Este paso incluye la planificación de la infraestructura necesaria para implementar un solo servidor. Incluye la planeación de la configuración de red y del servidor, los requisitos de certificados, la configuración de DNS, la implementación del servidor de ubicación de red, los servidores de administración de DirectAccess, la configuración de Active Directory y los objetos de directiva de grupo (GPO).  
  
-   [Paso 2: planear la implementación del servidor RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Paso 3: planeamiento de la implementación de certificados OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Paso 4: planear la OTP en el servidor de acceso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Una vez que haya completado estos pasos de planeación, consulte [configurar el acceso remoto con autenticación OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Para obtener información sobre cómo configurar una implementación multisitio como una prueba de concepto en un entorno de laboratorio, vea [Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


