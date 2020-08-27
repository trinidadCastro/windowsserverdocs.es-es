---
title: 'Recuperación de bosque de AD: restauración no autoritativa'
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.openlocfilehash: c46eb76f707b7285e06c01fef14534163df00823
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939655"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Realizar una restauración no autoritativa de Active Directory Domain Services

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Para realizar una restauración no autoritativa, complete el procedimiento siguiente.

En los procedimientos siguientes se usa el Wbadmin.exe para realizar una restauración no autoritativa de Active Directory o Active Directory Domain Services (AD DS). Si usa una solución de copia de seguridad diferente o si piensa completar la restauración autoritativa de SYSVOL más adelante en el proceso de recuperación del bosque, puede realizar una restauración autoritativa de SYSVOL mediante estos métodos alternativos:

- Si usa el servicio de replicación de archivos (FRS) para replicar SYSVOL, siga los pasos descritos en el [artículo 290762](https://go.microsoft.com/fwlink/?LinkId=148443) de Microsoft Knowledge base, uso de la clave del registro **BurFlags** para reinicializar conjuntos de réplicas FRS o, si es necesario, el artículo 315457 [315457](https://support.microsoft.com/kb/315457)para recompilar el árbol SYSVOL. Para determinar si FRS Replica SYSVOL, vea [determinar si DFSR o FRS Replica la carpeta Sysvol de un controlador de dominio](/windows/win32/vss/backing-up-and-restoring-an-frs-replicated-sysvol-folder#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).
- Si usa la replicación de Sistema de archivos distribuido (DFS) para replicar SYSVOL, consulte [realizar una sincronización autoritativa de SYSVOL replicada en DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).

## <a name="performing-a-nonauthoritative-restore"></a>Realizar una restauración no autoritativa

Use el procedimiento siguiente para realizar una restauración no autoritativa de AD DS y una restauración autoritativa de SYSVOL al mismo tiempo mediante wbadmin.exe en un controlador de dominio que ejecute Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. La copia de seguridad debe incluir explícitamente los datos de estado del sistema. una copia de seguridad completa del servidor que se usa para la recuperación completa del servidor no funcionará. Para obtener más información acerca de la creación de una copia de seguridad del estado del sistema, vea [realizar copias de seguridad de los datos de estado del sistema](AD-Forest-Recovery-Backing-up-System-State.md).

### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Para realizar una restauración no autoritativa de AD DS y la restauración autoritativa de SYSVOL mediante wbadmin.exe

- Incluya el modificador **-authsysvol** en el comando de recuperación, tal como se muestra en el ejemplo siguiente:

   ```
   wbadmin start systemstaterecovery <otheroptions> -authsysvol
   ```

   Por ejemplo:

   ```
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol
   ```

![Restauración](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
