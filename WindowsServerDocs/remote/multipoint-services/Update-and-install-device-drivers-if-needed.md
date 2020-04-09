---
title: Actualizar e instalar a controladores de dispositivo si es necesario
description: Obtenga información sobre cómo comprobar y actualizar los controladores de dispositivos en Multipoint Services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 16be3ef9-a05b-4621-a431-5806b567e997
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 6d20aa80edeafa4311262a380cfd7aad65ae0315
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820608"
---
# <a name="update-and-install-device-drivers-if-needed"></a>Actualizar e instalar a controladores de dispositivo si es necesario
Si usa periféricos o clientes USB que requieren controladores, debe instalar los controladores en este momento. También se recomienda comprobar **Device Manager** para las alertas de controlador e instalar los controladores para esos dispositivos.  
  
Por lo general, se necesitan los controladores más recientes para los siguientes tipos de dispositivos:  
  
-   Clientes USB cero  
  
-   Clientes de USB a través de Ethernet  
  
-   Controladores de disco  
  
-   Adaptadores de red  
  
-   Controladores de sonido  
  
-   Controladores de host USB

-   Tarjetas gráficas


## <a name="to-check-for-driver-alerts-in-device-manager"></a>Para comprobar las alertas de controlador en Device Manager  
  
1.  Abra la pantalla Inicio.  
  
2.  Escriba **Administración de equipos**y, a continuación, haga clic en **Administración de equipos** en los resultados.  
  
3.  En el árbol de la consola administración de equipos, haga clic en **Device Manager**.  
  
4.  En los dispositivos del sistema de la derecha, compruebe las alertas de controlador que pueden afectar a MultiPoint Server.  
  
## <a name="to-install-device-drivers-in-multipoint-manager"></a>Para instalar controladores de dispositivos en Multipoint Manager  
  
1.  Para abrir Multipoint Manager, busque "Multipoint Manager" y, a continuación, haga clic en **Multipoint Manager** en los resultados.  
  
2.  En Multipoint Manager, haga clic en la pestaña **Inicio** y, a continuación, haga clic en **cambiar al modo de consola**.  
  
3.  Para instalar un controlador de dispositivo, haga doble clic en el archivo del controlador y siga las instrucciones para instalar el controlador.  
  
4.  Repita el paso anterior para instalar todos los controladores necesarios.  
  
    > [!NOTE]  
    > Si una instalación requiere un reinicio del equipo, tendrá que volver al modo de consola antes de instalar el siguiente controlador. MultiPoint Server siempre se inicia en modo de estación. Para cambiar al modo de consola, vaya a la pestaña **Inicio** de Multipoint Manager y haga clic en **cambiar al modo de consola**.