---
title: PASO 9 configuración EDGE1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar una implementación de multisitio de DirectAccess para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0a47a436bbd11c795caa8b402054ae0d2c3282f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281373"
---
# <a name="step-9-configure-edge1"></a>PASO 9 configuración EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Los siguientes procedimientos se realizan en el servidor EDGE1:  
  
1. Configurar los servidores DNS en EDGE1. Es necesario configurar el servidor DNS del dominio corp2.corp.contoso.com en EDGE1.  
  
2. Configurar el enrutamiento entre subredes. Configurar el enrutamiento en EDGE1 para habilitar la comunicación entre las subredes de la red corporativa y 2 de la red corporativa.  
  
## <a name="IPv6"></a>Configurar los servidores DNS en EDGE1  
  
1.  En la consola de administrador del servidor, haga clic en **servidor Local**y, a continuación, en el **propiedades** área, junto a **Corpnet**, haga clic en el vínculo.  
  
2.  En la ventana Conexiones de red, haga clic en **Corpnet**y, a continuación, haga clic en **propiedades**.  
  
3.  Haga clic en **Protocolo de Internet versión 4 (TCP/IPv4)** y, a continuación, en **Propiedades**.  
  
4.  En **servidor DNS alternativo**, tipo **10.2.0.1**. A continuación, haga clic en **Aceptar**.  
  
5.  Haga clic en **Protocolo de Internet versión 6 (TCP/IPv6)** y, a continuación, haga clic en **Propiedades**.  
  
6.  En **servidor DNS alternativo**, tipo **2001:db8:2::1** y, a continuación, haga clic en **Aceptar**.  
  
7.  En el **propiedades de la red corporativa** cuadro de diálogo, haga clic en **cerrar**.  
  
8.  Cierre la ventana **Conexiones de red**.  
  
## <a name="ConfigRouting"></a>Configurar el enrutamiento entre subredes  
  
1.  En el **iniciar** , escriba**cmd.exe**, haga clic en **cmd**, haga clic en **avanzadas**y, a continuación, haga clic en **ejecutar como administrador**. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la ventana de símbolo del sistema, escriba los siguientes comandos. Después de escribir cada comando, presione ENTRAR.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Cierre la ventana del símbolo del sistema.  
  


