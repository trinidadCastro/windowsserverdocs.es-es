---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: "Cambiar el nombre de empresa en la página de inicio de sesión de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8e99f6fed8922ed15bf78a98b207b6f46767763a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Cambiar el nombre de empresa en la página de inicio de sesión de AD FS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2
 
Para cambiar el nombre de la empresa que se muestra en la página de sign\, usa el siguiente cmdlet de PowerShell de Windows PowerShell y la sintaxis. Este valor se establece de forma predeterminada mediante el valor del nombre de pantalla de servicios de federación que escribiste durante la instalación.  

![Cambiar nombre](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> También puedes usar la \(ISE\) entorno de Scripting integrado de Windows PowerShell para cambiar el nombre de la compañía. Al usar Windows PowerShell ISE, puede mostrar contenido en un entorno Unicode\ compatible. Para obtener más información, consulta [Introducción a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md)  
  
