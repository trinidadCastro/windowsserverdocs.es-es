---
title: Paso 2 preparación de los servidores de clúster
description: Obtenga información sobre cómo preparar servidores adicionales para agregarlos al clúster.
manager: brianlic
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 09c2a1a05d6800a3497bc6b8b15536eb73466770
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947391"
---
# <a name="step-2-prepare-cluster-servers"></a>Paso 2 preparación de los servidores de clúster

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Antes de poder configurar una implementación de clúster, debe preparar servidores adicionales para agregarlos al clúster.

|Tarea|Descripción|
|----|--------|
|[2,1 configuración de la infraestructura de acceso remoto](#BKMK_config)|En cada servidor que desee agregar al clúster, configure la topología del servidor, el direccionamiento IP, el enrutamiento y el reenvío. Si configura un clúster con equilibrio de carga de máquinas virtuales, debe configurar las máquinas virtuales para que usen la suplantación de direcciones MAC.<p>Además, combine cada servidor con el mismo dominio y conecte todos los servidores a la misma subred.|
|[2,2 instalar el rol de acceso remoto](#BKMK_Install)|En cada servidor adicional que desee agregar al clúster, instale el rol de acceso remoto.|
|[2,3 instalación de NLB](#BKMK_NLB)|En el servidor de acceso remoto implementado y en cada servidor adicional que desee agregar al clúster, instale la característica NLB. Tenga en cuenta que este paso no es necesario cuando se usa un Load Balancer externo.|

## <a name="21-configure-the-remote-access-infrastructure"></a><a name="BKMK_config"></a>2,1 configuración de la infraestructura de acceso remoto
Para configurar un clúster de acceso remoto, debe configurar la topología del servidor, el direccionamiento IP, el enrutamiento y el reenvío en cada servidor que formará parte del clúster.

### <a name="to-configure-the-remote-access-infrastructure"></a>Para configurar la infraestructura de acceso remoto

1.  Configure cada uno de los servidores que estarán en el clúster con la misma topología que el primer servidor de acceso remoto.

2.  Configure cada uno de los servidores que se incluirán en el clúster con la dirección IP, el enrutamiento y el reenvío adecuados en función de la configuración del primer servidor de acceso remoto. Tenga en cuenta que todos los servidores del clúster deben estar conectados a la misma subred.

3.  Unir cada uno de los servidores que estarán en el clúster al mismo dominio que el primer servidor de acceso remoto.

## <a name="22-install-the-remote-access-role"></a><a name="BKMK_Install"></a>2,2 instalar el rol de acceso remoto
Para configurar un clúster de acceso remoto, debe instalar el rol de acceso remoto en cada servidor que formará parte del clúster.

### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Para instalar el rol de acceso remoto en Always On servidores VPN

1.  En el servidor de DirectAccess, en la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.

2.  Haga clic en **Siguiente** tres veces para ir a la pantalla de selección de roles del servidor.

3.  En el cuadro de diálogo **Seleccionar roles de servidor** , seleccione **acceso remoto** y, a continuación, haga clic en **siguiente**.

4.  Haga clic en **siguiente** tres veces.

5.  En el cuadro de diálogo **seleccionar servicios de rol** , seleccione **DirectAccess y VPN (RAS)** y, a continuación, haga clic en **Agregar características**.

6.  Seleccione **enrutamiento**, **proxy de aplicación web**, haga clic en **Agregar características** y, a continuación, haga clic en **siguiente**.

7. Haga clic en **Siguiente** y, después, en **Instalar**.

8.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.

9.  Repita este procedimiento en todos los servidores que desee que sean miembros del clúster.

## <a name="23-install-nlb"></a><a name="BKMK_NLB"></a>2,3 instalación de NLB
Para configurar un clúster de acceso remoto, debe instalar la característica de equilibrio de carga de red en cada servidor que formará parte del clúster.

> [!NOTE]
> Este paso no es necesario si se usa un equilibrador de carga externo.

#### <a name="to-install-the-nlb-role"></a>Para instalar el rol NLB

1.  En el servidor de DirectAccess, en la consola de Administrador del servidor, en el **Panel**, haga clic en **Agregar roles y características**.

2.  Haga clic en **siguiente** cuatro veces para ir a la pantalla de selección de características del servidor.

3.  En el cuadro de diálogo **seleccionar características** , seleccione **equilibrio de carga de red**, haga clic en **Agregar características**, haga clic en **siguiente** y, a continuación, haga clic en **instalar**.

4.  En el cuadro de diálogo **Progreso de la instalación**, compruebe que la instalación se ha realizado correctamente y, a continuación, haga clic en **Cerrar**.

5.  Repita este procedimiento en todos los servidores que desee que sean miembros del clúster.

## <a name="see-also"></a><a name="BKMK_Links"></a>Otras referencias

-   [Paso 3: configurar un clúster con equilibrio de carga](Step-3-Configure-a-Load-Balanced-Cluster.md)



