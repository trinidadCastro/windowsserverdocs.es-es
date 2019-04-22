---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Adición del vínculo a la página principal
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823016"
---
# <a name="add-home-link"></a>Adición del vínculo a la página principal 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

Para agregar el vínculo principal que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis. 


![Agregar vínculo principal](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Home*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. después del signo\-en la página es personalizado, la personalización tendrá precedencia; por lo tanto, que deberás personalizarla para todos los idiomas que desea admitir.

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
