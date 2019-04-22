---
title: Almacenamiento de archivos con MultiPoint Services
description: Obtenga información sobre el almacenamiento de archivos en MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b432ca793b156997761f9fadab7340c394e3b553
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817346"
---
# <a name="storing-files-with-multipoint-services"></a>Almacenamiento de archivos con MultiPoint Services
MultiPoint Services admite el almacenamiento de archivos de usuario de las maneras siguientes:  
  
-   **En la partición de sistema operativo de la unidad de disco duro.** De forma predeterminada, MultiPoint Services almacena los archivos de usuario en la unidad de disco duro con el sistema operativo.  
  
-   **En una partición independiente de la unidad de disco duro.** Cuando el sistema MultiPoint Services se configura por primera vez, puede *partición* la unidad de disco duro. Es decir, puede configurar una sección de la unidad para que funcione como si fuera una unidad independiente. Esto facilita la restauración o actualizar el sistema operativo sin que afecte a los archivos de usuario. Para obtener más información, consulte [crear una partición o una unidad lógica](https://go.microsoft.com/fwlink/?LinkId=182618) en la biblioteca técnica de Windows Server.  
  
-   **En una adicional interna o externa de unidad de disco duro.** Puede adjuntar más unidades de disco duro internas o externas a MultiPoint Services para guardar y copia de datos.  
  
-   **En una carpeta compartida de red.** Para que los archivos de usuario esté disponible desde cualquier estación, puede crear una carpeta compartida en la red. Esto requiere otro equipo o servidor además del equipo que ejecuta MultiPoint Services. Este es el método recomendado para almacenar los archivos si hay disponible un servidor de archivos.  
  
    Pequeños sistemas de equipos 2 y 3 ejecutando MultiPoint Services con ningún servidor de archivos disponible, uno de los equipos de MultiPoint Services puede actuar como servidor de archivos para todos los equipos de MultiPoint Services. A continuación, crearía cuentas de usuario para todos los usuarios en los servicios de MultiPoint que actúa como el servidor de archivos.  
  
