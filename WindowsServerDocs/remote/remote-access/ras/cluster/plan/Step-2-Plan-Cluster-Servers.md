---
title: Paso 2 planear los servidores de clúster
description: Este tema forma parte de la guía deploy Remote Access in a Cluster in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 608bc4b2805639e2638ac12f74b712c812ce165f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308241"
---
# <a name="step-2-plan-cluster-servers"></a>Paso 2 planear los servidores de clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de implementar un solo servidor de acceso remoto, tiene previsto agregar más servidores al clúster.  
  
|Tarea|Descripción|  
|----|--------|  
|[2,1 instalar roles y características](#BKMK_Install).|Para cada servidor que se agregará al clúster, planee la instalación del rol de acceso remoto y la característica NLB de Windows (si es necesario), planee la topología, el direccionamiento IP, el enrutamiento y el reenvío.|  
|[2,2 configuración del servidor](#BKMK_Config)|Configure los valores de cada servidor que se agregará al clúster. Tenga en cuenta que puede configurar un clúster de servidores con equilibrio de carga mediante máquinas virtuales. Para que el enrutamiento y la conectividad funcionen correctamente, debe configurar las máquinas virtuales para que usen la suplantación de direcciones MAC.|  
  
## <a name="21-installing-roles-and-features"></a><a name="BKMK_Install"></a>2,1 instalar roles y características  
Para cada servidor que desee unir al clúster, Planee instalar el rol de acceso remoto. Además, tiene previsto instalar la característica de equilibrio de carga de red (NLB) Si desea equilibrar la carga del tráfico en el clúster mediante Windows NLB. Para obtener más información, consulte [equilibrio de carga de red](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="22-configure-server-settings"></a><a name="BKMK_Config"></a>2,2 configuración del servidor  
Para cada servidor que se agregará al clúster, planee la dirección IP y la configuración del dominio. Observe lo siguiente:  
  
1.  Todos los servidores del clúster deben pertenecer al mismo dominio.  
  
2.  Los servidores del clúster deben encontrarse en la misma subred.  
  
3.  Cada servidor del clúster debe tener el mismo número de adaptadores de red en uso para la implementación de DirectAccess.  
  
Al equilibrar la carga del clúster mediante Windows NLB se aplica la siguiente configuración de NLB de Windows:  
  
1.  Modo de operación: unidifusión. Esto se puede cambiar a multidifusión mediante el administrador de NLB. Esta configuración no se puede modificar en la consola de administración de acceso remoto.  
  
2.  Factor de peso de carga definido como igual, donde todos los servidores de clúster tienen la misma carga.  
  
3.  Modo de filtrado: se equilibrará la carga del tráfico entre varios hosts.  
  
4.  Affinity: se define la afinidad única.  
  
5.  Protocolos: ambos  

