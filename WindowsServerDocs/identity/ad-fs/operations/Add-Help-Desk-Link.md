---
description: Más información acerca de cómo agregar el vínculo del Departamento de soporte técnico
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Adición del vínculo al Servicio de asistencia
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c806390687d5293feacd2575b10569504334914f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97044373"
---
# <a name="add-help-desk-link"></a>Adición del vínculo al Servicio de asistencia


## <a name="to-add-a-help-desk-link"></a>Para agregar un vínculo al Departamento de soporte técnico
Para agregar el vínculo del Departamento de soporte técnico que se muestra en la \- Página de inicio de sesión, use el siguiente cmdlet de Windows PowerShell y la siguiente sintaxis.

![Agregar Departamento de soporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`


> [!IMPORTANT]
> El parámetro `linkText` de este cmdlet solo es obligatorio si usa un valor distinto del predeterminado, que es *Help*. La ventaja de usar el valor predeterminado es que las páginas están localizadas para todas las configuraciones regionales de los clientes. Después de personalizar la página, la personalización tendrá precedencia, así que deberás personalizarla para todos los idiomas que quieras admitir.


## <a name="additional-references"></a>Referencias adicionales
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md)
