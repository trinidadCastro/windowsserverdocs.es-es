---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: Introduction to Active Directory Administrative Center Enhancements (Level 100)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d16447425b86fef979732c4785db27d1542116b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840176"
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduction to Active Directory Administrative Center Enhancements (Level 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El centro de administración de Active Directory en Windows Server incluye características de administración para lo siguiente:

- [Papelera de reciclaje de Active Directory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)
- [Directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)
- [Visor del historial de PowerShell de Windows](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Papelera de reciclaje de Active Directory

La eliminación accidental de objetos de Active Directory es algo habitual entre los usuarios de los Servicios de dominio de Active Directory (AD DS) y Active Directory Lightweight Directory Services (AD LDS). En las versiones anteriores de Windows Server, antes de Windows Server 2008 R2, podían recuperarse los objetos eliminados accidentalmente en Active Directory, pero las soluciones tenían sus desventajas.

En Windows Server 2008, podía usar la característica Copias de seguridad de Windows Server y el comando de restauración autoritativa de **ntdsutil** para marcar objetos como autoritativos para garantizar que los datos restablecidos se replicaban a través del dominio. El inconveniente de la solución de restauración autoritativa era que debía realizarse en modo de restauración de servicios de directorio (DSRM). Durante DSRM, el controlador de dominio que se restauraba debía permanecer sin conexión. Por consiguiente, era incapaz de atender las solicitudes de los clientes.

En Windows Server 2003 Active Directory y Windows Server 2008 AD DS, podía recuperar los objetos de Active Directory eliminados a través de la reanimación de marcadores de exclusión. No obstante, no se recuperaban ni los atributos de valores vinculados de los objetos reanimados (por ejemplo, la pertenencia a grupos de las cuentas de usuario) que se quitaron físicamente, ni los atributos de valores no vinculados que se borraron. Por lo tanto, los administradores no podían confiar en la reanimación de marcadores de exclusión como solución final ante la eliminación accidental de objetos. Para obtener más información sobre la reanimación de marcadores de exclusión, vea [Reanimación de objetos de desecho de Active Directory](https://go.microsoft.com/fwlink/?LinkID=125452).

La papelera de reciclaje de Active Directory, desde Windows Server 2008 R2, aprovecha la infraestructura de reanimación de marcadores de exclusión existente y mejora su capacidad para preservar y recuperar los objetos de Active Directory eliminados accidentalmente.

Cuando se habilita la papelera de reciclaje de Active Directory, se conservan todos los atributos de valores vinculados y valores no vinculados de los objetos de Active Directory eliminados y, asimismo, se restauran los objetos completamente al mismo estado lógico y coherente en el que se encontraban antes de la eliminación. Por ejemplo, las cuentas de usuario restauradas recuperan automáticamente todas las pertenencias a grupos y los derechos de acceso correspondientes que tenían justo antes de la eliminación, tanto dentro de los dominios como entre ellos. La papelera de reciclaje de Active Directory funciona en entornos tanto AD DS como AD LDS. Para obtener una descripción detallada de la Papelera de reciclaje de Active Directory, consulte [Novedades en AD DS: Papelera de reciclaje de Active Directory](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**Novedades** En Windows Server 2012 o versiones posteriores, la característica Papelera de reciclaje de Active Directory se ha mejorado con una nueva interfaz gráfica de usuario para los usuarios administrar y restaurar objetos eliminados. Los usuarios ahora pueden ubicar visualmente una lista de objetos eliminados y restaurarlos a sus ubicaciones originales y deseadas.

Si va a habilitar la Papelera de Active Directory en Windows Server, tenga en cuenta lo siguiente:

- De manera predeterminada, la papelera de reciclaje de Active Directory está deshabilitada. Para habilitarla, primero debe elevar el nivel funcional del bosque de su entorno de AD DS o AD LDS en Windows Server 2008 R2 o superior. A su vez, esto requiere que todos los controladores de dominio del bosque o todos los servidores que hospedan las instancias de conjuntos de configuración de AD LDS estén ejecutando Windows Server 2008 R2 o posterior.
- El proceso de habilitar la papelera de reciclaje de Active Directory es irreversible. Una vez habilitada la papelera de reciclaje de Active Directory en el entorno, no se puede deshabilitar.
- Para administrar la característica de Papelera de reciclaje a través de una interfaz de usuario, debe instalar la versión del centro de administración de Active Directory en Windows Server 2012.

    > [!NOTE]
    > Puede usar **administrador del servidor** para instalar las herramientas de administración de servidor remoto (RSAT) para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.
    >
    > Para obtener información acerca de cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="active-directory-recycle-bin-step-by-step"></a>Procedimiento paso a paso de la papelera de reciclaje de Active Directory

En los pasos siguientes, usará el ADAC para realizar las siguientes tareas de la Papelera de reciclaje de Active Directory en Windows Server 2012:

- [Paso 1: Elevar el nivel funcional de bosque](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)
- [Paso 2: Habilitar la Papelera de reciclaje](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)
- [Paso 3: Crear unidad organizativa, grupo y usuarios de prueba](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)
- [Paso 4: Restaurar objetos eliminados](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> Se requiere membresía del grupo Administradores de empresa o permisos equivalentes para realizar los siguientes pasos.

### <a name="bkmk_raise_ffl"></a>Paso 1: Elevar el nivel funcional del bosque

En este paso, elevará el nivel funcional del bosque. Primero debe elevar el nivel funcional del bosque de destino sea Windows Server 2008 R2 como mínimo antes de habilitar la Papelera de reciclaje de Active Directory.

#### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Para elevar el nivel funcional en el bosque de destino

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. Haga clic en el dominio de destino en el panel de navegación izquierdo, y en el panel de **tareas**, haga clic en **Elevar el nivel funcional del bosque**. Seleccione un nivel funcional del bosque que tiene al menos Windows Server 2008 R2 o posterior y, a continuación, haga clic en **Aceptar**.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Para el **-identidad** argumento, especifique el nombre de dominio DNS completo.

### <a name="bkmk_enable_recycle_bin"></a>Paso 2: Enable Recycle Bin

En este paso, permitirá que la papelera de reciclaje restaure los objetos eliminados en AD DS.

#### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Para habilitar la papelera de reciclaje de Active Directory en ADAC en el dominio de destino

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. En el panel **Tareas**, haga clic en **Habilitar papelera de reciclaje...** en el panel **Tareas**, haga clic en **Aceptar** en el cuadro de mensaje de advertencia y después en **Aceptar** para actualizar el mensaje de ADAC.

4. Presione F5 para actualizar ADAC.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Paso 3: Crear unidad organizativa, grupo y usuarios de prueba

En los siguientes procedimientos, creará dos usuarios de prueba. Después creará un grupo de prueba y le agregará usuarios. Además, creará un OU.

#### <a name="to-create-test-users"></a>Para crear usuarios de prueba

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. En el panel de **tareas** , haga clic en **Nuevo** y, a continuación, en **Usuario**.

    ![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4. Escriba la siguiente información en **Cuenta** y haga clic en Aceptar:

   - Nombre completo: test1
   - Inicio de sesión SamAccountName de usuario: test1
   - Contraseña: p@ssword1
   - Confirmar contraseña: p@ssword1

5. Repita los pasos anteriores para crear un segundo usuario, test2.

#### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Para crear un grupo de prueba y agregarle usuarios

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.
2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.
3. En el panel de **tareas**, haga clic en **Nuevo** y, a continuación, haga clic en **Grupo**.
4. Escriba la siguiente información en **Grupo** y haga clic en **Aceptar**:

    -   **Nombre de grupo: grupo1**

5. Haga clic en **group1**y después en el panel **Tareas** , haga clic en **Propiedades**.
6. Haga clic en **Miembros**, en **Agregar**, escriba **test1;test2**y, a continuación, haga clic en **Aceptar**.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Add-ADGroupMember -Identity group1 -Member test1
```

#### <a name="to-create-an-organizational-unit"></a>Para crear una unidad organizativa

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.
2. Haga clic en **administrar**, haga clic en **agregar nodos de navegación** y seleccione el dominio de destino apropiado en el **agregar nodos de navegación** cuadro de diálogo y, a continuación, haga clic en ** Aceptar
3. En el panel de **tareas** , haga clic en **Nuevo** y, a continuación, haga clic en **Unidad organizativa**.
4. Escriba la siguiente información en **Unidad organizativa** y haga clic en **Aceptar**:

   - **NameOU1**

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>Paso 4: Restaurar objetos eliminados

En los siguientes procedimientos, restaurará los objetos eliminados del contenedor **Objetos eliminados** a su ubicación original o a una diferente.

#### <a name="to-restore-deleted-objects-to-their-original-location"></a>Para restaurar los objetos eliminados a su ubicación original

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. Seleccione los usuarios **test1** y **test2**, haga clic en **Eliminar** en el panel **Tareas** y después haga clic en **Sí** para confirmar la eliminación.

    ![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

    ```powershell
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4. Navegue al contenedor **Objetos eliminados** , seleccione **test2** y **test1** , y después haga clic en **Restaurar** en el panel de **tareas** .

5. Para confirmar que los objetos se restauraron a su ubicación original, navegue al dominio de destino y compruebe que se enumeren las cuentas de usuario.

    > [!NOTE]
    > Si navega a las **propiedades** de las cuentas de usuario **test1** y **test2**, y después hace clic en **Miembro de**, verá que su membresía del grupo también se restauró.

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

#### <a name="to-restore-deleted-objects-to-a-different-location"></a>Para restaurar objetos eliminados a una ubicación diferente

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. Seleccione los usuarios **test1** y **test2**, haga clic en **Eliminar** en el panel **Tareas** y después haga clic en **Sí** para confirmar la eliminación.

4. Navegue al contenedor **Objetos eliminados** , seleccione **test2** y **test1** , y después haga clic en **Restaurar** en el panel de **tareas** .

5. Seleccione **OU1** y haga clic en **Aceptar**.

6. Para confirmar que los objetos se restauraron a **OU1**, navegue al dominio de destino, haga doble clic en **OU1** y compruebe que las cuentas de usuario estén enumeradas.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Directiva de contraseña específica

El sistema operativo Windows Server 2008 proporciona a las organizaciones un modo de definir diferentes directivas de bloqueo de cuentas y contraseñas para diversos conjuntos de usuarios del dominio. En los dominios de Active Directory anteriores a Windows Server 2008, solamente una directiva de bloqueo de cuenta o directiva de contraseña puede aplicarse a todos los usuarios en el dominio. Estas directivas se especificaron en la directiva de dominio predeterminado para el dominio. Como resultado, las organizaciones que querían diferentes configuraciones de bloqueo de cuentas y contraseñas para diferentes conjuntos de usuarios tenían que crear un filtro de contraseñas o implementar múltiples dominios. Ambas son opciones costosas.

Puede usar directivas de contraseña específica para especificar múltiples directivas de contraseña dentro de un único dominio y aplicar diferentes restricciones para directivas de bloqueo de cuentas y contraseñas a diferentes conjuntos de usuarios de un dominio. Por ejemplo, puede aplicar configuraciones más estrictas a cuentas privilegiadas y configuraciones menos estrictas a las cuentas de otros usuarios. En otros casos, probablemente quieras aplicar una directiva de contraseñas especiales para las cuentas cuyas contraseñas están sincronizadas con otros orígenes de datos. Para obtener una descripción detallada de la directiva de contraseña específica, consulte [AD DS: Directivas de contraseña específica](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**Novedades**

En Windows Server 2012 y la contraseña más reciente, específica de administración de directivas se realiza más fácil y más visual proporcionando una interfaz de usuario para los administradores de AD DS las administren en el ADAC. Los administradores ahora pueden ver la directiva resultante de un usuario determinado, ver y ordenar todas las directivas de contraseña dentro de un dominio determinado y administrar directivas de contraseña individuales visualmente.

Si planea usar directivas de contraseña específica en Windows Server 2012, tenga en cuenta lo siguiente:

- Las directivas de contraseña específica se aplican solamente a objetos de usuario y grupos de seguridad globales (u objetos inetOrgPerson si se usan en lugar de los objetos de usuario). De manera predeterminada, solamente los miembros del grupo Administradores de dominio pueden establecer directivas de contraseña específica. No obstante, también puede delegar la capacidad de establecer estas directivas a otros usuarios. El nivel funcional del dominio debe ser Windows Server 2008 o posterior.

- Debe usar el Windows Server 2012 o una versión más reciente del centro de administración de Active Directory para administrar directivas de contraseña específica a través de una interfaz gráfica de usuario.

    > [!NOTE]
    > Puede usar **administrador del servidor** para instalar las herramientas de administración de servidor remoto (RSAT) para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.
    >
    > Para obtener información acerca de cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="fine-grained-password-policy-step-by-step"></a>Procedimiento paso a paso para las directivas de contraseña específica

En los pasos siguientes, usará el ADAC para realizar las siguientes tareas de directiva de contraseña específica:

- [Paso 1: Elevar el nivel funcional de dominio](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)
- [Paso 2: Crear unidad organizativa, grupo y usuarios de prueba](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)
- [Paso 3: Crear una nueva directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)
- [Paso 4: Ver un conjunto resultante de directivas para un usuario](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)
- [Paso 5: Editar una directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)
- [Paso 6: Eliminar una directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> Para realizar los siguientes pasos, es necesario pertenecer al grupo Administradores de dominio o tener permisos equivalentes.

#### <a name="bkmk_raise_dfl"></a>Paso 1: Elevar el nivel funcional del dominio

En el siguiente procedimiento, elevará el nivel funcional del dominio del dominio de destino en Windows Server 2008 o superior. Se requiere un nivel funcional del dominio de Windows Server 2008 o posterior para habilitar las directivas de contraseña específica.

##### <a name="to-raise-the-domain-functional-level"></a>Para elevar el nivel funcional del dominio

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. Haga clic en el dominio de destino en el panel de navegación izquierdo, y en el panel  **Tareas**, haga clic en **Elevar el nivel funcional del dominio**. Seleccione un nivel funcional del bosque que tiene al menos Windows Server 2008 o posterior y, a continuación, haga clic en **Aceptar**.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Paso 2: Crear unidad organizativa, grupo y usuarios de prueba

Para crear los usuarios de prueba y grupo necesarios para este paso, siga estos procedimientos: [Paso 3: Crear unidad organizativa, grupo y usuarios de prueba](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) (no es necesario crear la unidad organizativa para demostrar la directiva de contraseña específica).

#### <a name="bkmk_create_fgpp"></a>Paso 3: Crear una directiva de contraseña específica nueva

En el siguiente procedimiento, creará una directiva de contraseña específica nueva usando la UI en el ADAC.

##### <a name="to-create-a-new-fine-grained-password-policy"></a>Para crear una directiva de contraseña específica nueva

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. En el panel de navegación de ADAC, abra el contenedor **System** y, a continuación, haga clic en **Password Settings Container**.

4. En el panel **Tareas** , haga clic en **Nuevo**y, a continuación, haga clic en **Configuración de contraseña**.

    Complete o edite los campos dentro de la página de propiedades para crear un nuevo objeto de **Configuración de contraseña**. Los campos **Nombre** y **Precedencia** son necesarios.

    ![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5. En **Se aplica directamente a**, en **Agregar**, escriba **grupo1**y, a continuación, haga clic en **Aceptar**.

    Así se asocia el objeto de directiva de contraseña con los miembros del grupo global que creó para el entorno de prueba.

6. Haga clic en **Aceptar** para enviar la creación.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Paso 4: Ver un conjunto de directivas resultante para un usuario

En el siguiente procedimiento, verá la configuración de contraseña resultante para un usuario que sea miembro del grupo al que se le asignó una directiva de contraseña específica en [paso 3: Crear una nueva directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

##### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Para ver un conjunto de directivas resultante para un usuario

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. Seleccione un usuario, **test1** que pertenezca al grupo, **group1** que asoció una directiva de contraseña específica en [paso 3: Crear una nueva directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

4. Haga clic en **Ver configuración de contraseña resultante** en el panel **Tareas**.

5. Examine la directiva de configuración de contraseñas y haga clic en **Cancelar**.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Paso 5: Editar una directiva de contraseña específica

En el siguiente procedimiento, editará la directiva de contraseña específica creada en [paso 3: Crear una nueva directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

##### <a name="to-edit-a-fine-grained-password-policy"></a>Para editar una directiva de contraseña específica

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. En el **panel de navegación** del ADAC, expanda **Sistema** y haga clic en **Contenedor de configuraciones de contraseña**.

4. Seleccione la directiva de contraseña específica que creó en [paso 3: Crear una nueva directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) y haga clic en **propiedades** en el **tareas** panel.

5. En **Exigir historial de contraseñas**, cambie el valor de **Número de contraseñas recordadas** a **30**.

6. Haga clic en **Aceptar**.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Paso 6: Eliminar una directiva de contraseña específica

##### <a name="to-delete-a-fine-grained-password-policy"></a>Para eliminar una directiva de contraseña específica

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. En el panel de navegación del ADAC, expanda **Sistema** y haga clic en **Contenedor de configuraciones de contraseña**.

4. Seleccione la directiva de contraseña específica que creó en [paso 3: Crear una nueva directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) y en el **tareas** panel, haga clic **propiedades**.

5. Desactive la casilla **Proteger contra eliminación accidental** y haga clic en **Aceptar**.

6. Seleccione la directiva de contraseña específica y, en el panel de **tarea** , haga clic en **Eliminar**.

7. Haga clic en **Aceptar** en el cuadro de diálogo de confirmación.

![Introducción al centro de administración de AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.

```powershell
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Visor del historial de PowerShell de Windows

El ADAC es una herramienta de interfaz de usuario creada sobre Windows PowerShell. En Windows Server 2012 y versiones más recientes, los administradores de TI pueden aprovechar el ADAC para aprender a usar Windows PowerShell para los cmdlets de Active Directory utilizando el Visor del historial de Windows PowerShell. A medida que las acciones se ejecutan en la interfaz de usuario, el comando de Windows PowerShell equivalente se muestra al usuario en el Visor del historial de Windows PowerShell. Esto permite que los administradores creen scripts automatizados y reduzcan las tareas repetitivas, y así aumente la productividad de TI. Además, esta característica reduce el tiempo para obtener información sobre Windows PowerShell para Active Directory y aumenta la confianza del usuario en la presión de sus scripts de automatización.

Cuando se usa el Visor del historial de Windows PowerShell en Windows Server 2012 o versiones posteriores, tenga en cuenta lo siguiente:

- Para usar el Visor de secuencia de comandos de Windows PowerShell, debe usar el Windows Server 2012 o una versión más reciente del ADAC

    > [!NOTE]
    > Puede usar **administrador del servidor** para instalar las herramientas de administración de servidor remoto (RSAT) para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.
    >
    > Para obtener información acerca de cómo instalar RSAT, consulte el artículo [herramientas de administración remota del servidor](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

- Tener un conocimiento básico de Windows PowerShell. Por ejemplo, tiene que saber cómo canalizar trabajos de Windows PowerShell. Para obtener más información acerca de cómo canalizar en Windows PowerShell, vea el tema sobre la [canalización en Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Procedimiento paso a paso para el Visor del historial de Windows PowerShell

En el siguiente procedimiento, usará el Visor del historial de Windows PowerShell en el ADAC para crear un script de Windows PowerShell.  Antes de comenzar, quite el usuario **test1** del grupo **group1**.

#### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Para crear un script mediante el uso del Visor del historial de PowerShell

1. A la derecha, haga clic en el icono de Windows PowerShell, haga clic en **ejecutar como administrador** y tipo **dsac.exe** para abrir el ADAC.

2. Haga clic en **Administrar**, en **Agregar nodos de navegación** y seleccione el dominio de destino apropiado en el cuadro de diálogo **Agregar nodos de navegación** y, a continuación, haga clic en **Aceptar**.

3. Expanda el panel **Historial de Windows PowerShell** en la parte inferior de la pantalla del ADAC.

4. Seleccione el usuario **test1**.

5. Haga clic en **agregar al grupo...**  en el **tareas** panel.

6. Navegue a **group1** y haga clic en **Aceptar** en el cuadro de diálogo.

7. Navegue al panel **Historial de Windows PowerShell** y ubique el comando que acaba de generar.

8. Copie el comando y péguelo en el editor deseado para crear su script.

    Por ejemplo, puede modificar el comando para agregar un usuario diferente a **group1** o agregar **test1** a un grupo diferente.

## <a name="see-also"></a>Vea también

[Avanzada de administración de AD DS mediante Active Directory Administrative Center &#40;nivel 200&#41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)
