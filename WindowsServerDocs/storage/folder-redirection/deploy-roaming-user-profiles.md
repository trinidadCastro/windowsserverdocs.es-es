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
ms.openlocfilehash: 8feed2adb606edfb6068d7fe10c18baf142077ac
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "76822348"
---
# <a name="deploying-roaming-user-profiles"></a>Implementación de perfiles de usuario móviles

>Se aplica a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

En este tema se describe cómo usar Windows Server para implementar [perfiles de usuario móviles](folder-redirection-rup-overview.md) en equipos cliente Windows. Los perfiles de usuarios móviles redireccionan los perfiles de usuarios a un recurso compartido de archivos para que los usuarios reciban la misma configuración del sistema operativo y la aplicación en varios equipos.

Para ver una lista de los cambios recientes realizados en este tema, consulta la sección [Historial de cambios](#change-history) de este tema.

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), hemos actualizado el apartado [Paso 4: De manera opcional, crea un GPO para perfiles de usuario móviles](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) de este tema para que Windows pueda aplicar correctamente la directiva de perfiles de usuario móviles (y no revertir a las directivas locales en los equipos afectados).

> [!IMPORTANT]
>  Las personalizaciones de usuario del menú Inicio se pierden después de una actualización local del sistema operativo en la configuración siguiente:
> - Los usuarios se configuran para un perfil móvil.
> - Los usuarios pueden realizar cambios en el menú Inicio.
>
> Como resultado, el menú Inicio se restablece al valor predeterminado de la nueva versión del sistema operativo después de la actualización local del sistema operativo. Para obtener soluciones alternativas, consulta el apartado [Apéndice C: Soluciones alternativas para el restablecimiento de los diseños del menú Inicio después de las actualizaciones](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Requisitos previos

### <a name="hardware-requirements"></a>Requisitos de hardware

Los perfiles de usuario móviles requieren un equipo basado en x64 o x86; no son compatibles con Windows RT.

### <a name="software-requirements"></a>Requisitos de software

Los perfiles de usuario móviles tienen los siguientes requisitos de software:

- Si vas a implementar perfiles de usuario móviles con redirección de carpetas en un entorno con perfiles de usuario locales, implementa la redirección de carpetas antes que los perfiles de usuario móviles para minimizar el tamaño de los perfiles móviles. Después de completar correctamente la redirección de las carpetas de usuario existentes, podrás implementar los perfiles de usuario móviles.
- Para administrar los perfiles de usuario móviles debes iniciar sesión como un miembro del grupo de seguridad Administradores de dominio, el grupo de seguridad Administradores de empresa o el grupo de seguridad Propietarios del creador de directivas de grupo.
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- Los equipos cliente deben unirse a los Servicios de dominio de Active Directory (AD DS) que estés administrando.
- Los equipos deben estar disponibles con Administración de directivas de grupo y tener instalado el Centro de administración de Active Directory.
- Debe haber disponible en servidor de archivos para hospedar perfiles de usuario móviles.

    - Si el recurso compartido de archivos usa espacios de nombres DFS, las carpetas DFS (vínculos) deben tener el mismo destino para evitar que los usuarios realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos usa Replicación DFS para replicar los contenidos con otro servidor, los usuarios deben poder acceder únicamente al servidor de origen para evitar que se realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos está en clúster, deshabilite la disponibilidad continua en el recurso compartido de archivos para evitar problemas de rendimiento.
- Para usar la compatibilidad con equipos principales en perfiles de usuario móviles, existen requisitos adicionales para los equipos cliente y el esquema de Active Directory. Para obtener más información, consulta [Implementación de equipos principales para la redirección de carpetas y los perfiles de usuario móviles](deploy-primary-computers.md).
- El diseño del menú Inicio de un usuario no se transferirá en Windows 10, Windows Server 2019 o Windows Server 2016 al usar más de un equipo, host de sesión de Escritorio remoto o servidor de infraestructura de escritorio virtualizado (VDI). Como solución alternativa, puedes especificar un diseño del menú Inicio como se describe en este tema. O bien, puedes usar discos de perfil de usuario, que transfieren correctamente la configuración del menú Inicio cuando se usan con servidores host de sesión de Escritorio remoto o servidores VDI. Para obtener más información, consulta [Administración de datos de usuario más fácil con discos de perfil de usuario en Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Consideraciones al usar perfiles de usuario móviles en varias versiones de Windows

Si vas a usar perfiles de usuario móviles en diferentes versiones de Windows, te recomendamos que realices las acciones siguientes:

- Configura Windows para mantener versiones de perfiles separadas para cada versión de sistema operativo. Esto ayuda a evitar problemas no deseados y no previstos, como daños en perfiles.
- Usa la redirección de carpetas para almacenar archivos de usuario, como documentos e imágenes, fueran de los perfiles de usuario. Esto permite que los mismos archivos estén disponibles para usuarios de diferentes versiones de sistemas operativos. Además, reduce el tamaño de los perfiles y hace que los inicios de sesión sean más rápidos.
- Asigna espacio suficiente para perfiles de usuario móviles. Si permites el uso de dos versiones distintas del sistema operativo, los perfiles se doblarán en número (y, por lo tanto, el espacio total consumido), ya que se mantiene un perfil separado para cada versión del sistema operativo.
- No uses perfiles de usuario móviles en equipos que ejecuten Windows Vista o Windows Server 2008 y Windows 7 o Windows Server 2008 R2. No se permite la itinerancia entre estas dos versiones del sistema operativo, ya que las versiones de los perfiles no son compatibles.
- Informa a tus usuarios de que los cambios realizados en una versión del sistema operativo no se transferirán a otra.
- Al transferir el entorno a una versión de Windows que usa otra versión de perfil (como de Windows 10 a Windows 10, versión 1607; consulta el apartado [Apéndice B: Información de referencia sobre la versión del perfil](#appendix-b-profile-version-reference-information) para ver una lista), los usuarios reciben un nuevo perfil de usuario móvil vacío. Puedes minimizar el impacto de obtener un nuevo perfil mediante el redireccionamiento de carpetas a fin de redirigir las carpetas habituales. No existe ningún método compatible para migrar perfiles de usuario móviles de una versión de perfil a otra.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Paso 1: Habilitar el uso de versiones de perfiles separadas

Si vas a implementar perfiles de usuario móviles en equipos que ejecuten Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012, te recomendamos que realices un par de cambios en el entorno de Windows antes de la implementación. Estos cambios ayudarán a garantizar que las actualizaciones de sistemas operativos futuras se produzcan sin problemas y facilita la capacidad para ejecutar de forma simultánea diferentes versiones de Windows con perfiles de usuario móviles.

Para realizar estos cambios, haz lo siguiente.

1. Descarga e instala la actualización de software correspondiente en todos los equipos en los que vayas a usar perfiles móviles, obligatorios, superobligatorios o de dominio predeterminados:

    - Windows 8.1 o Windows Server 2012 R2: instala la actualización de software que se describe en el artículo [2887595](https://support.microsoft.com/kb/2887595) de Microsoft Knowledge Base (cuando se publique).
    - Windows 8 o Windows Server 2012: instale la actualización de software que se describe en el artículo [2887239](https://support.microsoft.com/kb/2887239) de Microsoft Knowledge Base.

2. En todos los equipos que ejecuten Windows 8.1, Windows 8, Windows Server 2012 R2 o Windows Server 2012 en los que vayas a usar perfiles de usuario móviles, usa el Editor del Registro o una directiva de grupo para crear el siguiente valor DWORD de clave del Registro y establécelo en `1`. Para obtener más información sobre cómo crear claves del Registro mediante Directiva de grupo, consulte [Configuración de un elemento del Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

A continuación, se muestra cómo crear un grupo de seguridad para perfiles de usuario móviles:

1. Abre el Administrador del servidor en un equipo que tenga instalado el Centro de administración de Active Directory.
2. En el menú **Herramientas**, selecciona **Active Directory Administration Center** (Centro de administración de Active Directory). Aparece el Centro de administración de Active Directory.
3. Haz clic con el botón derecho en el dominio o la unidad organizativa correspondiente, selecciona **Nuevo** y elige **Grupo**.
4. En la ventana **Crear grupo** , en la sección **Grupo** , especifique la configuración siguiente:

    - En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Usuarios y equipos de perfiles de usuario móviles**.
    - En **Ámbito de grupo**, selecciona **Seguridad** y, a continuación, selecciona **Global**.

5. En la sección **Miembros**, selecciona **Agregar**. Aparece el cuadro de diálogo Seleccionar Usuarios, Contactos, Equipos, Cuentas de servicio o Grupos.
6. Si quieres incluir cuentas de equipo en el grupo de seguridad, selecciona **Tipos de objeto**, marca la casilla **Equipos** y selecciona **Aceptar**.
7. Escribe los nombres de los usuarios, grupos o equipos en los que quieras implementar los perfiles de usuario móviles, selecciona **Aceptar** y, a continuación, vuelve a seleccionar **Aceptar**.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Paso 3: Crear un recurso compartido de archivos para perfiles de usuario móviles

Si aún no tienes ningún recurso compartido de archivos para perfiles de usuario móviles (independiente de los recursos compartidos de las carpetas redirigidas para evitar el almacenamiento en caché accidental de la carpeta de perfiles móviles), haz lo siguiente para crear un recurso compartido de archivos en un servidor que ejecute Windows Server.

> [!NOTE]
> Algunas funciones pueden ser distintas o no estar disponibles en función de la versión de Windows Server que uses.

A continuación, se muestra cómo crear un recurso compartido de archivos en Windows Server:

1. En el panel de navegación Administrador del servidor, selecciona **Servicios de archivos y almacenamiento** y, después, elije **Recursos compartidos** para mostrar la página Recursos compartidos.
2. En el icono Recursos compartidos, selecciona **Tareas** y, después, **Nuevo recurso compartido**. Se abrirá el Asistente para nuevo recurso compartido.
3. En la página **Seleccionar perfil**, selecciona **Recurso compartido SMB – Rápido**. Si tienes instalado el Administrador de recursos del servidor de archivos y usas las propiedades de administración de carpetas, en su lugar selecciona **Recurso compartido SMB - Avanzado**.
4. En la página **Ubicación del recurso compartido** , selecciona el servidor y el volumen donde quieras crear el recurso compartido.
5. En la página **Nombre del recurso compartido** , escribe un nombre para el recurso compartido (por ejemplo, **Perfiles de usuario$** ) en el cuadro **Nombre del recurso compartido** box.

    > [!TIP]
    > Al crear el recurso compartido, puedes ocultarlo si escribes el símbolo ```$``` después del nombre del recurso compartido. Esto hace que no se muestre el recurso compartido a los usuarios ocasionales.

6. En la página **Más opciones** , desactive la casilla **Habilitar disponibilidad continua** , si la hay, y, de manera opcional, seleccione las casillas **Habilitar enumeración basada en el acceso** y **Cifrar acceso a datos** .
7. En la página **Permisos**, selecciona **Personalizar permisos...** . Se abrirá el cuadro de diálogo Configuración de seguridad avanzada.
8. Selecciona **Deshabilitar herencia** y, después, **Convert inherited permissions into explicit permission on this object** (Convertir los permisos heredados en permisos explícitos en este objeto).
9. Establece los permisos como se describe en [Permisos obligatorios para los perfiles de usuario móviles de hospedaje de recurso compartido de archivos](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) y se muestra en la captura de pantalla siguiente, elimina los permisos de grupos y cuentas que no aparezcan en la lista y agrega permisos especiales al grupo Usuarios y equipos de los perfiles de usuario móviles que creaste en el paso 1.
    
    ![Ventana Configuración de seguridad avanzada que muestra los permisos que se describen en la tabla 1](media/advanced-security-user-profiles.jpg)
    
    **Ilustración 1** Establecer los permisos para el recurso compartido de perfiles de usuario móviles
10. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Propiedades de administración** , seleccione el valor de uso de carpeta **Archivos de usuario** .
11. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Cuota** , seleccione (de manera opcional) la cuota que quiera aplicar a los usuarios del recurso compartido.
12. En la página **Confirmación**, haz clic en **Crear**.

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Permisos obligatorios para los perfiles de usuario móviles de hospedaje de recurso compartido de archivos

| Cuenta de usuario | Acceso | Aplicable a |
|   -   |   -   |   -   |
|   System    |  Control total     |  Esta carpeta, subcarpetas y archivos     |
|  Administradores     |  Control total     |  Solo esta carpeta     |
|  Creador/propietario     |  Control total     |  Solo subcarpetas y archivos     |
| Grupo de seguridad de usuarios que necesitan colocar datos en recursos compartidos (usuarios y equipos de perfiles de usuario móviles)      |  Mostrar carpeta o leer datos *(permisos avanzados)* <br />Crear carpetas o anexar datos *(permisos avanzados)* |  Solo esta carpeta     |
| Otros grupos y cuentas   |  Ninguno (quitar)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Paso 4: De manera opcional, crea un GPO para perfiles de usuario móviles

Si aún no has creado un GPO para la configuración de los perfiles de usuario móviles, haz lo siguiente para crear un GPO vacío para usarlo con perfiles de usuario móviles. Este GPO permite configurar opciones de perfiles de usuario móviles (como soporte de equipo principal, que se explica por separado) y, además, también puede usarse para habilitar perfiles de usuario móviles en equipos, ya que esto suele hacerse al implementar en entornos de escritorio virtualizados o con Servicios de Escritorio remoto.

A continuación se muestra cómo crear un GPO para perfiles de usuario móviles:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. En el menú **Herramientas**, selecciona **Administración de directivas de grupo**. Se abrirá Administración de directivas de grupo.
3. Haz clic con el botón derecho en el dominio o la unidad organizativa en que quieras instalar los perfiles de usuario móviles y, después, selecciona **Create a GPO in this domain, and Link it here** (Crear un GPO en este dominio y vincularlo aquí).
4. En el cuadro de diálogo **Nuevo GPO**, escribe un nombre para el GPO (por ejemplo, **Configuración de perfil de usuario móvil**) y selecciona **Aceptar**.
5. Haz clic con el botón secundario en el nuevo GPO y desactiva la casilla **Vínculo habilitado** . Esto no permite que se aplique el GPO hasta que termine de configurarlo.
6. Selecciona el GPO. En la sección **Filtrado de seguridad** de la pestaña **Ámbito**, selecciona **Usuarios autenticados** y, a continuación, selecciona **Quitar** para evitar que el GPO se aplique a todos los usuarios.
7. En la sección **Filtrado de seguridad**, selecciona **Agregar**.
8. En el cuadro de diálogo **Select User, Computer, or Group** (Seleccionar usuario, equipo o grupo), escribe el nombre del grupo de seguridad que creaste en el paso 1 (por ejemplo, **Usuarios y equipos de perfiles de usuario móviles**) y selecciona **Aceptar**.
9. Selecciona la pestaña **Delegación**, elige **Agregar**, escribe **Usuarios autenticados**, selecciona **Aceptar** y, a continuación, **Aceptar** de nuevo para aceptar los permisos de lectura predeterminados.
    
    Este paso es necesario debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), ahora debes conceder al grupo Usuarios autenticados permisos de lectura para el GPO; de lo contrario, el GPO no se aplicará a los usuarios o, si ya se ha aplicado, se quitará y se redirigirán los perfiles de usuario de nuevo al equipo local. Para obtener más información, consulta [Implementación de la actualización de seguridad de la directiva de grupo MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Paso 5: Configurar los perfiles de usuario móviles en cuentas de usuario (opcional)

Si implementas perfiles de usuario móviles en cuentas de usuario, haz lo siguiente para especificar perfiles de usuario móviles para cuentas de usuario en Active Directory Domain Services. Si implementas perfiles de usuario móviles en equipos, como suele realizarse para Servicios de Escritorio remoto o implementaciones de escritorios virtualizados, en su lugar sigue el procedimiento descrito en el apartado [Paso 6: Configurar los perfiles de usuario móviles en equipos (opcional)](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Si configuras perfiles de usuario móviles en cuentas de usuario mediante Active Directory y en equipos mediante Directiva de grupo, la configuración de directiva basada en equipos tiene prioridad.

A continuación, se muestra cómo configurar perfiles de usuario móviles en cuentas de usuario:

1. En el Centro de administración de Active Directory, navega hasta el contenedor **Usuarios** (o bien OU) en el dominio correspondiente.
2. Selecciona todos los usuarios que quieras asignar a un perfil de usuario móvil, haz clic con el botón derecho en los usuarios seleccionados y, después, selecciona **Propiedades**.
3. En la sección **Perfil**, selecciona la casilla **Ruta de acceso al perfil:** y, después, escribe la ruta de acceso al recurso compartido de archivos en la que quieras guardar el perfil de usuario móvil del usuario, seguido de `%username%` (que se sustituirá automáticamente por el nombre de usuario la primera vez que el usuario inicie sesión). Por ejemplo:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Para especificar un perfil de usuario móvil obligatorio, especifica la ruta de acceso al archivo NTuser.man que creaste anteriormente como, por ejemplo, `fs1.corp.contoso.comUser Profiles$default`. Para obtener más información, consulta [Crear perfiles de usuario obligatorios](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Selecciona **Aceptar**.

> [!NOTE]
> De manera predeterminada, se permite implementar todas las aplicaciones basadas en Windows® en tiempo de ejecución (Tienda Windows) al usar los perfiles de usuario móviles. Pero, al usar un perfil especial, las aplicaciones no se implementan de manera predeterminada. Los perfiles especiales son perfiles de usuario donde se descartan los cambios cuando el usuario cierra la sesión:
> <br><br>Para eliminar las restricciones en la implementación de aplicaciones para perfiles especiales, habilite la configuración de directiva **Allow deployment operations in special profiles** (ubicada en Configuración del equipo\Directivas\Plantillas administrativas\Componentes de Windows\Implementación de paquetes de aplicaciones). Pero las aplicaciones implementadas en este escenario dejarán datos almacenados en el equipo, que podrían acumularse si, por ejemplo, hubiera cientos de usuarios en un mismo equipo. Para limpiar las aplicaciones, busca o desarrolla una herramienta que use la API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) a fin de limpiar los paquetes de aplicaciones de los usuarios que ya no tienen un perfil en el equipo.
> <br><br>Para obtener información general sobre las aplicaciones de la Tienda Windows, consulte [Administrar el acceso de cliente a la Tienda Windows](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Paso 6: Configurar los perfiles de usuario móviles en equipos (opcional)

Si implementas perfiles de usuario móviles en equipos, como suele realizarse para Servicios de Escritorio remoto o en implementaciones de escritorios virtualizados, haz lo siguiente. Si implementas perfiles de usuario móviles en cuentas de usuario, sigue los pasos descritos en el apartado [Paso 5: Configurar los perfiles de usuario móviles en cuentas de usuario (opcional)](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Puedes usar una directiva de grupo para aplicar perfiles de usuario móviles a equipos que ejecuten Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

> [!NOTE]
> Si configuras perfiles de usuario móviles en equipos mediante Directiva de grupo y en cuentas de usuario mediante Active Directory, la configuración de directiva basada en equipos tiene prioridad.

A continuación, se muestra cómo configurar perfiles de usuario móviles en equipos:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. En el menú **Herramientas**, selecciona **Administración de directivas de grupo**. Se mostrará Administración de directivas de grupo.
3. En Administración de directivas de grupo, haz clic con el botón derecho en el GPO que creaste en el paso 3 (por ejemplo, **Configuración de perfiles de usuario móviles**) y selecciona **Editar**.
4. En la ventana Editor de administración de directivas de grupo, navega hasta **Configuración del equipo**, **Directivas**, **Plantillas administrativas**, **Sistema**y, finalmente, **Perfiles de usuario**.
5. Haz clic con el botón derecho en **Establecer ruta de perfil móvil para todos los usuarios conectados a este equipo** y selecciona **Editar**.
    > [!TIP]
    > Si se configura una carpeta particular para los usuarios, esta será la carpeta predeterminada usada por algunos programas como Windows PowerShell. Puedes configurar una ubicación alternativa, ya sea local o de red, por usuario en la sección **Carpeta particular** de las propiedades de la cuenta de usuario en AD DS. Para configurar la ubicación de la carpeta particular de todos los usuarios de un equipo que ejecute Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en un entorno de escritorio virtual, habilita la configuración de directiva **Establecer la carpeta particular del usuario** y, después, especifica el recurso compartido de archivos y la letra de unidad a la que se asignará (o bien especifica una carpeta local). No uses variables de entorno ni puntos suspensivos. El alias del usuario se anexa al final de la ruta de acceso especificada durante el inicio de sesión del usuario.
6. En el cuadro de diálogo **Propiedades**, selecciona **Habilitado**
7. En el cuadro **Users logging onto this computer should use this roaming profile path** (Los usuarios que inician sesión en este equipo deben usar esta ruta de perfil móvil), escribe la ruta de acceso al recurso compartido de archivos en la que quieras guardar el perfil de usuario móvil del usuario, seguido de `%username%` (que se sustituirá automáticamente por el nombre de usuario la primera vez que el usuario inicie sesión). Por ejemplo:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Para especificar un perfil de usuario móvil obligatorio (un perfil preconfigurado en el que los usuarios no pueden realizar cambios permanentes, ya que estos se restauran cuando el usuario cierra la sesión), especifica la ruta de acceso al archivo NTuser.man que creaste anteriormente como, por ejemplo, `\\fs1.corp.contoso.com\User Profiles$\default`. Para obtener más información, consulte [Creación de un perfil de usuario obligatorio](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Selecciona **Aceptar**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Paso 7: Especificar un diseño del menú Inicio para equipos con Windows 10 (opcional)

Puedes usar una directiva de grupo para aplicar un diseño del menú Inicio específico para que los usuarios vean el mismo diseño del menú Inicio en todos los equipos. Si los usuarios inician sesión en más de un equipo y quieres que tengan un diseño del menú Inicio coherente entre los equipos, asegúrate de que el GPO se aplica a todos sus equipos.

Para especificar un diseño del menú Inicio, haz lo siguiente:

1. Actualiza los equipos Windows 10 a la versión 1607 de Windows 10 (también conocida como Actualización de aniversario) o posterior, e instala la actualización acumulativa del 14 de marzo de 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) o una versión más reciente.
2. Crea un archivo XML de diseño del menú Inicio completo o parcial. Para ello, consulta [Personalizar y exportar el diseño de la pantalla Inicio](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Si especificas un diseño del menú Inicio *completo*, un usuario no podrá personalizar ninguna parte del menú Inicio. Si especificas un diseño del menú Inicio *parcial*, los usuarios pueden personalizarlo todo, excepto los grupos de iconos bloqueados que especifiques. Sin embargo, con un diseño del menú Inicio parcial, las personalizaciones de los usuarios del menú Inicio no se transferirán a otros equipos.
3. Utiliza una directiva de grupo para aplicar el diseño del menú Inicio personalizado al GPO que creaste para los perfiles de usuario móviles. Para ello, consulta [Usar la directiva de grupo para aplicar un diseño de pantalla Inicio personalizado en un dominio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Usa una directiva de grupo para establecer el siguiente valor del Registro en los equipos Windows 10. Para ello, consulta [Configuración de un elemento del Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Acción**   | **Actualizar**                  |
| ------------ | ------------                |
| Subárbol         | **HKEY_LOCAL_MACHINE**      |
| Ruta de acceso de la clave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nombre del valor   | **SpecialRoamingOverrideAllowed** |
| Tipo de valor   | **REG_DWORD**               |
| Datos de valor   | **1** (o **0** para deshabilitarlo) |
| Base         | **Decimal**                 |

5. (Opcional) Habilita las optimizaciones de inicio de sesión por primera vez para que los usuarios puedan iniciar sesión más rápidamente. Para ello, consulta [Aplicar directivas para mejorar el tiempo de inicio de sesión](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Opcional) Reduce todavía más los tiempos de inicio de sesión quitando las aplicaciones innecesarias de la imagen base de Windows 10 que usas para implementar los equipos cliente. Windows Server 2019 y Windows Server 2016 no tienen ninguna aplicación aprovisionada previamente, por lo que puedes omitir este paso en las imágenes del servidor.
    - Para quitar aplicaciones, usa el cmdlet [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) en Windows PowerShell para desinstalar las siguientes aplicaciones. Si los equipos ya están implementados, puedes crear scripts para la eliminación de estas aplicaciones mediante [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
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
>La desinstalación de estas aplicaciones reduce los tiempos de inicio de sesión, pero puedes dejarlas instaladas si tu implementación necesita cualquiera de ellas.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Paso 8: Habilitar los GPO de perfiles de usuario móviles

Si configuras perfiles de usuario móviles en equipos mediante Directiva de grupo, o si personalizas otra configuración de perfiles de usuario móviles mediante Directiva de grupo, el paso siguiente es habilitar el GPO y permitir aplicarlo en los usuarios afectados.

>[!TIP]
>Si quieres implementar la compatibilidad con el equipo principal, hazlo ahora, antes de habilitar el GPO. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal. Para conocer la configuración de directivas específicas, consulta [Implementación de equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles](deploy-primary-computers.md).

A continuación, se muestra cómo habilitar el GPO de perfil de usuario móvil:

1. Abra Administración de directivas de grupo.
2. Haz clic con el botón derecho en el GPO que creaste y selecciona **Vínculo habilitado**. Aparecerá una casilla junto al elemento de menú.

## <a name="step-9-test-roaming-user-profiles"></a>Paso 9: Los perfiles de usuario móviles

Para probar perfiles de usuario móviles, inicia sesión en un equipo con una cuenta de usuario configurada para perfiles de usuario móviles, o bien inicia sesión en un equipo configurado para perfiles de usuario móviles. Después, confirma que el perfil es redirigido.

A continuación, se muestra cómo probar los perfiles de usuario móviles:

1. Inicia sesión en un equipo principal (si has habilitado el soporte de equipo principal) con una cuenta de usuario en la que hayas habilitado los perfiles de usuario móviles. Si has habilitado perfiles de usuario móviles en equipos específicos, inicia sesión en uno de estos equipos.
2. Si el usuario ha iniciado sesión en el equipo anteriormente, abre un símbolo del sistema con privilegios elevados y escribe el comando siguiente para asegurarte de que se aplique la última configuración de directiva de grupo en el equipo cliente:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Para confirmar que el perfil de usuario es móvil, abre el **Panel de control**, selecciona **Sistema y seguridad**, **Sistema**, **Configuración avanzada del sistema** y **Configuración** en la sección Perfiles de usuario y, después, busca **Móvil** en la columna **Tipo**.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Apéndice A: Lista de comprobación para la implementación de perfiles de usuario móviles

| Estado                     | Acción                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparar el dominio<br>- Unir los equipos al dominio<br>- Habilitar el uso de versiones de perfiles independientes<br>- Crear cuentas de usuario<br>- (Opcional) Implementar el redireccionamiento de carpetas |
| ☐<br><br><br>             | 2. Crear un grupo de seguridad para perfiles de usuario móviles<br>- Nombre del grupo:<br>- Miembros: |
| ☐<br><br>                 | 3. Crear un recurso compartido de archivos para perfiles de usuario móviles<br>- Nombre del recurso compartido de archivos: |
| ☐<br><br>                 | 4. Crear un GPO para perfiles de usuario móviles<br>- Nombre del GPO:|
| ☐                         | 5. Configurar la directiva de perfiles de usuario móviles    |
| ☐<br>☐<br>☐              | 6. Habilitar los perfiles de usuario móviles<br>- ¿Habilitados en AD DS en cuentas de usuario?<br>- ¿Habilitados en una directiva de grupo en las cuentas del equipo?<br> |
| ☐                         | 7. (Opcional) Especificar un diseño del menú Inicio para equipos Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Opcional) Habilitar la compatibilidad con el equipo principal<br>- Designar los equipos principales para los usuarios<br>- Ubicación del usuario y asignaciones de equipos principales:<br>- (Opcional) Habilitar la compatibilidad con el equipo principal para el redireccionamiento de carpetas<br>- ¿Basada en el equipo o en el usuario?<br>- (Opcional) Habilitar la compatibilidad con el equipo principal para los perfiles de usuario móviles |
| ☐                        | 9. Habilitar los GPO de perfiles de usuario móviles                |
| ☐                        | 10. Los perfiles de usuario móviles                         |

## <a name="appendix-b-profile-version-reference-information"></a>Apéndice B: Información de referencia sobre la versión del perfil

Cada perfil tiene una versión de perfil que se corresponde aproximadamente con la versión de Windows en la que se usa el perfil. Por ejemplo, las versiones 1703 y 1607 de Windows 10 usan la versión de perfil .V6. Microsoft crea una nueva versión de perfil solo cuando es necesario para mantener la compatibilidad, y es por ello que no todas las versiones de Windows incluyen una nueva versión de perfil.

La tabla siguiente contiene las ubicaciones de los perfiles de usuario móviles en diferentes versiones de Windows.

| Versión del sistema operativo | Ubicación del perfil de usuario móvil |
| --- | --- |
| Windows XP y Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista y Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 y Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 y Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (después de aplicar la actualización de software y la clave del Registro)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes de aplicar la actualización de software y la clave del Registro) |
| Windows 8.1 y Windows Server 2012 R2: | ```\\<servername>\<fileshare>\<username>.V4``` (después de aplicar la actualización de software y la clave del Registro)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes de aplicar la actualización de software y la clave del Registro) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versión 1703 y versiones posteriores | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Apéndice C: Soluciones alternativas para el restablecimiento de los diseños del menú Inicio después de las actualizaciones

Estas son algunas formas de solucionar el restablecimiento de los diseños del menú Inicio después de una actualización local:

- Si solo un usuario usa el dispositivo y el administrador de TI usa una estrategia de implementación de sistema operativo administrada como Configuration Manager, pueden hacer lo siguiente:
    
  1. Exportar el diseño del menú Inicio con Export-Startlayout antes de la actualización 
  2. Importar el diseño del menú Inicio con Import-StartLayout después de ejecutar la característica Bienvenida de Windows, pero antes de que el usuario inicie sesión  
 
     > [!NOTE] 
     > Al importar un diseño del menú Inicio se modifica el perfil de usuario predeterminado. Todos los perfiles de usuario creados después de la importación obtendrán el diseño del menú Inicio importado.
 
- Los administradores de TI pueden optar por administrar el diseño del menú Inicio con una directiva de grupo. El uso de una directiva de grupo proporciona una solución de administración centralizada para aplicar un diseño del menú Inicio estandarizado a los usuarios. Existen dos modos de usar la directiva de grupo para la administración del menú Inicio. Bloqueo completo y bloqueo parcial. El escenario de bloqueo completo impide que el usuario realice cambios en el diseño del menú Inicio. El escenario de bloqueo parcial permite al usuario realizar cambios en un área específica del menú Inicio. Para obtener más información, consulta [Personalizar y exportar el diseño de la pantalla Inicio](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Los cambios que realice el usuario en el escenario de bloqueo parcial se perderán durante la actualización.

- Permite que se restablezca el diseño del menú Inicio y que los usuarios finales puedan volver a configurar el menú Inicio. Se puede enviar un correo electrónico de notificación u otra notificación a los usuarios finales informando del restablecimiento de los diseños del menú Inicio después de la actualización del sistema operativo a fin de minimizar el impacto. 

## <a name="change-history"></a>Historial de cambios

En la tabla siguiente se resumen los cambios más importantes realizados en este tema.

| Fecha | Descripción |Razón|
| --- | ---         | ---   |
| 1 de mayo de 2019 | Se han agregado las actualizaciones para Windows Server 2019 |
| 10 de abril de 2018 | Se ha agregado una descripción de cuándo se pierden las personalizaciones del menú Inicio de los usuarios después de una actualización local del sistema operativo|Problema conocido con el aviso. |
| 13 de marzo de 2018 | Se ha actualizado para Windows Server 2016 | Se ha sacado de la biblioteca de versiones anteriores y se ha actualizado para la versión actual de Windows Server. |
| 13 de abril de 2017 | Se ha agregado información sobre los perfiles para la versión 1703 de Windows 10 y se ha aclarado cómo funcionan las versiones de perfil móvil al actualizar los sistemas operativos. Consulta [Consideraciones al usar perfiles de usuario móviles en varias versiones de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Comentarios de los clientes. |
| 14 de marzo de 2017 | Se ha agregado un paso opcional para especificar un diseño del menú Inicio obligatorio para equipos Windows 10 en el apartado [Apéndice A: Lista de comprobación para la implementación de perfiles de usuario móviles](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Cambios de características en la actualización más reciente de Windows. |
| 23 de enero de 2017 | Se ha agregado un paso al apartado [Paso 4: De manera opcional, crea un GPO para perfiles de usuario móviles](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) para delegar los permisos de lectura a los usuarios autenticados, lo que ahora es necesario debido a una actualización de seguridad de la directiva de grupo.|Cambios de seguridad en el procesamiento de las directivas de grupo. |
| 29 de diciembre de 2016 | Se ha agregado un vínculo al apartado [Paso 8: Habilitar el GPO de perfiles de usuario móviles](#step-8-enable-the-roaming-user-profiles-gpo) para que sea más fácil obtener información sobre cómo establecer una directiva de grupo para los equipos principales. También se han corregido un par de referencias a los pasos 5 y 6 que tenían los números incorrectos.|Comentarios de los clientes. |
| 5 de diciembre de 2016 | Se ha agregado información que describe un problema de itinerancia de la configuración del menú Inicio. | Comentarios de los clientes. |
| 6 de julio de 2016 | Se han agregado los sufijos de versión de perfil de Windows 10 en el apartado [Apéndice B: Información de referencia sobre la versión del perfil](#appendix-b-profile-version-reference-information). También se han quitado Windows XP y Windows Server 2003 de la lista de sistemas operativos compatibles. | Se han realizado actualizaciones para las nuevas versiones de Windows y se ha eliminado la información sobre las versiones de Windows que ya no son compatibles. |
| 7 de julio de 2015 | Se agregó el requisito y el paso para deshabilitar la disponibilidad continua al usar un servidor de archivos en clúster. | Los recursos compartidos de archivos en clúster tienen un rendimiento mejor para pequeñas operaciones de escritura (que son típicas con los perfiles de usuario móviles) cuando está deshabilitada la disponibilidad continua. |
| 19 de marzo de 2014 | Se han agregado mayúsculas en los sufijos de versión de perfil (.V2, .V3 y .V4) en el apartado [Apéndice B: Información de referencia sobre la versión del perfil](#appendix-b-profile-version-reference-information). | Aunque Windows no distingue las mayúsculas de las minúsculas, si usas NFS con el recurso compartido de archivos, es importante que el sufijo del perfil use las mayúsculas correctas. |
| 9 de octubre de 2013 | Se ha revisado para Windows Server 2012 R2 y Windows 8.1, se han aclarado algunos aspectos y se han agregado las secciones [Consideraciones al usar perfiles de usuario móviles en varias versiones de Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) y [Apéndice B: Información de referencia sobre la versión del perfil](#appendix-b-profile-version-reference-information). | Actualizaciones para la nueva versión; comentarios de los clientes. |

## <a name="more-information"></a>Información adicional

- [Implementación del redireccionamiento de carpetas, los archivos sin conexión y los perfiles de usuario móvil](deploy-folder-redirection.md)
- [Implementación de equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles](deploy-primary-computers.md)
- [Implementación de la administración de estado de usuario](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Declaración de Soporte técnico de Microsoft sobre los datos de perfil de usuario replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Transferencia local de aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)