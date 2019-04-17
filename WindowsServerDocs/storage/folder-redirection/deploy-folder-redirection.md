---
title: Implementar la redirección de carpetas con archivos sin conexión
description: Cómo usar Windows Server para implementar la redirección de carpetas con archivos sin conexión en los equipos cliente de Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 33942db34314e0ff60b24d4b9c8e5e33b4ca92fd
ms.sourcegitcommit: 5549ac178f8f3d116e88761a95223063a636ac94
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4376633"
---
# Implementar la redirección de carpetas con archivos sin conexión

>Se aplica a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Vista

En este tema se describe cómo usar Windows Server para implementar la redirección de carpetas con archivos sin conexión en los equipos cliente de Windows.

Para obtener una lista de los cambios recientes en este tema, consulta [historial de cambios](#change-history).

>[!IMPORTANT]
>Debido a los cambios de seguridad realizado en [MS16-072](https://support.microsoft.com/en-us/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), hemos actualizado [paso 3: crear un GPO para la redirección de carpetas](#step-3:-create-a-gpo-for-folder-redirection) de este tema para que Windows pueda aplicar correctamente la directiva de redirección de carpetas (y no revertir carpetas redirigidas en los equipos afectados).

## Requisitos previos

### Requisitos de hardware

Redirección de carpetas requiere un equipo basado en x86 o x64; no se admite por Rect de Windows®.

### Requisitos de software

Redirección de carpetas tiene los siguientes requisitos de software:

- Para administrar la redirección de carpetas, debe iniciar sesión como miembro del grupo de seguridad de administradores de dominio, el grupo de seguridad de administradores de empresa o el grupo de seguridad propietarios del creador de directiva de grupo.
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.
- Los equipos cliente deben estar unidos a la Active Directory Domain Services (AD DS) que se está administrando.
- Un equipo debe estar disponible con administración de directivas de grupo y centro de administración de Active Directory instalado.
- Un servidor de archivos debe estar disponible para hospedar carpetas redirigidas.
    - Si el recurso compartido de archivos usa espacios de nombres DFS, las carpetas DFS (vínculos) deben tener un único destino para impedir que los usuarios realicen modificaciones conflictivas en distintos servidores.
    - Si el recurso compartido de archivos usa la replicación DFS para replicar el contenido con otro servidor, los usuarios deben poder acceder a solo el servidor de origen para impedir que los usuarios realicen modificaciones conflictivas en distintos servidores.
    - Al usar un recurso compartido de archivos agrupados en clúster, deshabilita la disponibilidad continua en el recurso compartido de archivos para evitar problemas de rendimiento con redirección de carpetas y archivos sin conexión. Además, archivos sin conexión no es posible que pasar al modo sin conexión de 3 a 6 minutos después de que un usuario pierde el acceso a un recurso compartido de archivos disponibles continuamente, que podría frustrar a los usuarios que aún no usan el modo sin conexión siempre de archivos sin conexión.

>[!NOTE]
>Algunas características más recientes de redirección de carpetas tienen equipo cliente adicionales y requisitos de esquema de Active Directory. Para obtener más información, consulta [implementar principales equipos](deploy-primary-computers.md), [Deshabilitar archivos sin conexión en carpetas](disable-offline-files-on-folders.md), [Habilitar siempre modo sin conexión](enable-always-offline.md)y [habilitan el movimiento de carpeta optimizada](enable-optimized-moving.md).

## Paso 1: Crear un grupo de seguridad de redirección de carpetas

Si el entorno no ya está configurado con redirección de carpetas, el primer paso es crear un grupo de seguridad que contiene todos los usuarios a la que quieres aplicar la configuración de directiva de redirección de carpetas.

Aquí te mostramos cómo crear un grupo de seguridad para la redirección de carpetas:

1. Abre el Administrador de servidor en un equipo con el centro de administración de Active Directory instalado.
2. En el menú **Herramientas** , selecciona **El centro de administración de Active Directory**. Aparecerá el Centro de administración de Active Directory.
3. Haz clic en la unidad organizativa o el dominio adecuado, seleccione **nuevo**y, a continuación, selecciona el **grupo**.
4. En la ventana **Crear grupo** de la sección **Grupo**, especifica la configuración siguiente:
    - En **nombre de grupo**, escribe el nombre del grupo de seguridad, por ejemplo: **Los usuarios de redirección de carpetas**.
    - En el **ámbito de grupo**, seleccione **la seguridad**y, a continuación, seleccione **Global**.
5. En la sección de **miembros** , selecciona **Agregar**. Aparecerá el cuadro de diálogo Seleccionar usuarios, contactos, equipos, cuentas de servicio o grupos.
6. Escriba los nombres de los usuarios o grupos a los que quieres implementar la redirección de carpetas, haga **clic en Aceptar**y, a continuación, seleccione **Aceptar** de nuevo.

## Paso 2: Crear un recurso compartido de archivos para las carpetas redirigidas

Si aún no tienes un recurso compartido de archivos para las carpetas redirigidas, usa el siguiente procedimiento para crear un recurso compartido de archivos en un servidor que ejecuta Windows Server 2012.

>[!NOTE]
>Algunas funciones podrían diferir o no estar disponible si se crea el recurso compartido de archivos en un servidor que ejecute otra versión de Windows Server.

Aquí te mostramos cómo crear un recurso compartido de archivos en Windows Server 2012 y Windows Server 2016:

1. En el panel de navegación del Administrador de servidores, selecciona **File and Storage Services**y, a continuación, selecciona **los recursos compartidos** para mostrar la página de recursos compartidos.
2. En el icono de **recursos compartidos** , selecciona **las tareas**y, a continuación, selecciona el **Nuevo recurso compartido**. Aparece el Asistente de recurso compartido de nuevo.
3. En la página **Seleccionar perfil** , selecciona **Rápido recurso compartido de SMB**. Si tienes instalado el Administrador de recursos de servidor de archivos y estás usando las propiedades de administración de carpeta, en su lugar, selecciona el **Recurso compartido de SMB - avanzada**.
4. En la página **Ubicación de recurso compartido** , selecciona el servidor y el volumen en el que quieres crear el recurso compartido.
5. En la página de **Nombre de recurso compartido** , escribe un nombre para el recurso compartido (por ejemplo, **los usuarios$**) en el cuadro de **nombre del recurso compartido** .
    >[!TIP]
    >Al crear el recurso compartido, ocultar el recurso compartido poniendo un ```$``` después del nombre de recurso compartido. Esto ocultará el recurso compartido de exploradores ocasionales.
6. En la página de **Otras opciones de configuración** , desactiva la casilla de una disponibilidad continua habilitar, si está presente y, opcionalmente, activa las casillas de **Habilitar la enumeración basada en el acceso** y el **acceso a cifrar los datos** .
7. En la página de **permisos** , selecciona **Personalizar los permisos de …**. Aparece el cuadro de diálogo de configuración de seguridad avanzada.
8. Selecciona **Deshabilitar la herencia**y, a continuación, selecciona **convertir los permisos en permiso explícito sobre este objeto heredados**.
9. Establece los permisos como se describe en la tabla 1 y se muestra en la figura 1, quitar permisos de las cuentas y grupos en la lista y agregar permisos especiales para el grupo de usuarios de redirección de carpetas que creaste en el paso 1.
    
    ![Configuración de los permisos para el recurso compartido de carpetas redirigidas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Configuración de los permisos para el recurso compartido de carpetas redirigidas
10. Si has elegido el perfil de **Recurso compartido de SMB - avanzada** , en la página de **Propiedades de administración** , selecciona el valor de uso de la carpeta de **Archivos de usuario** .
11. Si has elegido el perfil de **Recurso compartido SMB - avanzada** , en la página de **cuota** , opcionalmente, selecciona una cuota para aplicar a los usuarios del recurso compartido.
12. En la página de **confirmación** , seleccione **Create.**

### Los permisos necesarios para el archivo compartan hospedar carpetas redirigidas

<table>
<tbody>
<tr class="odd">
<td>Cuenta de usuario</td>
<td>Access</td>
<td>Se aplica a</td>
</tr>
<tr class="even">
<td>Sistema</td>
<td>Control total</td>
<td>Esta carpeta, subcarpetas y archivos</td>
</tr>
<tr class="odd">
<td>Administradores</td>
<td>Control total</td>
<td>Esta carpeta solo</td>
</tr>
<tr class="even">
<td>Creador o propietario</td>
<td>Control total</td>
<td>Solo los archivos y subcarpetas</td>
</tr>
<tr class="odd">
<td>Grupo de seguridad de los usuarios que necesiten colocar datos en el recurso compartido (redirección de carpetas a los usuarios)</td>
<td>Lista de carpeta / leer datos<sup>1</sup><br />
<br />
Crear carpetas / Anexar datos<sup>1</sup><br />
<br />
Leer los atributos<sup>1</sup><br />
<br />
Atributos extendidos de lectura<sup>1</sup><br />
<br />
Permisos de lectura<sup>1</sup></td>
<td>Esta carpeta solo</td>
</tr>
<tr class="even">
<td>Otros grupos y cuentas</td>
<td>Ninguno (quitar)</td>
<td></td>
</tr>
</tbody>
</table>

1 permisos avanzados

## Paso 3: Crear un GPO para la redirección de carpetas

Si aún no tienes un GPO creado para la configuración de redirección de carpetas, usa el siguiente procedimiento para crear uno.

Aquí te mostramos cómo crear un GPO para la redirección de carpetas:

1. Abre el Administrador de servidor en un equipo con administración de directivas de grupo instalado.
2. En el menú **Herramientas** , selecciona **La administración de directivas**.
3. Haz clic en el dominio o unidad organizativa en la que quieres configurar redirección de carpetas y luego selecciona **crear un GPO en este dominio y vincularlo aquí**.
4. En el cuadro de diálogo **Nuevo GPO** , escribe un nombre para el GPO (por ejemplo, la **Configuración de redirección de carpetas**) y, a continuación, selecciona **Aceptar**.
5. Haz clic en el GPO recién creado y, a continuación, desactiva la casilla **Vínculo habilitado** . Esto impide que el GPO que se aplica hasta que termine de configuración.
6. Selecciona el GPO. En la sección de **Filtrado de seguridad** de la pestaña de **ámbito** , seleccione **Usuarios autenticados**y, a continuación, selecciona **Quitar** para impedir que el GPO que se aplica a todos los usuarios.
7. En la sección de **Filtrado de seguridad** , selecciona **Agregar**.
8. En el cuadro de diálogo **Seleccionar usuarios, equipos o grupo** , escribe el nombre del grupo de seguridad que creaste en el paso 1 (por ejemplo, **Los usuarios de redirección de carpetas**) y, a continuación, selecciona **Aceptar**.
9. Selecciona la pestaña de **delegación** , selecciona **Agregar**, escriba **Usuarios autenticados**, haga **clic en Aceptar**y, a continuación, selecciona **Aceptar** para aceptar los permisos de lectura de forma predeterminada.
    
    Este paso es necesario debido a cambios de seguridad realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

>[!IMPORTANT]
>Debido a la seguridad que se deben ofrecer los cambios realizados en [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), que ahora el grupo de usuarios autenticados delega permisos de lectura en el GPO de redirección de carpetas: lo contrario, el GPO no se aplican a los usuarios o, si ya se ha aplicado, se quita el GPO, redirigir copia de carpetas en el equipo local. Para obtener más información, consulta [Implementar grupo la directiva de seguridad Update MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## Paso 4: Configurar la redirección de carpetas con archivos sin conexión

Después de crear un GPO para la configuración de redirección de carpetas, modificar la configuración de directiva de grupo para habilitar y configurar la redirección de carpetas, como se explica en el siguiente procedimiento.

>[!NOTE]
>Archivos sin conexión está habilitada de manera predeterminada para las carpetas redirigidas en los equipos de cliente de Windows y deshabilitada en equipos que ejecutan Windows Server, a menos que el usuario cambia. Para usar la directiva de grupo para controlar si se habilita archivos sin conexión, usa la **Permitir o denegar el uso de la característica de archivos sin conexión** configuración de directiva.
> Para obtener información sobre algunas de las otras opciones de configuración de directiva de grupo de archivos sin conexión, consulta [Habilitar avanzada sin conexión archivos funcionalidad](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)y [Configurar la directiva de grupo de archivos sin conexión](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Aquí te mostramos cómo configurar la redirección de carpetas en la directiva de grupo:

1. En administración de directivas de grupo, haz clic en el GPO que creaste (por ejemplo, **Configuración de redirección de carpetas**) y, a continuación, seleccione **Editar**.
2. En la ventana del Editor de administración de directivas de grupo, ve a **Configuración de usuario**, **las directivas**, a continuación, **Configuración de Windows**y, a continuación, **Redirección de carpetas**.
3. Haz clic en una carpeta que desea redirigir (por ejemplo, **documentos**) y, a continuación, selecciona **Propiedades**.
4. En el cuadro de diálogo de **Propiedades** , en el cuadro de **configuración** , selecciona **básico: redirigir la carpeta de todos a la misma ubicación**.

    > [!NOTE]
    > Para aplicar la redirección de carpetas a los equipos cliente que ejecutan Windows XP o Windows Server 2003, selecciona la pestaña **configuración** y selecciona el **también se aplican la directiva de redirección a Windows 2000, Windows 2000 Server, Windows XP y Windows Server 2003 operativo sistemas** casilla de verificación.
5. En la sección de la **ubicación de la carpeta de destino** , seleccione **crear una carpeta para cada usuario en la ruta de acceso raíz** y, a continuación, en el cuadro de **Ruta de acceso raíz** , escriba la ruta de acceso al recurso compartido de archivo almacenar carpetas redirigidas, por ejemplo: **\\\fs1.corp.contoso.com\\ los usuarios$**
6. Selecciona la pestaña **configuración** y en la sección de **Eliminación de la directiva** , opcionalmente, seleccione **redirigir la carpeta a la ubicación de perfil de usuario local cuando se quita la directiva** (esta configuración puede ayudar a aumentar la redirección de carpetas se comportan más de manera predecible para adminisitrators y los usuarios).
7. Haga **clic en Aceptar**y, a continuación, selecciona **"Sí"** en el cuadro de diálogo de advertencia.

## Paso 5: Habilitar la redirección de carpetas GPO

Una vez que hayas terminado de establecer la configuración de directiva de grupo de redirección de carpetas, el siguiente paso es habilitar el GPO, que se le permite que se aplicará a los usuarios afectados.

>[!TIP]
>Si vas a implementar la compatibilidad de equipos principal u otras opciones de configuración de directiva, hazlo ahora, antes de habilitar el GPO. Esto impide que los datos de usuario se copien en los equipos no principal antes de habilita la compatibilidad de equipos principal.

Aquí te mostramos cómo habilitar el GPO de redirección de carpetas:

1. Administración de directivas de grupo de Open.
2. Haz clic en el GPO que creaste y, a continuación, selecciona el **Vínculo habilitado**. Una casilla de verificación aparecerá junto al elemento de menú.

## Paso 6: Probar la redirección de carpetas

Para probar la redirección de carpetas, inicia sesión en un equipo con una cuenta de usuario configurada para redirección de carpetas. A continuación, confirma que se redirigen las carpetas y perfiles.

Aquí te mostramos cómo probar la redirección de carpetas:

1. Inicia sesión en un equipo principal (si se habilita la compatibilidad de equipos principal) con una cuenta de usuario que se haya habilitado la redirección de carpetas.
2. Si el usuario haya iniciado previamente el equipo, abre un símbolo del sistema con privilegios elevados y, a continuación, escribe el siguiente comando para asegurarse de que se aplica la configuración de directiva de grupo más reciente en el equipo cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abre el Explorador de archivos.
4. Haz clic en una carpeta redirigida (por ejemplo, la carpeta Mis documentos en la biblioteca de documentos) y, a continuación, selecciona **Propiedades**.
5. Selecciona la pestaña de **ubicación** y confirma que la ruta de acceso muestre el recurso compartido de archivos que especificaste en lugar de una ruta de acceso local.

## Apéndice A: lista de comprobación para la implementación de redirección de carpetas

|Estado|Acción|
|:---:|---|
|☐<br>☐<br>☐|1. Preparar dominio<br>-Unir equipos al dominio<br>-Crear cuentas de usuario|
|☐<br><br><br>|2. Crear grupo de seguridad para la redirección de carpetas<br>-Nombre del grupo:<br>-Miembros:|
|☐<br><br>|3. crear un recurso compartido de archivos para las carpetas redirigidas<br>: Nombre del recurso compartido el archivo:|
|☐<br><br>|4. crear un GPO para la redirección de carpetas<br>-Nombre del GPO:|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. configurar opciones de directiva de redirección de carpetas y archivos sin conexión<br>-Redirigidas carpetas:<br>¿-Windows 2000, Windows XP y Windows Server 2003 compatibilidad con habilitado?<br>¿-Sin conexión archivos habilitado? (habilitada de manera predeterminada en los equipos de cliente de Windows)<br>¿: El modo sin conexión siempre habilitado?<br>¿-Sincronización de archivos en segundo plano habilitada?<br>¿-Optimizado el movimiento de carpetas redirigidas habilitado?|
|☐<br><br>☐<br><br>☐<br>☐|6. (habilitar el equipo principal compatibilidad con opcional)<br>¿-Basado en usuario o equipo basados en?<br>-Designar equipos principales para los usuarios<br>-Ubicación del usuario y las asignaciones de equipo principal:<br>-(Opcional) habilitar equipo principal la compatibilidad con la redirección de carpetas<br>-(Opcional) habilitar equipo principal la compatibilidad con perfiles de usuario móvil|
|☐|7. habilitar el redireccionamiento de carpetas GPO|
|☐|8. redirección de carpetas de prueba de|

## Historial de cambios

La siguiente tabla resume algunas de los cambios más importantes en este tema.

|Date|Descripción|Razón|
|---|---|---|
|18 de enero de 2017|Agrega un paso para [paso 3: crear un GPO para la redirección de carpetas](#step-3:-create-a-gpo-for-folder-redirection) delegar permisos de lectura a los usuarios autenticados, que ahora es necesario debido a una actualización de seguridad de la directiva de grupo.|Comentarios de los clientes.|

## Más información

* [Redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
* [Implementar equipos principales de redirección de carpetas y perfiles de usuario móvil](deploy-primary-computers.md)
* [Habilitar funciones avanzadas de archivos sin conexión](enable-always-offline.md)
* [Declaración de compatibilidad de Microsoft alrededor de los datos de perfil de usuario replicado](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Transferir localmente aplicaciones con DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solución de problemas de empaquetado, implementación y consulta de aplicaciones basadas en tiempo de ejecución de Windows](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)