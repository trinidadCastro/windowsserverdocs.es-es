---
title: Planear el acceso remoto con autenticación OTP
description: En este tema forma parte de la Guía de implementación de acceso remoto con autenticación OTP en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fafc41ef5dbd2cd7f98fa2552426a97622ec8634
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863226"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planear el acceso remoto con autenticación OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y enrutamiento y servicio de acceso remoto (RRAS) VPN en un solo rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2012 o de Windows Server 2016.  
  
  
-  Paso 1: [Implementar un único servidor de DirectAccess con configuración avanzada](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Este paso incluye la planificación de la infraestructura necesaria para implementar un único servidor. Comprende la planificación de la red y configuración del servidor, los requisitos de certificados, configuración de DNS, implementación de servidor de ubicación de red, servidores de administración de DirectAccess, configuración de Active Directory y objetos de directiva de grupo (GPO).  
  
-   [Paso 2: Planear la implementación de servidor RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Paso 3: Planear la implementación del certificado OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Paso 4: Plan para OTP en el servidor de acceso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Después de haber completado estos pasos de planificación, consulte [configuración del acceso remoto con autenticación OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Para obtener información sobre cómo configurar una implementación multisitio como una prueba de concepto en un entorno de laboratorio, consulte [Test Lab Guide: Demostrar DirectAccess con autenticación OTP y RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


