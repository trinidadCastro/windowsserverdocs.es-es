---
title: Actualizar e instalar a controladores de dispositivo si es necesario
description: Obtenga información sobre cómo comprobar y actualizar los controladores de dispositivos en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 66477634e06df217656876b084ae37be8cb311c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829136"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Actualizar e instalar a controladores de dispositivo si es necesario
Si usa clientes USB cero o periféricos que requieren los controladores, debe instalar a los controladores en este momento. Es una buena idea también comprobar **Device Manager** para cualquier controlador alertas y controladores de instalación para esos dispositivos.  
  
Por lo general, los controladores más recientes son necesarios para los siguientes tipos de dispositivos:  
  
-   Clientes USB cero  
  
-   Clientes de USB a través de Ethernet cero  
  
-   Controladores de disco  
  
-   Adaptadores de red  
  
-   Dispositivos de sonido  
  
-   Controladores host USB

-   Tarjetas gráficas


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Para comprobar si hay alertas de controlador en el Administrador de dispositivos  
  
1.  Abra la pantalla Inicio.  
  
2.  Tipo **administración de equipos**y, a continuación, haga clic en **administración de equipos** en los resultados.  
  
3.  En el árbol de consola de administración de equipos, haga clic en **Device Manager**.  
  
4.  En los dispositivos del sistema a la derecha, busque las alertas de controlador que podrían afectar a MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Para instalar a controladores de dispositivo en el Administrador de MultiPoint  
  
1.  Para abrir el Administrador de MultiPoint, busque "MultiPoint Manager" y, a continuación, haga clic en **MultiPoint Manager** en los resultados.  
  
2.  En el Administrador de MultiPoint, haga clic en el **inicio** pestaña y, a continuación, haga clic en **cambiar al modo de consola**.  
  
3.  Para instalar a un controlador de dispositivo, haga doble clic en el archivo del controlador y siga las instrucciones para instalar al controlador.  
  
4.  Repita el paso anterior para instalar todos los controladores necesarios.  
  
    > [!NOTE]  
    > Si una instalación requiere un reinicio del equipo, deberá cambiar al modo de consola antes de instalar al controlador siguiente. MultiPoint server siempre se inicia en modo de estación. Para cambiar al modo de consola, vaya a la **inicio** pestaña en el Administrador de MultiPoint y haga clic en **cambiar al modo de consola**.