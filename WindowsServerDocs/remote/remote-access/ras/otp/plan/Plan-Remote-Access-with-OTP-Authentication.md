---
title: Planear el acceso remoto con autenticación OTP
description: Este tema forma parte de la guía deploy Remote Access with OTP Authentication in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c268b52adddc7aa36c855e7bffa265eaa8dbe2c8
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964347"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planear el acceso remoto con autenticación OTP

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

 Windows Server 2016 y Windows Server 2012 combinan DirectAccess y la VPN del servicio de enrutamiento y acceso remoto (RRAS) en un único rol de acceso remoto. Esta información general proporciona una introducción a los pasos de configuración necesarios para implementar una única implementación de multisitio de acceso remoto de Windows Server 2016 o Windows Server 2012.  
  
  
-  Paso 1: [implementar un único servidor de DirectAccess con configuración avanzada](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md). Este paso incluye la planificación de la infraestructura necesaria para implementar un solo servidor. Incluye la planeación de la configuración de red y del servidor, los requisitos de certificados, la configuración de DNS, la implementación del servidor de ubicación de red, los servidores de administración de DirectAccess, la configuración de Active Directory y los objetos de directiva de grupo (GPO).  
  
-   [Paso 2: planear la implementación del servidor RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Paso 3: planeamiento de la implementación de certificados OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Paso 4: planear la OTP en el servidor de acceso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Una vez que haya completado estos pasos de planeación, consulte [configurar el acceso remoto con autenticación OTP](../configure/configure-ra-with-otp-authentication.md). Para obtener información sobre cómo configurar una implementación multisitio como una prueba de concepto en un entorno de laboratorio, vea [Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID](../../../directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid.md).  
  
