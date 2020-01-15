---
title: Implementación de perfiles de usuario móviles
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 87cf428482e2812e72d5cb527b35e90c46c8a3a1
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950273"
---
# <a name="deploying-roaming-user-profiles"></a>Implementación de perfiles de usuario móviles

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En este tema se describe cómo usar Windows Server para implementar [perfiles de usuario móviles](folder-redirection-rup-overview.md) en equipos cliente de Windows. Los perfiles de usuario móviles redirigen los perfiles de usuario a un recurso compartido de archivos para que los usuarios reciban la misma configuración del sistema operativo y de la aplicación en varios equipos.

Para obtener una lista de los cambios recientes realizados en este tema, consulte la sección [historial de cambios](#change-history) de este tema.

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), se ha actualizado el [paso 4: crear opcionalmente un GPO para perfiles de usuario móviles](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) en este tema para que Windows pueda aplicar correctamente la Directiva de perfiles de usuario móviles (y no revertir a directivas locales en equipos afectados).

> [!IMPORTANT]
>  Las personalizaciones de usuario que se inician se pierden después de una actualización local del sistema operativo en la configuración siguiente:
> - Los usuarios se configuran para un perfil móvil
> - Los usuarios pueden realizar cambios en el inicio
>
> Como resultado, el menú Inicio se restablece al valor predeterminado de la nueva versión del sistema operativo después de la actualización en contexto del sistema operativo. Para obtener soluciones alternativas, vea el [Apéndice C: trabajar con los diseños del menú Inicio de restablecimiento después](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades)de las actualizaciones.

## <a name="prerequisites"></a>Requisitos previos

### <a name="hardware-requirements"></a>Requisitos de hardware

Los perfiles de usuario móviles requieren un equipo basado en x64 o en x86; no es compatible con Windows RT.

### <a name="software-requirements"></a>Requisitos de software

Los perfiles de usuario móviles tienen los siguientes requisitos de software:

- Si vas a implementar perfiles de usuario móviles con redirección de carpetas en un entorno con perfiles de usuario locales, implementa la redirección de carpetas antes que los perfiles de usuario móviles para minimizar el tamaño de los perfiles móviles. Después de completar correctamente la redirección de las carpetas de usuario existentes, podrás implementar los perfiles de usuario móviles.
- Para administrar los perfiles de usuario móviles debes iniciar sesión como un miembro del grupo de seguridad Administradores de dominio, el grupo de seguridad Administradores de empresa o el grupo de seguridad Propietarios del creador de directivas de grupo.
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- Los equipos cliente deben unirse a los Servicios de dominio de Active Directory (AD DS) que estés administrando.
- Los equipos deben estar disponibles con Administración de directivas de grupo y tener instalado el Centro de administración de Active Directory.
- Debe haber disponible en servidor de archivos para hospedar perfiles de usuario móviles.

    - Si el recurso compartido de archivos usa espacios de nombres DFS, las carpetas DFS (vínculos) deben tener el mismo destino para evitar que los usuarios realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos usa Replicación DFS para replicar los contenidos con otro servidor, los usuarios deben poder acceder únicamente al servidor de origen para evitar que se realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos está en clúster, deshabilite la disponibilidad continua en el recurso compartido de archivos para evitar problemas de rendimiento.
