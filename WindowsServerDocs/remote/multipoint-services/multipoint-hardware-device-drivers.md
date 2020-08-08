---
title: Recopilar controladores de hardware y de dispositivo necesarios para la instalación
description: Información sobre los controladores que debe instalar para Multipoint Services
ms.topic: article
ms.assetid: 4cf5fdbe-b871-4360-b003-d65ac43b491e
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: d66c7fe216d5c2503f536e387b4e013e74092745
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87992777"
---
# <a name="collect-hardware-and-device-drivers-needed-for-the-installation"></a>Recopilar controladores de hardware y de dispositivo necesarios para la instalación
Antes de empezar a implementar el sistema Multipoint Services, necesitará lo siguiente:

-   **Componentes de hardware para el servidor** : Instale las tarjetas de vídeo adicionales u otros componentes del sistema en este momento.

-   **Componentes de hardware para las estaciones** : para obtener información sobre las estaciones de planeación de su entorno, consulte [selección de hardware para el sistema Multipoint Services](./select-hardware-mps.md).
-   **Los controladores más recientes para las tarjetas de vídeo** : Si el OEM o el fabricante del dispositivo no lo han proporcionado, deberá descargarlos desde el sitio web del fabricante del dispositivo.

-   **Los controladores de cliente cero USB más recientes** : Si usa estaciones de cliente USB sin, debe instalar los controladores de cliente USB más recientes.

    > [!IMPORTANT]
    > En el caso de una instalación de Multipoint Services, debe instalar la versión de 64 bits de los controladores.

> [!TIP]
> Si va a instalar Multipoint Services en un equipo con una versión diferente de Windows ya instalada, debe averiguar la marca y el modelo de la tarjeta de vídeo en Device Manager antes de iniciar la instalación de Windows Server y asegurarse de que puede obtener los controladores que están disponibles para Windows Server 2016. Abra Device Manager, Abra **Administración de equipos** en la pantalla **Inicio** . A continuación, en el árbol de consola, haga clic en **Device Manager**.