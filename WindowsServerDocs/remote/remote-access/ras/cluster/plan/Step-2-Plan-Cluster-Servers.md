---
title: 'Paso 2: Plan de clúster servidores'
description: En este tema forma parte de la Guía de implementación de acceso remoto en un clúster en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0dcb14a03f02f931d59743f1b1b8b24b84ba8351
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282866"
---
# <a name="step-2-plan-cluster-servers"></a>Paso 2: Plan de clúster servidores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Después de implementar un único servidor de acceso remoto, planee agregar servidores adicionales al clúster.  
  
|Tarea|Descripción|  
|----|--------|  
|[2.1 instalando roles y características](#BKMK_Install).|Para cada servidor que se agregará al clúster, planear la instalación del rol de acceso remoto y la característica NLB de Windows (si es necesario), planee la topología, el direccionamiento IP, enrutamiento y reenvío.|  
|[2.2 configurar el servidor](#BKMK_Config)|Configurar opciones para cada servidor que se agregará al clúster. Tenga en cuenta que puede configurar un clúster con equilibrio de carga de servidores con las máquinas virtuales. En el orden de enrutamiento y conectividad funcione correctamente, debe configurar las máquinas virtuales para usar la suplantación de direcciones MAC.|  
  
## <a name="BKMK_Install"></a>2.1 instalando roles y características  
Para cada servidor que desea unirse al clúster, debe instalar el rol de acceso remoto. Asimismo, planee instalar la característica de equilibrio de carga de red (NLB) si desea que la carga del tráfico de equilibrio en el clúster con NLB de Windows. Para obtener más información, consulte [Network Load Balancing](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="BKMK_Config"></a>2.2 configurar el servidor  
Para cada servidor que se agregará al clúster, planee la configuración de dominio y dirección IP. Tenga en cuenta lo siguiente:  
  
1.  Todos los servidores del clúster deben pertenecer al mismo dominio.  
  
2.  Servidores del clúster deben encontrarse en la misma subred.  
  
3.  Cada servidor del clúster debe tener el mismo número de adaptadores de red en uso para la implementación de DirectAccess.  
  
Al cargar equilibrar el clúster con NLB de Windows se aplican las siguientes opciones de NLB de Windows:  
  
1.  Modo de operación de unidifusión. Esto se puede cambiar para multidifusión mediante el Administrador de NLB. No se puede modificar esta configuración en la consola de administración de acceso remoto.  
  
2.  Peso definido por el factor como iguales, donde todos los servidores del clúster tienen igual de la carga de la carga.  
  
3.  Filtrado de tráfico de modo se carga equilibrará en varios hosts.  
  
4.  Se define la afinidad de la afinidad única.  
  
5.  Los protocolos  

