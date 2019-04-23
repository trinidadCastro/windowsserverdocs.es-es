---
title: Paso 2 preparar servidores del clúster
description: En este tema forma parte de la Guía de implementación de acceso remoto en un clúster en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab08c4ced9431fe17a4876f6df471624f22a6c44
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836596"
---
# <a name="step-2-prepare-cluster-servers"></a>Paso 2 preparar servidores del clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de configurar una implementación de clúster, preparar servidores adicionales que desea agregar al clúster.  
  
|Tarea|Descripción|  
|----|--------|  
|[2.1 configurar la infraestructura de acceso remoto](#BKMK_config)|En cada servidor que desea agregar al clúster, configure la topología de servidor, una dirección IP, enrutamiento y reenvío. Si configura un clúster con equilibrio de carga de máquinas virtuales, debe configurar las máquinas virtuales para usar la suplantación de direcciones MAC.<br /><br />Además, únase a cada servidor al mismo dominio y conectar todos los servidores a la misma subred.|  
|[2.2 instalar el rol de acceso remoto](#BKMK_Install)|En cada servidor adicional que desee agregar al clúster, instale el rol de acceso remoto|  
|[2.3 instalar NLB](#BKMK_NLB)|En el servidor de acceso remoto implementado y en cada servidor adicional que desee agregar al clúster, instale la característica NLB. Tenga en cuenta que este paso no es necesario cuando se usa un equilibrador de carga externo.|  
  
## <a name="BKMK_config"></a>2.1 configurar la infraestructura de acceso remoto  
Para configurar un clúster de acceso remoto, debe configurar la topología de servidor, una dirección IP, enrutamiento y reenvío en cada servidor que formará parte del clúster.  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>Para configurar la infraestructura de acceso remoto  
  
1.  Configure cada uno de los servidores que se incluirán en el clúster con la misma topología como el primer servidor de acceso remoto.  
  
2.  Configure cada uno de los servidores que se incluirán en el clúster con adecuado IP direccionamiento, enrutamiento y reenvío según la configuración del primer servidor de acceso remoto. Tenga en cuenta que todos los servidores del clúster deben estar conectados a la misma subred.  
  
3.  Únase a cada uno de los servidores que se incluirán en el clúster para el mismo dominio que el primer servidor de acceso remoto.  
  
## <a name="BKMK_Install"></a>2.2 instalar el rol de acceso remoto  
Para configurar un clúster de acceso remoto, debe instalar el rol de acceso remoto en cada servidor que formará parte del clúster.  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Para instalar el rol de acceso remoto en servidores VPN de Always On  
  
1.  En el servidor de DirectAccess, en la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.  
  
3.  En el **seleccionar Roles de servidor** cuadro de diálogo, seleccione **acceso remoto**y, a continuación, haga clic en **siguiente**.  
  
4.  Haga clic en **siguiente** tres veces.  
  
5.  En el **seleccionar servicios de rol** cuadro de diálogo, seleccione **DirectAccess y VPN (RAS)** y, a continuación, haga clic en **agregar características**.  
  
6.  Seleccione **enrutamiento**, seleccione **Proxy de aplicación Web**, haga clic en **agregar características**y, a continuación, haga clic en **siguiente**.  
  
7. Haga clic en **Siguiente**y después en **Instalar**.  
  
8.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
9.  Repita este procedimiento en todos los servidores que desea que los miembros del clúster.  
  
## <a name="BKMK_NLB"></a>2.3 instalar NLB  
Para configurar un clúster de acceso remoto, debe instalar la característica de equilibrio de carga de red en cada servidor que formará parte del clúster.  
  
> [!NOTE]  
> Este paso no es necesario si se usa un equilibrador de carga externo.  
  
#### <a name="to-install-the-nlb-role"></a>Para instalar el rol NLB  
  
1.  En el servidor de DirectAccess, en la consola de administrador del servidor, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **siguiente** cuatro veces para ir a la pantalla de selección de características de servidor.  
  
3.  En el **seleccionar características** cuadro de diálogo, seleccione **Network Load Balancing**, haga clic en **agregar características**, haga clic en **siguiente**y, a continuación, haga clic en **Instalar**.  
  
4.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.  
  
5.  Repita este procedimiento en todos los servidores que desea que los miembros del clúster.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 3: Configurar un clúster con equilibrio de carga](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


