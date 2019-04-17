---
title: "Recuperación de bosque de AD - restauración no autorizada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adfs
ms.openlocfilehash: 684672491a0574b12e9b117a8e3c9c1f0936f357
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Realizar una restauración no autorizada de los servicios de dominio de Active Directory 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
 Para realizar una restauración no autorizada, completa el siguiente procedimiento.  
  
 Los siguientes procedimientos usan la Wbadmin.exe para realizar una restauración no autorizada de Active Directory o los servicios de dominio de Active Directory (AD DS). Si estás usando una solución de copia de seguridad diferente o si tienes previsto completar la restauración de SYSVOL más adelante en el proceso de recuperación de bosque, puede realizar una restauración de SYSVOL mediante estos métodos alternativos:  
  
-   Si estás usando el servicio de replicación de archivos (FRS) para replicar SYSVOL, sigue los pasos de [artículo 290762](https://go.microsoft.com/fwlink/?LinkId=148443) en Microsoft Knowledge Base, con la **BurFlags** clave del registro para reinicializar los conjuntos de réplica FRS o, si es necesario, el artículo 315457 [315457](https://support.microsoft.com/kb/315457)volver a generar el árbol de SYSVOL. Para determinar si se replica SYSVOL FRS, consulta [carpeta SYSVOL de determinar si un controlador de dominio se replica DFSR o FRS](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
  
-   Si estás usando la replicación del sistema de archivos distribuido (DFS) para replicar SYSVOL, consulta [realizar una sincronización autorizada de SYSVOL replicados DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
 
## <a name="performing-a-nonauthoritative-restore"></a>Realizar una restauración no autorizada  
 Usa el siguiente procedimiento para realizar una restauración no autorizada de AD DS y una restauración de SYSVOL al mismo tiempo usando wbadmin.exe en un controlador de dominio que se ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. La copia de seguridad explícitamente debe incluir datos de estado del sistema; una copia de seguridad completa del servidor que se usa para la recuperación completa del servidor no funcionará. Para obtener más información sobre cómo crear una copia de seguridad de estado del sistema, consulta [copia los datos de estado del sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Para realizar una restauración no autorizada de AD DS y restauración de SYSVOL mediante wbadmin.exe  
  
-   Incluir la **- authsysvol** cambiar en el comando de recuperación, como se muestra en el siguiente ejemplo:  
  
    ```  
    wbadmin start systemstaterecovery <otheroptions> -authsysvol  
    ```  
  
     Por ejemplo:  
  
    ```  
    wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
    ```  
  
 ![Restaurar](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
