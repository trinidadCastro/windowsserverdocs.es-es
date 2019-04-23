---
title: Información general sobre el escenario de laboratorio de pruebas
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ebf19d75928e2a19732f2c6c4acaebbf8b34f25f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871166"
---
# <a name="overview-of-the-test-lab-scenario"></a>Información general sobre el escenario de laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Acceso remoto es un rol de servidor en Windows Server 2016, con DirectAccess de recursos de red de sistemas operativos Windows Server 2012 R2 y Windows Server 2012 que permite que los usuarios remotos acceder de forma segura interno o las redes privadas virtuales (VPN) con el Enrutamiento servicio y acceso remoto (RRAS). Esta guía contiene instrucciones paso a paso para ampliar el [Test Lab Guide: Demostrar la instalación de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para mostrar una configuración de contraseña de un solo uso (OTP) de acceso remoto.  
  
> [!WARNING]  
> El diseño de esta guía de laboratorio de pruebas incluye servidores de infraestructura, como un controlador de dominio y una entidad de certificación (CA) que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. Uso de esta guía de laboratorio de pruebas para configurar servidores de infraestructura que ejecutan otros sistemas operativos no se ha probado y no se incluyen instrucciones para configurar otros sistemas operativos en esta guía.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Acceso remoto en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 agrega compatibilidad para la autenticación de cliente con OTP. Para los fines de este laboratorio de pruebas solo RSA SecurID se usa para demostrar la funcionalidad OTP con el acceso remoto. Otros RADIUS en función de OTP soluciones también se admiten, pero están fuera del ámbito de este laboratorio de pruebas. Esta guía contiene instrucciones para configurar y mostrar el acceso remoto usando seis servidores y dos equipos cliente. El acceso remoto completada con el laboratorio de pruebas OTP simula una intranet, Internet y una red doméstica y se muestra la funcionalidad de acceso remoto en diferentes escenarios de conexión de Internet.  
  
> [!IMPORTANT]  
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.  
  


