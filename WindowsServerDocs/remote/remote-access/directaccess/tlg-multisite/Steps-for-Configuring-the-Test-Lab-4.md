---
title: Pasos para configurar el laboratorio de pruebas
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404701"
---
# <a name="steps-for-configuring-the-test-lab"></a>Pasos para configurar el laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los pasos siguientes se describe cómo configurar la infraestructura de acceso remoto, configurar los clientes y servidores de acceso remoto y probar la conectividad de DirectAccess desde las subredes Internet y HomeNet.  
  
En esta guía del laboratorio de pruebas, creará una implementación de acceso remoto multisitio realizando los pasos siguientes:  
  
-   [PASO 1: Complete la configuración de base @ no__t-0. Complete todos los pasos de la guía de laboratorio @no__t 0Test: Mostrar la configuración de servidor único de DirectAccess con IPv4 e IPv6 no__t-0.  
  
-   [PASO 2: Instale y configure ENRUTADOR1 @ no__t-0. ENRUTADOR1 proporciona funcionalidad de enrutamiento y reenvío entre las subredes corporativas y 2-CorpNet.  
  
-   [PASO 3: Instale y configure cliente2 @ no__t-0. Cliente2 es un equipo cliente de Windows 7 que se usa para mostrar la compatibilidad con versiones anteriores de una implementación de acceso remoto de Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.  
  
-   [PASO 4: Configure APP1 @ no__t-0. Configure APP1 con ENRUTADOR1 como la puerta de enlace predeterminada y 2-DC1 como servidor DNS alternativo.  
  
-   [PASO 5: Configure DC1 @ no__t-0. Configure DC1 con un sitio de Active Directory adicional y grupos de seguridad adicionales para equipos cliente de Windows 7.  
  
-   [PASO 6: Instale y configure 2-DC1 @ no__t-0. En una implementación multisitio, tiene dos o más dominios y sitios. 2-DC1 proporciona controladores de dominio y servicios DNS para el dominio corp2.corp.contoso.com.  
  
-   [PASO 7: Instale y configure 2-APP1 @ no__t-0. 2-APP1 es un servidor Web y de archivos en la red 2-CorpNet.  
  
-   [PASO 8: Configure INET1 @ no__t-0. INET1 simula Internet en esta guía del laboratorio de pruebas. Debe configurar una entrada DNS que se resuelva en la dirección IP pública de 2-EDGE1.  
  
-   [PASO 9: Configure EDGE1 @ no__t-0. Configure el servidor DNS de 2-CorpNet y el enrutamiento en EDGE1.  
  
-   [PASO 10: Instale y configure 2-EDGE1 @ no__t-0. Se requieren dos servidores de acceso remoto en una implementación multisitio. 2-EDGE1 proporciona servicios de acceso remoto para el segundo dominio.  
  
-   [PASO 11: Configure la implementación multisitio @ no__t-0. Después de configurar ambos servidores de acceso remoto, puede configurar la implementación multisitio.  
  
-   [PASO 12: Probar la conectividad de DirectAccess @ no__t-0. Pruebe la conectividad de DirectAccess desde ambos equipos cliente desde la subred de Internet a través de EDGE1 y 2-EDGE1.  
  
-   [PASO 13: Probar la conectividad de DirectAccess desde detrás de un dispositivo NAT @ no__t-0. Pruebe la conectividad de DirectAccess desde detrás de un dispositivo NAT.  
  
-   [PASO 14: Una instantánea de la configuración @ no__t-0. Después de completar el laboratorio de pruebas, tome una instantánea de la implementación multisitio de acceso remoto en funcionamiento para poder volver a ella más adelante y probar escenarios adicionales.  
  


