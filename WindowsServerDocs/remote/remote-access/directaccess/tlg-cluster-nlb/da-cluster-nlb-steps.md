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
  
-   [Paso 1: completar la configuración de DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Complete todos los pasos de la [Guía del laboratorio de pruebas: demostración de la configuración de servidor único de DirectAccess con IPv4 e IPv6 mixtos](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Paso 2: configurar EDGE1](STEP-2-Configure-EDGE1.md). Configure el rol de acceso remoto en EDGE1 para el equilibrio de carga.  
  
-   [Paso 3: instalar y configurar EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 actúa como segundo servidor de acceso remoto en un clúster de acceso remoto.  
  
-   [Paso 4: crear el clúster de acceso remoto con equilibrio de carga de red](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md): EDGE1 está configurado como el primer servidor de un clúster de acceso remoto. EDGE2 está unido al clúster y NLB está configurado para el clúster.  
  
-   [Paso 5: probar la conectividad de DirectAccess desde Internet y a través del clúster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Una vez completada la configuración de NLB y clúster, puede probar la Conectividad del cliente de DirectAccess a través del clúster con equilibrio de carga.  
  
-   [Paso 6: probar la Conectividad del cliente de DirectAccess desde detrás de un dispositivo NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Mueva el equipo cliente detrás de un dispositivo NAT para simular la comprobación de la Conectividad del cliente de DirectAccess desde detrás de un enrutador doméstico.  
  
-   [Paso 7: probar la conectividad al volver a la red corporativa](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Asegúrese de que el equipo cliente todavía puede acceder a los recursos corporativos al volver a la red corporativa.  
  
-   [Paso 8: instantánea de la configuración](da-cluster-nlb-s8-snapshot.md). Después de completar el laboratorio de pruebas, realice una instantánea del clúster NLB de acceso remoto en funcionamiento para poder volver a él más adelante y probar escenarios adicionales.  
  


