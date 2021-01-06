---
title: 'Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID'
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID para Windows Server 2016'
manager: brianlic
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 14a7af29c0c927f33cbc36313a080e46e35f3263
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949261"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guía del laboratorio de pruebas: demostración de DirectAccess con autenticación OTP y RSA SecurID

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El acceso remoto es un rol de servidor en el sistema operativo Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 que permite a los usuarios remotos acceder de forma segura a los recursos de la red interna mediante DirectAccess o redes privadas virtuales (VPN) con el servicio de enrutamiento y acceso remoto (RRAS). Esta guía contiene instrucciones paso a paso para ampliar la guía del [laboratorio de pruebas: demostrar la configuración de un solo servidor de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para mostrar una configuración de contraseña de un solo acceso remoto (OTP).

> [!WARNING]
> El diseño de esta guía del laboratorio de pruebas incluye servidores de infraestructura, como un controlador de dominio y una entidad de certificación (CA) que ejecutan Windows Server 2012 R2 o Windows Server 2012. El uso de esta guía del laboratorio de pruebas para configurar los servidores de infraestructura que ejecutan otros sistemas operativos no se ha probado y las instrucciones para configurar otros sistemas operativos no se incluyen en esta guía.

## <a name="about-this-guide"></a>Acerca de esta guía
El acceso remoto en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 agrega compatibilidad para la autenticación de cliente con OTP. Para los fines de este laboratorio de pruebas, RSA SecurID solo se usa para demostrar la funcionalidad de OTP con acceso remoto. También se admiten otras soluciones OTP basadas en RADIUS, pero están fuera del ámbito de este laboratorio de pruebas. Esta guía contiene instrucciones para configurar y mostrar el acceso remoto usando seis servidores y dos equipos cliente. El laboratorio de prueba de acceso remoto completado con OTP simula una intranet, Internet y una red doméstica, y muestra la funcionalidad de acceso remoto en distintos escenarios de conexión a Internet.

> [!IMPORTANT]
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.



