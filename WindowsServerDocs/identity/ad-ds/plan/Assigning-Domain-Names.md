---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: "Asignación de nombres de dominio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0d5ec9b76798f21503f650527a7961cff0b592b4
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="assigning-domain-names"></a>Asignación de nombres de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debes asignar un nombre a cada dominio en tu plan. Active Directory (AD DS) dominios tienen dos tipos de nombres: nombres de NetBIOS y sistema de nombres de dominio (DNS). En general, ambos nombres son visibles para los usuarios finales. Los nombres DNS de dominios de Active Directory incluyen dos partes: un prefijo y un sufijo. Al crear nombres de dominio, primero determinar el prefijo DNS. Esta es la primera etiqueta en el nombre DNS del dominio. Cuando se selecciona el nombre de dominio raíz del bosque, se determina el sufijo. La siguiente tabla enumera el prefijo reglas de nomenclatura de nombres DNS.  
  
|Regla|Explicación|  
|--------|---------------|  
|Selecciona un prefijo que no es probable que quedar obsoletos.|Evitar que los nombres, como una línea o el sistema operativo que se puede cambiar en el futuro. Te recomendamos que uses nombres geográficos.|  
|Selecciona un prefijo que incluye caracteres estándar de Internet.|A la Z, z, 0-9 y (-), pero no totalmente numéricos.|  
|Incluir 15 caracteres o menos en el prefijo.|Si eliges una longitud de prefijo de 15 caracteres o menos, el nombre NetBIOS es el mismo que el prefijo.|  
  
Para obtener más información, consulta las convenciones de nomenclatura de Active Directory para equipos, dominios, sitios y unidades organizativas ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Aunque Dcpromo.exe en Windows Server 2008 y Windows Server 2003 permite crear un nombre de dominio DNS de etiqueta única, no debes usar un nombre DNS de etiqueta única para un dominio por varios motivos. En Windows Server 2008 R2, Dcpromo.exe no permite crear un nombre DNS de etiqueta única para un dominio. Para obtener más información, consulta [https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Si el nombre NetBIOS actual del dominio es apropiado representar la región o no puede cumplir el prefijo reglas de nomenclatura, selecciona un prefijo nuevo. En este caso, el nombre NetBIOS del dominio es diferente del prefijo DNS del dominio.  
  
Para cada nuevo dominio que vas a implementar, selecciona un prefijo que resulte adecuado para la región y que cumplen las reglas de nomenclatura de prefijo. Te recomendamos que el nombre NetBIOS del dominio sea el mismo que el prefijo DNS.  
  
Documenta el prefijo DNS y nombres de NetBIOS que selecciones para cada dominio en el bosque. Puedes agregar la información del nombre DNS y NetBIOS a la hoja de cálculo "Dominio planear" que creaste para documentar el plan para dominios nuevos y actualizados. Para abrirlo, descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abre "Dominio planear" (DSSLOGI_5.doc).  
  


