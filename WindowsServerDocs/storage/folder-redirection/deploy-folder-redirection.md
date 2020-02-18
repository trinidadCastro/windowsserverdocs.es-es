---
title: Implementar la redirección de carpetas con Archivos sin conexión
description: Cómo usar Windows Server para implementar Redirección de carpetas con Archivos sin conexión en equipos cliente de Windows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 6d8f6bf0df67b76028945403352bd135e6641a5a
ms.sourcegitcommit: ab3967d71dcbb962079af194875de58e7c32c4e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2020
ms.locfileid: "76967418"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Implementar la redirección de carpetas con Archivos sin conexión

>Se aplica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canal semianual)

En este tema se describe cómo usar Windows Server para implementar Redirección de carpetas con Archivos sin conexión en equipos cliente de Windows.

Para obtener una lista de los cambios recientes de este tema, consulta [Historial de cambios](#change-history).

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), hemos actualizado el apartado [Paso 3: Crear un GPO para Redirección de carpetas](#step-3-create-a-gpo-for-folder-redirection) de este tema para que Windows pueda aplicar correctamente la directiva de Redirección de carpetas (y no revertir las carpetas redirigidas en los equipos afectados).

## <a name="prerequisites"></a>Requisitos previos

### <a name="hardware-requirements"></a>Requisitos de hardware

Redirección de carpetas requiere un equipo basado en x64 o en x86; no es compatible con Windows® RT.

### <a name="software-requirements"></a>Requisitos de software

Redirección de carpetas tiene los siguientes requisitos de software:

- Para administrar Redirección de carpetas, debes iniciar sesión como un miembro del grupo de seguridad Administradores de dominio, el grupo de seguridad Administradores de empresa o el grupo de seguridad Propietarios del creador de directivas de grupo.
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semianual), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- Los equipos cliente deben unirse a los Servicios de dominio de Active Directory (AD DS) que estés administrando.
- Los equipos deben estar disponibles con Administración de directivas de grupo y tener instalado el Centro de administración de Active Directory.
- Debe haber disponible un servidor de archivos para hospedar las carpetas redirigidas.
    - Si el recurso compartido de archivos usa espacios de nombres DFS, las carpetas DFS (vínculos) deben tener el mismo destino para evitar que los usuarios realicen ediciones en conflicto en diferentes servidores.
    - Si el recurso compartido de archivos usa Replicación DFS para replicar los contenidos con otro servidor, los usuarios deben poder acceder únicamente al servidor de origen para evitar que se realicen ediciones en conflicto en diferentes servidores.
    - Si usas un recurso compartido de archivos en clúster, deshabilita la disponibilidad continua en el recurso compartido de archivos para evitar problemas de rendimiento con Redirección de carpetas y Archivos sin conexión. Además, los archivos sin conexión no pueden pasar al modo sin conexión durante 3 a 6 minutos después de que un usuario pierda el acceso a un recurso compartido disponible de forma continua, lo que puede frustrar a los usuarios que todavía no están usando el modo de Siempre sin conexión de Archivos sin conexión.

