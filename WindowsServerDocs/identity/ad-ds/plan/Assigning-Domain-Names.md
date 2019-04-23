---
ms.assetid: 73897497-b189-4305-b234-e057ffda163a
title: Asignar nombres de dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba5a7ee8b7d9728c48e2798853aab8d55047e86f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866566"
---
# <a name="assigning-domain-names"></a>Asignar nombres de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Debe asignar un nombre a cada dominio en el plan. Los dominios de Active Directory Domain Services (AD DS) tienen dos tipos de nombres: Los nombres de dominio del sistema de nombres (DNS) y los nombres NetBIOS. En general, ambos nombres son visibles para los usuarios finales. Los nombres DNS de dominios de Active Directory incluyen dos partes, un prefijo y un sufijo. Al crear nombres de dominio, determine primero el prefijo de DNS. Se trata de la primera etiqueta en el nombre DNS del dominio. El sufijo se determina cuando se selecciona el nombre del dominio raíz del bosque. La tabla siguiente muestra el prefijo de reglas de nomenclatura para los nombres DNS.  
  
|Regla|Explicación|  
|--------|---------------|  
|Seleccione un prefijo que no es probable que quede obsoleta.|Evite nombres como una línea de producto o sistema operativo que podrían cambiar en el futuro. Se recomienda usar nombres geográficos.|  
|Seleccione un prefijo que incluye caracteres estándar de Internet.|A-z, a-z, 0-9 y (-), pero no totalmente numérica.|  
|Incluir 15 caracteres o menos en el prefijo.|Si elige una longitud de prefijo de 15 caracteres o menos, el nombre de NetBIOS es el mismo que el prefijo.|  
  
Para obtener más información, vea las convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas ([https://go.microsoft.com/fwlink/?LinkId=106629](https://go.microsoft.com/fwlink/?LinkId=106629)).  
  
> [!NOTE]  
>  Aunque Dcpromo.exe en Windows Server 2008 y Windows Server 2003 le permite crear un nombre de dominio DNS de etiqueta única, no debe usar un nombre DNS de etiqueta única para un dominio por varias razones. En Windows Server 2008 R2, Dcpromo.exe no permite crear un nombre DNS de etiqueta única para un dominio. Para obtener más información, consulte [ https://go.microsoft.com/fwlink/?LinkId=92467.](https://go.microsoft.com/fwlink/?LinkId=92467)   
  
Si el nombre de NetBIOS actual del dominio no es apropiado representar la región o se produce un error cumplir con el prefijo de reglas de nomenclatura, seleccione un nuevo prefijo. En este caso, el nombre NetBIOS del dominio es diferente del prefijo DNS del dominio.  
  
Para cada dominio nuevo que implemente, seleccione un prefijo que es adecuado para la región y que ajuste a las reglas de nomenclatura de prefijo. Se recomienda que el nombre NetBIOS del dominio sea el mismo que el prefijo de DNS.  
  
Documentar el prefijo DNS y los nombres de NetBIOS que seleccionó para cada dominio del bosque. Puede agregar la información de nombre NetBIOS y DNS en la hoja de cálculo "Planeación de dominio" que creó para documentar el plan para dominios nuevos y actualizados. Para abrirlo, descargue Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de trabajo ayudas para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)) y abra el "Dominio planeación" (DSSLOGI_5.doc).  
  


