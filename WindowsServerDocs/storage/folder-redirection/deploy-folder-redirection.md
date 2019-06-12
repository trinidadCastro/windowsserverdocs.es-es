---
title: Implementar la redirección de carpetas con archivos sin conexión
description: Cómo usar Windows Server para implementar la redirección de carpetas con archivos sin conexión en los equipos cliente de Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 4b6c58f0f33b45052e038a9af941297a294d17b2
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812509"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Implementar la redirección de carpetas con archivos sin conexión

>Se aplica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canal semianual)

Este tema describe cómo usar Windows Server para implementar la redirección de carpetas con archivos sin conexión en los equipos cliente de Windows.

Para obtener una lista de los cambios recientes en este tema, consulte [historial de cambios](#change-history).

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), actualizamos [paso 3: Crear un GPO para redirección de carpetas](#step-3-create-a-gpo-for-folder-redirection) de este tema para que Windows pueden aplicar correctamente la directiva de redirección de carpetas (y no revertir las carpetas redirigidas en los equipos afectados).

## <a name="prerequisites"></a>Requisitos previos

### <a name="hardware-requirements"></a>Requisitos de hardware

Redirección de carpetas requiere un equipo basado en x64 64 o x86; no se admite Windows® RT

### <a name="software-requirements"></a>Requisitos de software

Redirección de carpetas tiene los siguientes requisitos de software:

- Para administrar el redireccionamiento de carpetas, debe ser iniciado sesión como miembro del grupo de seguridad Administradores de dominio, el grupo de seguridad Administradores de empresa o el grupo de seguridad propietarios del creador de directivas de grupo.
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- Los equipos cliente deben unirse a los Servicios de dominio de Active Directory (AD DS) que estés administrando.
- Los equipos deben estar disponibles con Administración de directivas de grupo y tener instalado el Centro de administración de Active Directory.
- Un servidor de archivos debe estar disponible para hospedar las carpetas redirigidas.
    - Si el recurso compartido de archivos usa espacios de nombres DFS, las carpetas DFS (vínculos) deben tener el mismo destino para evitar que los usuarios realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos usa Replicación DFS para replicar los contenidos con otro servidor, los usuarios deben poder acceder únicamente al servidor de origen para evitar que se realicen ediciones en conflicto en diferentes servidores.
    - Cuando se usa un recurso compartido de archivos en clúster, deshabilite la disponibilidad continua en el recurso compartido de archivos para evitar problemas de rendimiento con redirección de carpetas y archivos sin conexión. Además, los archivos sin conexión no pueden pasar al modo sin conexión durante 3 a 6 minutos después de que un usuario pierde el acceso a un recurso compartido de archivos disponible continuamente, lo que puede frustrar a los usuarios que aún no están usando el modo siempre sin conexión de archivos sin conexión.

> [!NOTE]
> Algunas características más recientes de redirección de carpetas tienen adicionales para equipos cliente y los requisitos de esquema de Active Directory. Para obtener más información, consulte [implementar equipos principales](deploy-primary-computers.md), [deshabilitar archivos sin conexión en las carpetas](disable-offline-files-on-folders.md), [habilitar modo siempre sin conexión](enable-always-offline.md), y [Enable optimizado para mover carpeta ](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Paso 1: Crear un grupo de seguridad de redireccionamiento de carpetas

Si su entorno no está ya configurado con redirección de carpetas, el primer paso es crear un grupo de seguridad que contiene todos los usuarios a la que desea aplicar la configuración de directiva de redirección de carpetas.

Aquí le mostramos cómo crear un grupo de seguridad para la redirección de carpetas:

1. Abra el Administrador de servidor en un equipo con el centro de administración de Active Directory instalado.
2. En el **herramientas** menú, seleccione **centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Haga clic en el dominio u OU correspondiente, seleccione **New**y, a continuación, seleccione **grupo**.
4. En la ventana **Crear grupo** , en la sección **Grupo** , especifique la configuración siguiente:
    - En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Los usuarios de redirección de carpeta**.
    - En **ámbito de grupo**, seleccione **seguridad**y, a continuación, seleccione **Global**.
5. En el **miembros** sección, seleccione **agregar**. Aparece el cuadro de diálogo Seleccionar Usuarios, Contactos, Equipos, Cuentas de servicio o Grupos.
6. Escriba los nombres de los usuarios o grupos a los que desea implementar la redirección de carpetas, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** nuevo.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Paso 2: Crear un recurso compartido de archivos para las carpetas redirigidas

Si no dispone de un recurso compartido de archivos para las carpetas redirigidas, use el procedimiento siguiente para crear un recurso compartido de archivos en un servidor que ejecuta Windows Server 2012.

> [!NOTE]
> Algunas funciones pueden ser distintas o no estar disponibles si creas el recurso compartido de archivos en un servidor que ejecute otra versión de Windows Server.

Aquí le mostramos cómo crear un recurso compartido de archivos en Windows Server 2012, Windows Server 2016 y Windows Server 2019:

1. En el panel de navegación del administrador del servidor, seleccione **File and Storage Services**y, a continuación, seleccione **recursos compartidos** para mostrar la página de recursos compartidos.
2. En el **recursos compartidos** icono, seleccione **tareas**y, a continuación, seleccione **nuevo recurso compartido**. Se abrirá el Asistente para nuevo recurso compartido.
3. En el **Seleccionar perfil** , seleccione **recurso compartido SMB-rápido**. Si tiene instalado el Administrador de recursos de servidor de archivos y usas las propiedades de administración de carpetas, en su lugar, seleccione **recurso compartido SMB - avanzado**.
4. En la página **Ubicación del recurso compartido** , selecciona el servidor y el volumen donde quieras crear el recurso compartido.
5. En el **nombre del recurso compartido** página, escriba un nombre para el recurso compartido (por ejemplo, **usuarios$** ) en el **nombre del recurso compartido** cuadro.
    >[!TIP]
    >Al crear el recurso compartido, puedes ocultarlo colocando un ```$``` después del nombre del recurso compartido. Esto ocultará el recurso compartido a los usuarios ocasionales.
6. En el **otra configuración** página, desactive la casilla de verificación Habilitar disponibilidad continua, si está presente y, opcionalmente, seleccione el **Habilitar enumeración basada en acceso** y **cifrar acceso a datos** casillas de verificación.
7. En el **permisos** página, seleccione **personalizar permisos...** . Se abrirá el cuadro de diálogo Configuración de seguridad avanzada.
8. Seleccione **deshabilitar herencia**y, a continuación, seleccione **convertir permisos heredados en permiso explícito en este objeto**.
9. Establezca los permisos como se describe la tabla 1 y se muestra en la figura 1, elimina los permisos de cuentas y grupos que están ocultos y agrega permisos especiales para el grupo de usuarios de la redirección de carpeta que creó en el paso 1.
    
    ![Establecer los permisos para el recurso compartido de las carpetas redirigidas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** estableciendo permisos para compartan las carpetas redirigidas
10. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Propiedades de administración** , seleccione el valor de uso de carpeta **Archivos de usuario** .
11. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Cuota** , seleccione (de manera opcional) la cuota que quiera aplicar a los usuarios del recurso compartido.
12. En el **confirmación** página, seleccione **crear.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Permisos necesarios para el archivo de compartan las carpetas redirigidas de hospedaje

| Cuenta de usuario  | Acceso  | Se aplica a  |
| --------- | --------- | --------- |
| Cuenta de usuario | Acceso | Se aplica a |
| Sistema     | Control total        |    Esta carpeta, subcarpetas y archivos     |
| Administradores     | Control total       | Solo esta carpeta        |
| Creador/propietario     |   Control total      |   Solo subcarpetas y archivos      |
| Grupo de seguridad de los usuarios que necesitan colocar datos en el recurso compartido (usuarios de redirección de carpeta)     |   Listar carpeta / leer datos *(permisos avanzados)* <br /><br />Crear carpetas / Anexar datos *(permisos avanzados)* <br /><br />Leer atributos *(permisos avanzados)* <br /><br />Atributos extendidos de lectura *(permisos avanzados)* <br /><br />Permisos de lectura *(permisos avanzados)*      |  Solo esta carpeta       |
| Otros grupos y cuentas     |  Ninguno (quitar)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Paso 3: Crear un GPO para redirección de carpetas

Si no dispone de un GPO creado para la configuración de redirección de carpetas, use el procedimiento siguiente para crear uno.

Aquí le mostramos cómo crear un GPO para redirección de carpetas:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. Desde el **herramientas** menú, seleccione **Group Policy Management**.
3. Haga clic en el dominio o unidad organizativa en el que desea configurar la redirección de carpetas y, después, seleccione **crear un GPO en este dominio y vincularlo aquí**.
4. En el **nuevo GPO** cuadro de diálogo, escriba un nombre para el GPO (por ejemplo, **configuración de redirección de carpeta**) y, a continuación, seleccione **Aceptar**.
5. Haz clic con el botón secundario en el nuevo GPO y desactiva la casilla **Vínculo habilitado** . Esto no permite que se aplique el GPO hasta que termine de configurarlo.
6. Selecciona el GPO. En el **filtrado de seguridad** sección de la **ámbito** ficha, seleccione **usuarios autenticados**y, a continuación, seleccione **quitar** para evitar que el GPO desde que se va a aplicar a todo el mundo.
7. En el **filtrado de seguridad** sección, seleccione **agregar**.
8. En el **Seleccionar usuario, equipo o grupo** cuadro de diálogo, escriba el nombre de la seguridad de grupo que ha creado en el paso 1 (por ejemplo, **los usuarios de redirección de carpeta**) y, a continuación, seleccione **Aceptar**.
9. Seleccione el **delegación** ficha, seleccione **agregar**, tipo **usuarios autenticados**, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** nuevo Aceptar el valor predeterminado permisos de lectura.
    
    Este paso es necesario debido a cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), ahora debe proporcionar los permisos de lectura de grupo delegado usuarios autenticados en el GPO redirección de carpetas: en caso contrario, el GPO no se aplican a los usuarios, o si se ha aplicado, el GPO es quita, el redireccionamiento de carpetas hasta el equipo local. Para obtener más información, consulte [implementación de grupo Directiva de seguridad Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Paso 4: Configurar la redirección de carpetas con archivos sin conexión

Después de crear un GPO para la configuración de redirección de carpetas, edite la configuración de directiva de grupo para habilitar y configurar la redirección de carpetas, como se describe en el siguiente procedimiento.

> [!NOTE]
> Archivos sin conexión está habilitada de forma predeterminada para las carpetas redirigidas en los equipos cliente de Windows y deshabilitada en los equipos que ejecutan Windows Server, a menos que el usuario puede cambiar. Para utilizar Directiva de grupo para controlar si los archivos sin conexión está habilitado, utilice el **permitir o impedir el uso de la característica archivos sin conexión** configuración de directiva.
> Para obtener información acerca de algunas de las demás configuraciones de directiva de grupo de archivos sin conexión, vea [habilitar avanzadas sin conexión archivos de funcionalidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>), y [configuración de directiva de grupo para archivos sin conexión](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Aquí le mostramos cómo configurar la redirección de carpetas en la directiva de grupo:

1. En administración de directivas de grupo, haga clic en el GPO que creaste (por ejemplo, **configuración de redirección de carpeta**) y, a continuación, seleccione **editar**.
2. En la ventana Editor de administración de directivas de grupo, vaya a **configuración de usuario**, a continuación, **directivas**, a continuación, **configuración de Windows**y, a continuación, **carpeta Redirección**.
3. Haga clic en una carpeta que desea redirigir (por ejemplo, **documentos**) y, a continuación, seleccione **propiedades**.
4. En el **propiedades** cuadro de diálogo desde el **configuración** cuadro, seleccione **básico: redirigir la carpeta de todos a la misma ubicación**.

    > [!NOTE]
    > Para aplicar la redirección de carpetas para los equipos cliente que ejecutan Windows XP o Windows Server 2003, seleccione el **configuración** pestaña y seleccione el **aplicar también directiva de redirección a Windows 2000, Windows 2000 Server, Windows XP y Sistemas operativos Windows Server 2003** casilla de verificación.

5. En el **ubicación de carpeta de destino** sección, seleccione **cree una carpeta para cada usuario en la ruta de acceso raíz** y, a continuación, en el **ruta de acceso raíz** , escriba la ruta de acceso para el almacenamiento de recurso compartido de archivos redirige las carpetas, por ejemplo:  **\\ \\fs1.corp.contoso.com\\$ de los usuarios**
6. Seleccione el **configuración** ficha y en el **eliminación de la directiva** sección, seleccione opcionalmente **redirigir la carpeta a la ubicación del perfil de usuario local cuando se quita la directiva** (esta configuración puede ayudar a la redirección de carpetas se comportan de manera más predecible para adminisitrators y usuarios).
7. Seleccione **Aceptar**y, a continuación, seleccione **Sí** en el cuadro de diálogo de advertencia.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Paso 5: Habilitar la redirección de carpetas de GPO

Una vez haya completado la configuración de directiva de grupo de redirección de carpetas, el siguiente paso es habilitar el GPO, que se permite que se aplicará a los usuarios afectados.

> [!TIP]
> Si quieres implementar el soporte de equipo principal u otra configuración de directivas, hazlo ahora, antes de habilitar el GPO. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal.

Aquí le mostramos cómo habilitar el GPO redirección de carpetas:

1. Abra Administración de directivas de grupo.
2. Haga clic en el GPO que ha creado y, a continuación, seleccione **vínculo habilitado**. Aparecerá una casilla de verificación situada junto al elemento de menú.

## <a name="step-6-test-folder-redirection"></a>Paso 6: Probar la redirección de carpetas

Para probar la redirección de carpetas, inicie sesión en un equipo con una cuenta de usuario configurada para la redirección de carpeta. A continuación, confirme que se redirigen las carpetas y perfiles.

Aquí le mostramos cómo probar la redirección de carpetas:

1. Inicie sesión en un equipo principal (si ha habilitado el soporte de equipo principal) con una cuenta de usuario para el que ha habilitado la redirección de carpetas.
2. Si el usuario ha iniciado sesión en el equipo anteriormente, abre un símbolo del sistema con privilegios elevados y escribe el comando siguiente para asegurarte de que se aplique la última configuración de directiva de grupo en el equipo cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abra el Explorador de archivos.
4. Haga clic en una carpeta redirigida (por ejemplo, la carpeta Mis documentos en la biblioteca de documentos) y, a continuación, seleccione **propiedades**.
5. Seleccione el **ubicación** pestaña y confirme que muestra la ruta de acceso del recurso compartido de archivos especificado en lugar de una ruta de acceso local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Apéndice A: Lista de comprobación para implementar la redirección de carpeta

| Estado           | Acción |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparar el dominio<br>-Unir equipos al dominio<br>-Crear cuentas de usuario |
| ☐<br><br><br>   | 2. Crear grupo de seguridad para la redirección de carpetas<br>-Nombre del grupo:<br>-Miembros: |
| ☐<br><br>       | 3. Crear un recurso compartido de archivos para las carpetas redirigidas<br>: Nombre del recurso compartido archivo: |
| ☐<br><br>       | 4. Crear un GPO para redirección de carpetas<br>-Nombre del GPO: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurar opciones de directiva de redirección de carpetas y archivos sin conexión<br>-Redirigidas carpetas:<br>¿-Windows 2000, Windows XP y habilitada la compatibilidad con Windows Server 2003?<br>¿-Archivos sin conexión habilitados? (habilitado de forma predeterminada en los equipos cliente de Windows)<br>¿-Modo siempre sin conexión habilitado?<br>¿: Sincronización de archivos en segundo plano habilitada?<br>¿-Move optimizada de las carpetas redirigidas habilitado? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. (Opcional) Habilitar soporte de equipo principal<br>¿-Basadas en equipos o en función de usuario?<br>-Designar equipos principales para los usuarios<br>-Ubicación de usuario y asignaciones de equipos principales:<br>-(Opcional) habilitar soporte de equipo principal para redirección de carpetas<br>-(Opcional) habilitar soporte de equipo principal para perfiles de usuario móviles |
| ☐         | 7. Habilitar la redirección de carpetas de GPO |
| ☐         | 8. Probar la redirección de carpetas |

## <a name="change-history"></a>Historial de cambios

En la tabla siguiente se resumen los cambios más importantes realizados en este tema.

| Fecha | Descripción | Reason|
| --- | --- | --- |
| 18 de enero de 2017 | Agrega un paso para [paso 3: Crear un GPO para redirección de carpetas](#step-3-create-a-gpo-for-folder-redirection) para delegar permisos de lectura a los usuarios autenticados, que ahora es necesario debido a una actualización de seguridad de la directiva de grupo. | Comentarios del cliente |

## <a name="more-information"></a>Más información

* [Redirección de carpetas, archivos sin conexión y perfiles de usuario móviles](folder-redirection-rup-overview.md)
* [Implementar equipos principales para redirección de carpetas y perfiles de usuario móviles](deploy-primary-computers.md)
* [Habilitar la funcionalidad avanzada de archivos sin conexión](enable-always-offline.md)
* [Declaración de soporte técnico de Microsoft en torno a los datos de perfil de usuario replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Agregar o quitar aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en Windows en tiempo de ejecución](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)