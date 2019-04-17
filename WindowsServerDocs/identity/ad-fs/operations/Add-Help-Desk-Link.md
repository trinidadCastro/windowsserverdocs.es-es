---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: "Agregar el vínculo de Ayuda de asistencia al cliente"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d16cc0a75bfe636c29b44687b669e87f31b69ce
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2018
---
# <a name="add-help-desk-link"></a>Agregar el vínculo de Ayuda de asistencia al cliente 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>Para agregar un vínculo de Ayuda de asistencia al cliente  
Para agregar el vínculo de Ayuda de asistencia al cliente que se muestra en la página de sign\, usa el siguiente cmdlet de PowerShell de Windows PowerShell y la sintaxis.  

![Agregar el servicio de asistencia](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> La `linkText`parámetro en este cmdlet no es necesario a menos que use otro valor que el valor predeterminado, que es *ayuda*. La ventaja de usar el valor predeterminado es que están localizadas para todas las configuraciones regionales de cliente. Después de personalizar la página, la personalización tiene prioridad; por lo tanto, deberías personalizar para todos los idiomas que quieras admitir.  


## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
