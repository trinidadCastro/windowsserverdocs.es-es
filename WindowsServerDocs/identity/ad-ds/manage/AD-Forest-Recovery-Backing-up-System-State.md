---
title: Recuperación de bosques de AD - copia de seguridad completa del servidor
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adds
ms.openlocfilehash: a5306960bb2dca3849bdb4fc7304781af3f25335
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815876"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Recuperación de bosques de AD - copia los datos de estado del sistema  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para realizar una copia de seguridad del estado del sistema en un controlador de dominio mediante la copia de seguridad de Windows Server o wbadmin.exe.  

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Para realizar una copia de seguridad del estado del sistema mediante copias de seguridad de Windows Server

1. Abra **administrador del servidor**, haga clic en **herramientas**y, a continuación, haga clic en **copias de seguridad de Windows Server**.
   - En Windows Server 2008 R2 y Windows Server 2008, haga clic en **iniciar**, apunte a **herramientas administrativas**y, a continuación, haga clic en **copias de seguridad de Windows Server**. 

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Si se le pide, en el **User Account Control** cuadro de diálogo, proporcione credenciales de operador de copia de seguridad y, a continuación, haga clic en **Aceptar**.
3. Haga clic en **copia de seguridad Local**.
4. En el menú **Acción**, haga clic en **Hacer copia de seguridad una vez**.
5. En la copia de seguridad una vez asistente, en el **opciones de copia de seguridad** página, haga clic en **diferentes opciones**y, a continuación, haga clic en **siguiente**.

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. En el **seleccione Configuración de copia de seguridad** página, haga clic en **personalizado)** y, a continuación, haga clic en **siguiente**.
7. En el **seleccionar elementos para copia de seguridad** pantalla, haga clic en **agregar elementos** y seleccione **del estado del sistema** y haga clic en **Aceptar**.
   - En Windows Server 2008 R2 y Windows Server 2008, seleccione los volúmenes que se va a incluir en la copia de seguridad. Si selecciona el **habilita la recuperación del sistema** casilla de verificación, se seleccionan todos los volúmenes críticos. 

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  

8. En el **especificar tipo de destino** página, haga clic en **unidades locales** o **carpeta compartida remota**y, a continuación, haga clic en **siguiente**.  Si la copia de seguridad en una carpeta compartida remota, realice lo siguiente:  
   - Escriba la ruta de acceso a la carpeta compartida.
   - En **Control de acceso**, seleccione **no heredan** o **heredar** para determinar el acceso a la copia de seguridad y, a continuación, haga clic en **siguiente**.  
   - En el **proporcionar credenciales de usuario de copia de seguridad** cuadro de diálogo, proporcione el nombre de usuario y la contraseña para un usuario que tenga acceso de escritura a la carpeta compartida y, a continuación, haga clic en **Aceptar**.

9. Para Windows Server 2008 R2 y Windows Server 2008, en la **especificar opciones avanzadas** página, seleccione **copia de seguridad VSS** y, a continuación, haga clic en **siguiente**.
10. En el **Seleccionar destino de copia de seguridad** página, elija la ubicación de copia de seguridad.  Si ha seleccionado local unidad elija una unidad local o si ha seleccionado remoto compartido elija un recurso compartido de red.
11. En la pantalla de confirmación, haga clic en **copia de seguridad**.
12. Una vez que se haya completado, haga clic en **cerrar**.
13. Cierre la copia de seguridad de Windows Server.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Para realizar una copia de seguridad del estado del sistema con Wbadmin.exe

Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:  
  
   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Instale copias de seguridad](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
