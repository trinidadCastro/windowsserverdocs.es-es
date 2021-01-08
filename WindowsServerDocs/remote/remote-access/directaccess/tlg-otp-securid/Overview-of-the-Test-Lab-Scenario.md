---
title: Información general del escenario de laboratorio de pruebas autenticación de OTP y RSA SecurID
description: Obtenga información acerca de cómo ampliar la configuración de la configuración de un solo servidor de DirectAccess con la guía del laboratorio de pruebas IPv4 e IPv6 mixta para mostrar una configuración de contraseña de un solo acceso remoto (OTP).
manager: brianlic
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: fc6f8696401f2284da19a989153ab2b87c453757
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040095"
---
# <a name="overview-of-the-test-lab-scenario-otp-authentication-and-rsa-securid"></a>Información general del escenario de laboratorio de pruebas autenticación de OTP y RSA SecurID

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El acceso remoto es un rol de servidor de los sistemas operativos Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 que permite a los usuarios remotos acceder de forma segura a los recursos de la red interna mediante DirectAccess o redes privadas virtuales (VPN) con el servicio de enrutamiento y acceso remoto (RRAS). Esta guía contiene instrucciones paso a paso para ampliar la guía del [laboratorio de pruebas: demostrar la configuración de un solo servidor de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para mostrar una configuración de contraseña de un solo acceso remoto (OTP).

> [!WARNING]
> El diseño de esta guía del laboratorio de pruebas incluye servidores de infraestructura, como un controlador de dominio y una entidad de certificación (CA) que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012. El uso de esta guía del laboratorio de pruebas para configurar los servidores de infraestructura que ejecutan otros sistemas operativos no se ha probado y las instrucciones para configurar otros sistemas operativos no se incluyen en esta guía.

## <a name="about-this-guide"></a>Acerca de esta guía
El acceso remoto en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 agrega compatibilidad para la autenticación de cliente con OTP. Para los fines de este laboratorio de pruebas, RSA SecurID solo se usa para demostrar la funcionalidad de OTP con acceso remoto. También se admiten otras soluciones OTP basadas en RADIUS, pero están fuera del ámbito de este laboratorio de pruebas. Esta guía contiene instrucciones para configurar y mostrar el acceso remoto usando seis servidores y dos equipos cliente. El laboratorio de prueba de acceso remoto completado con OTP simula una intranet, Internet y una red doméstica, y muestra la funcionalidad de acceso remoto en distintos escenarios de conexión a Internet.

> [!IMPORTANT]
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.



