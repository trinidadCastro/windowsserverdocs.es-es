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
ms.openlocfilehash: a9a043390f5bfb412e549779ed4a9048d1c8a0b5
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190180"
---
# <a name="add-home-link"></a>Adición del vínculo a la página principal 

Para agregar el vínculo principal que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis. 


![Agregar vínculo principal](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Home*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. después del signo\-en la página es personalizado, la personalización tendrá precedencia; por lo tanto, que deberás personalizarla para todos los idiomas que desea admitir.

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
