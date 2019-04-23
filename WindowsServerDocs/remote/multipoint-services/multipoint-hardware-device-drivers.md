---
title: Recopilar controladores de hardware y de dispositivo necesarios para la instalación
description: Información acerca de los controladores que necesita para instalar para MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: a9d902e2599cdcd69e156d1fabec87a067b1d8ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833426"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Recopilar controladores de hardware y de dispositivo necesarios para la instalación
Para empezar a implementar el sistema MultiPoint Services, necesitará:  
  
-   **Componentes de hardware para el servidor** -instalar otros componentes del sistema o las tarjetas de vídeo adicionales en este momento.  
  
-   **Componentes de hardware para las estaciones** : para obtener información acerca de las estaciones de planeación para su entorno, consulte [seleccionar Hardware para su sistema de MultiPoint Services](Selecting-Hardware-for-Your-MultiPoint-services-System.md).
-   **Los controladores más recientes para las tarjetas de vídeo** -si el fabricante OEM o el dispositivo no proporcionó estos, deberá descargarlos desde el sitio Web del fabricante del dispositivo.  
  
-   **Los controladores de cliente más recientes de USB cero** -si usa las estaciones de cliente de cero USB, debe instalar los controladores más recientes de cliente USB cero.  
  
    > [!IMPORTANT]  
    > Para una instalación de MultiPoint Services, debe instalar la versión de 64 bits de todos los controladores.  
  
> [!TIP]  
> Si va a instalar MultiPoint Services en un equipo con una versión diferente de Windows ya instalada, debe averiguar la marca de la tarjeta de vídeo y modelo en el Administrador de dispositivos antes de iniciar la instalación de Windows Server y asegúrese de que puede obtener los controladores que son está disponible para Windows Server 2016. Abra el Administrador de dispositivos, abrir **administración de equipos** desde el **iniciar** pantalla. A continuación, en el árbol de consola, haga clic en **Device Manager**.