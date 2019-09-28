---
title: Consideraciones de la aplicación
description: Información de compatibilidad para aplicaciones en Multipoint Services
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 21531273b1dd6d643df3f816a880a0efb3117c70
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405131"
---
# <a name="application-considerations"></a>Consideraciones de la aplicación
  
## <a name="application-compatibility"></a>Compatibilidad de aplicaciones

Cualquier aplicación que desee ejecutar en un sistema Multipoint Services debe cumplir los siguientes requisitos:
  
- Debe instalarse y ejecutarse en Windows Server 2016 
- Debe tener en cuenta la sesión, por lo que cada usuario puede ejecutar una instancia de la aplicación en un sistema multipoint.
  
Si la aplicación especifica este requisito, se recomienda intentar instalar la aplicación y usarla en una sesión de escritorio remoto. 

## <a name="addressing-application-compatibility-problems"></a>Solucionar problemas de compatibilidad de aplicaciones  
Multipoint Services ofrece la opción de asociar estaciones con instancias completas de las ediciones de Windows 10 Enterprise que se ejecutan prácticamente en el mismo equipo host. En el caso de las aplicaciones críticas que no van a ejecutar varias instancias para varios usuarios o que no se instalarán en un sistema operativo de 64 bits, puede tratarse de una solución. Para implementar escritorios de esta manera, es necesario usar la pestaña escritorios virtuales de Multipoint Manager para:  
  
-   Habilitar escritorios virtuales  
-   Crear una plantilla de escritorio  
-   Personalización de la plantilla con la aplicación problemática  
-   Asociar estaciones con la plantilla personalizada  

Cada estación se inicia a partir de la misma plantilla, por lo que los cambios se borran cada vez que se inicia el equipo.  
  
>[!NOTE] 
>Es importante comprobar los requisitos de licencia para las aplicaciones que desea ejecutar en un multipoint. Aunque está instalando una copia de una aplicación, es posible que necesite licencias por usuario.  
  
