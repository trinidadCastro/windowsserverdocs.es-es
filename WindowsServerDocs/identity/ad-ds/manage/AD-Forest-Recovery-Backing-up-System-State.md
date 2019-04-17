---
title: "Recuperación de bosque de AD - copia de seguridad completa del servidor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.technology: identity-adfs
ms.openlocfilehash: a86d61536f8b426e1a5258c661d4e53da63d4162
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Recuperación de bosque de AD - copia los datos de estado del sistema  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
Usa el siguiente procedimiento para realizar una copia de seguridad de estado del sistema en un controlador de dominio mediante la copia de seguridad de Windows Server o wbadmin.exe.  
  
## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Para realizar una copia de seguridad del estado de sistema con copias de seguridad de Windows Server  
1. Abre **administrador del servidor**, haz clic en **herramientas**y, a continuación, haz clic en **copias de seguridad de Windows Server**.
    - En Windows Server 2008 R2 y Windows Server 2008, haz clic en **inicio**, elija **herramientas administrativas**y, a continuación, haz clic en **copias de seguridad de Windows Server**. 
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png) 
2. Si se le pide, en la **Control de cuentas de usuario** cuadro de diálogo, proporciona las credenciales de operador de copia de seguridad y, a continuación, haz clic en **Aceptar**.
3. Haz clic en **copia de seguridad Local**.
4. En la **acción** menú, haz clic en **una copia de seguridad una vez**.
5. En la copia de seguridad Asistente para una vez, en la **opciones de copia de seguridad** página, haz clic en **diferentes opciones**y, a continuación, haz clic en **siguiente**.
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)
6. En la **selecciona configuración copia de seguridad** página, haz clic en **personalizada)**y, a continuación, haz clic en **siguiente**.
7. En la **seleccionar elementos de copia de seguridad** de pantalla, haz clic en **agregar elementos** y selecciona **estado del sistema** y haz clic en **Aceptar**.
    - En Windows Server 2008 R2 y Windows Server 2008, selecciona los volúmenes que se incluirán en la copia de seguridad. Si seleccionas la **habilitar la recuperación del sistema** casilla, se seleccionan todos los volúmenes críticos. 
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)  
8. En la **especificar el tipo de destino** página, haz clic en **unidades locales** o **remoto carpeta compartida**y, a continuación, haz clic en **siguiente**.  Si se copia en una carpeta compartida remota, haz lo siguiente:  
  
 1.  Escribe la ruta de acceso a la carpeta compartida.  
 2.  En **el Control de acceso**, selecciona **no heredan** o **heredar** para determinar el acceso a la copia de seguridad y, a continuación, haz clic en **siguiente**.  
 3.  En la **proporcionar credenciales de usuario para la copia de seguridad** cuadro de diálogo, proporciona el nombre de usuario y contraseña para un usuario que tiene acceso de escritura a la carpeta compartida y, a continuación, haz clic en **Aceptar**.
9. Para Windows Server 2008 R2 y Windows Server 2008, en la **especificar opción avanzada** página, seleccione **copia de seguridad de copia** y, a continuación, haz clic en **siguiente**.
10. En la **seleccionar un destino de copia de seguridad** página, elige la ubicación de copia de seguridad.  Si has seleccionado local unidad elegir una unidad local o si has seleccionado remoto compartir elegir un recurso compartido de red.
11. En la pantalla de confirmación, haz clic en **copia de seguridad**.
12. Una vez que se haya completado, haga clic en **cerrar**.
13. Cierra la copia de seguridad de Windows Server.

  
## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Para realizar una copia de seguridad con Wbadmin.exe  
  
1.  Abre un símbolo del sistema con privilegios elevados, escribe el siguiente comando y presiona ENTRAR:  
  
    ```  
    wbadmin start systemstatebackup -backuptarget:<targetDrive>:
    ```  
![Instalar la copia de seguridad](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
