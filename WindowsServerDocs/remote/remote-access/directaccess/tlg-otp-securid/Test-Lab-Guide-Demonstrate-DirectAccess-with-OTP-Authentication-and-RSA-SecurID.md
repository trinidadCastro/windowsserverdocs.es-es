---
title: 'Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID'
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ac60bdd5d8259197641a6aeaf332d8e671a7e97
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281249"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guía del laboratorio de pruebas para la Demostrar DirectAccess con autenticación OTP y RSA SecurID

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Acceso remoto es un rol de servidor en el sistema operativo Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 que permite que los usuarios remotos obtener acceso seguro a recursos de red internos mediante DirectAccess o redes privadas virtuales (VPN) con el enrutamiento y el servicio de acceso remoto (RRAS). Esta guía contiene instrucciones paso a paso para ampliar el [Test Lab Guide: Demostrar la instalación de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para mostrar una configuración de contraseña de un solo uso (OTP) de acceso remoto.  
  
> [!WARNING]  
> El diseño de esta guía de laboratorio de pruebas incluye servidores de infraestructura, como un controlador de dominio y una entidad de certificación (CA) que ejecutan Windows Server 2012 R2 o Windows Server 2012. Uso de esta guía de laboratorio de pruebas para configurar servidores de infraestructura que ejecutan otros sistemas operativos no se ha probado y no se incluyen instrucciones para configurar otros sistemas operativos en esta guía.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Acceso remoto en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 agrega compatibilidad para la autenticación de cliente con OTP. Para los fines de este laboratorio de pruebas solo RSA SecurID se usa para demostrar la funcionalidad OTP con el acceso remoto. Otros RADIUS en función de OTP soluciones también se admiten, pero están fuera del ámbito de este laboratorio de pruebas. Esta guía contiene instrucciones para configurar y mostrar el acceso remoto usando seis servidores y dos equipos cliente. El acceso remoto completada con el laboratorio de pruebas OTP simula una intranet, Internet y una red doméstica y se muestra la funcionalidad de acceso remoto en diferentes escenarios de conexión de Internet.  
  
> [!IMPORTANT]  
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.  
  


