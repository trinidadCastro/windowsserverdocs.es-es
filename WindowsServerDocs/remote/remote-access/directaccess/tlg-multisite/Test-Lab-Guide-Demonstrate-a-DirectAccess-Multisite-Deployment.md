---
title: 'Guía del laboratorio de pruebas: demostrar una implementación multisitio de DirectAccess'
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47b6848789a7e61bdb3cc12e6339777ed1a4f1b3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811986"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guía del laboratorio de pruebas para la Mostrar una implementación multisitio de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Acceso remoto es un rol de servidor en los sistemas operativos Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 que permite que los usuarios remotos obtener acceso seguro a recursos de red internos mediante DirectAccess o RRAS VPN. Esta guía contiene instrucciones paso a paso para ampliar el [Test Lab Guide: Demostrar la instalación de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demostrar el acceso remoto en un escenario multisitio.  
  
Implementación de acceso remoto en un escenario de multisitio le permite configurar servidores de acceso remoto en ubicaciones geográficamente diversas. Anteriormente, eran necesario que los usuarios remotos conectarse siempre a la red corporativa a través de un servidor de DirectAccess específico. Con Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y Windows 10 o Windows 8, puede configurar puntos de entrada para cada ubicación geográfica en la implementación. Cada punto de entrada puede ser un único servidor de acceso remoto o un clúster de servidores de acceso remoto. Los usuarios remotos tener la opción de conectarse a cualquiera de los puntos de entrada de acceso remoto de la organización. Por ejemplo, si un usuario remoto normalmente se conecta al punto de entrada de acceso remoto ubicado en Asia, pero, a continuación, entra en un viaje de negocios en Europa, el equipo cliente se conecta automáticamente al punto de entrada de acceso remoto más cercano.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía contiene instrucciones para configurar y demostrar el acceso remoto con nueve servidores y tres equipos cliente. El laboratorio de pruebas de multisitio de acceso remoto completado simula una intranet, Internet y una red doméstica y muestra la funcionalidad de acceso remoto en diferentes escenarios de conexión de Internet.  
  
> [!IMPORTANT]  
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.  
  


