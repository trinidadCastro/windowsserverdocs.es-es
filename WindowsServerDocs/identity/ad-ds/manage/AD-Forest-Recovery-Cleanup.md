---
title: "Recuperación de bosque de AD - Liberador de espacio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 2f652d08304a17ecbfde51bbb6f35e4666cd9eca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---cleanup"></a>Recuperación de bosque de AD - Liberador de espacio 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Realiza los siguientes pasos de recuperación de post según sea necesario:  
  
-   Una vez recuperado todo el bosque, puede volver a la configuración original de DNS, incluida la configuración de los servidores DNS preferidos y alternativos en cada uno de los controladores de dominio. Después de configuran los servidores DNS como estaban antes del error de funcionamiento, se restaurará sus funcionalidades de resolución de nombre anterior. Eliminar los registros DNS para controladores de dominio que no se han recuperado.  
  
-   Eliminar registros de servicio de nombres de Windows Internet (WINS) para todos los controladores de dominio que no se han recuperado.  
  
-   Puedes transferir los roles de maestro de operaciones a otros controladores de dominio en el dominio o bosque y agregar los servidores de catálogo global según la configuración antes del error.  
  
-   Como a un estado anterior, se restaura todo el bosque, todos los objetos (como los usuarios y equipos) que se agregaron y todas las actualizaciones (como cambios de contraseña) que se realizaron a objetos existentes después de este punto se pierden. Por lo tanto, debes volver a crear estos objetos que faltan y volver a aplicar las actualizaciones que faltan según corresponda.  
  
-   Es posible que también debas restaurar confianzas de salida con externos dominios y bosques, porque estas relaciones de confianza externa no se restauran automáticamente de las copias de seguridad.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)  



