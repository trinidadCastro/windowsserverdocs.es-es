---
title: 'Guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess'
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c98106c-67cc-406a-810e-f2e09f7e2c5e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb4d69f914331e8fa7a06c6dd77ea342d5eefc6a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388221"
---
# <a name="test-lab-guide-demonstrate-a-directaccess-multisite-deployment"></a>Guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El acceso remoto es un rol de servidor de los sistemas operativos Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 que permite a los usuarios remotos acceder de forma segura a los recursos de red internos mediante DirectAccess o RRAS VPN. Esta guía contiene instrucciones paso a paso para ampliar la guía del [laboratorio de pruebas: demostración de la configuración de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004) para demostrar el acceso remoto en un escenario de varios sitios.  
  
La implementación del acceso remoto en un escenario multisitio le permite configurar servidores de acceso remoto en ubicaciones geográficamente distintas. Anteriormente, los usuarios remotos debían conectarse siempre a la red corporativa a través de un servidor de DirectAccess determinado. Con Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 y Windows 10 o Windows 8, puede configurar puntos de entrada para cada ubicación geográfica de la implementación. Cada punto de entrada puede ser un único servidor de acceso remoto o un clúster de servidores de acceso remoto. Los usuarios remotos tienen la opción de conectarse a cualquiera de los puntos de entrada de acceso remoto de la organización. Por ejemplo, si un usuario remoto normalmente se conecta al punto de entrada de acceso remoto que se encuentra en Asia, pero después llega a un viaje de negocio a Europe, el equipo cliente se conecta automáticamente al punto de entrada de acceso remoto más cercano.  
  
## <a name="about-this-guide"></a>Acerca de esta guía  
Esta guía contiene instrucciones para configurar y mostrar el acceso remoto con nueve servidores y tres equipos cliente. El laboratorio de prueba de acceso remoto completado de multisitio simula una intranet, Internet y una red doméstica, y demuestra la funcionalidad de acceso remoto en distintos escenarios de conexión a Internet.  
  
> [!IMPORTANT]  
> Este laboratorio sirve como prueba de concepto con la cantidad mínima de equipos. La configuración que se detalla en esta guía es para fines de laboratorio únicamente y no se debe usar en un entorno de producción.  
  


