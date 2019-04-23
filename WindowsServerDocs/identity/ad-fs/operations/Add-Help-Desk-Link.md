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
ms.openlocfilehash: 1654add6a81169b3d4831d6ebba320402e0734c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849866"
---
# <a name="add-help-desk-link"></a>Adición del vínculo al Servicio de asistencia 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>Para agregar un vínculo de Ayuda del servicio de asistencia  
Para agregar el vínculo de Ayuda del servicio de asistencia que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis.  

![agregar soporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Help*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Después de personalizar la página, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir.  


## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
