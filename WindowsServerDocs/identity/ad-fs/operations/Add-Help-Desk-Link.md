---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Adición del vínculo al Servicio de asistencia
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb186c3ba5cfb3acb9bfd0c3139b09b992fb8863
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190207"
---
# <a name="add-help-desk-link"></a>Adición del vínculo al Servicio de asistencia 


## <a name="to-add-a-help-desk-link"></a>Para agregar un vínculo de Ayuda del servicio de asistencia  
Para agregar el vínculo de Ayuda del servicio de asistencia que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis.  

![agregar soporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Help*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Después de personalizar la página, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir.  


## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
