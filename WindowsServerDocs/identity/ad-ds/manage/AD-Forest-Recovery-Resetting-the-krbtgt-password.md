---
title: 'Recuperación de bosque de AD: restablecimiento de la contraseña de krbtgt'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 3bd6c1d0-d316-4b03-b7b4-557d4537635c
ms.technology: identity-adds
ms.openlocfilehash: 14dd09c6177d473547a67e1d79e9714f0a7a29b3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390302"
---
# <a name="ad-forest-recovery---resetting-the-krbtgt-password"></a>Recuperación de bosque de AD: restablecimiento de la contraseña de krbtgt

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Utilice el siguiente procedimiento para restablecer la contraseña de krbtgt para el dominio. El siguiente procedimiento se aplica a los controladores de dominio grabables, pero no a los controladores de dominio de solo lectura (RODC).
  
> [!IMPORTANT]
> Si tiene previsto recuperar los RODC en línea durante la recuperación del bosque, no elimine las cuentas krbtgt para los RODC. La cuenta krbtgt para un RODC se muestra en el formato krbtgt_*número*.
>
> Si utiliza un filtro de contraseña personalizado (como passfilt. dll) en un controlador de dominio, es posible que reciba un error al intentar restablecer la contraseña de krbtgt. Para obtener más información, incluida una solución alternativa, vea el [artículo 2549833](https://support.microsoft.com/kb/2549833) de Microsoft Knowledge Base (https://support.microsoft.com/kb/2549833).
  
## <a name="to-reset-the-krbtgt-password"></a>Para restablecer la contraseña de krbtgt  
  
1. Haga clic en **Inicio**, seleccione **Panel de control**, seleccione **herramientas administrativas**y, a continuación, haga clic en **Active Directory usuarios y equipos**.
2. Haga clic en **Ver** y, a continuación, haga clic en **Características avanzadas**.
3. En el árbol de consola, haga doble clic en el contenedor de dominio y, a continuación, haga clic en **usuarios**.
4. En el panel de detalles, haga clic con el botón secundario en la cuenta de usuario de **krbtgt** y, a continuación, haga clic en **Restablecer contraseña**.
   ![restablecer contraseña](media/AD-Forest-Recovery-Resetting-the-krbtgt-password/resetpass1.png)
5. En **nueva contraseña**, escriba una nueva contraseña, vuelva a escribir la contraseña en **Confirmar contraseña**y, a continuación, haga clic en **Aceptar**. La contraseña que especifique no es significativa, ya que el sistema generará automáticamente una contraseña segura independiente de la contraseña que especifique.
  
> [!NOTE]
> Esta operación debe realizarse dos veces. El historial de contraseñas de la cuenta krbtgt es dos, lo que significa que incluye las dos contraseñas más recientes. Al restablecer la contraseña dos veces, se borran de forma eficaz las contraseñas antiguas del historial, por lo que no hay ninguna manera en que otro controlador de dominio se replicará con este controlador de dominio con una contraseña anterior.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md) 
