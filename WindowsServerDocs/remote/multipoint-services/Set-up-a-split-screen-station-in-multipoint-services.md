---
title: Configurar una estación de pantalla dividida
description: Describe cómo configurar MultiPoint Services para que dos usuarios puedan compartir un único sistema
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b2d01df6175eee99fd1374cd5af270cbcc764ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849946"
---
# <a name="set-up-a-split-screen-station"></a>Configurar una estación de pantalla dividida
Puede configurar una estación de pantalla dividida por lo que dos usuarios pueden usar simultáneamente el sistema.

Cualquier monitor que tenga una resolución de 1200 mínima x 720, cuando se conecta a una estación que admite la característica de pantalla dividida, se puede dividir en dos estaciones. Una vez haya dividido la estación, el escritorio que tenía muestra el monitor se mueve a la mitad izquierda de la pantalla y se muestra una nueva estación en la mitad derecha de la pantalla. Para terminar de crear la nueva estación, deberá asignar un teclado, mouse y concentrador USB a la estación. Una vez que se haya dividido la estación, un usuario puede iniciar sesión en la estación izquierda mientras que otro usuario lo hace en la estación derecha.  
  
Estaciones de pantalla dividida tienen varias ventajas:  
  
-   Puede reducir el costo y el espacio al albergar a más usuarios en un sistema MultiPoint Services.  
  
-   Dos usuarios pueden colaborar juntos, en paralelo en un proyecto.  
  
-   Un usuario de MultiPoint Dashboard puede demostrar un procedimiento en una estación mientras un estudiante sigue la explicación en otra estación.  
  
La siguiente ilustración muestra un sistema MultiPoint Services con una estación de pantalla de división (a la derecha).  
  
![Estaciones con pantalla dividida](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Requisitos para una estación de pantalla dividida  
Para crear una estación de pantalla dividida, la estación y del monitor deben cumplir estos requisitos:  
  
-   El monitor debe tener una resolución de 1200 x720 o superior.  
  
-   Si usa a un cliente de USB a través de Ethernet cero, póngase en contacto con su proveedor de hardware para averiguar si se admiten las estaciones de pantalla dividida. Muchos de los dispositivos de cliente de USB a través de Ethernet cero tienen limitaciones que impiden su configuración como estaciones de pantalla dividida.  
  
## <a name="setting-up-a-split-screen-station"></a>Configurar una estación de pantalla dividida  
Use los procedimientos siguientes para agregar un segundo centro de una estación de pantalla dividida y, a continuación, dividir la estación de MultiPoint Services. El procedimiento final explica cómo devolver una estación de pantalla dividida a una única estación.  
  
> [!NOTE]  
> Cuando divide una estación, la sesión activa de la estación se suspende. El usuario debe iniciar sesión en la estación de nuevo para reanudar el trabajo después de que se produce la división.  
  
**Para agregar un segundo centro con teclado y mouse:**  
  
1.  Conectar un concentrador USB a un puerto USB abierto en el equipo, como se muestra en la siguiente ilustración.  
  
    ![Imagen de conexión de concentrador USB de MultiPoint server](./media/WMSUSBHubConnection.gif)  
  
2.  Conecte un teclado y mouse al concentrador USB.  
  
    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Conecte los periféricos adicionales, como los auriculares al concentrador USB.  
  
4.  Si usas un concentrador alimentado de forma externa, conecte el cable de alimentación del concentrador a una toma de corriente.  
  
**Para dividir una estación:**  
  
1.  En el Administrador de MultiPoint, haga clic en el **estaciones** ficha.  
  
2.  En **estación**, haga clic en el nombre de la estación que quiera dividir.  
  
3.  En **tareas del elemento seleccionado**, haga clic en **dividir estación**.  
  
    La pantalla original se mueve a la mitad izquierda del monitor y se crea la pantalla de una nueva estación en la mitad derecha del mismo monitor.  
  
4.  Crear la nueva estación presionando la carta especificados en el teclado recién agregado como se indica cuándo el **crear una estación de MultiPoint Server** pantalla aparece en la mitad derecha del monitor.  
  
Una vez haya dividido la estación, un usuario puede iniciar sesión en la estación izquierda mientras otro usuario inicia sesión en la estación derecha.  
  
**En una estación dividida vuelva a una única estación:**  
  
1.  En el Administrador de MultiPoint, haga clic en el **estaciones** ficha.  
  
2.  En **estación**, haga clic en el nombre de la estación que quiera no dividido.  
  
3.  En **tareas del elemento seleccionado**, haga clic en **unir estación**.