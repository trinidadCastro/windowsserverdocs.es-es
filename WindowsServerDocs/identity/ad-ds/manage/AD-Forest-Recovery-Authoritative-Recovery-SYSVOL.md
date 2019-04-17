---
title: "Recuperación de bosque de AD - sincronización autorizado de SYSVOL"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.technology: identity-adfs
ms.openlocfilehash: 5bdc619ddf9f28fb074e90dfbf22a7be076a05d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperación de bosque de AD - realizando una sincronización autorizada de SYSVOL replicados DFSR  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Hay diferentes formas de realizar una restauración de SYSVOL. Puede modificar la **msDFSR opciones** atributo o realizar una restauración del estado del sistema mediante wbadmin – authsysvol. Si tienes la opción para restaurar una copia de seguridad de estado del sistema (es decir, está restaurando AD DS en la misma instancia de hardware y del sistema operativo), a continuación, usar wbadmin – authsysvol es más fácil. Pero si necesitas realizar una recuperación, debes editar el **msDFSR opciones** atributo.  
  
 Usa los siguientes pasos para realizar una sincronización autorizada de SYSVOL (si se replica con DFSR) modificando la **msDFSR opciones** atributo. Si se replica SYSVOL con FRS, consulta [artículo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).  
  
## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para realizar una sincronización autorizada de SYSVOL replicados DFSR  
  
1.  Abre equipos y usuarios de Active Directory.  
2.  Haz clic en **vista**y, a continuación, selecciona **a los usuarios, contactos, grupos y equipos como contenedores** y **características avanzadas**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png) 
3.  En la vista de árbol, haga clic en **controladores de dominio**, el nombre del controlador de dominio ha restaurado, **DFSR LocalSettings**y luego **el volumen del sistema de dominio**. 
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)  
4.  En el panel de detalles, haz clic en **SYSVOL suscripción**, haz clic en **propiedades**y haz clic en **atributo Editor**.  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png) 
5.  Haz clic en **msDFSR opciones**, haz clic en **editar**, tipo **1**y haz clic en **Aceptar**  
![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png) 
6.  Haz clic en **Aceptar** para cerrar el Editor de atributo.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