> [!NOTE]
> Algunas características de Redirección de carpetas más recientes tienen requisitos adicionales para el equipo cliente y el esquema de Active Directory. Para obtener más información, consulta los temas sobre [implementación de equipos principales](deploy-primary-computers.md), [deshabilitación de Archivos sin conexión en carpetas](disable-offline-files-on-folders.md), [habilitación del modo Siempre sin conexión](enable-always-offline.md) y [habilitación del movimiento de carpetas optimizado](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Paso 1: Crear un grupo de seguridad de redirección de carpetas

Si tu entorno aún no está configurado con Redirección de carpetas, el primer paso es crear un grupo de seguridad que contenga todos los usuarios o equipos donde quieras aplicar la configuración de directivas de Redirección de carpetas.

Aquí se muestra cómo crear un grupo de seguridad para Redirección de carpetas:

1. Abre el Administrador del servidor en un equipo que tenga instalado el Centro de administración de Active Directory.
2. En el menú **Herramientas**, selecciona **Centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Haz clic con el botón derecho en el dominio o la unidad organizativa correspondiente, selecciona **Nuevo** y elige **Grupo**.
4. En la ventana **Crear grupo** , en la sección **Grupo** , especifique la configuración siguiente:
    - En **Nombre de grupo**, escribe el nombre del grupo de seguridad, como por ejemplo: **Usuarios de Redirección de carpetas**.
    - En **Ámbito de grupo**, selecciona **Seguridad** y, a continuación, selecciona **Global**.
5. En la sección **Miembros**, selecciona **Agregar**. Aparece el cuadro de diálogo Seleccionar Usuarios, Contactos, Equipos, Cuentas de servicio o Grupos.
6. Escribe los nombres de los usuarios o grupos en los que quieres implementar la característica Redirección de carpetas, selecciona **Aceptar** y, después, **Aceptar** de nuevo.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Paso 2: Crear un recurso compartido de archivos para carpetas redirigidas

Si aún no tienes un recurso compartido de archivos para las carpetas redirigidas, utiliza el procedimiento siguiente para crear un recurso compartido de archivos en un servidor que ejecute Windows Server 2012.

> [!NOTE]
> Algunas funciones pueden ser distintas o no estar disponibles si creas el recurso compartido de archivos en un servidor que ejecute otra versión de Windows Server.

Aquí se muestra cómo crear un recurso compartido de archivos en Windows Server 2019, Windows Server 2016 y Windows Server 2012:

1. En el panel de navegación del Administrador del servidor, selecciona **Servicios de archivos y almacenamiento** y, después, elije **Recursos compartidos** para mostrar la página Recursos compartidos.
2. En el icono **Recursos compartidos**, selecciona **Tareas** y, después, **Nuevo recurso compartido**. Se abrirá el Asistente para nuevo recurso compartido.
3. En la página **Seleccionar perfil**, selecciona **Recurso compartido SMB - Rápido**. Si tienes instalado el Administrador de recursos del servidor de archivos y usas las propiedades de administración de carpetas, en su lugar selecciona **Recurso compartido SMB - Avanzado**.
4. En la página **Ubicación del recurso compartido** , selecciona el servidor y el volumen donde quieras crear el recurso compartido.
5. En la página **Nombre del recurso compartido**, escribe un nombre para el recurso compartido (por ejemplo, **Users$** ) en el cuadro **Nombre del recurso compartido**.
    >[!TIP]
    >Al crear el recurso compartido, puedes ocultarlo si escribes el símbolo ```$``` después del nombre del recurso compartido. Con esta acción se oculta el recurso compartido en los exploradores ocasionales.
6. En la página **Más opciones**, desactiva la casilla Habilitar disponibilidad continua, si se muestra, y, de manera opcional, selecciona las casillas **Habilitar enumeración basada en el acceso** y **Cifrar acceso a datos**.
7. En la página **Permisos**, selecciona **Personalizar permisos**. Se abrirá el cuadro de diálogo Configuración de seguridad avanzada.
8. Selecciona **Deshabilitar herencia** y, después, **Convertir permisos heredados en permiso explícito en este objeto**.
9. Establece los permisos como se describe en la tabla 1 y en la figura 1, elimina los permisos de los grupos y las cuentas que no aparecen en la lista y agrega permisos especiales al grupo Usuarios de Redirección de carpetas que creaste en el paso 1.
    
    ![Establecer los permisos para el recurso compartido de carpetas redirigidas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Establecer los permisos para el recurso compartido de carpetas redirigidas
10. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Propiedades de administración** , seleccione el valor de uso de carpeta **Archivos de usuario** .
11. Si elige el perfil **Recurso compartido SMB - Avanzado** , en la página **Cuota** , seleccione (de manera opcional) la cuota que quiera aplicar a los usuarios del recurso compartido.
12. En la página **Confirmación**, haz clic en **Crear**.

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Permisos necesarios para las carpetas redirigidas de hospedaje de recursos compartidos de archivos

| Cuenta de usuario  | Acceso  | Aplicable a  |
| --------- | --------- | --------- |
| Cuenta de usuario | Acceso | Aplicable a |
| System     | Control total        |    Esta carpeta, subcarpetas y archivos     |
| Administradores     | Control total       | Solo esta carpeta        |
| Creador/propietario     |   Control total      |   Solo subcarpetas y archivos      |
| Grupo de seguridad de usuarios que deben colocar datos en recurso compartido (Usuarios de Redirección de carpetas)     |   Mostrar carpeta o leer datos *(permisos avanzados)* <br /><br />Crear carpetas o anexar datos *(permisos avanzados)* <br /><br />Leer atributos *(permisos avanzados)* <br /><br />Leer atributos extendidos *(permisos avanzados)* <br /><br />Permisos de lectura *(permisos avanzados)*      |  Solo esta carpeta       |
| Otros grupos y cuentas     |  Ninguno (quitar)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Paso 3: Crear un GPO para Redirección de carpetas

Si aún no tienes ningún GPO creado para la configuración de Redirección de carpetas, usa el procedimiento siguiente para crear uno.

A continuación se indica cómo crear un GPO para Redirección de carpetas:

1. Abre el Administrador del servidor en un equipo que tenga instalada Administración de directivas de grupo.
2. En el menú **Herramientas**, selecciona **Administración de directivas de grupo**.
3. Haz clic con el botón derecho en el dominio o la unidad organizativa donde quieres configurar Redirección de carpetas y, después, selecciona **Create a GPO in this domain, and Link it here** (Crear un GPO en este dominio y vincularlo aquí).
4. En el cuadro de diálogo **Nuevo GPO**, escribe un nombre para el GPO (por ejemplo, **Configuración de Redirección de carpetas**) y selecciona **Aceptar**.
5. Haz clic con el botón secundario en el nuevo GPO y desactiva la casilla **Vínculo habilitado** . Esto no permite que se aplique el GPO hasta que termine de configurarlo.
6. Selecciona el GPO. En la sección **Filtrado de seguridad** de la pestaña **Ámbito**, selecciona **Usuarios autenticados** y, a continuación, selecciona **Quitar** para evitar que el GPO se aplique a todos los usuarios.
7. En la sección **Filtrado de seguridad**, selecciona **Agregar**.
8. En el cuadro de diálogo **Seleccionar usuario, equipo o grupo**, escribe el nombre del grupo de seguridad que creaste en el paso 1 (por ejemplo, **Usuarios de Redirección de carpetas**) y haz clic en **Aceptar**.
9. Selecciona la pestaña **Delegación**, elige **Agregar**, escribe **Usuarios autenticados**, selecciona **Aceptar** y, a continuación, **Aceptar** de nuevo para aceptar los permisos de lectura predeterminados.
    
    Este paso es necesario debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> Debido a los cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), ahora debes conceder al grupo Usuarios autenticados permisos de lectura para el GPO de Redirección de carpetas; de lo contrario, el GPO no se aplicará a los usuarios o, si ya se ha aplicado, se quitará y se redirigirán las carpetas de nuevo al equipo local. Para más información, consulta [Implementación de la actualización de seguridad de la directiva de grupo MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Paso 4: Configurar la redirección de carpetas con Archivos sin conexión

Después de crear un GPO para la configuración de Redirección de carpetas, edita la configuración de la directiva de grupo para habilitar y configurar la característica Redirección de carpetas, como se describe en el procedimiento siguiente.

> [!NOTE]
> La característica Archivos sin conexión está habilitada de forma predeterminada para las carpetas redirigidas en equipos cliente de Windows y deshabilitada en los equipos que ejecutan Windows Server, a menos que el usuario lo cambie. Para usar directiva de grupo con el fin de controlar si Archivos sin conexión está habilitado, usa la configuración de directiva **Permitir o denegar el uso de la característica Archivos sin conexión**.
> Para obtener información acerca de algunas de las demás opciones de configuración de directiva de grupo de Archivos sin conexión, consulta [Habilitar la funcionalidad de Archivos sin conexión avanzada](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>) y [Configuración de la directiva de grupo para Archivos sin conexión](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Aquí se muestra cómo configurar Redirección de carpetas en Directiva de grupo:

1. En Administración de directivas de grupo, haz clic con el botón derecho en el GPO que creaste (por ejemplo, **Configuración de Redirección de carpetas**) y, a continuación, selecciona **Editar**.
2. En la ventana Editor de administración de directivas de grupo, navega hasta **Configuración de usuario**, y expande **Directivas**, **Configuración de Windows** y **Redirección de carpetas**.
3. Haz clic con el botón derecho en una carpeta que quieras redirigir (por ejemplo, **Documentos**) y, a continuación, selecciona **Propiedades**.
4. En el cuadro de diálogo **Propiedades**, en el cuadro **Configuración**, selecciona **Basic - Redirect everyone's folder to the same location** (Básica: Redirigir la carpeta de todos los usuarios a la misma ubicación).

    > [!NOTE]
    > Para aplicar la característica Redirección de carpetas a los equipos cliente que ejecutan Windows XP o Windows Server 2003, selecciona la pestaña **Configuración** y activa la casilla **Aplicar también la directiva de redirección a los sistemas operativos Windows 2000, Windows 2000 Server, Windows XP y Windows Server 2003**.

5. En la sección **Ubicación de la carpeta de destino**, selecciona **Crear una carpeta para cada usuario en la ruta raíz** y, a continuación, en el cuadro **Ruta de acceso raíz**, escribe la ruta de acceso al recurso compartido de archivos donde se almacenan las carpetas redirigidas, como, por ejemplo: **\\\\fs1.corp.contoso.com\\users$**
6. Selecciona la pestaña **Configuración** y, en la sección **Eliminación de la directiva**, selecciona de forma opcional **Devolver la carpeta a la ubicación local de perfil de usuario cuando la directiva se haya quitado** (esta opción puede hacer que Redirección de carpetas se comporte de forma más predecible para administradores y usuarios).
7. Selecciona **Aceptar** y, a continuación, elige **Sí** en el cuadro de diálogo Advertencia.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Paso 5: Habilitar el GPO de Redirección de carpetas

Una vez completada la configuración de la directiva de grupo de Redirección de carpetas, el paso siguiente consiste en habilitar el GPO, lo que permite que se aplique a los usuarios afectados.

> [!TIP]
> Si quieres implementar el soporte de equipo principal u otra configuración de directivas, hazlo ahora, antes de habilitar el GPO. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal.

A continuación se explica cómo habilitar el GPO de Redirección de carpetas:

1. Abra Administración de directivas de grupo.
2. Haz clic con el botón derecho en el GPO que creaste y selecciona **Vínculo habilitado**. Aparecerá una casilla junto al elemento de menú.

## <a name="step-6-test-folder-redirection"></a>Paso 6: Usar Redirección de carpetas

Para probar la característica Redirección de carpetas, inicia sesión en un equipo con una cuenta de usuario configurada para Redirección de carpetas. A continuación, confirma que se han redirigido las carpetas y los perfiles.

A continuación se indica cómo probar Redirección de carpetas:

1. Inicia sesión en un equipo principal (si has habilitado el soporte de equipo principal) con una cuenta de usuario en la que hayas habilitado Redirección de carpetas.
2. Si el usuario ha iniciado sesión en el equipo anteriormente, abre un símbolo del sistema con privilegios elevados y escribe el comando siguiente para asegurarte de que se aplique la última configuración de directiva de grupo en el equipo cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abra el Explorador de archivos.
4. Haz clic con el botón derecho en una carpeta redirigida (por ejemplo, la carpeta Mis documentos de la biblioteca Documentos) y, a continuación, selecciona **Propiedades**.
5. Selecciona la pestaña **Ubicación** y confirma que la ruta de acceso muestra el recurso compartido de archivos especificado en lugar de una ruta de acceso local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Apéndice A: Lista de comprobación para implementar Redirección de carpetas

| Estado           | Acción |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparar el dominio<br>- Unir los equipos al dominio<br>- Crear cuentas de usuario |
| ☐<br><br><br>   | 2. Crear un grupo de seguridad para Redirección de carpetas<br>- Nombre del grupo:<br>- Miembros: |
| ☐<br><br>       | 3. Crear un recurso compartido de archivos para carpetas redirigidas<br>- Nombre del recurso compartido de archivos: |
| ☐<br><br>       | 4. Crear un GPO para Redirección de carpetas<br>- Nombre de GPO: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Definir la configuración de directiva de Redirección de carpetas y Archivos sin conexión<br>- Carpetas redirigidas:<br>- ¿Está habilitada la compatibilidad con Windows 2000, Windows XP y Windows Server 2003?<br>- ¿La característica Archivos sin conexión está habilitada? (está habilitada de forma predeterminada en los equipos cliente de Windows)<br>- ¿El modo Siempre sin conexión está habilitado?<br>- ¿La sincronización de archivos en segundo plano está habilitada?<br>- ¿El movimiento optimizado de carpetas redirigidas está habilitado? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. (Opcional) habilitar el soporte de equipo principal<br>- ¿Está basado en el equipo o en el usuario?<br>- Designar equipos principales para usuarios<br>- Ubicación del usuario y asignaciones de equipos principales:<br>- (Opcional) habilitar el soporte de equipo principal para Redirección de carpetas<br>- (Opcional) habilitar el soporte de equipo principal para Perfiles de usuario móvil |
| ☐         | 7. Habilitar el GPO de Redirección de carpetas |
| ☐         | 8. Usar Redirección de carpetas |

## <a name="change-history"></a>Historial de cambios

En la tabla siguiente se resumen los cambios más importantes realizados en este tema.

| Fecha | Descripción | Razón|
| --- | --- | --- |
| 18 de enero de 2017 | Se ha agregado un paso a [Paso 3: Crear un GPO para Redirección de carpetas](#step-3-create-a-gpo-for-folder-redirection) para delegar los permisos de lectura a los usuarios autenticados, lo que ahora es necesario debido a una actualización de seguridad de la directiva de grupo. | Comentarios de los clientes |

## <a name="more-information"></a>Información adicional

* [Redireccionamiento de carpetas, Archivos sin conexión y Perfiles de usuario móvil](folder-redirection-rup-overview.md)
* [Implementar equipos principales para Redirección de carpetas y Perfiles de usuario móvil](deploy-primary-computers.md)
* [Habilitar la funcionalidad de Archivos sin conexión avanzada](enable-always-offline.md)
* [Declaración de soporte técnico de Microsoft sobre los datos de perfil de usuario replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Transferir localmente aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)
