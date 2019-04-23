---
title: Recuperación de bosques de AD - copia de seguridad completa del servidor
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: fec8de8ea1dadb392f6a3bd1c881e8df2266f404
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846526"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Recuperación de bosques de AD - copia de seguridad completa del servidor  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Para prepararse para una recuperación de bosques porque se puede restaurar a un hardware diferente o una instancia de sistema operativo diferente, se recomienda una copia de seguridad completa del servidor.  Uso de copias de seguridad de Windows Server puede realizar una copia de seguridad completa del servidor. 

## <a name="windows-server-backup"></a>Copias de seguridad de Windows Server

Copia de seguridad de Windows Server no está instalado de forma predeterminada. En Windows Server 2016 y Windows Server 2012 R2, debe instalarlo siguiendo los pasos siguientes.

>[!NOTE]
>Ten en cuenta que los pasos pueden variar ligeramente entre Windows Server 2016 y Windows Server 2012 R2.

Para que conocer los pasos instalarlo en Windows Server 2008 y Windows Server 2008 R2, consulte [instalar Windows Server Backup](https://technet.microsoft.com/library/cc771232.aspx).  

### <a name="to-install-windows-server-backup"></a>Para instalar copias de seguridad de Windows Server

1. Abra **administrador del servidor** y haga clic en **agregar roles y características**.
2. En el **agregar Roles y características Asistente** haga clic en **siguiente**.
3. En el **tipo de instalación** pantalla, deje el valor predeterminado **instalación basada en roles o basada en características** y haga clic en **siguiente**.
4. En el **selección de servidor** pantalla, haga clic en **siguiente**.
5. En el **Roles de servidor** pantalla clic **siguiente**.
6. En el **características** pantalla, seleccione **copias de seguridad de Windows Server** y haga clic en **siguiente**
   ![instalar copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Haga clic en **Instalar**.
8. Una vez completada la instalación, haga clic en **cerrar**.

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Para realizar una copia de seguridad con copias de seguridad de Windows Server

1. Abra **administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **copias de seguridad de Windows Server**.
   - En Windows Server 2008 R2 y Windows Server 2008, haga clic en **iniciar**, apunte a **herramientas administrativas**y, a continuación, haga clic en **copias de seguridad de Windows Server**.

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 

2. Si se le pide, en el **User Account Control** cuadro de diálogo, proporcione credenciales de operador de copia de seguridad y, a continuación, haga clic en **Aceptar**.
3. Haga clic en **copia de seguridad Local**.
4. En el menú **Acción**, haga clic en **Hacer copia de seguridad una vez**.
5. En la copia de seguridad una vez asistente, en el **opciones de copia de seguridad** página, haga clic en **diferentes opciones**y, a continuación, haga clic en **siguiente**.

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. En el **seleccione Configuración de copia de seguridad** página, haga clic en **servidor completo (recomendado)** y, a continuación, haga clic en **siguiente**.
7. En el **especificar tipo de destino** página, haga clic en **unidades locales** o **carpeta compartida remota**y, a continuación, haga clic en **siguiente**.
8. En el **Seleccionar destino de copia de seguridad** página, elija la ubicación de copia de seguridad.  Si ha seleccionado local unidad elija una unidad local o si ha seleccionado remoto compartido elija un recurso compartido de red.
9. En la pantalla de confirmación, haga clic en **copia de seguridad**.

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Una vez que se haya completado, haga clic en **cerrar**.
11. Cierre la copia de seguridad de Windows Server.

>[!NOTE]
>Si recibe un error que indica que no está disponible ninguna ubicación de almacenamiento de copia de seguridad, deberá excluir uno de los volúmenes que se ha seleccionado o agregar un nuevo volumen o recurso compartido remoto.
>Si recibe una advertencia que indica que el volumen seleccionado también se incluye en la lista de elementos para copia de seguridad, determinar si se deben quitar y haga clic en **Aceptar**.

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Uso de Wbadmin.exe para copia de seguridad de windows server

Wbadmin.exe es una utilidad de línea de comandos que permite realizar copias de seguridad y restaurar el sistema operativo, volúmenes, archivos, carpetas y las aplicaciones desde un símbolo del sistema.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Para realizar una copia de seguridad completa con Wbadmin.exe
  
- Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:  

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
