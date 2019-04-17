---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: "Agregar un vínculo principal"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="add-home-link"></a>Agregar un vínculo principal 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para agregar el vínculo principal que se muestra en la página de sign\, usa el siguiente cmdlet de PowerShell de Windows y la sintaxis. 


![Agregar un vínculo principal](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> La `linkText`parámetro en este cmdlet no es necesario a menos que use otro valor que el valor predeterminado, que es *Home*. La ventaja de usar el valor predeterminado es que están localizadas para todas las configuraciones regionales de cliente. Una vez la página de sign\ personalizada, la personalización tiene prioridad; por lo tanto, deberías personalizar para todos los idiomas que quieras admitir.

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
