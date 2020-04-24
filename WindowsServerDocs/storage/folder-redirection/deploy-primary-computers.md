---
title: Implementación de equipos principales para Redirección de carpetas y Perfiles de usuario móvil
description: Cómo habilitar el soporte de equipo principal y designar equipos principales para usuarios con Redirección de carpetas y Perfiles de usuario móvil.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394441"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Implementación de equipos principales para Redirección de carpetas y Perfiles de usuario móvil

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

En este tema se describe cómo habilitar el soporte de equipo principal y cómo designar equipos principales para los usuarios. Esto te permite controlar qué equipos usan las características Redirección de carpetas y Perfiles de usuario móvil.

> [!IMPORTANT]
> Al habilitar el soporte de equipo principal para Perfiles de usuario móvil, también debes habilitarlo siempre para Redirección de carpetas. De este modo, los documentos y otros archivos de usuario se mantienen fuera de los perfiles de usuario, lo que ayuda a mantener perfiles reducidos y tiempos de inicio de sesión rápidos.

## <a name="prerequisites"></a>Requisitos previos

## <a name="software-requirements"></a>Requisitos de software

El soporte de equipo principal tiene los siguientes requisitos:

- El esquema de Active Directory Domain Services (AD DS) debe actualizarse para incluir las adiciones del esquema de Windows Server 2012 (la instalación de un controlador de dominio de Windows Server 2012 actualiza automáticamente el esquema). Para obtener información acerca de la actualización del esquema de AD DS, consulta [Integración de Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) y [Ejecución de Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

> [!TIP]
> Aunque el soporte de equipo principal requiere las características Redirección de carpetas o Perfiles de usuario móvil, si implementas estas tecnologías por primera vez, es mejor configurar el soporte de equipo principal antes de habilitar los GPO que configuran dichas características. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal. Para obtener información sobre la configuración, consulta [Implementar la redirección de carpetas](deploy-folder-redirection.md) e [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Paso 1: Designar equipos principales para usuarios

El primer paso para implementar el soporte de equipos principales es designar los equipos principales para cada usuario. Para ello, usa el Centro de administración de Active Directory para obtener el nombre distintivo de los equipos pertinentes y, a continuación, establece el atributo **msDs-PrimaryComputer**.

> [!TIP]
> Si quieres usar Windows PowerShell para trabajar con equipos principales, consulta la entrada de blog [Profundizar un poco más en el equipo principal de Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Aquí se muestra cómo especificar los equipos principales para los usuarios:

1. Abra el Administrador del servidor en un equipo que tenga instaladas las Herramientas de administración de Active Directory.
2. En el menú **Herramientas** , selecciona **Centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Ve al contenedor **Equipos** en el dominio adecuado.
4. Haz clic con el botón derecho en un equipo que quieras designar como equipo principal y, a continuación, selecciona **Propiedades**.
5. En el panel de navegación, selecciona **Extensiones**.
6. Selecciona la pestaña **Editor de atributos**, desplázate hasta **distinguishedName**, selecciona **Ver**, haz clic con el botón derecho en el valor que aparece, y selecciona **Copiar**, **Aceptar** y, a continuación, **Cancelar**.
7. Navega al contenedor **Usuarios** en el dominio adecuado, haz clic con el botón derecho en el usuario al cual quieres asignar el equipo y posteriormente haz clic en **Propiedades**.
8. En el panel de navegación, selecciona **Extensiones**.
9. Selecciona la pestaña **Editor de atributos**, y selecciona **msDs-PrimaryComputer** y, a continuación, **Editar**. Aparece el cuadro de diálogo Editor de cadena multivalor.
10. Haz clic con el botón derecho en el cuadro de texto y selecciona **Pegar**, **Agregar**, **Aceptar** y, a continuación, **Aceptar** de nuevo.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Paso 2: Opcionalmente, habilita los equipos principales para Redirección de carpetas en Directiva de grupo

El siguiente paso es opcional y consiste en configurar la directiva de grupo para habilitar el soporte de equipo principal para Redirección de carpetas. Esto permite que las carpetas de un usuario se redirijan en equipos designados como equipos principales del usuario, pero no en otros equipos. Puedes controlar los equipos principales para Redirección de carpetas por equipo o por usuario.

A continuación se explica cómo habilitar los equipos principales para Redirección de carpetas:

1. En Administración de directivas de grupo, haz clic con el botón derecho en el GPO que creaste para la configuración inicial de Redirección de carpetas o Perfiles de usuario móvil (por ejemplo, **Configuración de redirección** o **Configuración de perfil de usuario móvil**) y, a continuación, selecciona **Editar**.
2. Para habilitar el soporte de equipos principales mediante la directiva de grupo basada en el equipo, ve a **Configuración del equipo**. Para la directiva de grupo basada en el usuario, ve a **Configuración de usuario**.
    - La directiva de grupo basada en el equipo se aplica el procesamiento de equipo principal para todos los equipos a los que se aplica el GPO, lo que afecta a todos los usuarios de los equipos.
    - La directiva de grupo basada en el usuario se aplica el procesamiento de equipo principal para todas las cuentas de usuario a las que se aplica el GPO, lo que afecta a todos los equipos en los que los usuarios inician sesión.
3. En **Configuración del equipo** o **Configuración de usuario**, ve a **Directivas**, **Plantillas administrativas**, **Sistema** y **Redirección de carpetas**.
4. Haz clic con el botón derecho en **Redirigir carpetas en los equipos primarios solamente** y, después, selecciona **Editar**.
5. Selecciona **Habilitado** y después selecciona **Aceptar**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Paso 3: Opcionalmente, habilita los equipos principales para Perfiles de usuario móvil en Directiva de grupo

El siguiente paso es opcional y consiste en configurar la directiva de grupo para habilitar el soporte de equipo principal para Perfiles de usuario móvil. Esto permite la itinerancia de un perfil de usuario en equipos designados como equipos principales del usuario, pero no en otros equipos.

A continuación se explica cómo habilitar los equipos principales para Perfiles de usuario móvil:

1. Habilita el soporte de equipo principal para Redirección de carpetas, si no lo has hecho aún.<br>De este modo, los documentos y otros archivos de usuario se mantienen fuera de los perfiles de usuario, lo que ayuda a mantener perfiles reducidos y tiempos de inicio de sesión rápidos.
2. En Administración de directivas de grupo, haz clic con el botón derecho en el GPO que creaste (por ejemplo, **Redirección de carpetas y Configuración de perfil de usuario móvil**) y, a continuación, selecciona **Editar**.
3. Navega a **Configuración del equipo** y, a continuación, a **Directivas**, **Plantillas administrativas**, **Sistema** y **Perfiles de usuario**.
4. Haz clic con el botón derecho en **Descargar perfiles móviles solo en equipos primarios** y, después, selecciona **Editar**.
5. Selecciona **Habilitado** y después selecciona **Aceptar**.

## <a name="step-4-enable-the-gpo"></a>Paso 4: Habilitar el GPO

Una vez completada la configuración de Redirección de carpetas y Perfiles de usuario móvil, habilita el GPO si todavía no lo has hecho. De este modo, se aplicará a los usuarios y equipos afectados.

Aquí se muestra cómo habilitar los GPO de Redirección de carpetas y Perfiles de usuario móvil:

1. Abre Administración de directivas de grupo.
2. Haz clic con el botón derecho en el GPO que creaste y selecciona **Vínculo habilitado**. Debería aparecer una casilla junto al elemento de menú.

## <a name="step-5-test-primary-computer-function"></a>Paso 5: Probar la funcionalidad del equipo principal

Para probar el soporte de equipo principal, inicia sesión en un equipo principal, confirma que se han redirigido las carpetas y los perfiles y, a continuación, inicia sesión en un equipo no principal y confirma que no se han redirigido las carpetas y los perfiles.

A continuación se explica cómo probar la funcionalidad del equipo principal:

1. Inicia sesión en un equipo principal designado con una cuenta de usuario para la que hayas habilitado Redirección de carpetas o Perfiles de usuario móvil.
2. Si la cuenta de usuario se ha usado para iniciar sesión en el equipo previamente, abre una sesión de Windows PowerShell o una ventana del símbolo del sistema como administrador, escribe el siguiente comando y, a continuación, cierra la sesión cuando se te solicite para asegurarte de que se aplica la configuración de directiva de grupo más reciente al equipo cliente:

    ```PowerShell
    Gpupdate /force
    ```

3. Abra el Explorador de archivos.
1. Haz clic con el botón derecho en una carpeta redirigida (por ejemplo, la carpeta Mis documentos de la biblioteca Documentos) y, a continuación, selecciona **Propiedades**.
1. Selecciona la pestaña **Ubicación** y confirma que la ruta de acceso muestra el recurso compartido de archivos especificado en lugar de una ruta de acceso local. Para confirmar que el perfil de usuario es móvil, abre el **Panel de control**, selecciona **Sistema y seguridad**, **Sistema**, **Configuración avanzada del sistema** y **Configuración** en la sección Perfiles de usuario y, después, busca **Móvil** en la columna **Tipo**.
1. Inicia sesión con la misma cuenta de usuario en un equipo que no esté designado como el equipo principal del usuario.
1. Repite los pasos 2 a 5, en lugar de buscar las rutas de acceso locales y un tipo de perfil **Local**.

> [!NOTE]
> Si las carpetas se redirigieron en un equipo antes de habilitar el soporte de equipo principal, las carpetas permanecerán redirigidas a menos que se configure el siguiente ajuste en la configuración de la directiva de redirección de carpetas de cada carpeta: **Devolver la carpeta a la ubicación local de perfil de usuario cuando la directiva se haya quitado**. Del mismo modo, los perfiles que se han movido previamente en un equipo determinado mostrarán **Móvil** en la columna **Tipo**; sin embargo, la columna **Estado** mostrará **Local**.

## <a name="more-information"></a>Información adicional

- [Implementar la redirección de carpetas con Archivos sin conexión](deploy-folder-redirection.md)
- [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)
- [Introducción al redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
- [Profundizar un poco más en el equipo principal de Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)