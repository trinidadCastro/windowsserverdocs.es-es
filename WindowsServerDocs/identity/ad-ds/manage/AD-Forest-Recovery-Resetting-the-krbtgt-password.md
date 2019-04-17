---
title: "Recuperación de bosque de AD - restablecer la contraseña de krbtgt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adfs
ms.openlocfilehash: 445fa18503e26d04e20a61cfe652424f78631abe
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Recuperación de bosque de AD - restablecer la contraseña de krbtgt 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Usa el siguiente procedimiento para restablecer la contraseña de krbtgt para el dominio. El siguiente procedimiento aplica a controladores de dominio grabables, pero los controladores de dominio de solo lectura no (RODC).  
  
> [!IMPORTANT]
>  Si vas a recuperar RODC en línea durante la recuperación de bosque, no se puede eliminar las cuentas krbtgt para el RODC. La cuenta krbtgt para un RODC aparece en el formato krbtgt_*número*.  
>   
>  Si usas un filtro de contraseñas personalizado (por ejemplo, passfilt.dll) en un controlador de dominio, a continuación, es posible que recibes un error al intentar restablecer la contraseña de krbtgt. Para obtener más información, incluida una solución, consulta el tema de Microsoft Knowledge Base [artículo 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).  
  
## <a name="to-reset-the-krbtgt-password"></a>Para restablecer la contraseña de krbtgt  
  
1.  Haz clic en **inicio**, elija **Panel de Control**, elija **herramientas administrativas**y, a continuación, haz clic en **equipos y usuarios de Active Directory**.  
2.  2.  Haz clic en **vista**y, a continuación, haz clic en **características avanzadas**.  
3.  En el árbol de consola, haz doble clic en el contenedor de dominio y, a continuación, haz clic en **usuarios**.  
4.  En el panel de detalles, haz clic en el **krbtgt** cuenta de usuario y, a continuación, haz clic en **restablecer contraseña**.  
![Restablecer la contraseña](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5.  En **nueva contraseña**, escribe una nueva contraseña, vuelva a escribir la contraseña en **Confirmar contraseña**y, a continuación, haz clic en **Aceptar**. La contraseña que especifiques no es importante porque el sistema generará una contraseña segura automáticamente independiente de la contraseña que especifiques.  
  
    > [!NOTE]
    >  Debes realizar esta operación dos veces. El historial de contraseñas de la cuenta krbtgt es dos, lo que significa que incluye las contraseñas de dos las más recientes. Al restablecer la contraseña de dos veces eficazmente borrar las contraseñas anteriores desde el historial, así que no hay ninguna manera de otro DC replicará con este controlador de dominio mediante el uso de una contraseña anterior.  
 
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md) 
  
