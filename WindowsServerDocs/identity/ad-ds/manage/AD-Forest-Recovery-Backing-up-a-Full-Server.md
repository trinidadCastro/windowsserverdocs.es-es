---
title: "Recuperación de bosque de AD - copia de seguridad completa del servidor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: b1af97c2eb23d65c2d106906bc0f5bb1f10b23ec
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Recuperación de bosque de AD - copia de seguridad completa del servidor  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Se recomienda una copia de seguridad completa del servidor para preparar la recuperación de un bosque porque se puede restaurar a diferentes tipos de hardware o una instancia de otro sistema operativo.  Con la copia de seguridad de Windows Server pueden realizar una copia de seguridad completa del servidor. 

## <a name="windows-server-backup"></a>Copia de seguridad de Windows Server
Copia de seguridad de Windows Server no está instalado de manera predeterminada. En Windows Server 2016 y Windows Server 2012 R2, instalarlo siguiendo los pasos siguientes.

>[!NOTE]
>Ten en cuenta que los pasos pueden variar ligeramente entre Windows Server 2016 y Windows Server 2012 R2.

Para que conocer los pasos instalarlo en Windows Server 2008 y Windows Server 2008 R2, consulta [copias de seguridad de instalación de Windows Server](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Para instalar la copia de seguridad de Windows Server
1. Abre **administrador del servidor** y haz clic en **agregar roles y características**.
2. En la **agregar Roles and Features Wizard** haga clic en **siguiente**.
3. En la **tipo de instalación** de pantalla, deja el valor predeterminado **instalación basada en rol o característica** y haz clic en **siguiente**.
4. En la **selección de servidor** de pantalla, haz clic en **siguiente**.
5. En la **Roles de servidor** pantalla clic **siguiente**.
6. En la **características** pantalla, selecciona **copias de seguridad de Windows Server** y haz clic en **siguiente**<ph x="4">
! [</ph> Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Haz clic en **instalar**.
8. Una vez completada la instalación, haz clic en **cerrar**.


### <a name="to-perform-a-backup-with-windows-server-backup"></a>Para realizar una copia de seguridad con copias de seguridad de Windows Server

1. Abre **administrador del servidor**, haz clic en **herramientas**y, a continuación, haz clic en **copias de seguridad de Windows Server**.
    - En Windows Server 2008 R2 y Windows Server 2008, haz clic en **inicio**, elija **herramientas administrativas**y, a continuación, haz clic en **copias de seguridad de Windows Server**. 
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Si se le pide, en la **Control de cuentas de usuario** cuadro de diálogo, proporciona las credenciales de operador de copia de seguridad y, a continuación, haz clic en **Aceptar**.
3. Haz clic en **copia de seguridad Local**.
4. En la **acción** menú, haz clic en **una copia de seguridad una vez**.
5. En la copia de seguridad Asistente para una vez, en la **opciones de copia de seguridad** página, haz clic en **diferentes opciones**y, a continuación, haz clic en **siguiente**.
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. En la **selecciona configuración copia de seguridad** página, haz clic en **servidor completo (recomendado)**y, a continuación, haz clic en **siguiente**.
7. En la **especificar el tipo de destino** página, haz clic en **unidades locales** o **remoto carpeta compartida**y, a continuación, haz clic en **siguiente**.
8. En la **seleccionar un destino de copia de seguridad** página, elige la ubicación de copia de seguridad.  Si has seleccionado local unidad elegir una unidad local o si has seleccionado remoto compartir elegir un recurso compartido de red.
9. En la pantalla de confirmación, haz clic en **copia de seguridad**.
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)
10. Una vez que se haya completado, haga clic en **cerrar**.
11. Cierra la copia de seguridad de Windows Server.

>[!NOTE]
>Si recibes un error que indica que ninguna ubicación de almacenamiento está disponible, debes excluir uno de los volúmenes que se ha seleccionado o agregar un nuevo volumen o un recurso compartido remoto.
>Si recibes una advertencia que indica que el volumen seleccionado también se incluye en la lista de elementos para realizar una copia, determinar si quitar y haz clic o no **Aceptar**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Usar Wbadmin.exe para hacer copia de un servidor de windows
Wbadmin.exe es una utilidad de línea de comandos que te permite hacer copia de seguridad y restaurar el sistema operativo, volúmenes, archivos, carpetas y aplicaciones desde un símbolo del sistema.

#### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Para realizar una copia de seguridad completa del servidor mediante Wbadmin.exe  
  
1.  Abre un símbolo del sistema con privilegios elevados, escribe el siguiente comando y presiona ENTRAR:  

        wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:

![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
