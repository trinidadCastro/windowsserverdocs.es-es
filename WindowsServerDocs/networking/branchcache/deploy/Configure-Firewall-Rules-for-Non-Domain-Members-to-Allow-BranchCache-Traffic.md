---
title: Configurar reglas de firewall para que los miembros que no son del dominio permitan el tráfico de BranchCache
description: Obtenga información acerca de cómo configurar productos de Firewall de terceros y configurar manualmente un equipo cliente con reglas de firewall que permitan que BranchCache se ejecute en modo caché distribuida.
manager: brianlic
ms.topic: get-started-article
ms.assetid: da956be0-c92d-46ea-99eb-85e2bd67bf07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b52a0c212ba7f0d30097aef6c7bd65e6d900b336
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904240"
---
# <a name="configure-firewall-rules-for-non-domain-members-to-allow-branchcache-traffic"></a>Configurar reglas de firewall para que los miembros que no son del dominio permitan el tráfico de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar la información de este tema configurar productos de firewall de terceros y configurar manualmente un equipo cliente con reglas de firewall que permitan la ejecución de BranchCache en modo Caché distribuida.

> [!NOTE]
> -   Si ha configurado equipos cliente de BranchCache utilizando la directiva de grupo, la configuración de la directiva de grupo invalida cualquier configuración manual de equipos cliente a los que se aplican las directivas.
> -   Si ha implementado BranchCache con DirectAccess, puede utilizar la configuración que se incluye en este tema para configurar las reglas IPsec de forma que permitan el tráfico de BranchCache.

El requisito mínimo para realizar estos cambios de configuración es la pertenencia al grupo **Administradores** o un grupo equivalente.

## <a name="ms-pccrd-peer-content-caching-and-retrieval-discovery-protocol"></a>[MS-PCCRD]: Protocolo de detección de recuperación y almacenamiento en caché de contenido del mismo nivel
Los clientes de caché distribuida deben permitir el tráfico entrante y saliente de MS-PCCRD, que se transporta en el protocolo de detección dinámica de servicios web (WS-Discovery).

La configuración del firewall debe permitir el tráfico de multidifusión además del tráfico entrante y saliente. Puede utilizar la configuración siguiente para configurar las excepciones de firewall para el modo Caché distribuida.

Multidifusión IPv4:239.255.255.250

Multidifusión IPv6: FF02:: C

Tráfico entrante: puerto local: 3702, Puerto remoto: efímero

Tráfico saliente: puerto local: efímero, Puerto remoto: 3702

Programa:% SystemRoot% \system32\svchost.exe (servicio BranchCache [PeerDistSvc])

## <a name="ms-pccrr-peer-content-caching-and-retrieval-retrieval-protocol"></a>[MS-PCCRR]: Recuperación y almacenamiento en caché de contenido del mismo nivel: protocolo de recuperación
Los clientes de caché distribuida deben permitir el tráfico entrante y saliente de MS-PCCRR, que se transporta en el protocolo HTTP 1.1 que se documenta en la solicitud de comentarios (RFC) 2616.

La configuración del firewall debe permitir el tráfico entrante y saliente. Puede utilizar la configuración siguiente para configurar las excepciones de firewall para el modo Caché distribuida.

Tráfico entrante: puerto local: 80, Puerto remoto: efímero

Tráfico saliente: puerto local: efímero, Puerto remoto: 80



