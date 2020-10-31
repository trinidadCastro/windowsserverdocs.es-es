---
title: 'Recuperación de bosque de AD: copia de seguridad de un servidor completo'
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.openlocfilehash: 93771bcac84680f07a39ce2791b67c2b35aff8dc
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067967"
---
# <a name="ad-forest-recovery---backing-up-a-full-server"></a>Recuperación de bosque de AD: copia de seguridad de un servidor completo

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Se recomienda una copia de seguridad completa del servidor para preparar la recuperación de un bosque, ya que se puede restaurar en otro hardware o en una instancia de sistema operativo diferente.  Con Copias de seguridad de Windows Server puede realizar una copia de seguridad completa del servidor.

## <a name="windows-server-backup"></a>Copias de seguridad de Windows Server

Copias de seguridad de Windows Server no se instala de forma predeterminada. En Windows Server 2016 y Windows Server 2012 R2, instálelo siguiendo los pasos que se indican a continuación.

>[!NOTE]
>Tenga en cuenta que los pasos pueden variar ligeramente entre Windows Server 2016 y Windows Server 2012 R2.

Para conocer los pasos para instalarlo en Windows Server 2008 y Windows Server 2008 R2, consulte [instalación de copias de seguridad de Windows Server](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771232(v=ws.10)).

### <a name="to-install-windows-server-backup"></a>Para instalar Copias de seguridad de Windows Server

1. Abra **Administrador del servidor** y haga clic en **Agregar roles y características** .
2. En el **Asistente para agregar roles y características,** haga clic en **siguiente** .
3. En la pantalla **tipo de instalación** , deje la instalación predeterminada basada en **características o en roles** y haga clic en **siguiente** .
4. En la pantalla **selección de servidor** , haga clic en **siguiente** .
5. En la pantalla **roles de servidor** , haga clic en **siguiente** .
6. En la pantalla **características** , seleccione **copias de seguridad de Windows Server** y haga clic en **siguiente** 
    ![ instalar copia de seguridad.](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup2.png)
7. Haga clic en **Instalar** .
8. Una vez completada la instalación, haga clic en **cerrar** .

### <a name="to-perform-a-backup-with-windows-server-backup"></a>Para realizar una copia de seguridad con Copias de seguridad de Windows Server

1. Abra **Administrador del servidor** , haga clic en **herramientas** y, a continuación, haga clic en **copias de seguridad de Windows Server** .
   - En Windows Server 2008 R2 y Windows Server 2008, haga clic en **Inicio** , seleccione **herramientas administrativas** y, a continuación, haga clic en **copias de seguridad de Windows Server** .

   ![Instalar copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup1.png)

2. Si se le solicita, en el cuadro de diálogo **control de cuentas de usuario** , proporcione credenciales de operador de copia de seguridad y, a continuación, haga clic en **Aceptar** .
3. Haga clic en **copia de seguridad local** .
4. En el menú **Acción** , haga clic en **Hacer copia de seguridad una vez** .
5. En el Asistente para hacer copia de seguridad una vez, en la página **Opciones de copia de seguridad** , haga clic en **opciones diferentes** y, a continuación, en **siguiente** .

   ![Instalar copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup3.png)

6. En la página **seleccionar configuración de copia de seguridad** , haga clic en **servidor completo (recomendado)** y, a continuación, haga clic en **siguiente** .
7. En la página **especificar tipo de destino** , haga clic en **unidades locales** o en **carpeta compartida remota** y, a continuación, haga clic en **siguiente** .
8. En la página **Seleccionar destino** de la copia de seguridad, elija la ubicación de copia de seguridad.  Si seleccionó unidad local, elija una unidad local o, si seleccionó recurso compartido remoto, elija un recurso compartido de red.
9. En la pantalla confirmación, haga clic en **copia de seguridad** .

   ![Instalar copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup4.png)

10. Una vez que se haya completado, haga clic en **cerrar** .
11. Cierre Copias de seguridad de Windows Server.

>[!NOTE]
>Si recibe un error que indica que no hay ninguna ubicación de almacenamiento de copia de seguridad disponible, tendrá que excluir uno de los volúmenes seleccionados o agregar un nuevo volumen o recurso compartido remoto.
>Si recibe una advertencia que indica que el volumen seleccionado también se incluye en la lista de elementos de la copia de seguridad, determine si desea quitarlo y haga clic en **Aceptar** .

## <a name="using-wbadminexe-to-backup-a-windows-server"></a>Uso de Wbadmin.exe para hacer una copia de seguridad de Windows Server

Wbadmin.exe es una utilidad de línea de comandos que permite realizar copias de seguridad y restaurar el sistema operativo, los volúmenes, los archivos, las carpetas y las aplicaciones desde el símbolo del sistema.

### <a name="to-perform-a-full-server-backup-using-wbadminexe"></a>Para realizar una copia de seguridad completa del servidor mediante Wbadmin.exe

- Abra un símbolo del sistema con privilegios elevados, escriba el siguiente comando y presione ENTRAR:

   ```
   wbadmin start backup -backuptarget:<Drive_letter_to store_backup>: -include:<Drive_letter_to_include>:
   ```

   ![Instalar copia de seguridad](media/AD-Forest-Recovery-Backing-up-a-Full-Server/fullbackup5.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
