---
title: Implementar el redireccionamiento de carpetas con Archivos sin conexión
description: Cómo usar Windows Server para implementar el redireccionamiento de carpetas con Archivos sin conexión en equipos cliente de Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 90b3e3d0b5030f8c0140e54c8b0bf55317437427
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867306"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Implementar el redireccionamiento de carpetas con Archivos sin conexión

>Se aplica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canal semianual)

En este tema se describe cómo usar Windows Server para implementar el redireccionamiento de carpetas con Archivos sin conexión en equipos cliente de Windows.

Para obtener una lista de los cambios recientes realizados en este tema, vea [historial de cambios](#change-history).

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), hemos [actualizado el paso 3: Cree un GPO para la redirección](#step-3-create-a-gpo-for-folder-redirection) de carpetas de este tema para que Windows pueda aplicar correctamente la Directiva de redirección de carpetas (y no revertir las carpetas redirigidas en los equipos afectados).

## <a name="prerequisites"></a>Requisitos previos

### <a name="hardware-requirements"></a>Requisitos de hardware

La redirección de carpetas requiere un equipo basado en x64 o en x86; no es compatible con Windows® RT.

### <a name="software-requirements"></a>Requisitos de software

La redirección de carpetas tiene los siguientes requisitos de software:

- Para administrar el redireccionamiento de carpetas, debe haber iniciado sesión como miembro del grupo de seguridad administradores de dominio, el grupo de seguridad administradores de empresa o el grupo de seguridad propietarios del creador de directiva de grupo.
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- Los equipos cliente deben unirse a los Servicios de dominio de Active Directory (AD DS) que estés administrando.
- Los equipos deben estar disponibles con Administración de directivas de grupo y tener instalado el Centro de administración de Active Directory.
- Un servidor de archivos debe estar disponible para hospedar carpetas redirigidas.
    - Si el recurso compartido de archivos usa espacios de nombres DFS, las carpetas DFS (vínculos) deben tener el mismo destino para evitar que los usuarios realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos usa Replicación DFS para replicar los contenidos con otro servidor, los usuarios deben poder acceder únicamente al servidor de origen para evitar que se realicen ediciones en conflicto en diferentes servidores.
    - Cuando use un recurso compartido de archivos en clúster, deshabilite la disponibilidad continua en el recurso compartido de archivos para evitar problemas de rendimiento con la redirección de carpetas y el Archivos sin conexión. Además, es posible que Archivos sin conexión no pase al modo sin conexión durante 3-6 minutos después de que un usuario pierda el acceso a un recurso compartido de archivos disponible continuamente, lo que podría frustrar a los usuarios que todavía no utilicen el modo Always offline de Archivos sin conexión.

> [!NOTE]
> Algunas de las características más recientes del redireccionamiento de carpetas tienen requisitos adicionales de los esquemas de equipos cliente y Active Directory. Para obtener más información, vea [implementar equipos primarios](deploy-primary-computers.md), [deshabilitar archivos sin conexión en carpetas](disable-offline-files-on-folders.md), [Habilitar el modo siempre sin conexión](enable-always-offline.md)y [Habilitar el movimiento de carpetas optimizadas](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Paso 1: Crear un grupo de seguridad de redirección de carpetas

Si el entorno aún no está configurado con redirección de carpetas, el primer paso es crear un grupo de seguridad que contenga todos los usuarios a los que desea aplicar la configuración de la Directiva de redireccionamiento de carpetas.

Aquí se muestra cómo crear un grupo de seguridad para la redirección de carpetas:

1. Abra Administrador del servidor en un equipo que tenga instalado Active Directory centro de administración.
2. En el menú **herramientas** , seleccione **centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Haga clic con el botón derecho en el dominio o la unidad organizativa correspondiente, seleccione **nuevo**y, a continuación, seleccione **Grupo**.
4. En la ventana **Crear grupo** , en la sección **Grupo** , especifique la configuración siguiente:
    - En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Usuarios de redirección de carpetas**.
    - En **ámbito de grupo**, seleccione **seguridad**y, a continuación, seleccione **global**.
5. En la sección **miembros** , seleccione **Agregar**. Aparece el cuadro de diálogo Seleccionar Usuarios, Contactos, Equipos, Cuentas de servicio o Grupos.
6. Escriba los nombres de los usuarios o grupos en los que desea implementar el redireccionamiento de carpetas, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** de nuevo.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Paso 2: Crear un recurso compartido de archivos para carpetas redirigidas

Si aún no tiene un recurso compartido de archivos para las carpetas redirigidas, utilice el procedimiento siguiente para crear un recurso compartido de archivos en un servidor que ejecute Windows Server 2012.

> [!NOTE]
> Algunas funciones pueden ser distintas o no estar disponibles si creas el recurso compartido de archivos en un servidor que ejecute otra versión de Windows Server.

Aquí se muestra cómo crear un recurso compartido de archivos en Windows Server 2019, Windows Server 2016 y Windows Server 2012:

1. En el panel de navegación Administrador del servidor, seleccione **servicios de archivos y almacenamiento**y, a continuación, seleccione **recursos compartidos** para mostrar la página recursos compartidos.
2. En el icono **recursos compartidos** , seleccione **tareas**y, después, seleccione **nuevo recurso compartido**. Se abrirá el Asistente para nuevo recurso compartido.
3. En la página **Seleccionar perfil** , seleccione **recurso compartido SMB-rápido**. Si tiene instalado Administrador de recursos servidor de archivos y está usando las propiedades de administración de carpetas, en su lugar seleccione **recurso compartido SMB-avanzado**.
4. En la página **Ubicación del recurso compartido** , selecciona el servidor y el volumen donde quieras crear el recurso compartido.
5. En la página **nombre del recurso compartido** , escriba un nombre para el recurso compartido (por ejemplo, **usuarios $** ) en el cuadro **nombre del recurso compartido** .
    >[!TIP]
    >Al crear el recurso compartido, oculte el recurso compartido colocando una ```$``` después del nombre del recurso compartido. Esto ocultará el recurso compartido de los exploradores ocasionales.
6. En la página **otras opciones** , desactive la casilla habilitar disponibilidad continua, si está presente, y, si lo desea, seleccione las casillas **Habilitar enumeración basada en el acceso** y **cifrar acceso a datos** .
7. En la página **permisos** , seleccione **personalizar permisos.** . Se abrirá el cuadro de diálogo Configuración de seguridad avanzada.
8. Seleccione **deshabilitar herencia**y, a continuación, seleccione **convertir permisos heredados en permiso explícito en este objeto**.
9. Establezca los permisos como se describe en la tabla 1 y se muestra en la figura 1, quitando los permisos de grupos y cuentas que no figuran en la lista y agregando permisos especiales al grupo usuarios de redirección de carpetas que creó en el paso 1.
    
    ![Establecer los permisos para el recurso compartido de carpetas redirigidas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Establecer los permisos para el recurso compartido de carpetas redirigidas
10. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Propiedades de administración** , seleccione el valor de uso de carpeta **Archivos de usuario** .
11. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Cuota** , seleccione (de manera opcional) la cuota que quiera aplicar a los usuarios del recurso compartido.
12. En la página **confirmación** , seleccione **crear.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Permisos necesarios para el recurso compartido de archivos que hospeda carpetas redirigidas

| Cuenta de usuario  | Acceso  | Se aplica a  |
| --------- | --------- | --------- |
| Cuenta de usuario | Acceso | Se aplica a |
| Sistema     | Control total        |    Esta carpeta, subcarpetas y archivos     |
| Administradores     | Control total       | Solo esta carpeta        |
| Creador/propietario     |   Control total      |   Solo subcarpetas y archivos      |
| Grupo de seguridad de usuarios que necesitan colocar datos en recursos compartidos (usuarios de redirección de carpetas)     |   Mostrar carpeta/leer datos *(permisos avanzados)* <br /><br />Crear carpetas/anexar datos *(permisos avanzados)* <br /><br />Atributos de lectura *(permisos avanzados)* <br /><br />Leer atributos extendidos *(permisos avanzados)* <br /><br />Permisos de lectura *(permisos avanzados)*      |  Solo esta carpeta       |
| Otros grupos y cuentas     |  Ninguno (quitar)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Paso 3: Crear un GPO para la redirección de carpetas

Si aún no tiene un GPO creado para la configuración de redirección de carpetas, use el procedimiento siguiente para crear uno.

Aquí se muestra cómo crear un GPO para la redirección de carpetas:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. En el menú **herramientas** , seleccione **Administración de directiva de grupo**.
3. Haga clic con el botón secundario en el dominio o la unidad organizativa en la que desea configurar el redireccionamiento de carpetas, seleccione **crear un GPO en este dominio y vincularlo aquí**.
4. En el cuadro de diálogo **nuevo GPO** , escriba un nombre para el GPO (por ejemplo, **configuración de redirección de carpetas**) y seleccione **Aceptar**.
5. Haz clic con el botón secundario en el nuevo GPO y desactiva la casilla **Vínculo habilitado** . Esto no permite que se aplique el GPO hasta que termine de configurarlo.
6. Selecciona el GPO. En la sección **filtrado de seguridad** de la pestaña **ámbito** , seleccione **usuarios autenticados**y, a continuación, seleccione **quitar** para evitar que el GPO se aplique a todos.
7. En la sección **filtrado de seguridad** , seleccione **Agregar**.
8. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupos** , escriba el nombre del grupo de seguridad que creó en el paso 1 (por ejemplo, **usuarios de redirección de carpetas**) y seleccione **Aceptar**.
9. Seleccione la pestaña **delegación** , haga clic en **Agregar**, escriba **usuarios autenticados**, haga clic en **Aceptar**y, a continuación, seleccione **Aceptar** de nuevo para aceptar los permisos de lectura predeterminados.
    
    Este paso es necesario debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), ahora debe conceder al grupo usuarios autenticados permisos de lectura en el GPO de redirección de carpetas; de lo contrario, el GPO no se aplicará a los usuarios o, si ya se ha aplicado, se quitará el GPO, se redirigirá de nuevo en el equipo local. Para obtener más información, consulte [implementación de la actualización de seguridad de directiva de grupo MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Paso 4: Configurar el redireccionamiento de carpetas con Archivos sin conexión

Después de crear un GPO para la configuración de redirección de carpetas, edite la configuración de directiva de grupo para habilitar y configurar el redireccionamiento de carpetas, como se describe en el procedimiento siguiente.

> [!NOTE]
> Archivos sin conexión está habilitada de forma predeterminada para las carpetas redirigidas en los equipos cliente de Windows y se deshabilita en los equipos que ejecutan Windows Server, a menos que el usuario lo cambie. Para usar directiva de grupo con el fin de controlar si Archivos sin conexión está habilitado, use la configuración **de directiva permitir o impedir el uso de la característica archivos sin conexión** .
> Para obtener información acerca de algunas de las otras opciones de configuración de Archivos sin conexión directiva de grupo, consulte [Habilitar la funcionalidad de archivos sin conexión avanzada](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)y [configurar directiva de grupo para archivos sin conexión](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Aquí se muestra cómo configurar el redireccionamiento de carpetas en directiva de grupo:

1. En administración de directiva de grupo, haga clic con el botón secundario en el GPO que creó (por ejemplo, **configuración de redirección de carpetas**) y, a continuación, seleccione **Editar**.
2. En la ventana de Editor de administración de directivas de grupo, vaya **a configuración de usuario**, **directivas**, **configuración de Windows**y, a continuación, **redirección de carpetas**.
3. Haga clic con el botón secundario en una carpeta que desee redirigir (por ejemplo, **documentos**) y, a continuación, seleccione **propiedades**.
4. En el cuadro de diálogo **propiedades** , en el cuadro **configuración** , seleccione **básico-redirigir la carpeta de todos a la misma ubicación**.

    > [!NOTE]
    > Para aplicar la redirección de carpetas a equipos cliente que ejecutan Windows XP o Windows Server 2003, seleccione la pestaña **configuración** y seleccione la **Directiva aplicar también la Directiva de redirección a Windows 2000, Windows 2000 Server, Windows XP y Windows Server 2003 casilla sistemas** .

5. En la sección Ubicación de la **carpeta de destino** , seleccione **crear una carpeta para cada usuario en la ruta de acceso raíz** y, en el cuadro Ruta de **acceso raíz** , escriba la ruta de acceso al recurso compartido de archivos que almacena las carpetas redirigidas, por ejemplo:  **\\ \\ usuarios\\de FS1.Corp.contoso.com $**
6. Seleccione la pestaña **configuración** y, en la sección eliminación de la **Directiva** , seleccione **redirigir la carpeta de nuevo a la ubicación local de userprofile cuando se quite la Directiva** (esta configuración puede ayudar a hacer que la redirección de carpetas se comporte de forma más previsible para adminisitrators y usuarios).
7. Seleccione **Aceptar**y, a continuación, seleccione **sí** en el cuadro de diálogo de advertencia.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Paso 5: Habilitar el GPO de redirección de carpetas

Una vez que haya terminado de configurar la redirección de carpetas directiva de grupo la configuración, el paso siguiente consiste en habilitar el GPO, lo que permite que se aplique a los usuarios afectados.

> [!TIP]
> Si quieres implementar el soporte de equipo principal u otra configuración de directivas, hazlo ahora, antes de habilitar el GPO. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal.

Aquí se muestra cómo habilitar el GPO de redirección de carpetas:

1. Abra Administración de directivas de grupo.
2. Haga clic con el botón secundario en el GPO que creó y, a continuación, seleccione **vínculo habilitado**. Aparecerá una casilla junto al elemento de menú.

## <a name="step-6-test-folder-redirection"></a>Paso 6: Probar la redirección de carpetas

Para probar el redireccionamiento de carpetas, inicie sesión en un equipo con una cuenta de usuario configurada para la redirección de carpetas. A continuación, confirme que se redirigen las carpetas y los perfiles.

Aquí se muestra cómo probar la redirección de carpetas:

1. Inicia sesión en un equipo principal (si has habilitado el soporte de equipo principal) con una cuenta de usuario para la que hayas habilitado el redireccionamiento de carpetas.
2. Si el usuario ha iniciado sesión en el equipo anteriormente, abre un símbolo del sistema con privilegios elevados y escribe el comando siguiente para asegurarte de que se aplique la última configuración de directiva de grupo en el equipo cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abra el Explorador de archivos.
4. Haga clic con el botón secundario en una carpeta redirigida (por ejemplo, la carpeta Mis documentos de la biblioteca documentos) y, a continuación, seleccione **propiedades**.
5. Seleccione la pestaña **Ubicación** y confirme que la ruta de acceso muestra el recurso compartido de archivos especificado en lugar de una ruta de acceso local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Apéndice A: Lista de comprobación para la implementación de la redirección de carpetas

| Status           | . |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparar el dominio<br>-Unir equipos al dominio<br>-Crear cuentas de usuario |
| ☐<br><br><br>   | 2. Crear grupo de seguridad para redirección de carpetas<br>-Nombre del Grupo:<br>Registrados |
| ☐<br><br>       | 3. Crear un recurso compartido de archivos para carpetas redirigidas<br>-Nombre del recurso compartido de archivos: |
| ☐<br><br>       | 4. Crear un GPO para la redirección de carpetas<br>-Nombre del GPO: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurar la redirección de carpetas y la configuración de la Directiva de Archivos sin conexión<br>-Carpetas redirigidas:<br>-¿Está habilitada la compatibilidad con Windows 2000, Windows XP y Windows Server 2003?<br>-Archivos sin conexión habilitado? (habilitado de forma predeterminada en los equipos cliente de Windows)<br>-¿Está habilitado el modo siempre sin conexión?<br>-Sincronización de archivos en segundo plano habilitada<br>-Se ha habilitado el movimiento optimizado de carpetas redirigidas |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. Opta Habilitar la compatibilidad con el equipo principal<br>-Basado en el equipo o en el usuario<br>-Designar equipos principales para los usuarios<br>-Ubicación de las asignaciones de equipo principal y de usuario:<br>-(Opcional) habilitar la compatibilidad con el equipo principal para redirección de carpetas<br>-(Opcional) habilitar la compatibilidad con equipos primarios para perfiles de usuario móviles |
| ☐         | 7. Habilitar el GPO de redirección de carpetas |
| ☐         | 8. Probar la redirección de carpetas |

## <a name="change-history"></a>Historial de cambios

En la tabla siguiente se resumen los cambios más importantes realizados en este tema.

| Date | Descripción | Reason|
| --- | --- | --- |
| 18 de enero de 2017 | Se ha agregado un [paso al paso 3: Cree un GPO para la redirección](#step-3-create-a-gpo-for-folder-redirection) de carpetas para delegar permisos de lectura a los usuarios autenticados, lo que ahora es necesario debido a una actualización de seguridad Directiva de grupo. | Comentarios del cliente |

## <a name="more-information"></a>Más información

* [Redirección de carpetas, Archivos sin conexión y perfiles de usuario móviles](folder-redirection-rup-overview.md)
* [Implementar equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles](deploy-primary-computers.md)
* [Habilitar la funcionalidad de Archivos sin conexión avanzada](enable-always-offline.md)
* [Declaración de soporte técnico de Microsoft sobre los datos de Perfil de usuario replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Transferir localmente aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)