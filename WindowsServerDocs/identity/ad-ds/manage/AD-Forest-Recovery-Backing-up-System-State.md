---
description: 'Más información acerca de la recuperación del bosque de AD: copia de seguridad de los datos de estado del sistema'
title: 'Recuperación de bosque de AD: copia de seguridad de los datos de estado del sistema'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 9238cb27-0020-42f7-90d6-fcebf7e3c0bc
ms.openlocfilehash: b5dccb17f9cfa1ca9a6a09824d12659abb28707b
ms.sourcegitcommit: e57536e28902ae52d3040141bbd2aa00e91bbdd3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97644675"
---
# <a name="ad-forest-recovery---backing-up-the-system-state-data"></a>Recuperación de bosque de AD: copia de seguridad de los datos de estado del sistema

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para realizar una copia de seguridad del estado del sistema en un controlador de dominio mediante Copias de seguridad de Windows Server o wbadmin.exe.

## <a name="to-perform-a-system-state-backup-using-windows-server-backup"></a>Para realizar una copia de seguridad del estado del sistema mediante Copias de seguridad de Windows Server

1. Abra **Administrador del servidor**, haga clic en **herramientas** y, a continuación, haga clic en **copias de seguridad de Windows Server**.
   - En Windows Server 2008 R2 y Windows Server 2008, haga clic en **Inicio**, seleccione **herramientas administrativas** y, a continuación, haga clic en **copias de seguridad de Windows Server**.

   ![Captura de pantalla que resalta la opción de menú Copias de seguridad de Windows Server.](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Si se le solicita, en el cuadro de diálogo **control de cuentas de usuario** , proporcione credenciales de operador de copia de seguridad y, a continuación, haga clic en **Aceptar**.
3. Haga clic en **copia de seguridad local**.
4. En el menú **Acción**, haga clic en **Hacer copia de seguridad una vez**.
5. En el Asistente para hacer copia de seguridad una vez, en la página **Opciones de copia de seguridad** , haga clic en **opciones diferentes** y, a continuación, en **siguiente**.

   ![Captura de pantalla que muestra la página Opciones de copia de seguridad con la opción opciones diferentes seleccionada.](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. En la página **seleccionar configuración de copia de seguridad** , haga clic en **personalizado)** y, a continuación, haga clic en **siguiente**.
7. En la pantalla **seleccionar elementos para copia de seguridad** , haga clic en **Agregar elementos** , seleccione **Estado del sistema** y haga clic en **Aceptar**.
   - En Windows Server 2008 R2 y Windows Server 2008, seleccione los volúmenes que se van a incluir en la copia de seguridad. Si activa la casilla **Habilitar la recuperación del sistema** , se seleccionan todos los volúmenes críticos.

   ![Captura de pantalla que muestra la casilla estado del sistema activada. ](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup.png)

8. En la página **especificar tipo de destino** , haga clic en **unidades locales** o en **carpeta compartida remota** y, a continuación, haga clic en **siguiente**.  Si va a realizar una copia de seguridad en una carpeta compartida remota, haga lo siguiente:
   - Escriba la ruta de acceso a la carpeta compartida.
   - En **Access Control**, seleccione no **heredar** o **heredar** para determinar el acceso a la copia de seguridad y, a continuación, haga clic en **siguiente**.
   - En el cuadro de diálogo **proporcionar credenciales de usuario para copia de seguridad** , proporcione el nombre de usuario y la contraseña de un usuario que tenga acceso de escritura a la carpeta compartida y, a continuación, haga clic en **Aceptar**.

9. En Windows Server 2008 R2 y Windows Server 2008, en la página **especificar opciones avanzadas** , seleccione **VSS copiar copia de seguridad** y, a continuación, haga clic en **siguiente**.
10. En la página **Seleccionar destino** de la copia de seguridad, elija la ubicación de copia de seguridad.  Si seleccionó unidad local, elija una unidad local o, si seleccionó recurso compartido remoto, elija un recurso compartido de red.
11. En la pantalla confirmación, haga clic en **copia de seguridad**.
12. Una vez que se haya completado, haga clic en **cerrar**.
13. Cierre Copias de seguridad de Windows Server.

## <a name="to-perform-a-system-state-backup-using-wbadminexe"></a>Para realizar una copia de seguridad del estado del sistema mediante Wbadmin.exe

Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:

   ```
   wbadmin start systemstatebackup -backuptarget:<targetDrive>:
   ```

   ![Instalar copia de seguridad](media/AD-Forest-Recovery-Backing-up-System-State/systemstatebackup2.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
