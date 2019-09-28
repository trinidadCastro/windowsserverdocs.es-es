---
title: Trabajar con dispositivos de vídeo
description: Obtenga información sobre cómo funcionan los monitores de vídeo y los proyectores con las estaciones en Multipoint Services
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: b7019000c99295204f196ee918129cded02e084f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389247"
---
# <a name="work-with-video-devices"></a>Trabajar con dispositivos de vídeo
En este tema se describe cómo funcionan los dispositivos de vídeo, como un monitor o un proyector, cuando están conectados a un equipo en el sistema MultiPoint Services o en una *estación* de MultiPoint Services.  
  
## <a name="working-with-video-monitors"></a>Trabajar con monitores de vídeo  
En función del hardware del sistema MultiPoint Services, el monitor de vídeo se puede conectar de dos maneras:  
  
-   En el caso de *los sistemas basados en concentrador USB*, conecte el cable del monitor de vídeo a un puerto de vídeo abierto en el equipo, como se muestra en la siguiente ilustración:  
  
    ![Image of Video connection to USB hub-based system](./media/WMSVideoConnection.gif)  
  
-   En el caso de *los sistemas basados en concentradores multifunción* con soporte de vídeo integrado, conecte el cable del monitor de vídeo al puerto de vídeo en el concentrador multifunción:  
  
    ![Imagen de conexión de vídeo y concentrador multifunción](./media/WMSMultifunctionHubVideoConnection.gif)  
  
Para más información, vea el tema [Configurar una estación](Set-Up-a-Station.md).  
  
## <a name="working-with-video-projectors"></a>Trabajar con proyectores de vídeo  
Puede conectar un proyector de vídeo al sistema MultiPoint Services si quiere proyectar una imagen grande para que otros la vean (por ejemplo, en un entorno de laboratorio). En el caso de las estaciones basadas en concentrador USB y multifunción, tiene dos opciones para conectar un proyector a una estación:  
  
-   Reemplace el monitor por un proyector y úselo como pantalla para esa estación, tal como se muestra en la siguiente ilustración:  
  
    ![Image of a projector connected to computer](./media/WMSVideoProjectorConnection.gif)  
  
-   Obtenga un dispositivo divisor de vídeo para conectar un proyector y el monitor al puerto de vídeo de la estación.  
  
    MultiPoint Services mostrará la misma imagen en ambas pantallas. Cuando no esté proyectando, puede apagar el proyector y usar únicamente el monitor de vídeo.  
  
Al usar cualquiera de las opciones, tenga en cuenta lo siguiente:  
  
-   Al conectar una pantalla de vídeo, puede ser necesario *asociar la estación* de nuevo para que MultiPoint Services reconozca correctamente la nueva pantalla. Siga las instrucciones que aparecen en el dispositivo de pantalla de vídeo de la estación.  
  
-   Es posible que tenga que conseguir dispositivos adaptadores o convertidores para convertir conectores DVI y VGA.  
  
-   El uso de un cable separador en “Y” puede disminuir la calidad de vídeo en ambos dispositivos de vídeo.  
  
-   Cuando se usa un proyector y un monitor mediante un cable separador en “Y”, MultiPoint Services ajusta la resolución de la pantalla de ambos dispositivos a la resolución mínima de uno de los dispositivos, habitualmente el proyector.  
  
-   Multipoint Services no admite la ampliación de la pantalla de una sola estación en varios monitores.  
  
## <a name="see-also"></a>Vea también  
[Administrar hardware de la estación](Manage-Station-Hardware.md)  
[Configurar una estación](Set-Up-a-Station.md) 