---
title: Consideraciones de la aplicación
description: Información de compatibilidad para las aplicaciones en MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 445e6184-4e1e-4f10-ad3c-042f2a6c2f5f
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: 400f87c09f1b2e897d67f94e9b7ac12ae0a1e799
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839836"
---
# <a name="application-considerations"></a>Consideraciones de la aplicación
  
## <a name="application-compatibility"></a>Compatibilidad de aplicaciones

Cualquier aplicación que se va a ejecutar en un sistema MultiPoint Services debe cumplir los siguientes requisitos:
  
- Debe instalar y ejecutar en Windows Server 2016 
- Debe ser consciente de sesión para que cada usuario pueda ejecutar una instancia de la aplicación en un sistema MultiPoint.
  
Si la aplicación especificar este requisito, se recomienda que intente instalar la aplicación y su uso en una sesión de escritorio remoto. 

## <a name="addressing-application-compatibility-problems"></a>Abordar problemas de compatibilidad de aplicaciones  
MultiPoint Services ofrece la opción para asociar las estaciones con instancias completas de las ediciones de Windows 10 Enterprise que se ejecutan prácticamente en el mismo equipo host. Para obtener información sobre las aplicaciones críticas que no se ejecutarán varias instancias para varios usuarios, o no se instalará en un sistema operativo de 64 bits, esto puede ser una solución. Para implementar escritorios de este modo requiere el uso de la ficha de escritorios virtuales en MultiPoint Manager para:  
  
-   Habilitar escritorios virtuales  
-   Crear una plantilla de escritorio  
-   Personalizar la plantilla con la aplicación de problema  
-   Asociar estaciones con la plantilla personalizada  

Cada estación se inicia desde la misma plantilla, por lo que los cambios se borran cada vez que se inicia el equipo.  
  
>[!NOTE] 
>Es importante comprobar los requisitos de licencia para las aplicaciones que desea ejecutar en un MultiPoint. Aunque se va a instalar aplicaciones de una copia podrían requerir las licencias por usuario.  
  
