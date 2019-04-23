---
title: 'Recuperación de bosques de AD: restablecer la contraseña de krbtgt'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 1ac0dcb9da1d10a417c128cb8498a5d8362d9a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883746"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Recuperación de bosques de AD: restablecer la contraseña de krbtgt

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para restablecer la contraseña de krbtgt para el dominio. El siguiente procedimiento aplica a los controladores de dominio grabables, pero los controladores de dominio no sea de sólo lectura (RODC).
  
> [!IMPORTANT]
> Si va a recuperar los RODC en línea durante la recuperación de bosque, no elimine las cuentas krbtgt para el RODC. La cuenta krbtgt para un RODC se muestra en el formato krbtgt_*número*.
>
> Si utiliza un filtro de contraseñas personalizado (por ejemplo, passfilt.dll) en un controlador de dominio, puede recibir un error al intentar restablecer la contraseña de krbtgt. Para obtener más información, incluida una solución, consulte Microsoft Knowledge Base [artículo 2549833](https://support.microsoft.com/kb/2549833) (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Para restablecer la contraseña de krbtgt  
  
1. Haga clic en **iniciar**, apunte a **Panel de Control**, apunte a **herramientas administrativas**y, a continuación, haga clic en **equipos y usuarios de Active Directory**.
2. Haga clic en **Ver** y, a continuación, haga clic en **Características avanzadas**.
3. En el árbol de consola, haga doble clic en el contenedor de dominio y, a continuación, haga clic en **usuarios**.
4. En el panel de detalles, haga clic en el **krbtgt** cuenta de usuario y, a continuación, haga clic en **restablecer contraseña**.
   ![Restablecimiento de contraseña](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. En **nueva contraseña**, escriba una nueva contraseña, vuelva a escribir la contraseña en **Confirmar contraseña**y, a continuación, haga clic en **Aceptar**. La contraseña que especifique no es significativa porque el sistema generará una contraseña segura automáticamente independiente de la contraseña que especifique.
  
> [!NOTE]
> Debe realizar esta operación dos veces. El historial de contraseñas de la cuenta krbtgt es dos, lo que significa que incluye las dos contraseñas más reciente. Al restablecer la contraseña dos veces desactiva eficazmente las contraseñas antiguas del historial, así que no hay ninguna manera de otro DC replicará con este controlador de dominio mediante el uso de una contraseña antigua.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md) 
