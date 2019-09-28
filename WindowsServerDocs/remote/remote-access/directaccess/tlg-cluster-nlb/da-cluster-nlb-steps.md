---
title: Pasos para configurar el laboratorio de pruebas NLB de clúster de DirectAccess
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108c63298ad3382f5ece790258f2d278bb03b78b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388405"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Pasos para configurar el laboratorio de pruebas NLB de clúster de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En los pasos siguientes se describe cómo configurar la infraestructura de acceso remoto, configurar los clientes y servidores de acceso remoto y probar la conectividad de DirectAccess desde las subredes Internet y HomeNet.  
  
En esta guía del laboratorio de pruebas, creará un clúster de acceso remoto habilitado para equilibrio de carga de red (NLB) mediante los pasos siguientes:  
  
-   [Paso 1: completar la configuración de DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Complete todos los pasos de la guía de laboratorio @no__t 0Test: Mostrar la configuración de servidor único de DirectAccess con IPv4 e IPv6 no__t-0.  
  
-   [PASO 2: Configure EDGE1 @ no__t-0. Configure el rol de acceso remoto en EDGE1 para el equilibrio de carga.  
  
-   [PASO 3: Instale y configure EDGE2 @ no__t-0. EDGE2 actúa como segundo servidor de acceso remoto en un clúster de acceso remoto.  
  
-   [PASO 4: Cree el clúster de acceso remoto con equilibrio de carga de red @ no__t-0-EDGE1 está configurado como el primer servidor de un clúster de acceso remoto. EDGE2 está unido al clúster y NLB está configurado para el clúster.  
  
-   [PASO 5: Pruebe la conectividad de DirectAccess desde Internet y a través del clúster @ no__t-0. Una vez completada la configuración de NLB y clúster, puede probar la Conectividad del cliente de DirectAccess a través del clúster con equilibrio de carga.  
  
-   [PASO 6: Probar la Conectividad del cliente de DirectAccess desde detrás de un dispositivo NAT @ no__t-0. Mueva el equipo cliente detrás de un dispositivo NAT para simular la comprobación de la Conectividad del cliente de DirectAccess desde detrás de un enrutador doméstico.  
  
-   [PASO 7: Pruebe la conectividad al volver a la red corporativa @ no__t-0. Asegúrese de que el equipo cliente todavía puede acceder a los recursos corporativos al volver a la red corporativa.  
  
-   [PASO 8: Una instantánea de la configuración @ no__t-0. Después de completar el laboratorio de pruebas, realice una instantánea del clúster NLB de acceso remoto en funcionamiento para poder volver a él más adelante y probar escenarios adicionales.  
  


