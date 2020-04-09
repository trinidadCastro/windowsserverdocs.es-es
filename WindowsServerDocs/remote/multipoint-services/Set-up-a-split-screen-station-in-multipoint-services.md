---
title: Configurar una estación de pantalla dividida
description: Describe cómo configurar Multipoint Services para que dos usuarios puedan compartir un solo sistema.
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 35d1d434-79b2-4e0a-835f-d94ed8ee6b21
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 006422614b87cd331dd157218ac910b6f7af1602
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853918"
---
# <a name="set-up-a-split-screen-station"></a>Configurar una estación de pantalla dividida
Puede configurar una estación de pantalla dividida para que dos usuarios puedan usar el sistema simultáneamente.

Cualquier monitor que tenga una resolución de 1200 x720 como mínimo, cuando se conecta a una estación que admite la característica de pantalla dividida, se puede dividir en dos estaciones. Una vez que se divide una estación, el escritorio que el monitor ha mostrado se desplaza a la mitad izquierda de la pantalla y se muestra una nueva estación en la mitad derecha de la pantalla. Para terminar de crear la nueva estación, deberá asignar un teclado, un mouse y un concentrador USB a la estación. Una vez que se haya dividido la estación, un usuario puede iniciar sesión en la estación izquierda mientras que otro usuario lo hace en la estación derecha.  
  
Las estaciones de pantalla dividida tienen varias ventajas:  
  
-   Puede reducir el costo y el espacio al acomodar a más usuarios en un sistema Multipoint Services.  
  
-   Dos usuarios pueden colaborar juntos, en paralelo, en un proyecto.  
  
-   Un usuario de Multipoint Dashboard puede mostrar un procedimiento en una estación mientras un estudiante sigue el resto de la estación.  
  
En la ilustración siguiente se muestra un sistema Multipoint Services con una estación de pantalla dividida (a la derecha).  
  
![Estaciones con pantalla dividida](./media/WMS_diagram3.gif)  
   
## <a name="requirements-for-a-split-screen-station"></a>Requisitos para una estación de pantalla dividida  
Para crear una estación de pantalla dividida, el monitor y la estación deben cumplir estos requisitos:  
  
-   El monitor debe tener una resolución de 1200 x720 o superior.  
  
-   Si usa un cliente de cero USB a través de Ethernet, consulte con su proveedor de hardware para averiguar si se admiten las estaciones de pantalla dividida. Muchos dispositivos cliente de cero USB a través de Ethernet tienen limitaciones que impiden su configuración como estaciones de pantalla dividida.  
  
## <a name="setting-up-a-split-screen-station"></a>Configuración de una estación de pantalla dividida  
Use los procedimientos siguientes para agregar un segundo concentrador para una estación de pantalla dividida y, después, divida la estación en Multipoint Services. El procedimiento final explica cómo devolver una estación de pantalla dividida a una sola estación.  
  
> [!NOTE]  
> Cuando divide una estación, la sesión activa de la estación se suspende. El usuario debe volver a iniciar sesión en la estación para reanudar el trabajo después de que se produzca la división.  
  
**Para agregar un segundo concentrador con el teclado y el mouse:**  
  
1.  Conecte un concentrador USB a un puerto USB abierto en el equipo, como se muestra en la siguiente ilustración.  
  
    ![Imagen de conexión del concentrador USB de MultiPoint Server](./media/WMSUSBHubConnection.gif)  
  
2.  Conecte un teclado y un mouse al concentrador USB.  
  
    ![Imagen de conexiones de dispositivos de entrada de concentrador USB](./media/WMSUSBDeviceConnection.gif)  
  
3.  Conecte los periféricos adicionales, como los auriculares al concentrador USB.  
  
4.  Si usa un concentrador alimentado externamente, conecte el cable de alimentación del centro a una toma de corriente.  
  
**Para dividir una estación:**  
  
1.  En el administrador de Multipoint, haga clic en la pestaña **estaciones** .  
  
2.  En **estación**, haga clic en el nombre de la estación que desee dividir.  
  
3.  En **tareas de elementos seleccionados**, haga clic en **estación de división**.  
  
    La pantalla original se mueve a la mitad izquierda del monitor y se crea una nueva pantalla de la estación en la mitad derecha del mismo monitor.  
  
4.  Cree la nueva estación presionando la letra especificada en el teclado recién agregado, tal como se indica cuando la pantalla **crear una estación de MultiPoint Server** aparece en la mitad derecha del monitor.  
  
Una vez que se divide una estación, un usuario puede iniciar sesión en la estación izquierda mientras otro usuario inicia sesión en la estación adecuada.  
  
**Para devolver una estación dividida a una sola estación:**  
  
1.  En el administrador de Multipoint, haga clic en la pestaña **estaciones** .  
  
2.  En **estación**, haga clic en el nombre de la estación que desea desdividir.  
  
3.  En **tareas de elementos seleccionados**, haga clic en **separar estación**.