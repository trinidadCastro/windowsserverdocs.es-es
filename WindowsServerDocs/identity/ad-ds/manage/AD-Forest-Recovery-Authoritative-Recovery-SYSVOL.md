---
description: 'Más información acerca de la recuperación del bosque de AD: realización de una sincronización autoritativa de SYSVOL replicada en DFSR'
title: 'Recuperación de bosque de AD: sincronización autoritativa de SYSVOL'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 38a1c543-c76d-4b8e-a06b-53742aaa172f
ms.openlocfilehash: e3e18b9dd8e9b04a0b7ee784872a62c87415cb9a
ms.sourcegitcommit: 6fbe337587050300e90340f9aa3e899ff5ce1028
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97599778"
---
# <a name="ad-forest-recovery---performing-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Recuperación de bosque de AD: realización de una sincronización autoritativa de SYSVOL replicada en DFSR

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Hay diferentes maneras de realizar una restauración autoritativa de SYSVOL. Puede editar el atributo **msDFSR-Options** o realizar una restauración del estado del sistema mediante Wbadmin – authsysvol. Si tiene la opción de restaurar una copia de seguridad del estado del sistema (es decir, está restaurando AD DS en el mismo hardware y la misma instancia de sistema operativo), el uso de Wbadmin – authsysvol es más sencillo. Pero si necesita realizar una restauración sin sistema operativo, tendrá que editar el atributo **msDFSR-Options** .

Siga los pasos siguientes para realizar una sincronización autoritativa de SYSVOL (si se replica con DFSR) editando el atributo **msDFSR-Options** . Si SYSVOL se replica con FRS, consulte el [artículo 290762](https://go.microsoft.com/fwlink/?LinkId=148443).

## <a name="to-perform-an-authoritative-synchronization-of-dfsr-replicated-sysvol"></a>Para realizar una sincronización autoritativa de SYSVOL replicada en DFSR

1. Abra Usuarios y equipos de Active Directory.
2. Haga clic en **Ver** y, a continuación, seleccione **usuarios, contactos, grupos y equipos como contenedores** y **características avanzadas**.

   ![Captura de pantalla que muestra la opción características avanzadas y la opción usuarios, contactos, grupos y equipos seleccionada.](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol1.png)

3. En la vista de árbol, haga clic en **controladores de dominio**, el nombre del DC que restauró, **DFSR-LocalSettings** y el **volumen del sistema de dominio**.

   ![Captura de pantalla que resalta la carpeta de volumen del sistema de dominio.](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol2.png)

4. En el panel de detalles, haga clic con el botón secundario en **suscripción de SYSVOL**, seleccione **propiedades** y haga clic en **Editor de atributos**.

   ![Captura de pantalla que muestra la pestaña Editor de atributos del cuadro de diálogo Propiedades de suscripciones de SYSVOL.](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol3.png)

5. Haga clic en **msDFSR-Options**, haga clic en **Editar**, escriba **1** y haga clic en **Aceptar** .

   ![SYSVOL](media/AD-Forest-Recovery-Authoritative-Recovery-SYSVOL/sysvol4.png)

6. Haga clic en **Aceptar** para cerrar el editor de atributos.

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
