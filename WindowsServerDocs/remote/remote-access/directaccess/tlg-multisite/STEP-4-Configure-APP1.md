---
title: PASO 4 configurar APP1
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
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ccfac583f64d40012881a2d3f6a0897beb16c02a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867226"
---
# <a name="step-4-configure-app1"></a>PASO 4 configurar APP1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Configurar una dirección IPv6 estática y tener acceso la configuración de puerta de enlace para habilitar APP1 a la subred Corpnet de 2.  
  
- Para configurar la puerta de enlace predeterminada y el servidor DNS. La configuración multisitio, utiliza el equipo ENRUTADOR1 como puerta de enlace predeterminada. Configurar la puerta de enlace predeterminado en APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Para configurar la puerta de enlace predeterminada y el servidor DNS  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En el **las conexiones de red** ventana, haga clic en **conexión cableada Ethernet**y, a continuación, haga clic en **propiedades**.  
  
3.  En el **propiedades de conexión cableada Ethernet** cuadro de diálogo, haga clic en **protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, haga clic en **propiedades**.  
  
4.  En **puerta de enlace predeterminada**, tipo **10.0.0.254**y en **servidor DNS alternativo**, tipo **10.2.0.1**y, a continuación, haga clic en **Aceptar** .  
  
5.  En el **propiedades de conexión cableada Ethernet** cuadro de diálogo, haga clic en **protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **propiedades**.  
  
6.  En **puerta de enlace predeterminada**, tipo **2001:db8:1::fe**. En **servidor DNS alternativo**, tipo **2001:db8:2::1**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el **propiedades de conexión cableada Ethernet** cuadro de diálogo, haga clic en **cerrar**y, a continuación, cierre el **las conexiones de red** ventana.  
  