- Para usar la compatibilidad con equipos primarios en perfiles de usuario móviles, existen requisitos adicionales de los esquemas de equipos cliente y Active Directory. Para obtener más información, vea [implementar equipos principales para redirección de carpetas y perfiles de usuario móviles](deploy-primary-computers.md).
- El diseño del menú Inicio de un usuario no se moverá en Windows 10, Windows Server 2019 o Windows Server 2016 si están usando más de un equipo, Escritorio remoto host de sesión o un servidor de infraestructura de escritorio virtualizado (VDI). Como solución alternativa, puede especificar un diseño de inicio como se describe en este tema. O bien, puede usar discos de Perfil de usuario, que tienen la configuración de menú de inicio de itinerancia correcta cuando se usa con servidores host de sesión de Escritorio remoto o servidores VDI. Para obtener más información, consulte el [Administración de datos de usuario más sencillo con discos de Perfil de usuario en Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Consideraciones al usar perfiles de usuario móviles en varias versiones de Windows

Si vas a usar perfiles de usuario móviles en diferentes versiones de Windows, te recomendamos que realices las acciones siguientes:

- Configura Windows para mantener versiones de perfiles separadas para cada versión de sistema operativo. Esto ayuda a evitar problemas no deseados y no previstos, como daños en perfiles.
- Usa la redirección de carpetas para almacenar archivos de usuario, como documentos e imágenes, fueran de los perfiles de usuario. Esto permite que los mismos archivos estén disponibles para usuarios de diferentes versiones de sistemas operativos. Además, reduce el tamaño de los perfiles y hace que los inicios de sesión sean más rápidos.
- Asigna espacio suficiente para perfiles de usuario móviles. Si permites el uso de dos versiones distintas del sistema operativo, los perfiles se doblarán en número (y, por lo tanto, el espacio total consumido), ya que se mantiene un perfil separado para cada versión del sistema operativo.
- No use perfiles de usuario móviles en equipos que ejecuten Windows Vista/Windows Server 2008 y Windows 7/Windows Server 2008 R2. No se admite la itinerancia entre estas versiones de sistema operativo debido a las incompatibilidades en sus versiones de perfil.
- Informar a los usuarios de que los cambios realizados en una versión del sistema operativo no se desplazarán a otra versión del sistema operativo.
- Al mover el entorno a una versión de Windows que usa una versión de perfil diferente (por ejemplo, de Windows 10 a Windows 10, versión 1607; consulte el [Apéndice B: información de referencia](#appendix-b-profile-version-reference-information) de la versión de perfil para una lista), los usuarios reciben un nuevo perfil de usuario móvil vacío. Puede minimizar el impacto de obtener un nuevo perfil mediante el redireccionamiento de carpetas para redirigir carpetas comunes. No existe un método compatible para migrar perfiles de usuario móviles de una versión de perfil a otra.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Paso 1: Habilitar el uso de versiones de perfiles separadas

Si va a implementar perfiles de usuario móviles en equipos que ejecutan Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012, se recomienda realizar un par de cambios en el entorno de Windows antes de la implementación. Estos cambios ayudarán a garantizar que las actualizaciones de sistemas operativos futuras se produzcan sin problemas y facilita la capacidad para ejecutar de forma simultánea diferentes versiones de Windows con perfiles de usuario móviles.

Para realizar estos cambios, haz lo siguiente.

1. Descargue e instale la actualización de software correspondiente en todos los equipos en los que vaya a usar perfiles móviles, obligatorios, superobligatorios o de dominio predeterminados:

    - Windows 8.1 o Windows Server 2012 R2: Instale la actualización de software que se describe en el artículo [2887595](https://support.microsoft.com/kb/2887595) de Microsoft Knowledge base (cuando se publique).
    - Windows 8 o Windows Server 2012: instale la actualización de software que se describe en el artículo [2887239](https://support.microsoft.com/kb/2887239) de Microsoft Knowledge Base.

2. En todos los equipos que ejecutan Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012 en el que se usarán los perfiles de usuario móviles, use el editor del registro o directiva de grupo para crear el siguiente valor DWORD de clave del registro y establézcalo en `1`. Para obtener más información sobre cómo crear claves del Registro mediante Directiva de grupo, consulte [Configuración de un elemento del Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    > [!WARNING]
    > La edición incorrecta del Registro puede dañar gravemente el sistema. Antes de realizar cambios en el Registro, debe hacer una copia de seguridad de los datos de valor guardados en el equipo.
3. Reinicie los equipos.

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>Paso 2: Crear un grupo de seguridad de perfiles de usuario móviles

Si tu entorno aún no está configurado con perfiles de usuario móviles, el primer paso es crear un grupo de seguridad que contenga todos los usuarios o equipos donde quieras aplicar la configuración de directivas de perfiles de usuario móviles.

- Los administradores de implementaciones de perfiles de usuario móviles de uso general suelen crear un grupo de seguridad para usuarios.
- Los administradores de servicios de Escritorio remoto o implementaciones de escritorios virtualizados suelen usar un grupo de seguridad para usuarios y para los equipos compartidos.

Aquí se muestra cómo crear un grupo de seguridad para perfiles de usuario móviles:

1. Abra Administrador del servidor en un equipo que tenga instalado Active Directory centro de administración.
2. En el menú **herramientas** , seleccione **centro de administración de Active Directory**. Aparecerá el Centro de administración de Active Directory.
3. Haga clic con el botón derecho en el dominio o la unidad organizativa correspondiente, seleccione **nuevo**y, a continuación, seleccione **Grupo**.
4. En la ventana **Crear grupo** de la sección **Grupo**, especifica la configuración siguiente:

    - En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Usuarios y equipos de perfiles de usuario móviles**.
    - En **ámbito de grupo**, seleccione **seguridad**y, a continuación, seleccione **global**.

5. En la sección **miembros** , seleccione **Agregar**. Aparecerá el cuadro de diálogo Seleccionar usuarios, contactos, equipos, cuentas de servicio o grupos.
6. Si desea incluir cuentas de equipo en el grupo de seguridad, seleccione **tipos de objeto**, active la casilla **equipos** y, a continuación, haga clic en **Aceptar**.
7. Escriba los nombres de los usuarios, grupos o equipos en los que desea implementar perfiles de usuario móviles, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** de nuevo.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Paso 3: Crear un recurso compartido de archivos para perfiles de usuario móviles

Si aún no tiene un recurso compartido de archivos independiente para los perfiles de usuario móviles (independiente de los recursos compartidos de las carpetas redirigidas para evitar el almacenamiento en caché involuntaria de la carpeta de perfil móvil), use el procedimiento siguiente para crear un recurso compartido de archivos en un servidor que ejecute Windows. Servidor.

> [!NOTE]
> Algunas funciones pueden ser distintas o no estar disponibles en función de la versión de Windows Server que esté usando.

Aquí se muestra cómo crear un recurso compartido de archivos en Windows Server:

1. En el panel de navegación Administrador del servidor, seleccione **servicios de archivos y almacenamiento**y, a continuación, seleccione **recursos compartidos** para mostrar la página recursos compartidos.
2. En el icono recursos compartidos, seleccione **tareas**y, después, seleccione **nuevo recurso compartido**. Se abrirá el Asistente para nuevo recurso compartido.
3. En la página **Seleccionar perfil** , seleccione **recurso compartido SMB-rápido**. Si tiene instalado Administrador de recursos servidor de archivos y está usando las propiedades de administración de carpetas, en su lugar seleccione **recurso compartido SMB-avanzado**.
4. En la página **Ubicación del recurso compartido** , selecciona el servidor y el volumen donde quieras crear el recurso compartido.
5. En la página **Nombre del recurso compartido** , escribe un nombre para el recurso compartido (por ejemplo, **Perfiles de usuario$** ) en el cuadro **Nombre del recurso compartido** box.

    > [!TIP]
    > Al crear el recurso compartido, oculte el recurso compartido colocando un ```$``` después del nombre del recurso compartido. Esto hace que no se muestre el recurso compartido a los usuarios ocasionales.

6. En la página **Más opciones** , desactive la casilla **Habilitar disponibilidad continua** , si la hay, y, de manera opcional, seleccione las casillas **Habilitar enumeración basada en el acceso** y **Cifrar acceso a datos** .
7. En la página **permisos** , seleccione **personalizar permisos.** . Se abrirá el cuadro de diálogo Configuración de seguridad avanzada.
8. Seleccione **deshabilitar herencia**y, a continuación, seleccione **convertir permisos heredados en permiso explícito en este objeto**.
9. Establezca los permisos como se describe en [permisos necesarios para los perfiles de usuario móviles de hospedaje de recurso compartido de archivos](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) y que se muestran en la siguiente captura de pantalla, quitando los permisos de grupos y cuentas que no figuran en la lista y agregando permisos especiales al grupo usuarios y equipos de perfiles de usuario móvil que creó en el paso 1.
    
    ![Ventana Configuración de seguridad avanzada que muestra los permisos que se describen en la tabla 1](media/advanced-security-user-profiles.jpg)
    
    **Ilustración 1** Establecer los permisos para el recurso compartido de perfiles de usuario móviles
10. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Propiedades de administración** , seleccione el valor de uso de carpeta **Archivos de usuario** .
11. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Cuota** , seleccione (de manera opcional) la cuota que quiera aplicar a los usuarios del recurso compartido.
12. En la página **confirmación** , seleccione **crear.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Permisos necesarios para los perfiles de usuario móviles de hospedaje de recurso compartido de archivos

| Cuenta de usuario | Acceso | Aplicable a |
|   -   |   -   |   -   |
|   Sistema    |  Control total     |  Esta carpeta, subcarpetas y archivos     |
|  Administradores     |  Control total     |  Solo esta carpeta     |
|  Creador/propietario     |  Control total     |  Solo subcarpetas y archivos     |
| Grupo de seguridad de usuarios que necesitan colocar datos en recursos compartidos (usuarios y equipos de perfiles de usuario móviles)      |  Mostrar carpeta/leer datos *(permisos avanzados)* <br />Crear carpetas/anexar datos *(permisos avanzados)* |  Solo esta carpeta     |
| Otros grupos y cuentas   |  Ninguno (quitar)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Paso 4: De manera opcional, crea un GPO para perfiles de usuario móviles

Si aún no has creado un GPO para la configuración de los perfiles de usuario móviles, haz lo siguiente para crear un GPO vacío para usarlo con perfiles de usuario móviles. Este GPO permite configurar opciones de perfiles de usuario móviles (como soporte de equipo principal, que se explica por separado) y, además, también puede usarse para habilitar perfiles de usuario móviles en equipos, ya que esto suele hacerse al implementar en entornos de escritorio virtualizados o con Servicios de Escritorio remoto.

Aquí se muestra cómo crear un GPO para perfiles de usuario móviles:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. En el menú **herramientas** , seleccione **Administración de directiva de grupo**. Se abrirá Administración de directivas de grupo.
3. Haga clic con el botón secundario en el dominio o la unidad organizativa en la que desea configurar los perfiles de usuario móviles y, a continuación, seleccione **crear un GPO en este dominio y vincularlo aquí**.
4. En el cuadro de diálogo **nuevo GPO** , escriba un nombre para el GPO (por ejemplo, **configuración de Perfil de usuario móvil**) y, a continuación, seleccione **Aceptar**.
5. Haz clic con el botón secundario en el nuevo GPO y desactiva la casilla **Vínculo habilitado** . Esto no permite que se aplique el GPO hasta que termine de configurarlo.
6. Selecciona el GPO. En la sección **filtrado de seguridad** de la pestaña **ámbito** , seleccione **usuarios autenticados**y, a continuación, seleccione **quitar** para evitar que el GPO se aplique a todos.
7. En la sección **filtrado de seguridad** , seleccione **Agregar**.
8. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre del grupo de seguridad que creó en el paso 1 (por ejemplo, **usuarios y equipos de perfiles de usuario móviles**) y seleccione **Aceptar**.
9. Seleccione la pestaña **delegación** , haga clic en **Agregar**, escriba **usuarios autenticados**, haga clic en **Aceptar**y, a continuación, seleccione **Aceptar** de nuevo para aceptar los permisos de lectura predeterminados.
    
    Este paso es necesario debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>Debido a los cambios de seguridad realizados en [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), ahora debe conceder al grupo usuarios autenticados permisos de lectura en el GPO; de lo contrario, el GPO no se aplicará a los usuarios, o bien, si ya se ha aplicado, se quitará el GPO y se redirigirán los perfiles de usuario de nuevo al equipo local. Para obtener más información, consulte [implementación de la actualización de seguridad de directiva de grupo MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Paso 5: Configurar los perfiles de usuario móviles en cuentas de usuario (opcional)

Si implementas perfiles de usuario móviles en cuentas de usuario, haz lo siguiente para especificar perfiles de usuario móviles para cuentas de usuario en Active Directory Domain Services. Si va a implementar perfiles de usuario móviles en equipos, como suele realizarse para Servicios de Escritorio remoto o implementaciones de escritorios virtualizados, en su lugar, use el procedimiento descrito en [paso 6: configurar opcionalmente perfiles de usuario móviles en equipos](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Si configuras perfiles de usuario móviles en cuentas de usuario mediante Active Directory y en equipos mediante Directiva de grupo, la configuración de directiva basada en equipos tiene prioridad.

Aquí se muestra cómo configurar perfiles de usuario móviles en cuentas de usuario:

1. En el Centro de administración de Active Directory, navega hasta el contenedor **Usuarios** (o bien OU) en el dominio correspondiente.
2. Seleccione todos los usuarios a los que desea asignar un perfil de usuario móvil, haga clic con el botón secundario en los usuarios y, a continuación, seleccione **propiedades**.
3. En la sección **perfil** , seleccione la casilla **ruta de acceso del perfil:** y escriba la ruta de acceso al recurso compartido de archivos donde desea almacenar el perfil de usuario móvil del usuario, seguido de `%username%` (que se reemplaza automáticamente con el nombre de usuario la primera vez que el usuario inicia sesión). Por ejemplo:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Para especificar un perfil de usuario móvil obligatorio, especifique la ruta de acceso al archivo NTuser. Man que creó anteriormente, por ejemplo, `fs1.corp.contoso.comUser Profiles$default`. Para obtener más información, vea [crear perfiles de usuario obligatorios](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Selecciona **Aceptar**.

> [!NOTE]
> De manera predeterminada, se permite implementar todas las aplicaciones basadas en Windows® en tiempo de ejecución (Tienda Windows) al usar los perfiles de usuario móviles. Pero, al usar un perfil especial, las aplicaciones no se implementan de manera predeterminada. Los perfiles especiales son perfiles de usuario donde se descartan los cambios cuando el usuario cierra la sesión:
> <br><br>Para eliminar las restricciones en la implementación de aplicaciones para perfiles especiales, habilite la configuración de directiva **Allow deployment operations in special profiles** (ubicada en Configuración del equipo\Directivas\Plantillas administrativas\Componentes de Windows\Implementación de paquetes de aplicaciones). Pero las aplicaciones implementadas en este escenario dejarán datos almacenados en el equipo, que podrían acumularse si, por ejemplo, hubiera cientos de usuarios en un mismo equipo. Para limpiar las aplicaciones, busque o desarrolle una herramienta que use la API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) para limpiar paquetes de aplicaciones para usuarios que ya no tienen un perfil en el equipo.
> <br><br>Para obtener información general sobre las aplicaciones de la Tienda Windows, consulte [Administrar el acceso de cliente a la Tienda Windows](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Paso 6: Configurar los perfiles de usuario móviles en equipos (opcional)

Si implementas perfiles de usuario móviles en equipos, como suele realizarse para Servicios de Escritorio remoto o en implementaciones de escritorios virtualizados, haz lo siguiente. Si va a implementar perfiles de usuario móviles en cuentas de usuario, en su lugar, use el procedimiento descrito en [paso 5: configurar opcionalmente perfiles de usuario móviles en cuentas de usuario](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Puede usar directiva de grupo para aplicar perfiles de usuario móviles a equipos que ejecuten Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

> [!NOTE]
> Si configuras perfiles de usuario móviles en equipos mediante Directiva de grupo y en cuentas de usuario mediante Active Directory, la configuración de directiva basada en equipos tiene prioridad.

Aquí se muestra cómo configurar perfiles de usuario móviles en equipos:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. En el menú **herramientas** , seleccione **Administración de directiva de grupo**. Aparecerá la administración de directiva de grupo.
3. En administración de directiva de grupo, haga clic con el botón secundario en el GPO que creó en el paso 3 (por ejemplo, **configuración de perfiles de usuario móviles**) y, a continuación, seleccione **Editar**.
4. En la ventana Editor de administración de directivas de grupo, navega hasta **Configuración del equipo**, **Directivas**, **Plantillas administrativas**, **Sistema**y, finalmente, **Perfiles de usuario**.
5. Haga clic con el botón secundario en **establecer ruta de acceso de perfil móvil para todos los usuarios que inician sesión en este equipo** y seleccione **Editar**.
    > [!TIP]
    > Si se configura una carpeta particular para los usuarios, esta será la carpeta predeterminada usada por algunos programas como Windows PowerShell. Puedes configurar una ubicación alternativa, ya sea local o de red, por usuario en la sección **Carpeta particular** de las propiedades de la cuenta de usuario en AD DS. Para configurar la ubicación de la carpeta principal para todos los usuarios de un equipo que ejecute Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en un entorno de escritorio virtual, habilite la configuración de directiva **establecer carpeta particular de usuario** y, después, especifique el recurso compartido de archivos y la letra de unidad para asignar (o especifique una carpeta local) No uses variables de entorno ni puntos suspensivos. El alias del usuario se anexa al final de la ruta de acceso especificada durante el inicio de sesión del usuario.
6. En el cuadro de diálogo **propiedades** , seleccione **habilitado** .
7. En el cuadro **los usuarios que inician sesión en este equipo deben usar esta ruta de acceso de perfil móvil** , escriba la ruta de acceso al recurso compartido de archivos donde desea almacenar el perfil de usuario móvil del usuario, seguido de `%username%` (que se reemplaza automáticamente con el nombre de usuario la primera vez que el usuario inicia sesión). Por ejemplo:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Para especificar un perfil de usuario móvil obligatorio, que es un perfil preconfigurado en el que los usuarios no pueden realizar cambios permanentes (los cambios se restablecen cuando el usuario cierra sesión), especifique la ruta de acceso al archivo NTuser. Man que creó anteriormente, por ejemplo, `\\fs1.corp.contoso.com\User Profiles$\default`. Para obtener más información, consulte [Creación de un perfil de usuario obligatorio](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Selecciona **Aceptar**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Paso 7: especificar opcionalmente un diseño de inicio para equipos con Windows 10

Puede usar directiva de grupo para aplicar un diseño de menú Inicio específico para que los usuarios vean el mismo diseño de inicio en todos los equipos. Si los usuarios inician sesión en más de un equipo y desea que tengan un diseño de inicio coherente entre los equipos, asegúrese de que el GPO se aplica a todos sus equipos.

Para especificar un diseño de inicio, haga lo siguiente:

1. Actualice los equipos con Windows 10 a Windows 10, versión 1607 (también conocida como actualización de aniversario) o más recientes, e instale la actualización acumulativa del 14 de marzo de 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) o una versión más reciente.
2. Cree un archivo XML de diseño del menú Inicio completo o parcial. Para ello, consulte [personalizar y exportar el diseño de inicio](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Si especifica un diseño de inicio *completo* , un usuario no podrá personalizar ninguna parte del menú Inicio. Si especifica un diseño de inicio *parcial* , los usuarios pueden personalizar todo excepto los grupos de mosaicos bloqueados que especifique. Sin embargo, con un diseño de inicio parcial, las personalizaciones del usuario en el menú Inicio no se desplazan a otros equipos.
3. Utilice directiva de grupo para aplicar el diseño de inicio personalizado al GPO que creó para los perfiles de usuario móviles. Para ello, consulte [usar Directiva de grupo para aplicar un diseño de inicio personalizado en un dominio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Use directiva de grupo para establecer el siguiente valor del registro en los equipos con Windows 10. Para ello, consulte [configuración de un elemento del registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Acción**   | **Update**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| Ruta de acceso de la clave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nombre de valor   | **SpecialRoamingOverrideAllowed** |
| Tipo de valor   | **REG_DWORD**               |
| Datos de valor   | **1** (o **0** para deshabilitar) |
| Base         | **Decimal**                 |

5. Opta Habilite las optimizaciones de inicio de sesión por primera vez para que los usuarios puedan iniciar sesión más rápidamente. Para ello, consulte [aplicación de directivas para mejorar el inicio de sesión](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. Opta Reduzca los tiempos de inicio de sesión quitando las aplicaciones innecesarias de la imagen base de Windows 10 que usa para implementar los equipos cliente. Windows Server 2019 y Windows Server 2016 no tienen ninguna aplicación aprovisionada previamente, por lo que puede omitir este paso en las imágenes de servidor.
    - Para quitar aplicaciones, use el cmdlet [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) en Windows PowerShell para desinstalar las siguientes aplicaciones. Si los equipos ya están implementados, puede crear scripts para la eliminación de estas aplicaciones mediante [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft. windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft. BingWeather\_8wekyb3d8bbwe
      - Microsoft. DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft. GetStarted\_8wekyb3d8bbwe
      - Microsoft. Windows. photos\_8wekyb3d8bbwe
      - Microsoft. WindowsCamera\_8wekyb3d8bbwe
      - Microsoft. WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft. WindowsStore\_8wekyb3d8bbwe
      - Microsoft. XboxApp\_8wekyb3d8bbwe
      - Microsoft. XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft. ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>La desinstalación de estas aplicaciones reduce los tiempos de inicio de sesión, pero puede dejarlos instalados Si su implementación necesita cualquiera de ellos.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Paso 8: habilitar el GPO de perfiles de usuario móviles

Si configuras perfiles de usuario móviles en equipos mediante Directiva de grupo, o si personalizas otra configuración de perfiles de usuario móviles mediante Directiva de grupo, el paso siguiente es habilitar el GPO y permitir aplicarlo en los usuarios afectados.

>[!TIP]
>Si tiene previsto implementar la compatibilidad con el equipo principal, hágalo ahora antes de habilitar el GPO. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal. Para la configuración de directiva específica, vea [implementar equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles](deploy-primary-computers.md).

Aquí se muestra cómo habilitar el GPO de Perfil de usuario móvil:

1. Abra Administración de directivas de grupo.
2. Haga clic con el botón secundario en el GPO que creó y seleccione **vincular habilitado**. Aparecerá una casilla junto al elemento de menú.

## <a name="step-9-test-roaming-user-profiles"></a>Paso 9: probar perfiles de usuario móviles

Para probar perfiles de usuario móviles, inicia sesión en un equipo con una cuenta de usuario configurada para perfiles de usuario móviles, o bien inicia sesión en un equipo configurado para perfiles de usuario móviles. Después, confirma que el perfil es redirigido.

Aquí se muestra cómo probar perfiles de usuario móviles:

1. Inicia sesión en un equipo principal (si has habilitado el soporte de equipo principal) con una cuenta de usuario en la que hayas habilitado los perfiles de usuario móviles. Si has habilitado perfiles de usuario móviles en equipos específicos, inicia sesión en uno de estos equipos.
2. Si el usuario ha iniciado sesión en el equipo anteriormente, abre un símbolo del sistema con privilegios elevados y escribe el comando siguiente para asegurarte de que se aplique la última configuración de directiva de grupo en el equipo cliente:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Para confirmar que el perfil de usuario es móvil, Abra **el panel de control**, seleccione **sistema y seguridad**, seleccione **sistema**, **Configuración avanzada del sistema**, seleccione **configuración** en la sección perfiles de usuario y, a continuación, busque **itinerancia** en la columna **tipo** .

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Apéndice A: Lista de comprobación para la implementación de perfiles de usuario móviles

| Estado                     | Acción                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparar el dominio<br>-Unir equipos al dominio<br>-Habilitar el uso de versiones de perfil independientes<br>-Crear cuentas de usuario<br>-(Opcional) implementar redirección de carpetas |
| ☐<br><br><br>             | 2. Crear un grupo de seguridad para perfiles de usuario móviles<br>-Nombre del Grupo:<br>Registrados |
| ☐<br><br>                 | 3. Crear un recurso compartido de archivos para perfiles de usuario móviles<br>-Nombre del recurso compartido de archivos: |
| ☐<br><br>                 | 4. Crear un GPO para perfiles de usuario móviles<br>-Nombre del GPO:|
| ☐                         | 5. Configurar la directiva de perfiles de usuario móviles    |
| ☐<br>☐<br>☐              | 6. habilitar perfiles de usuario móviles<br>-Habilitado en AD DS en las cuentas de usuario<br>-Habilitado en directiva de grupo en las cuentas de equipo?<br> |
| ☐                         | 7. (opcional) especificar un diseño de inicio obligatorio para equipos con Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (opcional) habilitar la compatibilidad con el equipo principal<br>-Designar equipos principales para los usuarios<br>-Ubicación de las asignaciones de equipo principal y de usuario:<br>-(Opcional) habilitar la compatibilidad con el equipo principal para redirección de carpetas<br>-Basado en el equipo o en el usuario<br>-(Opcional) habilitar la compatibilidad con equipos primarios para perfiles de usuario móviles |
| ☐                        | 9. habilitar el GPO de perfiles de usuario móviles                |
| ☐                        | 10. probar perfiles de usuario móviles                         |

## <a name="appendix-b-profile-version-reference-information"></a>Apéndice B: Información de referencia sobre la versión del perfil

Cada perfil tiene una versión de perfil que se corresponde aproximadamente con la versión de Windows en la que se usa el perfil. Por ejemplo, Windows 10, la versión 1703 y la versión 1607 usan el. Versión del perfil V6. Microsoft crea una nueva versión de perfil solo cuando es necesario para mantener la compatibilidad, por lo que no todas las versiones de Windows incluyen una nueva versión de perfil.

La tabla siguiente contiene las ubicaciones de los perfiles de usuario móviles en diferentes versiones de Windows.

| Versión del sistema operativo | Ubicación del perfil de usuario móvil |
| --- | --- |
| Windows XP y Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista y Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 y Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 y Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (después de aplicar la actualización de software y la clave del registro)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes de que se apliquen la actualización de software y la clave del registro) |
| Windows 8.1 y Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (después de aplicar la actualización de software y la clave del registro)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes de que se apliquen la actualización de software y la clave del registro) |
| 10 de Windows | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versión 1703 y versión 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Apéndice C: trabajar con los diseños del menú Inicio de restablecimiento después de las actualizaciones

Estas son algunas formas de solucionar los diseños del menú Inicio que se restablecen después de una actualización local:

- Si solo un usuario usa el dispositivo y el administrador de ti usa una estrategia de implementación de SO administrada como SCCM, puede hacer lo siguiente:
    
  1. Exportar el diseño del menú Inicio con Export-Startlayout antes de la actualización 
  2. Importe el diseño del menú Inicio con Import-StartLayout después de OOBE, pero antes de que el usuario inicie sesión  
 
     > [!NOTE] 
     > Al importar un StartLayout se modifica el perfil de usuario predeterminado. Todos los perfiles de usuario creados después de la importación obtendrán el diseño de inicio importado.
 
- Los administradores de TI pueden optar por administrar el diseño del inicio con directiva de grupo. El uso de directiva de grupo proporciona una solución de administración centralizada para aplicar un diseño de inicio estandarizado a los usuarios. Existen dos modos de uso de directiva de grupo para la administración de inicio. Bloqueo completo y bloqueo parcial. El escenario de bloqueo completo impide que el usuario realice cambios en el diseño de inicio. El escenario de bloqueo parcial permite al usuario realizar cambios en un área específica de inicio. Para obtener más información, vea [personalizar y exportar el diseño de inicio](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Los cambios realizados por el usuario en el escenario de bloqueo parcial se perderán durante la actualización.

- Permite que se restablezca el diseño inicial y que los usuarios finales puedan volver a configurar el inicio. Se puede enviar un correo electrónico de notificación u otra notificación a los usuarios finales para esperar que se restablezcan los diseños iniciales después de la actualización del sistema operativo a un impacto minimizado. 

## <a name="change-history"></a>Historial de cambios

En la tabla siguiente se resumen los cambios más importantes realizados en este tema.

| Fecha | Descripción |Razón|
| --- | ---         | ---   |
| 1 de mayo de 2019 | Se han agregado actualizaciones para Windows Server 2019 |
| 10 de abril de 2018 | Se ha agregado una descripción de Cuándo se pierden las personalizaciones de usuario después de una actualización local del sistema operativo|Problema conocido de llamada. |
| 13 de marzo de 2018 | Actualizado para Windows Server 2016 | Se ha sacado de la biblioteca de versiones anteriores y se ha actualizado para la versión actual de Windows Server. |
| 13 de abril de 2017 | Se ha agregado información de perfil para la versión 1703 de Windows 10 y se ha aclarado cómo funcionan las versiones de perfil móvil al actualizar los sistemas operativos; Consulte [consideraciones al usar perfiles de usuario móviles en varias versiones de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Comentarios del cliente. |
| 14 de marzo de 2017 | Se ha agregado un paso opcional para especificar un diseño de inicio obligatorio para equipos con Windows 10 en [el apéndice a: lista de comprobación para la implementación de perfiles de usuario móviles](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Cambios de características en la versión más reciente de Windows Update. |
| 23 de enero de 2017 | Se ha agregado un paso al [paso 4: opcionalmente, crear un GPO para perfiles de usuario móviles](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) para delegar permisos de lectura a los usuarios autenticados, lo que ahora es necesario debido a una actualización de seguridad Directiva de grupo.|Cambios de seguridad en el procesamiento de directiva de grupo. |
| 29 de diciembre de 2016 | Se ha agregado un vínculo en [el paso 8: habilitar el GPO de perfiles de usuario móviles](#step-8-enable-the-roaming-user-profiles-gpo) para que sea más fácil obtener información sobre cómo establecer Directiva de grupo para equipos principales. También se han corregido un par de referencias a los pasos 5 y 6 que tenían los números incorrectos.|Comentarios del cliente. |
| 5 de diciembre de 2016 | Se ha agregado información que explica un problema de movilidad de configuración del menú Inicio. | Comentarios del cliente. |
| 6 de julio de 2016 | Se han agregado sufijos de versión de Perfil de Windows 10 en el [Apéndice B: información de referencia](#appendix-b-profile-version-reference-information)de la versión de perfil. También se han quitado Windows XP y Windows Server 2003 de la lista de sistemas operativos compatibles. | Actualizaciones para las nuevas versiones de Windows y la información sobre las versiones de Windows que ya no se admiten. |
| 7 de julio de 2015 | Se agregó el requisito y el paso para deshabilitar la disponibilidad continua al usar un servidor de archivos en clúster. | Los recursos compartidos de archivos en clúster tienen un rendimiento mejor para pequeñas operaciones de escritura (que son típicas con los perfiles de usuario móviles) cuando está deshabilitada la disponibilidad continua. |
| 19 de marzo de 2014 | Sufijos de versión de perfil en mayúsculas (. V2,. V3,. V4) en el [Apéndice B: información de referencia](#appendix-b-profile-version-reference-information)de la versión de perfil. | Aunque Windows no distingue entre mayúsculas y minúsculas, si usa NFS con el recurso compartido de archivos, es importante tener las mayúsculas y minúsculas correctas para el sufijo del perfil. |
| 9 de octubre de 2013 | Se revisó para Windows Server 2012 R2 y Windows 8.1, se aclararon algunas cosas y se agregaron las [consideraciones al usar perfiles de usuario móviles en varias versiones de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) y [Apéndice B: información de referencia](#appendix-b-profile-version-reference-information) de la versión de perfil. | Actualizaciones para nueva versión; Comentarios de los clientes. |

## <a name="more-information"></a>Información adicional

- [Implementar el redireccionamiento de carpetas, los Archivos sin conexión y los perfiles de usuario móviles](deploy-folder-redirection.md)
- [Implementar equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles](deploy-primary-computers.md)
- [Implementación de la administración de estado de usuario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Declaración de soporte técnico de Microsoft sobre los datos de Perfil de usuario replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Transferir localmente aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)