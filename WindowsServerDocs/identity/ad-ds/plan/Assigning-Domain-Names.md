---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Asignar nombres de dominio
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 84a74c9c210f4e3c826b7efd3451b76bcec96616
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071167"
---
# <a name="assigning-domain-names"></a>Asignar nombres de dominio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debe asignar un nombre a cada dominio del plan. Los dominios de Active Directory Domain Services (AD DS) tienen dos tipos de nombres: nombres de sistema de nombres de dominio (DNS) y nombres NetBIOS. En general, ambos nombres son visibles para los usuarios finales. Los nombres DNS de Active Directory dominios incluyen dos partes, un prefijo y un sufijo. Al crear nombres de dominio, determine primero el prefijo DNS. Esta es la primera etiqueta en el nombre DNS del dominio. El sufijo se determina cuando se selecciona el nombre del dominio raíz del bosque. En la tabla siguiente se enumeran las reglas de nomenclatura de prefijos para los nombres DNS.

|Regla|Explicación|
|--------|---------------|
|Seleccione un prefijo que no sea probable que quede obsoleto.|Evite nombres como una línea de productos o un sistema operativo que podrían cambiar en el futuro. Se recomienda usar nombres geográficos.|
|Seleccione un prefijo que incluya solo caracteres estándar de Internet.|A-Z, a-z, 0-9 y (-), pero no totalmente numérica.|
|Incluya 15 caracteres o menos en el prefijo.|Si elige una longitud de prefijo de 15 caracteres o menos, el nombre NetBIOS es el mismo que el prefijo.|

Para obtener más información, consulte [convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/help/909264/).

> [!NOTE]
> Aunque Dcpromo.exe en Windows Server 2008 y Windows Server 2003 le permite crear un nombre de dominio DNS de etiqueta única, no debe usar un nombre DNS de etiqueta única para un dominio por varias razones. En Windows Server 2008 R2, Dcpromo.exe no permite crear un nombre DNS de etiqueta única para un dominio. Para obtener más información, consulte [implementación y funcionamiento de Active Directory dominios que se configuran mediante nombres DNS de etiqueta única](https://support.microsoft.com/help/300684/).

Si el nombre NetBIOS actual del dominio no es apropiado para representar la región o no cumple las reglas de nomenclatura de prefijos, seleccione un nuevo prefijo. En este caso, el nombre NetBIOS del dominio es diferente del prefijo DNS del dominio.

Para cada nuevo dominio que implemente, seleccione un prefijo adecuado para la región y que cumpla las reglas de nomenclatura de prefijos. Se recomienda que el nombre NetBIOS del dominio sea el mismo que el prefijo DNS.

Documente el prefijo DNS y los nombres NetBIOS que seleccione para cada dominio del bosque. Puede Agregar la información de nombres DNS y NetBIOS a la hoja de cálculo "planeamiento de dominios" que creó para documentar el plan de dominios nuevos y actualizados. Para abrirlo, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de la [ayuda del trabajo para el kit de implementación de Windows Server 2003](https://microsoft.com/download/details.aspx?id=9608) y abra "planeamiento de dominios" (DSSLOGI_5.doc).
