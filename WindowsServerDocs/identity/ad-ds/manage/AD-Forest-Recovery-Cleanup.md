---
title: Recuperación de bosques de AD - limpieza
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: fa7193cc800eac0fee6425a66bd5cd82d8c822c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868966"
---
# <a name="ad-forest-recovery---cleanup"></a>Recuperación de bosques de AD - limpieza

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Realice los siguientes pasos de recuperación de post según sea necesario:  
  
- Una vez recuperado todo el bosque, puede revertir a la configuración de DNS original, incluida la configuración de los servidores DNS preferidos y alternativos en cada uno de los controladores de dominio. Después de que los servidores DNS están configurados como eran antes del error de funcionamiento, se restauran sus capacidades de resolución de nombre anterior. Eliminar los registros DNS para los controladores de dominio que no se han recuperado.  
- Eliminar registros del servicio de nombres Internet de Windows (WINS) para todos los controladores de dominio que no se han recuperado.  
- Puede transferir las funciones de maestro de operaciones a otros controladores de dominio en el dominio o bosque y agregar servidores de catálogo global en función de la configuración antes del error.  
- Debido a un estado anterior, se restaura todo el bosque, se pierden todos los objetos (usuarios y equipos) que se han agregado y todas las actualizaciones (por ejemplo, los cambios de contraseña) que se realizaron en los objetos existentes después de este punto. Por lo tanto, debe volver a crear estos objetos que faltan y volver a aplicar las actualizaciones que faltan según corresponda.  
- Es posible que también deba restaurar confianzas de salida con dominios externos y bosques, ya que estas relaciones de confianza externa no se restauran automáticamente de las copias de seguridad.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)  
