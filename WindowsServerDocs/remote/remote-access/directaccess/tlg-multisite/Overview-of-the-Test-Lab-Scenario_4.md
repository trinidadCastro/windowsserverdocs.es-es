---
title: Información general sobre el escenario de laboratorio de pruebas
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
ms.assetid: 9afeced4-1a9b-4cb3-9fc4-d7e44c675755
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5943986620328049f89b4613bb415fefca8a764f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875276"
---
# <a name="overview-of-the-test-lab-scenario"></a>Información general sobre el escenario de laboratorio de pruebas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este escenario de laboratorio de prueba, se implementa DirectAccess con:  
  
-   **DC1**- un servidor que está configurado como un controlador de dominio, servidor DNS y servidor DHCP para el dominio corp.contoso.com.  
  
-   **2-DC1**- un servidor que está configurado como un controlador de dominio y servidor DNS para el dominio corp2.corp.contoso.com.  
  
-   **EDGE1 y 2 EDGE1**-dos servidores en la red interna que están configurados como servidores de acceso remoto. Cada servidor tiene dos adaptadores de red; uno conectado a la red interna y el otro conectado a la red externa.  
  
-   **App1 y 2-APP1**-dos servidores en la red interna que están configurados como servidores web y de archivo.  
  
-   **APP2**- un equipo en la red interna que está configurado como un servidor web y el archivo solo de IPv4. Este equipo se usa para resaltar las capacidades de NAT64/DNS64.  
  
-   **ENRUTADOR1**- un servidor que está configurado para proporcionar enrutamiento entre las dos redes corporativas internas.  
  
-   **INET1**- un servidor que está configurado como un servidor DHCP y DNS de Internet.  
  
-   **NAT1**- un equipo cliente que está configurado como un dispositivo de traductor (NAT) de direcciones de red mediante conexión compartida a Internet.  
  
-   **Client1 y CLIENT2**-dos equipos cliente que están configurados como clientes de DirectAccess que se usará para probar la conectividad de DirectAccess cuando se mueve entre la red interna, la red Internet simulada y una red doméstica. **Client2** es Windows 7&reg; cliente.  
  
El laboratorio de pruebas consta de cuatro subredes que simulan la forma siguiente:  
  
-   Una red doméstica denominada Homenet (192.168.137.0/24) conectado a Internet mediante un dispositivo NAT.  
  
-   La red externa representada por la subred de Internet (131.107.0.0/24).  
  
-   Una red interna denominada Corpnet (10.0.0.0/24; 2001:db8:1:: / 64) separados de Internet por el servidor de acceso remoto EDGE1.  
  
-   Una red interna denominada 2 Corpnet1 (10.2.0.0/24; 2001:db8:2:: / 64) separados de Internet por el servidor de acceso remoto 2 EDGE1.  
  
Los equipos de cada subred se conectan mediante un concentrador físico o virtual o un conmutador, tal como se muestra en la ilustración siguiente.  
  
![Información general sobre el laboratorio de pruebas](../../../media/Overview-of-the-Test-Lab-Scenario_4/TLG_DA_Multisite.png)  
  


