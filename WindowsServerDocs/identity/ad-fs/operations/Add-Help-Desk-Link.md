---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Adición del vínculo al Servicio de asistencia
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1673e6ee6357a9d59e8ac5891625d453bb434088
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358458"
---
# <a name="add-help-desk-link"></a>Adición del vínculo al Servicio de asistencia 


## <a name="to-add-a-help-desk-link"></a>Para agregar un vínculo al Departamento de soporte técnico  
Para agregar el vínculo del Departamento de soporte técnico que se muestra en la página Sign @ no__t-0in, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.  

![Agregar Departamento de soporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Help*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Después de personalizar la página, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir.  


## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
