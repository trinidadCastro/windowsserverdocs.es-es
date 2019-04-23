---
title: Información general sobre el escenario de laboratorio de prueba de NLB de clúster de DirectAccess
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 118a07b761979d3c32a8f7cf4f149aed56a1a893
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863586"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Información general sobre el escenario de laboratorio de prueba de NLB de clúster de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este escenario de laboratorio de prueba, se implementa DirectAccess con:  
  
-   **DC1**- un servidor que está configurado como un controlador de dominio, servidor de sistema de nombres de dominio (DNS) y el servidor de protocolo de configuración dinámica de Host (DHCP).  
  
-   **EDGE1**- un servidor en la red interna que está configurado como el primer servidor de acceso remoto en un clúster de servidores de acceso remoto. Este servidor tiene dos adaptadores de red; uno conectado a la red interna y el otro conectado a la red externa.  
  
-   **EDGE2**- un servidor en la red interna que está configurado como el segundo servidor de acceso remoto en un clúster de servidores de acceso remoto. Este servidor tiene dos adaptadores de red; uno conectado a la red interna y el otro conectado a la red externa.  
  
-   **App1**- un servidor en la red interna que está configurado como un servidor de archivos y web y como una entidad de certificación de raíz (CA) empresarial  
  
-   **APP2**- un equipo en la red interna que está configurado como un servidor web y el archivo solo de IPv4. Este equipo se usa para resaltar las capacidades de NAT64/DNS64.  
  
-   **INET1**- un servidor que está configurado como un servidor DHCP y DNS de Internet.  
  
-   **NAT1**- un equipo cliente que está configurado como un dispositivo de traductor (NAT) de direcciones de red mediante conexión compartida a Internet.  
  
-   **Client1**- un equipo cliente que está configurado como un cliente de DirectAccess que se usará para probar la conectividad de DirectAccess cuando se mueve entre la red interna, la red Internet simulada y una red doméstica.  
  
El laboratorio de pruebas consta de tres subredes que simulan la forma siguiente:  
  
-   Una red doméstica denominada Homenet (192.168.137.0/24) conectado a Internet mediante un dispositivo NAT.  
  
-   La red externa representada por la subred de Internet (131.107.0.0/24).  
  
-   Una red interna denominada Corpnet (10.0.0.0/24; 2001:db8:1:: / 64) separados de Internet por el servidor de acceso remoto.  
  
Los equipos de cada subred se conectan mediante un concentrador físico o virtual o un conmutador, tal como se muestra en la ilustración siguiente.  
  
![Información general sobre el laboratorio de pruebas](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


