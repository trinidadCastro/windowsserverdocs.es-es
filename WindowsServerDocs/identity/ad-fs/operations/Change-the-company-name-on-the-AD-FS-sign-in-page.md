---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Cambiar el nombre de la compañía en la página de inicio de sesión de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e2b5c7228094305759344d5094cffa7f24a0da7a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190025"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Cambiar el nombre de la compañía en la página de inicio de sesión de AD FS
 
Para cambiar el nombre de la compañía que se muestra en el inicio de sesión\-en la página, use el siguiente cmdlet de Windows PowerShell y la sintaxis. Este valor se establece de forma predeterminada con el valor del nombre para mostrar del Servicio de federación que introdujiste durante la configuración.  

![Cambiar nombre](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> También puede usar el entorno de Scripting integrado de Windows PowerShell \(ISE\) para cambiar el nombre de la empresa. Mediante el uso de Windows PowerShell ISE, puede mostrar contenido en un formato Unicode\-entorno compatible con. Para obtener más información, consulte [Introducción a Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md)  
  
