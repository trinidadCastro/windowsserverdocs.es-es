---
title: Información general sobre el escenario de laboratorio de prueba de NLB de clúster de DirectAccess
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de DirectAccess en un clúster con Windows NLB para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cd1e9efd-19e9-49e7-8432-881f661c9792
ms.author: lizross
author: eross-msft
ms.openlocfilehash: abc3038cfe0dacb09c115f37289fe72f1c14b96d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308855"
---
# <a name="overview-of-the-directaccess-cluster-nlb-test-lab-scenario"></a>Información general sobre el escenario de laboratorio de prueba de NLB de clúster de DirectAccess

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este escenario de laboratorio de pruebas, DirectAccess se implementa con:  
  
-   **DC1**: un servidor que está configurado como un controlador de dominio, un servidor de sistema de nombres de dominio (DNS) y un servidor de protocolo de configuración dinámica de host (DHCP).  
  
-   **EDGE1**: un servidor de la red interna que está configurado como primer servidor de acceso remoto en un clúster de servidores de acceso remoto. Este servidor tiene dos adaptadores de red; uno conectado a la red interna y el otro conectado a la red externa.  
  
-   **EDGE2**: un servidor de la red interna que está configurado como segundo servidor de acceso remoto en un clúster de servidores de acceso remoto. Este servidor tiene dos adaptadores de red; uno conectado a la red interna y el otro conectado a la red externa.  
  
-   **App1**: un servidor de la red interna que está configurado como un servidor Web y de archivos, y como una entidad de certificación (CA) raíz de empresa  
  
-   **APP2**: un equipo de la red interna que está configurado como un servidor Web y de archivos de solo IPv4. Este equipo se usa para resaltar las capacidades de NAT64/DNS64.  
  
-   **INET1**: un servidor que está configurado como servidor DNS y DHCP de Internet.  
  
-   **NAT1**: un equipo cliente que está configurado como un dispositivo de traductor de direcciones de red (NAT) mediante conexión compartida A Internet.  
  
-   **CLIENT1**: un equipo cliente que está configurado como un cliente de DirectAccess y que se usará para probar la conectividad de DirectAccess al moverse entre la red interna, la Internet simulada y una red doméstica.  
  
El laboratorio de pruebas consta de tres subredes que simulan lo siguiente:  
  
-   Una red doméstica llamada HomeNet (192.168.137.0/24) conectada a Internet por una NAT.  
  
-   La red externa representada por la subred de Internet (131.107.0.0/24).  
  
-   Una red interna denominada CorpNet (10.0.0.0/24; 2001: db8:1::/64) separada de Internet por el servidor de acceso remoto.  
  
Los equipos de cada subred se conectan mediante un concentrador o un concentrador físico o virtual, tal y como se muestra en la ilustración siguiente.  
  
![Información general sobre el laboratorio de pruebas](../../../media/Overview-of-the-Test-Lab-Scenario_5/TLG_DA_Cluster.png)  
  


