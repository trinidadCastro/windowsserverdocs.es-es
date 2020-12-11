---
description: 'Más información sobre: Agregar vínculo de privacidad'
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adición del vínculo de la privacidad
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c15491ed9540c68c76e9a6ddfbcdf830690c9dc1
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044313"
---
# <a name="add-privacy-link"></a>Adición del vínculo de la privacidad


Para agregar el vínculo de privacidad que se muestra en la \- Página de inicio de sesión, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.

![Agregar vínculo de privacidad](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`


> [!IMPORTANT]
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Privacy*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Después de personalizar la página de inicio \- de sesión, la personalización tiene precedencia, por lo que debe personalizarla para todos los idiomas que desee admitir. Todo el contenido personalizado toma un parámetro de configuración regional. Al configurar contenido localizado, debe configurarlo con un \- nombre de país menos local, por ejemplo, "en", antes de configurar la configuración regional específica de país y región \- , como "en \- EE. UU.".

## <a name="additional-references"></a>Referencias adicionales
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
