---
title: Recuperación de bosques de AD - restauración no autoritativa
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: 65e33e6507d2affc4d07cc0780a7baf91a170a09
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280587"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Realizar una restauración no autoritativa de Active Directory Domain Services 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Para llevar a cabo una restauración no autoritativa, complete el procedimiento siguiente.  
  
Los siguientes procedimientos utilizan el Wbadmin.exe para realizar una restauración no autoritativa de Active Directory o servicios de dominio de Active Directory (AD DS). Si usa una solución de copia de seguridad diferente o si tiene previsto completar la restauración autoritativa de SYSVOL más adelante en el proceso de recuperación del bosque, puede realizar una restauración autoritativa de SYSVOL mediante estos métodos alternativos:  
  
- Si usa el servicio de replicación de archivos (FRS) para replicar SYSVOL, siga los pasos descritos en [artículo 290762](https://go.microsoft.com/fwlink/?LinkId=148443) en Microsoft Knowledge Base, con el **BurFlags** clave del registro para reinicializar réplica FRS Establece o, si es necesario, el artículo 315457 [315457](https://support.microsoft.com/kb/315457)para volver a generar el árbol SYSVOL. Para determinar si se replica SYSVOL mediante FRS, consulte [carpeta SYSVOL de determinar si un controlador de dominio se replica mediante DFSR o FRS](https://msdn.microsoft.com/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
- Si se usa la replicación del sistema de archivos distribuido (DFS) para replicar SYSVOL, consulte [realizar una sincronización autoritativa de SYSVOL DFSR replicado](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  

## <a name="performing-a-nonauthoritative-restore"></a>Realizar una restauración no autoritativa

Utilice el procedimiento siguiente para realizar una restauración no autoritativa de AD DS y una restauración autoritativa de SYSVOL al mismo tiempo mediante el uso de wbadmin.exe en un controlador de dominio que ejecuta Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008. La copia de seguridad debe incluir explícitamente los datos de estado del sistema; una copia de seguridad completa del servidor que se usa para la recuperación de servidor completo no funcionará. Para obtener más información acerca de cómo crear una copia de seguridad del estado del sistema, consulte [copia los datos de estado del sistema](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Para llevar a cabo una restauración no autoritativa de AD DS y autorizado la restauración de SYSVOL con wbadmin.exe  
  
- Incluir el **- authsysvol** cambiar en el comando de recuperación, como se muestra en el ejemplo siguiente:  

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
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
