---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Adición del vínculo a la página principal
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5f1fad340629304fdf960139be05b8dbc2690e0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407770"
---
# <a name="add-home-link"></a>Adición del vínculo a la página principal 

Para agregar el vínculo de inicio que se muestra en la página firmar\-en, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis. 


![Agregar vínculo de inicio](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Home*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. una vez que se ha personalizado el signo\-en la página, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir.

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
