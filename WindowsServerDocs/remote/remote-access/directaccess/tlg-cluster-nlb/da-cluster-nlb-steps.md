---
title: Pasos para configurar el laboratorio de pruebas NLB de clúster de DirectAccess
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2ebda017b41f27c2f69c7b850de44e771732415d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283331"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Pasos para configurar el laboratorio de pruebas NLB de clúster de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los pasos siguientes describen cómo configurar la infraestructura de acceso remoto, los clientes y servidores de acceso remoto y probar la conectividad de DirectAccess desde las subredes de Internet y Homenet.  
  
En esta prueba Guía del laboratorio se compilará un equilibrio de carga de red (NLB) habilitado clúster de acceso remoto mediante los pasos siguientes:  
  
-   [PASO 1 completar la configuración de DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Complete los pasos descritos en el [Test Lab Guide: Demostrar la instalación de servidor único de DirectAccess con IPv4 e IPv6 mixto](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [PASO 2: Configurar EDGE1](STEP-2-Configure-EDGE1.md). Configurar el rol de acceso remoto en EDGE1 para equilibrar la carga.  
  
-   [PASO 3: Instalar y configurar EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 actúa como el segundo servidor de acceso remoto en un clúster de acceso remoto.  
  
-   [PASO 4: Crear el clúster de acceso remoto con equilibrio de carga de red](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 está configurado como el primer servidor en un clúster de acceso remoto. EDGE2 se une al clúster y NLB está configurado para el clúster.  
  
-   [PASO 5: Probar la conectividad de DirectAccess desde Internet y a través del clúster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Una vez completada la configuración de NLB y clúster puede probar la conectividad de cliente de DirectAccess a través del clúster con equilibrio de carga.  
  
-   [PASO 6: Probar la conectividad de cliente de DirectAccess desde detrás de un dispositivo NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Mover el equipo cliente detrás de un dispositivo NAT para simular probar conectividad del cliente de DirectAccess desde detrás de un enrutador doméstico.  
  
-   [PASO 7: Probar la conectividad al volver a la red corporativa](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Asegúrese de que el equipo cliente puede aún tener acceso a recursos corporativos cuando se devuelven a la red corporativa.  
  
-   [PASO 8: Instantánea de la configuración](da-cluster-nlb-s8-snapshot.md). Después de completar el laboratorio de pruebas, tome una instantánea del uso de clúster de NLB de acceso remoto para que puede volver a él más adelante para probar los escenarios adicionales.  
  


