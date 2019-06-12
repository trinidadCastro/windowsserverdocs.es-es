---
Title: Implementar perfiles de usuario móviles
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: e6e2e32ff9aeb1b3bcfc8fed9027c7e92e13b118
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812486"
---
# <a name="deploying-roaming-user-profiles"></a>Implementar perfiles de usuario móviles

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En este tema se describe cómo usar Windows Server para implementar [perfiles de usuario móviles](folder-redirection-rup-overview.md) a los equipos cliente de Windows. Los perfiles de usuario móviles redirecciona los perfiles de usuario a un recurso compartido de archivos para que los usuarios reciben el mismo sistema operativo y la configuración de la aplicación en varios equipos.

Para obtener una lista de los cambios recientes en este tema, consulte el [historial de cambios](#change-history) sección de este tema.

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), actualizamos [paso 4: Opcionalmente, crear un GPO para perfiles de usuario móviles](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) en este tema para que Windows pueden aplicar correctamente la directiva de perfiles de usuario móviles (y no volverá a directivas locales en los equipos afectados).

> [!IMPORTANT]
>  Las personalizaciones de inicio se pierde después de una actualización en contexto de sistema operativo en la configuración siguiente:
> - Los usuarios están configurados para un perfil móvil
> - Los usuarios pueden realizar cambios en el inicio
>
> Como resultado, el menú Inicio se restablece al valor predeterminado de la nueva versión del sistema operativo después de que el sistema operativo actualización en contexto. Soluciones alternativas, consulte [Apéndice C: Trabajo restablece unos diseños de menú Inicio después de las actualizaciones](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Requisitos previos

### <a name="hardware-requirements"></a>Requisitos de hardware

Perfiles de usuario móvil requiere un equipo basado en x64 64 o x86; no se admite Windows RT

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
- Para usar el soporte de equipo principal en perfiles de usuario móviles, hay adicionales para equipos cliente y los requisitos de esquema de Active Directory. Para obtener más información, consulte [implementar equipos principales para redirección de carpetas y perfiles de usuario móviles](deploy-primary-computers.md).
- El diseño de menú no se transferirán en Windows 10, Windows Server 2019 o Windows Server 2016 si usa más de un PC, Host de sesión de escritorio remoto o infraestructura de escritorio virtualizado (VDI) el servidor de inicio de un usuario. Como alternativa, puede especificar un diseño de inicio como se describe en este tema. O bien puede hacer uso de discos de perfil de usuario, que se mueven correctamente la configuración del menú Inicio cuando se usa con servidores de Host de sesión de escritorio remoto o VDI. Para obtener más información, consulte [facilitar la administración de datos de usuario con discos de perfil de usuario en Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Consideraciones al usar perfiles de usuario móviles en varias versiones de Windows

Si vas a usar perfiles de usuario móviles en diferentes versiones de Windows, te recomendamos que realices las acciones siguientes:

- Configura Windows para mantener versiones de perfiles separadas para cada versión de sistema operativo. Esto ayuda a evitar problemas no deseados y no previstos, como daños en perfiles.
- Usa la redirección de carpetas para almacenar archivos de usuario, como documentos e imágenes, fueran de los perfiles de usuario. Esto permite que los mismos archivos estén disponibles para usuarios de diferentes versiones de sistemas operativos. Además, reduce el tamaño de los perfiles y hace que los inicios de sesión sean más rápidos.
- Asigna espacio suficiente para perfiles de usuario móviles. Si permites el uso de dos versiones distintas del sistema operativo, los perfiles se doblarán en número (y, por lo tanto, el espacio total consumido), ya que se mantiene un perfil separado para cada versión del sistema operativo.
- No use los perfiles de usuario móviles en equipos que ejecuten Windows Vista o Windows Server 2008 y Windows 7 o Windows Server 2008 R2. No se admite la itinerancia entre estas versiones de sistema operativo debido a incompatibilidades en sus versiones de perfiles.
- Informa a tus usuarios que los cambios realizados en una versión del sistema operativo no se transferirán a la otra versión del sistema operativo.
- Al mover el entorno a una versión de Windows que usa una versión diferente de perfil (por ejemplo, para Windows 10, versión 1607 de Windows 10: consulte [Apéndice B: Información de referencia de la versión de perfil](#appendix-b-profile-version-reference-information) para obtener una lista), los usuarios reciben un perfil de usuario móvil nuevo y vacío. Puede minimizar el impacto de la obtención de un nuevo perfil mediante el uso de la redirección de carpetas para redirigir carpetas comunes. No hay un método admitido para migrar perfiles de usuario móviles de la versión de un perfil a otro.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Paso 1: Habilitar el uso de versiones de perfiles separadas

Si va a implementar perfiles de usuario móviles en equipos que ejecutan Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012, se recomienda realizar un par de cambios en el entorno de Windows antes de implementar. Estos cambios ayudarán a garantizar que las actualizaciones de sistemas operativos futuras se produzcan sin problemas y facilita la capacidad para ejecutar de forma simultánea diferentes versiones de Windows con perfiles de usuario móviles.

Para realizar estos cambios, haz lo siguiente.

1. Descarga e instala la actualización de software correspondiente en todos los equipos donde vayas a usar perfiles móviles, obligatorios, superobligatorios o perfiles de dominio predeterminados:

    - Windows 8.1 o Windows Server 2012 R2: instale la actualización de software descrita en el artículo [2887595](http://support.microsoft.com/kb/2887595) en Microsoft Knowledge Base (cuando se publique).
    - Windows 8 o Windows Server 2012: instale la actualización de software que se describe en el artículo [2887239](http://support.microsoft.com/kb/2887239) de Microsoft Knowledge Base.

2. En todos los equipos que ejecutan Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012 que vaya a usar perfiles de usuario móviles, use el Editor del registro o directiva de grupo para crear el registro siguiente valor DWORD de clave y establecerla para `1`. Para obtener más información sobre cómo crear claves del Registro mediante Directiva de grupo, consulte [Configuración de un elemento del Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

Aquí le mostramos cómo crear un grupo de seguridad para perfiles de usuario móviles:

1. Abra el Administrador de servidor en un equipo con el centro de administración de Active Directory instalado.
2. En el **herramientas** menú, seleccione **centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Haga clic en el dominio u OU correspondiente, seleccione **New**y, a continuación, seleccione **grupo**.
4. En la ventana **Crear grupo** , en la sección **Grupo** , especifique la configuración siguiente:

    - En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Usuarios y equipos de perfiles de usuario móviles**.
    - En **ámbito de grupo**, seleccione **seguridad**y, a continuación, seleccione **Global**.

5. En el **miembros** sección, seleccione **agregar**. Aparece el cuadro de diálogo Seleccionar Usuarios, Contactos, Equipos, Cuentas de servicio o Grupos.
6. Si desea incluir las cuentas de equipo en el grupo de seguridad, seleccione **tipos de objeto**, seleccione el **equipos** casilla de verificación y, a continuación, seleccione **Aceptar**.
7. Escriba los nombres de los usuarios, grupos o equipos a la que va a implementar perfiles de usuario móviles, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** nuevo.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Paso 3: Crear un recurso compartido de archivos para perfiles de usuario móviles

Si aún no tiene un recurso compartido de archivos para la itinerancia perfiles de usuario (independientemente de los recursos compartidos para carpetas redirigidas para evitar el almacenamiento en caché accidental de la carpeta de perfiles móviles), use el procedimiento siguiente para crear un recurso compartido de archivos en un servidor que ejecuta Windows Servidor.

> [!NOTE]
> Alguna funcionalidad podría diferir o no estar disponible según la versión de Windows Server que está usando.

Aquí le mostramos cómo crear un recurso compartido de archivos en Windows Server:

1. En el panel de navegación del administrador del servidor, seleccione **File and Storage Services**y, a continuación, seleccione **recursos compartidos** para mostrar la página de recursos compartidos.
2. En el icono recursos compartidos, seleccione **tareas**y, a continuación, seleccione **nuevo recurso compartido**. Se abrirá el Asistente para nuevo recurso compartido.
3. En el **Seleccionar perfil** , seleccione **recurso compartido SMB-rápido**. Si tiene instalado el Administrador de recursos de servidor de archivos y usas las propiedades de administración de carpetas, en su lugar, seleccione **recurso compartido SMB - avanzado**.
4. En la página **Ubicación del recurso compartido** , selecciona el servidor y el volumen donde quieras crear el recurso compartido.
5. En la página **Nombre del recurso compartido**, escribe un nombre para el recurso compartido (por ejemplo, **Perfiles de usuario$** ) en el cuadro **Nombre del recurso compartido** box.

    > [!TIP]
    > Al crear el recurso compartido, puedes ocultarlo colocando un ```$``` después del nombre del recurso compartido. Esto hace que no se muestre el recurso compartido a los usuarios ocasionales.

6. En la página **Más opciones**, desactive la casilla **Habilitar disponibilidad continua**, si la hay, y, de manera opcional, seleccione las casillas **Habilitar enumeración basada en el acceso** y **Cifrar acceso a datos**.
7. En el **permisos** página, seleccione **personalizar permisos...** . Se abrirá el cuadro de diálogo Configuración de seguridad avanzada.
8. Seleccione **deshabilitar herencia**y, a continuación, seleccione **convertir permisos heredados en permiso explícito en este objeto**.
9. Establezca los permisos tal como se describe en [perfiles de usuario móviles de hospedaje de recurso compartido los permisos necesarios para el archivo](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) y se muestra en la siguiente captura de pantalla, elimina los permisos de cuentas y grupos que están ocultos y agregar especiales permisos para el grupo de usuarios de perfiles de usuario móviles y equipos que creó en el paso 1.
    
    ![Ventana de configuración de seguridad que muestra los permisos tal como se describe en la tabla 1 avanzada](media/advanced-security-user-profiles.jpg)
    
    **Ilustración 1** Establecer los permisos para el recurso compartido de perfiles de usuario móviles
10. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Propiedades de administración** , seleccione el valor de uso de carpeta **Archivos de usuario** .
11. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Cuota** , seleccione (de manera opcional) la cuota que quiera aplicar a los usuarios del recurso compartido.
12. En el **confirmación** página, seleccione **crear.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Permisos necesarios para el archivo compartido hospedaje perfiles de usuario móviles

| Cuenta de usuario | Acceso | Se aplica a |
|   -   |   -   |   -   |
|   Sistema    |  Control total     |  Esta carpeta, subcarpetas y archivos     |
|  Administradores     |  Control total     |  Solo esta carpeta     |
|  Creador/propietario     |  Control total     |  Solo subcarpetas y archivos     |
| Grupo de seguridad de usuarios que necesitan colocar datos en recursos compartidos (usuarios y equipos de perfiles de usuario móviles)      |  Listar carpeta / leer datos *(permisos avanzados)* <br />Crear carpetas / Anexar datos *(permisos avanzados)* |  Solo esta carpeta     |
| Otros grupos y cuentas   |  Ninguno (quitar)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Paso 4: De manera opcional, crea un GPO para perfiles de usuario móviles

Si aún no has creado un GPO para la configuración de los perfiles de usuario móviles, haz lo siguiente para crear un GPO vacío para usarlo con perfiles de usuario móviles. Este GPO permite configurar opciones de perfiles de usuario móviles (como soporte de equipo principal, que se explica por separado) y, además, también puede usarse para habilitar perfiles de usuario móviles en equipos, ya que esto suele hacerse al implementar en entornos de escritorio virtualizados o con Servicios de Escritorio remoto.

Aquí le mostramos cómo crear un GPO para perfiles de usuario móviles:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. Desde el **herramientas** menú, seleccione **Group Policy Management**. Se abrirá Administración de directivas de grupo.
3. Haga clic en el dominio o unidad organizativa en la que desea instalar perfiles de usuario móviles, a continuación, seleccione **crear un GPO en este dominio y vincularlo aquí**.
4. En el **nuevo GPO** cuadro de diálogo, escriba un nombre para el GPO (por ejemplo, **configuración de perfil de usuario móvil**) y, a continuación, seleccione **Aceptar**.
5. Haz clic con el botón secundario en el nuevo GPO y desactiva la casilla **Vínculo habilitado** . Esto no permite que se aplique el GPO hasta que termine de configurarlo.
6. Selecciona el GPO. En el **filtrado de seguridad** sección de la **ámbito** ficha, seleccione **usuarios autenticados**y, a continuación, seleccione **quitar** para evitar que el GPO desde que se va a aplicar a todo el mundo.
7. En el **filtrado de seguridad** sección, seleccione **agregar**.
8. En el **Seleccionar usuario, equipo o grupo** cuadro de diálogo, escriba el nombre de la seguridad de grupo que ha creado en el paso 1 (por ejemplo, **usuario perfiles de usuarios y equipos móviles**) y, a continuación, seleccione **Aceptar** .
9. Seleccione el **delegación** ficha, seleccione **agregar**, tipo **usuarios autenticados**, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** nuevo Aceptar el valor predeterminado permisos de lectura.
    
    Este paso es necesario debido a cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>Debido a los cambios de seguridad realizados en [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), ahora debe proporcionar los permisos de lectura de grupo delegado usuarios autenticados al GPO: en caso contrario, el GPO no se aplican a los usuarios, o si se ha aplicado, se quita el GPO, Redireccionamiento de perfiles de usuario hasta que el equipo local. Para obtener más información, consulte [implementación de grupo Directiva de seguridad Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Paso 5: Configurar los perfiles de usuario móviles en cuentas de usuario (opcional)

Si implementas perfiles de usuario móviles en cuentas de usuario, haz lo siguiente para especificar perfiles de usuario móviles para cuentas de usuario en Servicios de dominio de Active Directory. Si va a implementar perfiles de usuario móviles a equipos, como suele realizarse para servicios de escritorio remoto o implementaciones de escritorios virtualizados, en su lugar, use el procedimiento descrito en [paso 6: Opcionalmente, configurar perfiles de usuario móviles en equipos](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Si configuras perfiles de usuario móviles en cuentas de usuario mediante Active Directory y en equipos mediante Directiva de grupo, la configuración de directiva basada en equipos tiene prioridad.

Aquí le mostramos cómo configurar perfiles de usuario móviles en cuentas de usuario:

1. En el Centro de administración de Active Directory, navega hasta el contenedor **Usuarios** (o bien OU) en el dominio correspondiente.
2. Seleccione todos los usuarios a la que desea asignar un perfil de usuario móvil, haga clic en los usuarios y, a continuación, seleccione **propiedades**.
3. En el **perfil** sección, seleccione el **ruta de acceso de perfil:** casilla de verificación y, a continuación, escriba la ruta de acceso al recurso compartido de archivo donde desea almacenar el perfil de usuario móvil del usuario, seguido de `%username%` (que es sustituido automáticamente con el usuario nombre de la primera vez que el usuario inicia sesión). Por ejemplo:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Para especificar un perfil de usuario móvil obligatorio, especifique la ruta de acceso al archivo NTuser.man que creaste anteriormente, por ejemplo, `fs1.corp.contoso.comUser Profiles$default`. Para obtener más información, consulte [crear perfiles de usuario obligatorios](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Seleccione **Aceptar**.

> [!NOTE]
> De manera predeterminada, se permite implementar todas las aplicaciones basadas en Windows® en tiempo de ejecución (Tienda Windows) al usar los perfiles de usuario móviles. Pero, al usar un perfil especial, las aplicaciones no se implementan de manera predeterminada. Los perfiles especiales son perfiles de usuario donde se descartan los cambios cuando el usuario cierra la sesión:
> <br><br>Para eliminar las restricciones en la implementación de aplicaciones para perfiles especiales, habilite la configuración de directiva **Allow deployment operations in special profiles** (ubicada en Configuración del equipo\Directivas\Plantillas administrativas\Componentes de Windows\Implementación de paquetes de aplicaciones). Pero las aplicaciones implementadas en este escenario dejarán datos almacenados en el equipo, que podrían acumularse si, por ejemplo, hubiera cientos de usuarios en un mismo equipo. Para limpiar las aplicaciones, busque o desarrolle una herramienta que utiliza el [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) API para limpiar paquetes de aplicaciones para los usuarios que ya no tienen un perfil en el equipo.
> <br><br>Para obtener información general sobre las aplicaciones de la Tienda Windows, consulte [Administrar el acceso de cliente a la Tienda Windows](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Paso 6: Configurar los perfiles de usuario móviles en equipos (opcional)

Si implementas perfiles de usuario móviles en equipos, como suele realizarse para Servicios de Escritorio remoto o en implementaciones de escritorios virtualizados, haz lo siguiente. Si implementas perfiles de usuario móviles en cuentas de usuario, en su lugar, use el procedimiento descrito en [paso 5: Opcionalmente, configurar perfiles de usuario móviles en cuentas de usuario](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Puede usar la directiva de grupo para aplicar perfiles de usuario móviles a equipos que ejecutan Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

> [!NOTE]
> Si configuras perfiles de usuario móviles en equipos mediante Directiva de grupo y en cuentas de usuario mediante Active Directory, la configuración de directiva basada en equipos tiene prioridad.

Aquí le mostramos cómo configurar perfiles de usuario móviles en equipos:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. Desde el **herramientas** menú, seleccione **Group Policy Management**. Aparecerá la administración de directivas de grupo.
3. En administración de directivas de grupo, haga clic en el GPO que creaste en el paso 3 (por ejemplo, **configuración de perfiles de usuario móviles**) y, a continuación, seleccione **editar**.
4. En la ventana Editor de administración de directivas de grupo, navega hasta **Configuración del equipo**, **Directivas**, **Plantillas administrativas**, **Sistema** y, finalmente, **Perfiles de usuario**.
5. Haga clic en **establecer ruta de perfil para todos los usuarios conectados a este equipo móvil** y, a continuación, seleccione **editar**.
    > [!TIP]
    > Si se configura una carpeta particular para los usuarios, esta será la carpeta predeterminada usada por algunos programas como Windows PowerShell. Puedes configurar una ubicación alternativa, ya sea local o de red, por usuario en la sección **Carpeta particular** de las propiedades de la cuenta de usuario en AD DS. Para configurar la ubicación de carpeta particular para todos los usuarios de un equipo que ejecuta Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en un entorno de escritorio virtual, habilite el **carpeta principal de usuario de conjunto**  configuración de directiva y, a continuación, especifique la letra de unidad y recursos compartidos de archivos de mapa (o especificar una carpeta local). No uses variables de entorno ni puntos suspensivos. El alias del usuario se anexa al final de la ruta de acceso especificada durante el inicio de sesión del usuario.
6. En el **propiedades** cuadro de diálogo, seleccione **habilitado**
7. En el **a los usuarios conectados a este equipo deben usar esta ruta de perfil móvil** , escriba la ruta de acceso al recurso compartido de archivo donde desea almacenar el perfil de usuario móvil del usuario, seguido de `%username%` (que será sustituido automáticamente con el usuario nombre la primera vez que el usuario inicia sesión). Por ejemplo:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Para especificar un perfil usuario móvil obligatorio, que es un perfil preconfigurado en la que los usuarios no se puede realizar cambios permanentes (los cambios se restablecen cuando el usuario cierra), especifique la ruta de acceso al archivo NTuser.man que creaste anteriormente, por ejemplo, `\\fs1.corp.contoso.com\User Profiles$\default`. Para obtener más información, consulte [Creación de un perfil de usuario obligatorio](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Seleccione **Aceptar**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Paso 7: Especificar opcionalmente un diseño de inicio para equipos Windows 10

Puede usar la directiva de grupo para aplicar un diseño específico de menú de inicio para que los usuarios ven el mismo diseño de inicio en todos los equipos. Si desea que los usuarios inicien sesión en más de un PC, que tengan un diseño coherente de inicio a través de los equipos, asegúrese de que el GPO se aplica a todos sus equipos.

Para especificar un diseño de inicio, haga lo siguiente:

1. Actualizar los equipos con Windows 10 a Windows 10 versión 1607 (también conocida como la actualización de aniversario) o versiones más recientes e instalar la actualización acumulativa del 14 de marzo de 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) o una versión posterior.
2. Cree un archivo completo o parcial inicio menú Diseño XML. Para ello, consulte [personalizar y diseño de inicio de exportación](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Si especifica un *completa* diseño de inicio, un usuario no puede personalizar cualquier parte del menú Inicio. Si especifica un *parcial* diseño de inicio, los usuarios pueden personalizar todo excepto los grupos bloqueados de iconos que especifique. Sin embargo, con un diseño parcial de inicio, las personalizaciones del usuario al menú Inicio no se transferirán a otros equipos.
3. Usar Directiva de grupo para aplicar el diseño de inicio personalizado para el GPO que creaste en los perfiles de usuario móviles. Para ello, consulte [usar Directiva de grupo para aplicar un diseño personalizado de inicio en un dominio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Usar Directiva de grupo para establecer el valor del registro siguiente en los equipos de Windows 10. Para ello, consulte [configurar un elemento del registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Acción**   | **Update**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| Ruta de acceso de clave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nombre de valor   | **SpecialRoamingOverrideAllowed** |
| Tipo de valor   | **REG_DWORD**               |
| Datos de valor   | **1** (o **0** para deshabilitar) |
| Base         | **Decimal**                 |

5. (Opcional) Habilitar las optimizaciones de inicio de sesión por primera vez realizar el inicio de sesión más rápido por los usuarios. Para ello, consulte [aplicar directivas para mejorar el tiempo de inicio de sesión](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Opcional) Disminuyen aún más los tiempos de inicio de sesión mediante la eliminación de las aplicaciones innecesarias de la imagen base de Windows 10 que se usa para implementar los equipos cliente. 2019 de Windows Server y Windows Server 2016 no tienen ninguna aplicación aprovisionada previamente, por lo que puede omitir este paso en las imágenes de servidor.
    - Para quitar aplicaciones, use el [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) cmdlet de Windows PowerShell para desinstalar las aplicaciones siguientes. Si ya se han implementado los equipos para crear un script la eliminación de estas aplicaciones mediante el [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft.windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft.BingWeather\_8wekyb3d8bbwe
      - Microsoft.DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft.Getstarted\_8wekyb3d8bbwe
      - Microsoft.Windows.Photos\_8wekyb3d8bbwe
      - Microsoft.WindowsCamera\_8wekyb3d8bbwe
      - Microsoft.WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft.WindowsStore\_8wekyb3d8bbwe
      - Microsoft.XboxApp\_8wekyb3d8bbwe
      - Microsoft.XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft.ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>Desinstalación de estas aplicaciones reduce los tiempos de inicio de sesión, pero puede dejarlos instalados si necesita la implementación de cualquiera de ellos.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Paso 8: Habilitar los GPO de perfiles de usuario móviles

Si configuras perfiles de usuario móviles en equipos mediante Directiva de grupo, o si personalizas otra configuración de perfiles de usuario móviles mediante Directiva de grupo, el paso siguiente es habilitar el GPO y permitir aplicarlo en los usuarios afectados.

>[!TIP]
>Si tiene previsto implementar la compatibilidad con equipo principal, hágalo ahora, antes de habilitar el GPO. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal. Para la configuración de directiva específica, consulte [implementar equipos principales para redirección de carpetas y perfiles de usuario móviles](deploy-primary-computers.md).

Aquí le mostramos cómo habilitar el GPO de perfil de usuario móviles:

1. Abra Administración de directivas de grupo.
2. Haga clic en el GPO que ha creado y, a continuación, seleccione **vínculo habilitado**. Aparecerá una casilla junto al elemento de menú.

## <a name="step-9-test-roaming-user-profiles"></a>Paso 9: Los perfiles de usuario móviles

Para probar perfiles de usuario móviles, inicia sesión en un equipo con una cuenta de usuario configurada para perfiles de usuario móviles, o bien inicia sesión en un equipo configurado para perfiles de usuario móviles. Después, confirma que el perfil es redirigido.

Aquí le mostramos cómo probar perfiles de usuario móviles:

1. Inicia sesión en un equipo principal (si has habilitado el soporte de equipo principal) con una cuenta de usuario en la que hayas habilitado los perfiles de usuario móviles. Si has habilitado perfiles de usuario móviles en equipos específicos, inicia sesión en uno de estos equipos.
2. Si el usuario ha iniciado sesión en el equipo anteriormente, abre un símbolo del sistema con privilegios elevados y escribe el comando siguiente para asegurarte de que se aplique la última configuración de directiva de grupo en el equipo cliente:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Para confirmar que el perfil de usuario está en itinerancia, abra **Panel de Control**, seleccione **sistema y seguridad**, seleccione **sistema**, seleccione **configuración avanzada del sistema** , seleccione **configuración** en los perfiles de usuario de la sección y, a continuación, busque **Roaming** en el **tipo** columna.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Apéndice A: Lista de comprobación para la implementación de perfiles de usuario móviles

| Estado                     | Acción                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparar el dominio<br>-Unir equipos al dominio<br>-Habilitar el uso de versiones de perfiles separadas<br>-Crear cuentas de usuario<br>-(Opcional) implementar la redirección de carpetas |
| ☐<br><br><br>             | 2. Crear un grupo de seguridad para perfiles de usuario móviles<br>-Nombre del grupo:<br>-Miembros: |
| ☐<br><br>                 | 3. Crear un recurso compartido de archivos para perfiles de usuario móviles<br>: Nombre del recurso compartido archivo: |
| ☐<br><br>                 | 4. Crear un GPO para perfiles de usuario móviles<br>-Nombre del GPO:|
| ☐                         | 5. Configurar la directiva de perfiles de usuario móviles    |
| ☐<br>☐<br>☐              | 6. Habilitar perfiles de usuario móviles<br>¿-Habilitada en AD DS en cuentas de usuario?<br>¿-Habilitado en directiva de grupo en cuentas de equipo?<br> |
| ☐                         | 7. (Opcional) Especificar un diseño de inicio obligatorio para equipos Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Opcional) Habilitar soporte de equipo principal<br>-Designar equipos principales para los usuarios<br>-Ubicación de usuario y asignaciones de equipos principales:<br>-(Opcional) habilitar soporte de equipo principal para redirección de carpetas<br>¿-Basadas en equipos o en función de usuario?<br>-(Opcional) habilitar soporte de equipo principal para perfiles de usuario móviles |
| ☐                        | 9. Habilitar los GPO de perfiles de usuario móviles                |
| ☐                        | 10. Los perfiles de usuario móviles                         |

## <a name="appendix-b-profile-version-reference-information"></a>Apéndice B: Información de referencia sobre la versión del perfil

Cada perfil tiene una versión de perfil que se corresponde aproximadamente a la versión de Windows en el que se usa el perfil. Por ejemplo, Windows 10, versión 1703 y versión 1607 ambos usan la. Versión del perfil V6. Microsoft crea una nueva versión de perfil solo cuando sea necesario para mantener la compatibilidad, que es ¿por qué no todas las versiones de Windows incluyen una nueva versión de perfil.

La tabla siguiente contiene las ubicaciones de los perfiles de usuario móviles en diferentes versiones de Windows.

| Versión del sistema operativo | Ubicación del perfil de usuario móvil |
| --- | --- |
| Windows XP y Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista y Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 y Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 y Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (después de aplicar la clave de registro y actualización de software)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes el software se aplican clave del registro y actualización) |
| Windows 8.1 y Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (después de aplicar la clave de registro y actualización de software)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes el software se aplican clave del registro y actualización) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versión 1703 y versión 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Apéndice C: Trabajo restablece unos diseños de menú Inicio después de las actualizaciones

Estas son algunas maneras de solucionar los diseños de menú Inicio reinicia después de una actualización en contexto:

- Si solo un usuario nunca usa el dispositivo y el Administrador de TI usa una estrategia de implementación de sistema operativo administrada como SCCM puede hacer lo siguiente:
    
  1. Exportar el diseño del menú Inicio con Export Startlayout antes de la actualización 
  2. Importar el diseño del menú Inicio con importación StartLayout después de la bienvenida de Windows, pero antes de que el usuario inicia sesión  
 
     > [!NOTE] 
     > Importar un StartLayout modifica el perfil de usuario predeterminado. Todos los perfiles de usuario creados después de la importación se ha producido obtendrá el diseño de inicio importado.
 
- Pueden optar por los administradores de TI para administrar el diseño de inicio con la directiva de grupo. La directiva de grupo proporciona una solución de administración centralizada para aplicar un diseño de inicio de estandarizado a los usuarios. Existen 2 modos a los modos de mediante la directiva de grupo para la administración de inicio. Bloqueo de seguridad completa y bloqueo de seguridad parcial. El escenario completo de bloqueo impide que los usuarios de realizar cambios en el diseño de inicio. El escenario de bloqueo parcial permite el usuario realizar cambios en un área específica de inicio. Para obtener más información, consulte [personalizar y diseño de inicio de exportación](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Los cambios realizados por usuario en el escenario de bloqueo parcial todavía se perderán durante la actualización.

- Permitir que el inicio de restablecimiento de diseño se producen y permitir que los usuarios finales volver a configurar el inicio. A los usuarios finales puede enviarse una notificación por correo electrónico u otra notificación esperar sus diseños inicio restablecerán después de actualiza el sistema operativo a menor impacto. 

# <a name="change-history"></a>Historial de cambios

En la tabla siguiente se resumen los cambios más importantes realizados en este tema.

| Fecha | Descripción |Reason|
| --- | ---         | ---   |
| El 1 de mayo de 2019 | Se ha agregado las actualizaciones para Windows Server 2019 |
| 10 de abril de 2018 | Se ha agregado discusión de cuando se pierden las personalizaciones del usuario para que se inicie tras la actualización en contexto del sistema operativo|Problema conocido de llamada. |
| 13 de marzo de 2018 | Actualizado para Windows Server 2016 | Movido fuera de la biblioteca de las versiones anteriores y se actualiza para la versión actual de Windows Server. |
| 13 de abril de 2017 | Se ha agregado información de perfil para Windows 10, versión 1703 y se ha aclarado el trabajo de las versiones de perfil móvil cómo al actualizar los sistemas operativos, consulte [consideraciones al usar perfiles de usuario móviles en varias versiones de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Comentarios del cliente. |
| 14 de marzo de 2017 | Se ha agregado el paso opcional para especificar un diseño de inicio obligatorio para equipos Windows 10 en [Apéndice A: Lista de comprobación para implementar perfiles de usuario móviles](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Cambios de las características de actualización más reciente de Windows. |
| 23 de enero de 2017 | Agrega un paso para [paso 4: Opcionalmente, crear un GPO para perfiles de usuario móviles](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) para delegar permisos de lectura a los usuarios autenticados, que ahora es necesario debido a una actualización de seguridad de la directiva de grupo.|Cambios de seguridad para el procesamiento de directiva de grupo. |
| 29 de diciembre de 2016 | Agrega un vínculo en [paso 8: Habilitar el GPO de perfiles de usuario móviles](#step-8-enable-the-roaming-user-profiles-gpo) para que sea más fácil obtener información sobre cómo establecer la directiva de grupo para equipos principales. También se ha corregido un par de las referencias a mal los pasos 5 y 6 que tenían los números.|Comentarios del cliente. |
| 5 de diciembre de 2016 | Se ha agregado información que explica la itinerancia problema una configuración del menú Inicio. | Comentarios del cliente. |
| 6 de julio de 2016 | Agregar sufijos de versión de perfil de Windows 10 en [Apéndice B: Información de referencia de la versión de perfil](#appendix-b-profile-version-reference-information). También quita Windows XP y Windows Server 2003 en la lista de sistemas operativos compatibles. | Actualizaciones de las nuevas versiones de Windows y quitado información acerca de las versiones de Windows que ya no se admiten. |
| 7 de julio de 2015 | Se agregó el requisito y el paso para deshabilitar la disponibilidad continua al usar un servidor de archivos en clúster. | Los recursos compartidos de archivos en clúster tienen un rendimiento mejor para pequeñas operaciones de escritura (que son típicas con los perfiles de usuario móviles) cuando está deshabilitada la disponibilidad continua. |
| 19 de marzo de 2014 | Sufijos de versión de perfil en mayúscula (. V2. V3. V4) en [Apéndice B: Información de referencia de la versión de perfil](#appendix-b-profile-version-reference-information). | Aunque Windows distingue mayúsculas de minúsculas, si usas NFS con el recurso compartido de archivos, es importante contar con las mayúsculas correctas (en mayúsculas) para el sufijo del perfil. |
| 9 de octubre de 2013 | Revisado para Windows Server 2012 R2 y Windows 8.1, se ha aclarado algunas cosas y agrega el [consideraciones al usar perfiles de usuario móviles en varias versiones de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) y [Apéndice B: Información de referencia de la versión de perfil](#appendix-b-profile-version-reference-information) secciones. | Actualizaciones para la nueva versión; comentarios del cliente. |

## <a name="more-information"></a>Más información

- [Implementar la redirección de carpetas, archivos sin conexión y perfiles de usuario móviles](deploy-folder-redirection.md)
- [Implementar equipos principales para redirección de carpetas y perfiles de usuario móviles](deploy-primary-computers.md)
- [Implementación de la administración de estado de usuario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Declaración de soporte técnico de Microsoft en torno a los datos de perfil de usuario replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Agregar o quitar aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)