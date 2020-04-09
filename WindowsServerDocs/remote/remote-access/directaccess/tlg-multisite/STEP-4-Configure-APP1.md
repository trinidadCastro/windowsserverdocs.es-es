---
title: Paso 4 configurar APP1
description: 'Este tema forma parte de la guía del laboratorio de pruebas: demostración de una implementación multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8011d85f742ebde5fc2013bb483546133e1618ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861588"
---
# <a name="step-4-configure-app1"></a>Paso 4 configurar APP1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Configure las opciones de puerta de enlace y direccionamiento IPv6 estático para permitir el acceso de APP1 a la subred 2-CorpNet.  
  
- Para configurar la puerta de enlace predeterminada y el servidor DNS. La configuración multisitio usa el equipo ENRUTADOR1 como puerta de enlace predeterminada. Configurar la puerta de enlace predeterminada en APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Para configurar la puerta de enlace predeterminada y el servidor DNS  
  
1.  En la consola de Administrador del servidor, haga clic en **servidor local**y, a continuación, en el área **propiedades** , junto a **conexión cableada Ethernet**, haga clic en el vínculo.  
  
2.  En la ventana **conexiones de red** , haga clic con el botón secundario en **conexión cableada Ethernet**y, a continuación, haga clic en **propiedades**.  
  
3.  En el cuadro de diálogo **propiedades de conexión cableada Ethernet** , haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, haga clic en **propiedades**.  
  
4.  En **puerta de enlace predeterminada**, escriba **10.0.0.254**y, en **servidor DNS alternativo**, escriba **10.2.0.1**y, a continuación, haga clic en **Aceptar**.  
  
5.  En el cuadro de diálogo **propiedades de conexión cableada Ethernet** , haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **propiedades**.  
  
6.  En **puerta de enlace predeterminada**, escriba **2001: db8:1:: fe**. En **servidor DNS alternativo**, escriba **2001: db8:2:: 1**y, a continuación, haga clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **propiedades de conexión cableada Ethernet** , haga clic en **cerrar**y, a continuación, cierre la ventana **conexiones de red** .  
  


