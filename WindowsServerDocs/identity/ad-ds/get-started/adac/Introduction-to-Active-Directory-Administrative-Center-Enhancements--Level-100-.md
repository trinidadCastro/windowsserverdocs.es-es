---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: "Introducción a las mejoras del centro de administración de Active Directory (nivel 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7f52b22ec74ba12c383952e68b412f871a56474c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introducción a las mejoras del centro de administración de Active Directory (nivel 100)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

ADAC en Windows Server 2012 incluye características de administración de las siguientes acciones:

-   [Papelera de reciclaje de Active Directory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)

-   [Directiva de contraseña específica](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)

-   [Visor de Windows PowerShell historial](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Papelera de reciclaje de Active Directory
La eliminación accidental de objetos de Active Directory es una situación común para los usuarios de servicios de dominio de Active Directory (AD DS) y Active Directory Lightweight Directory Services (AD LDS). En versiones anteriores de Windows Server, antes de Windows Server 2008 R2, uno podría recuperar eliminados accidentalmente objetos de Active Directory, pero las soluciones tenían sus desventajas.

En Windows Server 2008, puedes usar la característica copia de seguridad de Windows Server y **ntdsutil** comando de restauración para marcar objetos como autorizados para garantizar que los datos restaurados se replican en todo el dominio. La desventaja de la solución de restauración fue que tenía que se realice en el modo de restauración de servicios de directorio (DSRM). Durante DSRM, el controlador de dominio que se está restaurando tenía permanezca sin conexión. Por lo tanto, no pudo solicitudes de cliente de servicio.

En Active Directory de Windows Server 2003 y Windows Server 2008 AD DS, se podrían recuperar los objetos de Active Directory eliminados a través de reanimación. Sin embargo, reactivarse con valores de vínculo atributos de los objetos (por ejemplo, pertenencias a grupos de cuentas de usuario) que se quitaron físicamente y no se han recuperado atributos no son valores de enlace que se han borrado. Por lo tanto, los administradores podrían no depender reanimación como la solución final a la eliminación accidental de objetos. Para obtener más información sobre reanimación, consulta [reanimación de objetos de Active Directory desecho](https://go.microsoft.com/fwlink/?LinkID=125452).

Active Directory Papelera de reciclaje, a partir de Windows Server 2008 R2, se basa en la infraestructura existente de reanimación de desecho y mejora la capacidad para conservar y recuperar eliminados accidentalmente objetos de Active Directory.

Cuando habilitas Active Directory Papelera de reciclaje, todos los valores de enlace y no vínculo-con valores de atributos de los objetos de Active Directory eliminados: se conservan y se restauran los objetos en su totalidad para el mismo estado coherente lógico que se encontraban inmediatamente antes de su eliminación. Por ejemplo, cuentas de usuario restaurados recuperan automáticamente todas las pertenencias a grupos y los derechos de acceso que tenían antes de la eliminación, dentro y entre dominios. La Papelera de reciclaje de Active Directory funciona en entornos de AD DS y AD LDS. Para obtener una descripción detallada de la Papelera de reciclaje de Active Directory, consulte [Novedades en AD DS: Papelera de reciclaje de Active Directory](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**¿Qué novedades?** En Windows Server 2012, la característica de la Papelera de reciclaje de Active Directory se ha mejorado con una nueva interfaz gráfica de usuario para que los usuarios administrar y restaurar objetos eliminados. Los usuarios pueden ahora visualmente encontrar una lista de objetos eliminados y restaurarlos a su ubicación original o deseado.

Si tienes previsto habilitar Active Directory Papelera de reciclaje en Windows Server 2012, ten en cuenta lo siguiente:

-   De manera predeterminada, está deshabilitada la Papelera de reciclaje de Active Directory. Para habilitarlo, primero debe generar el nivel funcional del bosque de AD DS o AD LDS entorno de Windows Server 2008 R2 o posterior. A su vez, esto requiere que todos los controladores de dominio del bosque o todos los servidores que hospedan instancias de los conjuntos de configuración de AD LDS sea ejecuta Windows Server 2008 R2 o posterior.

-   El proceso de habilitar la Papelera de reciclaje de Active Directory es irreversible. Después de habilitar la Papelera de reciclaje de Active Directory en el entorno, no se puede deshabilitar.

-   Para administrar la característica de la Papelera de reciclaje a través de una interfaz de usuario, debes instalar la versión del centro de administración de Active Directory en Windows Server 2012.

    > [!NOTE]
    > Puedes usar **administrador del servidor** para instalar herramientas de administración remota del servidor (RSAT) en equipos de Windows Server 2012 para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.
    > 
    > Puedes usar [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) en Windows&reg; 8 equipos para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.

### <a name="active-directory-recycle-bin-step-by-step"></a>Active Directory Papelera de reciclaje paso a paso
En los siguientes pasos, usarás ADAC para realizar las siguientes tareas de la Papelera de reciclaje de Active Directory en Windows Server 2012:

-   [Paso 1: Generar el nivel funcional del bosque](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)

-   [Paso 2: Habilita la Papelera de reciclaje](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)

-   [Paso 3: Crear los usuarios de prueba, el grupo y la unidad organizativa](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)

-   [Paso 4: Restauración de objetos eliminados](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> Es necesario pertenecer al grupo Administradores de organización o los permisos equivalentes para llevar a cabo los siguientes pasos.

### <a name="bkmk_raise_ffl"></a>Paso 1: Generar el nivel funcional del bosque
En este paso, generará el nivel funcional del bosque. Primero, debe generar el nivel funcional del bosque de destino para ser Windows Server 2008 R2 como mínimo antes de habilitar la Papelera de reciclaje de Active Directory.

##### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Para aumentar el nivel funcional del bosque de destino

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  Haz clic en el dominio de destino en el panel de navegación izquierdo y en la **tareas** panel, haz clic en **aumentar el nivel funcional del bosque**. Selecciona un nivel funcional del bosque que tenga al menos Windows Server 2008 R2 o superior y, a continuación, haz clic en **Aceptar**.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Para la **-identidad** argumento, especificar el nombre completo de DNS.

### <a name="bkmk_enable_recycle_bin"></a>Paso 2: Habilita la Papelera de reciclaje
En este paso, te permitirá la Papelera de reciclaje restaurar objetos eliminados en AD DS.

##### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Para habilitar Active Directory Papelera de reciclaje en ADAC en el dominio de destino

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En la **tareas** panel, haz clic en **habilitar la Papelera de reciclaje... **en la **tareas** panel, haz clic en **Aceptar** en el cuadro de mensaje de advertencia y, a continuación, haz clic en **Aceptar** para el mensaje de ADAC de actualización.

4.  Presiona F5 para actualizar ADAC.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Paso 3: Crear los usuarios de prueba, el grupo y la unidad organizativa
En los procedimientos siguientes, crearás dos usuarios de prueba. A continuación, se crea un grupo de prueba y agregar los usuarios de prueba al grupo. Además, se crea una unidad organizativa.

##### <a name="to-create-test-users"></a>Para crear los usuarios de prueba

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En la **tareas** panel, haz clic en **nueva** y, a continuación, haz clic en **usuario**.

    ![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4.  Escribe la siguiente información en **cuenta** y, a continuación, haz clic en Aceptar:

    -   Nombre completo: Prueba1

    -   Inicio de sesión de usuario SamAccountName: Prueba1

    -   Contraseña:p@ssword1

    -   Confirmar contraseña:p@ssword1

5.  Repite los pasos anteriores para crear un segundo usuario, test2.

##### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Para crear un grupo de prueba y agregar los usuarios al grupo

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En la **tareas** panel, haz clic en **nueva** y, a continuación, haz clic en **grupo**.

4.  Escribe la siguiente información en **grupo** y, a continuación, haz clic en **Aceptar**:

    -   **Nombre de grupo: grupo 1**

5.  Haz clic en **grupo 1**y, a continuación, en la **tareas** panel, haz clic en **propiedades**.

6.  Haz clic en **miembros**, haz clic en **agregar**, tipo **Prueba1; test2**y, a continuación, haz clic en **Aceptar**.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Add-ADGroupMember -Identity group1 -Member test1
```

##### <a name="to-create-an-organizational-unit"></a>Para crear una unidad organizativa

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En la **tareas** panel, haz clic en **nueva** y, a continuación, haz clic en **unidad organizativa**.

4.  Escribe la siguiente información en **unidad organizativa** y, a continuación, haz clic en **Aceptar**:

    -   **NameOU1**

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>Paso 4: Restauración de objetos eliminados
En los siguientes procedimientos, se restaurará objetos eliminados desde el **objetos eliminados** contenedor a su ubicación original y a una ubicación diferente.

##### <a name="to-restore-deleted-objects-to-their-original-location"></a>Para restaurar los objetos a su ubicación original eliminados

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  Selecciona los usuarios **Prueba1** y **test2**, haz clic en **eliminar** en la **tareas** panel y haz clic en **Sí** para confirmar la eliminación.

    ![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

    El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

    ```
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4.  Navegar a la **objetos eliminados** contenedor, selecciona **test2** y **Prueba1** y, a continuación, haz clic en **restaurar** en la **tareas** panel.

5.  Para confirmar que los objetos se restauraron a su ubicación original, navega al dominio de destino y comprueba que se muestran las cuentas de usuario.

    > [!NOTE]
    > Si navegas a la **propiedades** las cuentas de usuario **Prueba1** y **test2** y, a continuación, haz clic en **miembro de**, verás que también se ha restaurado su pertenencia al grupo.

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

##### <a name="to-restore-deleted-objects-to-a-different-location"></a>Para restaurar los objetos a una ubicación diferente eliminados

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  Selecciona los usuarios **Prueba1** y **test2**, haz clic en **eliminar** en la **tareas** panel y haz clic en **Sí** para confirmar la eliminación.

4.  Navegar a la **objetos eliminados** contenedor, selecciona **test2** y **Prueba1** y, a continuación, haz clic en **restaurar en** en la **tareas** panel.

5.  Selecciona **UO1** y, a continuación, haz clic en **Aceptar**.

6.  Para confirmar los objetos se restauraron a **UO1**, navega hasta el dominio de destino, haz doble clic en **UO1** y comprueba que se muestran las cuentas de usuario.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Directiva de contraseña específica
El sistema operativo Windows Server 2008 proporciona a las organizaciones una forma de definir contraseña diferente y directivas de bloqueo de cuenta para diferentes grupos de usuarios en un dominio. Los dominios de Active Directory antes de Windows Server 2008, solo una directiva de contraseña y directiva de bloqueo de cuenta se puede aplicar a todos los usuarios del dominio. Estas directivas se hayan especificado en la directiva predeterminada de dominio del dominio. Como resultado, las organizaciones que querían distintas configuraciones de bloqueo de cuenta y la contraseña para diferentes grupos de usuarios debían crear un filtro de contraseñas o implementar varios dominios. Ambos son opciones costosas.

Puedes usar directivas de contraseña específicas para especificar varias directivas de contraseña en un solo dominio y aplicar restricciones diferentes para las directivas de bloqueo de cuenta y la contraseña a diferentes grupos de usuarios en un dominio. Por ejemplo, puede aplicar la configuración de clasificación más estricta para las cuentas con privilegios y menos estricta a las cuentas de otros usuarios. En otros casos, es posible que quieras aplicar una directiva de contraseña especial para las cuentas cuyos contraseñas se sincronizan con otros orígenes de datos. Para obtener una descripción detallada de la directiva de contraseña muy específicas, consulta [AD DS: directivas de contraseña específicas](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**¿Qué novedades?** En Windows Server 2012, administración de directivas de contraseña muy específicas es más fácil y más visual proporcionando una interfaz de usuario para los administradores de AD DS administrarlas en ADAC. Ahora los administradores pueden ver resultante directivas de un usuario determinado, ver y ordenar todas las directivas de contraseña de un dominio y administrar directivas de contraseña individuales visualmente.

Si tienes previsto usar directivas de contraseña específicas en Windows Server 2012, ten en cuenta lo siguiente:

-   Solo los grupos de seguridad global y objetos de usuario (o los objetos inetOrgPerson si se usan en lugar de objetos de usuario), aplican directivas de contraseña específicas. De manera predeterminada, solo los miembros del grupo Administradores de dominio pueden establecer directivas de contraseña específicas. Sin embargo, también puede delegar la capacidad de establecer estas directivas a otros usuarios. El nivel funcional del dominio debe ser Windows Server 2008 o posterior.

-   Debes usar la versión de Windows Server 2012 del centro de administración de Active Directory para administrar directivas de contraseña específicas a través de una interfaz gráfica de usuario.

    > [!NOTE]
    > Puedes usar **administrador del servidor** para instalar herramientas de administración remota del servidor (RSAT) en equipos de Windows Server 2012 para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.
    > 
    > Puedes usar [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) en Windows&reg; 8 equipos para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.

### <a name="fine-grained-password-policy-step-by-step"></a>Paso a paso de directiva de contraseña muy específicas
En los siguientes pasos, usarás ADAC para realizar las siguientes tareas de directiva de contraseña muy específicas:

-   [Paso 1: Generar el nivel funcional del dominio](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)

-   [Paso 2: Crear unidad organizativa, grupos y usuarios de prueba](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)

-   [Paso 3: Crear una nueva directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

-   [Paso 4: Ver un conjunto resultante de directivas para un usuario](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)

-   [Paso 5: Modificar una directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)

-   [Paso 6: Eliminar una directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> Es necesario pertenecer al grupo Administradores de dominio o permisos equivalentes para llevar a cabo los siguientes pasos.

#### <a name="bkmk_raise_dfl"></a>Paso 1: Generar el nivel funcional del dominio
En el siguiente procedimiento, se generará el nivel funcional de dominio del dominio de destino en Windows Server 2008 o superior. Un nivel funcional del dominio de Windows Server 2008 o posterior es necesario para habilitar las directivas de contraseña muy específicas.

###### <a name="to-raise-the-domain-functional-level"></a>Para aumentar el nivel funcional de dominio

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  Haz clic en el dominio de destino en el panel de navegación izquierdo y en la **tareas** panel, haz clic en **aumentar el nivel funcional del dominio**. Selecciona un nivel funcional del bosque que tenga al menos Windows Server 2008 o superior y, a continuación, haz clic en **Aceptar**.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Paso 2: Crear unidad organizativa, grupos y usuarios de prueba
Para crear los usuarios de prueba y agrupar la necesidad de este paso, sigue los procedimientos que se encuentra aquí: [paso 3: crear los usuarios de prueba, el grupo y la unidad organizativa](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) (no es necesario crear la unidad organizativa para demostrar la directiva de contraseña específica).

#### <a name="bkmk_create_fgpp"></a>Paso 3: Crear una nueva directiva de contraseña muy específicas
En el siguiente procedimiento se crea una nueva directiva de contraseña específica mediante la interfaz de usuario en ADAC.

###### <a name="to-create-a-new-fine-grained-password-policy"></a>Para crear una nueva directiva de contraseña minucioso

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En el panel de navegación ADAC, abre el **sistema** contenedor y, a continuación, haz clic en **contenedor de configuraciones de contraseña**.

4.  En la **tareas** panel, haz clic en **nueva**y, a continuación, haz clic en **contraseña configuración**.

    Rellenar o editar los campos dentro de la página de propiedades para crear un nuevo **contraseña configuración** objeto. La **nombre** y **prioridad** campos son obligatorios.

    ![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5.  En **directamente se aplica a**, haz clic en **agregar**, tipo **grupo 1**y, a continuación, haz clic en **Aceptar**.

    Asocia el objeto de directiva de contraseñas de los miembros del grupo global que creaste para el entorno de prueba.

6.  Haz clic en **Aceptar** para enviar la creación.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Paso 4: Ver un conjunto resultante de directivas para un usuario
En el siguiente procedimiento, verá la configuración de un usuario que sea miembro del grupo al que se asigna una directiva de contraseña minucioso en contraseña resultante [paso 3: crear una nueva directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

###### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Para ver un conjunto resultante de directivas para un usuario

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  Seleccionar un usuario, **Prueba1** que pertenezcan al grupo, **grupo 1** que asociados con una directiva de contraseña muy específicas dentro de [paso 3: crear una nueva directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

4.  Haz clic en **configuración de vista resultante contraseña** en la **tareas** panel.

5.  Examinar la directiva de configuración de la contraseña y, a continuación, haz clic en **cancelar**.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Paso 5: Modificar una directiva de contraseña muy específicas
En el siguiente procedimiento, va a modificar la directiva de contraseñas minucioso que creaste en [paso 3: crear una nueva directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

###### <a name="to-edit-a-fine-grained-password-policy"></a>Para editar una directiva de contraseña muy específicas

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En la ADAC **panel de navegación**, expanda **sistema** y, a continuación, haz clic en **contenedor de configuraciones de contraseña**.

4.  Selecciona la directiva de contraseñas minucioso que creaste en [paso 3: crear una nueva directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) y haz clic en **propiedades** en la **tareas** panel.

5.  En **Exigir historial de contraseñas**, cambia el valor de **número de contraseñas recordadas** a **30**.

6.  Haz clic en **Aceptar**.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Paso 6: Eliminar una directiva de contraseña muy específicas

###### <a name="to-delete-a-fine-grained-password-policy"></a>Para eliminar una directiva de contraseña muy específicas

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  En el panel de navegación ADAC, expanda **sistema** y, a continuación, haz clic en **contenedor de configuraciones de contraseña**.

4.  Selecciona la directiva de contraseñas minucioso que creaste en [paso 3: crear una nueva directiva de contraseña muy específicas](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) y, en la **tareas** panel, haga clic **propiedades**.

5.  Borrar la **proteger contra eliminación accidental** casilla de verificación y haz clic en **Aceptar**.

6.  Selecciona la directiva de contraseñas minucioso y en la **tareas** panel, haga clic **eliminar**.

7.  Haz clic en **Aceptar** en el cuadro de diálogo de confirmación.

![Introducción al centro de administración de anuncios](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos ***

El siguiente cmdlet de Windows PowerShell o cmdlets de realizar la misma función que el procedimiento anterior. Especifique cada cmdlet en una sola línea, aunque pueden aparecer ajustados en varias líneas debido a restricciones de formato.

```
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Visor de Windows PowerShell historial
ADAC es una herramienta de la interfaz de usuario creada por encima de Windows PowerShell.  En Windows Server 2012, los administradores de TI pueden aprovechar ADAC para obtener información sobre Windows PowerShell para los cmdlets de Active Directory mediante el Visor de historial de Windows PowerShell. Como se ejecutan las acciones en la interfaz de usuario, se muestra el comando equivalente de Windows PowerShell para el usuario en el Visor de historial de Windows PowerShell. Esto permite a los administradores crear scripts automatizados y reducir los pasos, lo que aumenta la productividad de TI.  Además, esta característica reduce el tiempo para obtener información sobre Windows PowerShell para Active Directory y aumenta la confianza de los usuarios en la exactitud de los scripts de automatización.

Al usar el Visor de historial de Windows PowerShell en Windows Server 2012, ten en cuenta lo siguiente:

-   Para usar el Visor de Script de PowerShell de Windows, debes usar la versión de Windows Server 2012 de ADAC

    > [!NOTE]
    > Puedes usar **administrador del servidor** para instalar herramientas de administración remota del servidor (RSAT) en equipos de Windows Server 2012 para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.
    > 
    > Puedes usar [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) en Windows&reg; 8 equipos para usar la versión correcta del centro de administración de Active Directory para administrar la Papelera de reciclaje a través de una interfaz de usuario.

-   Tener un conocimiento básico de Windows PowerShell. Por ejemplo, debes saber cómo funciona la canalización en Windows PowerShell. Para obtener más información acerca de la canalización en Windows PowerShell, consulta [diseño y la canalización de Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Visor de historial de Windows PowerShell paso a paso
En el siguiente procedimiento, usarás el Visor de historial de Windows PowerShell en ADAC para crear un script de PowerShell de Windows.  Antes de comenzar este procedimiento, quitar usuarios, **Prueba1** desde el grupo, **grupo 1**.

##### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Para crear un script mediante PowerShell historial Visor

1.  Haga clic con el botón secundario del mouse en el icono de Windows PowerShell, haz clic en **ejecutar como administrador** y el tipo de **dsac.exe** para abrir ADAC.

2.  Haz clic en **administrar**, haz clic en **agregar nodos de navegación** y selecciona el dominio de destino apropiado de la **agregar nodos de navegación** cuadro de diálogo y, a continuación, haz clic en **Aceptar**.

3.  Expande la **historia de Windows PowerShell** panel en la parte inferior de la pantalla ADAC.

4.  El usuario selecciona, **Prueba1**.

5.  Haz clic en **agregar al grupo... **en la **tareas** panel.

6.  Navegar a **grupo 1** y haz clic en **Aceptar** en el cuadro de diálogo.

7.  Navegar a la **historia de Windows PowerShell** panel y busque el comando que acabas de generar.

8.  Copia el comando y pégalo en el editor de deseado para construir el script.

    Por ejemplo, puedes modificar el comando para agregar otro usuario **grupo 1**, o agregar **Prueba1** a un grupo diferente.

## <a name="see-also"></a>Consulta también
[Administración de avanzada de AD DS con el centro de administración de Active Directory & #40; Nivel 200 & #41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)


