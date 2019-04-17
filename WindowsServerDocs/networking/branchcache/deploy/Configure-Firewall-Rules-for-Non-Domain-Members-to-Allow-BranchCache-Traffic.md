---
title: Configurar las reglas de Firewall para los miembros no del dominio permitir el tráfico BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e5f744141efc35bb493bcd95fad53eafbbc3f78d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurar las reglas de Firewall para los miembros no del dominio permitir el tráfico BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar la información de este tema para configurar los firewall productos de terceros y para configurar manualmente un equipo cliente con las reglas de firewall que permitan BranchCache ejecutar en modo de caché distribuido.  
  
> [!NOTE]  
> -   Si has configurado BranchCache los equipos cliente mediante Directiva de grupo, la configuración de directiva de grupo invalida cualquier configuración manual de los equipos cliente a la que se aplican las directivas.  
> -   Si se ha implementado BranchCache con DirectAccess, puedes usar la configuración en este tema para configurar las reglas de IPsec para permitir el tráfico de BranchCache.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para realizar estos cambios de configuración.  
  
## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: del mismo nivel de protocolo de detección de recuperación y almacenamiento en caché de contenido  
Los clientes de la memoria caché distribuida deben permitir el tráfico de MS-PCCRD entrante y saliente, que se realiza en el protocolo de detección dinámica de los servicios Web (WS-Discovery).  
  
Configuración del firewall debe permitir el tráfico de multidifusión además de tráfico entrante y saliente. Puedes usar la siguiente configuración para configurar excepciones de firewall para el modo de caché distribuida.  
  
La multidifusión IPv4: 239.255.255.250  
  
Multidifusión IPv6: FF02::C  
  
El tráfico entrante: puerto Local: 3702, puerto remoto: efímera  
  
El tráfico saliente: puerto Local: puerto remoto efímera: 3702  
  
Programa: %systemroot%\system32\svchost.exe (servicio de BranchCache [PeerDistSvc])  
  
## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: del mismo nivel de almacenamiento en caché de contenido y la recuperación: protocolo de recuperación  
Los clientes de la memoria caché distribuida deben permitir el tráfico de MS-PCCRR entrante y saliente, que se realiza en el protocolo HTTP 1.1, como se indica en la solicitud de comentarios (RFC) 2616.  
  
Configuración del firewall debe permitir el tráfico entrante y saliente. Puedes usar la siguiente configuración para configurar excepciones de firewall para el modo de caché distribuida.  
  
El tráfico entrante: puerto Local: 80, puerto remoto: efímera  
  
El tráfico saliente: puerto Local: puerto remoto efímera: 80  
  


