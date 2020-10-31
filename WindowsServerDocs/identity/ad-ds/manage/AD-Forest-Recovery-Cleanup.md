---
title: 'Recuperación de bosque de AD: limpieza'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: 6adb7a79ff05ca803f0d472e6d94f6d3d2d9a80d
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067927"
---
# <a name="ad-forest-recovery---cleanup"></a>Recuperación de bosque de AD: limpieza

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Realice los siguientes pasos posteriores a la recuperación según sea necesario:

- Una vez recuperado todo el bosque, puede revertir a la configuración de DNS original, incluida la configuración de los servidores DNS preferidos y alternativos en cada uno de los controladores de sesión. Una vez que los servidores DNS estén configurados como estaban antes del funcionamiento incorrecto, se restaurarán las capacidades de resolución de nombres anteriores. Elimine los registros DNS de los controladores de archivos que no se hayan recuperado.
- Elimine los registros del servicio de nombres Internet de Windows (WINS) de todos los controladores de red que no se hayan recuperado.
- Puede transferir los roles de maestro de operaciones a otros controladores de dominio del dominio o bosque y agregar más servidores de catálogo global en función de la configuración antes del error.
- Dado que todo el bosque se restaura a un estado anterior, se pierden todos los objetos (como usuarios y equipos) que se agregaron y todas las actualizaciones (por ejemplo, cambios de contraseña) que se realizaron en los objetos existentes a partir de este punto. Por lo tanto, debe volver a crear estos objetos que faltan y volver a aplicar las actualizaciones que faltan según corresponda.
- También es posible que necesite restaurar confianzas de salida con bosques y dominios externos, ya que estas relaciones de confianza externas no se restauran automáticamente a partir de las copias de seguridad.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
