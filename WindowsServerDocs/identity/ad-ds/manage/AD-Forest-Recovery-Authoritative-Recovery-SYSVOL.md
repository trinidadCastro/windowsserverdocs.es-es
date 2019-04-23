---
title: Recuperación de bosques de AD - sincronización autoritaria de SYSVOL
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adds
ms.openlocfilehash: 246a2ea589ee05110362cff99d50e93a0a18c2b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827006"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperación de bosques de AD - realizar una sincronización autoritativa de SYSVOL replicado mediante DFSR  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Hay diferentes maneras de realizar una restauración autoritativa de SYSVOL. Puede modificar el **msDFSR-Options** de atributo o realizar una restauración del estado del sistema mediante wbadmin – authsysvol. Si tiene la opción para restaurar una copia de seguridad del estado del sistema (es decir, va a restaurar AD DS en la misma instancia de hardware y sistema operativo), a continuación, usar wbadmin – authsysvol es más fácil. Pero si necesita realizar una restauración completa, deberá editar la **msDFSR-Options** atributo.  

Siga estos pasos para realizar una sincronización autoritativa de SYSVOL (si está replicado mediante DFSR) editando el **msDFSR-Options** atributo. Si se replica SYSVOL mediante FSR, consulte [artículo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para realizar una sincronización autoritativa de SYSVOL replicado mediante DFSR  

1. Abra Usuarios y equipos de Active Directory.  
2. Haga clic en **vista**y, a continuación, seleccione **usuarios, contactos, grupos y equipos como contenedores** y **características avanzadas**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 

3. En la vista de árbol, haga clic en **controladores de dominio**, el nombre del controlador de dominio que ha restaurado, **DFSR-LocalSettings**y, a continuación, **Domain System Volume**. 

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  

4. En el panel de detalles, haga clic en **SYSVOL Subscription**, haga clic en **propiedades**y haga clic en **Editor de atributos**.  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 

5. Haga clic en **msDFSR-Options**, haga clic en **editar**, tipo **1**y haga clic en **Aceptar**  

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 

6. Haga clic en **Aceptar** para cerrar el Editor de atributos.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
