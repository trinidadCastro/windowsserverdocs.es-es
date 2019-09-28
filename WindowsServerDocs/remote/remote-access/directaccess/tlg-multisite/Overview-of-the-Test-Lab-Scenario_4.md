---
title: Información general sobre el escenario de laboratorio de pruebas
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bcee25c4a13afc2b41d6b1a9a43a1489054ef43d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388408"
---
# <a name="overview-of-the-test-lab-scenario"></a>Información general sobre el escenario de laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este escenario de laboratorio de pruebas, DirectAccess se implementa con:  
  
-   **DC1**: un servidor que está configurado como controlador de dominio, servidor DNS y servidor DHCP para el dominio Corp.contoso.com.  
  
-   **2-DC1**: un servidor que está configurado como un controlador de dominio y un servidor DNS para el dominio Corp2.Corp.contoso.com.  
  
-   **EDGE1 y 2-EDGE1**: dos servidores de la red interna que están configurados como servidores de acceso remoto. Cada servidor tiene dos adaptadores de red; uno conectado a la red interna y el otro conectado a la red externa.  
  
-   **App1 y 2-app1**: dos servidores de la red interna que están configurados como servidores web y de archivos.  
  
-   **APP2**: un equipo de la red interna que está configurado como un servidor Web y de archivos de solo IPv4. Este equipo se usa para resaltar las capacidades de NAT64/DNS64.  
  
-   **ENRUTADOR1**: un servidor que está configurado para proporcionar enrutamiento entre las dos redes internas corporativas.  
  
-   **INET1**: un servidor que está configurado como servidor DNS y DHCP de Internet.  
  
-   **NAT1**: un equipo cliente que está configurado como un dispositivo de traductor de direcciones de red (NAT) mediante conexión compartida A Internet.  
  
-   **CLIENT1 y cliente2**: dos equipos cliente que están configurados como clientes de DirectAccess que se usarán para probar la conectividad de DirectAccess al moverse entre la red interna, la Internet simulada y una red doméstica. **Cliente2** es un cliente de Windows 7 @ no__t-1.  
  
El laboratorio de pruebas consta de cuatro subredes que simulan lo siguiente:  
  
-   Una red doméstica llamada HomeNet (192.168.137.0/24) conectada a Internet por una NAT.  
  
-   La red externa representada por la subred de Internet (131.107.0.0/24).  
  
-   Una red interna denominada CorpNet (10.0.0.0/24; 2001: db8:1::/64) separada de Internet por el servidor de acceso remoto de EDGE1.  
  
-   Una red interna denominada 2-Corpnet1 (10.2.0.0/24; 2001: db8:2::/64) separada de Internet por el servidor de acceso remoto 2-EDGE1.  
  
Los equipos de cada subred se conectan mediante un concentrador o un concentrador físico o virtual, tal y como se muestra en la ilustración siguiente.  
  
![Información general sobre el laboratorio de pruebas](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


