---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Adición del vínculo a la página principal
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4a6210b1b7475a4ec34bbe0575915f376381fe1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859388"
---
# <a name="add-home-link"></a>Adición del vínculo a la página principal 

Para agregar el vínculo de inicio que se muestra en la página firmar\-en, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis. 


![Agregar vínculo de inicio](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Home*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Una vez que se ha personalizado el signo\-en la página, la personalización tiene prioridad; por lo tanto, debe personalizar para todos los idiomas que desee admitir.

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)  
