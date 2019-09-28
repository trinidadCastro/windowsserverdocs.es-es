---
title: Implementar equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles
description: Cómo habilitar la compatibilidad con equipos principales y designar equipos principales para usuarios con redirección de carpetas y perfiles de usuario móviles.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394441"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Implementar equipos principales para el redireccionamiento de carpetas y los perfiles de usuario móviles

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

En este tema se describe cómo habilitar la compatibilidad con equipos principales y cómo designar equipos principales para los usuarios. Esto le permite controlar qué equipos usan el redireccionamiento de carpetas y los perfiles de usuario móviles.

> [!IMPORTANT]
> Al habilitar la compatibilidad con equipos primarios para perfiles de usuario móviles, habilite siempre la compatibilidad con el equipo principal también para la redirección de carpetas. De este modo, los documentos y otros archivos de usuario se mantienen fuera de los perfiles de usuario, lo que ayuda a los perfiles a permanecer reducidos y los tiempos de inicio de sesión permanecen con rapidez.

## <a name="prerequisites"></a>Requisitos previos

## <a name="software-requirements"></a>Requisitos de software

El soporte de equipo principal tiene los siguientes requisitos:

- El esquema de Active Directory Domain Services (AD DS) debe actualizarse para incluir las adiciones de esquema de Windows Server 2012 (la instalación de un controlador de dominio de Windows Server 2012 actualiza automáticamente el esquema). Para obtener información acerca de cómo actualizar el esquema de AD DS, consulte [integración de adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) y [ejecución de adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

> [!TIP]
> Aunque la compatibilidad con equipos principales requiere redirección de carpetas y perfiles de usuario móvil, si va a implementar estas tecnologías por primera vez, es mejor configurar la compatibilidad con el equipo principal antes de habilitar los GPO que configuran el redireccionamiento de carpetas y Perfiles de usuario móviles. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal. Para obtener información sobre la configuración, vea [implementar el redireccionamiento de carpetas](deploy-folder-redirection.md) e [implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Paso 1: Designar equipos principales para usuarios

El primer paso para implementar la compatibilidad con equipos principales es designar los equipos principales para cada usuario. Para ello, use el centro de administración de Active Directory para obtener el nombre distintivo de los equipos pertinentes y, a continuación, establezca el atributo **msDs-PrimaryComputer** .

> [!TIP]
> Para usar Windows PowerShell para trabajar con equipos primarios, consulte la entrada de blog para [profundizar un poco más en el equipo principal de Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Aquí se muestra cómo especificar los equipos principales para los usuarios:

1. Abra el Administrador del servidor en un equipo que tenga instaladas las Herramientas de administración de Active Directory.
2. En el menú **herramientas** , seleccione **centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Navegue hasta el contenedor **equipos** en el dominio adecuado.
4. Haga clic con el botón secundario en un equipo que desee designar como equipo principal y, a continuación, seleccione **propiedades**.
5. En el panel de navegación, seleccione **extensiones**.
6. Seleccione la **pestaña Editor de atributos** , desplácese hasta **DistinguishedName**, seleccione **vista**, haga clic con el botón secundario en el valor que aparece, seleccione **copiar**, seleccione **Aceptar**y, a continuación, seleccione **Cancelar**.
7. Navegue hasta el contenedor **usuarios** en el dominio adecuado, haga clic con el botón secundario en el usuario al que desea asignar el equipo y, a continuación, seleccione **propiedades**.
8. En el panel de navegación, seleccione **extensiones**.
9. Seleccione la pestaña **Editor de atributos** , seleccione **msDs-PrimaryComputer** y, a continuación, seleccione **Editar**. Aparece el cuadro de diálogo Editor de cadena multivalor.
10. Haga clic con el botón derecho en el cuadro de texto, seleccione **pegar**, **Agregar**, seleccione **Aceptar**y, después, vuelva a seleccionar **Aceptar** .

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Paso 2: Opcionalmente, habilite los equipos principales para el redireccionamiento de carpetas en directiva de grupo

El siguiente paso consiste en configurar opcionalmente directiva de grupo para habilitar la compatibilidad con el equipo principal para la redirección de carpetas. Esto permite que las carpetas de un usuario se redirijan en equipos designados como equipos principales del usuario, pero no en otros equipos. Puede controlar los equipos principales para el redireccionamiento de carpetas por equipo o por usuario.

Aquí se muestra cómo habilitar los equipos principales para la redirección de carpetas:

1. En administración de directiva de grupo, haga clic con el botón secundario en el GPO que creó al realizar la configuración inicial de la redirección de carpetas y los perfiles de usuario móviles (por ejemplo, **configuración de redirección de carpetas** o **configuración de perfiles de usuario móviles**) y, a continuación, Seleccione **Editar**.
2. Para habilitar la compatibilidad de los equipos principales con el directiva de grupo basado en equipo, vaya a **configuración del equipo**. En el caso de directiva de grupo basado en usuario, vaya a **configuración de usuario**.
    - El directiva de grupo basado en equipo aplica el procesamiento del equipo principal a todos los equipos a los que se aplica el GPO, lo que afecta a todos los usuarios de los equipos.
    - Directiva de grupo basado en el usuario para aplicar el procesamiento del equipo principal a todas las cuentas de usuario a las que se aplica el GPO, afectando a todos los equipos en los que los usuarios inician sesión.
3. En **configuración del equipo** o **configuración de usuario**, vaya a **directivas**, a **plantillas administrativas**, a continuación, a **sistema**y a **redirección de carpetas**.
4. Haga clic con el botón derecho en **redirigir carpetas solo en equipos principales**y, a continuación, seleccione **Editar**.
5. Seleccione **habilitado**y haga clic en **Aceptar**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Paso 3: Opcionalmente, habilite los equipos primarios para perfiles de usuario móviles en directiva de grupo

El siguiente paso consiste en configurar opcionalmente directiva de grupo para habilitar la compatibilidad del equipo principal con los perfiles de usuario móviles. Esto permite que el perfil de un usuario se desplace en los equipos designados como equipos primarios del usuario, pero no en otros equipos.

Aquí se muestra cómo habilitar los equipos principales para los perfiles de usuario móviles:

1. Habilite la compatibilidad con el equipo principal para el redireccionamiento de carpetas, si no lo ha hecho ya.<br>De este modo, los documentos y otros archivos de usuario se mantienen fuera de los perfiles de usuario, lo que ayuda a los perfiles a permanecer reducidos y los tiempos de inicio de sesión permanecen con rapidez.
2. En administración de directiva de grupo, haga clic con el botón secundario en el GPO que creó (por ejemplo, **configuración de redirección de carpetas y perfiles de usuario móvil**) y, a continuación, seleccione **Editar**.
3. Vaya a **configuración del equipo**, **directivas**, **plantillas administrativas**, luego **sistema**y, a continuación, a **perfiles de usuario**.
4. Haga clic con el botón derecho en **Descargar perfiles móviles solo en equipos principales** y, a continuación, seleccione **Editar**.
5. Seleccione **habilitado**y haga clic en **Aceptar**.

## <a name="step-4-enable-the-gpo"></a>Paso 4: Habilitar el GPO

Una vez que haya completado la configuración de la redirección de carpetas y los perfiles de usuario móviles, habilite el GPO si todavía no lo ha hecho. Esto permite que se aplique a usuarios y equipos afectados.

Aquí se muestra cómo habilitar los GPO de redirección de carpetas y de perfiles de usuario móviles:

1. Administración de directiva de grupo abierto
2. Haga clic con el botón secundario en los GPO que ha creado y seleccione **vincular habilitado**. Debe aparecer una casilla junto al elemento de menú.

## <a name="step-5-test-primary-computer-function"></a>Paso 5: Probar la función de equipo principal

Para probar la compatibilidad con el equipo principal, inicie sesión en un equipo principal, confirme que se redirigen las carpetas y los perfiles y, a continuación, inicie sesión en un equipo no principal y confirme que no se redirigen las carpetas y los perfiles.

Aquí se muestra cómo probar la funcionalidad del equipo principal:

1. Inicie sesión en un equipo principal designado con una cuenta de usuario para la que haya habilitado el redireccionamiento de carpetas o los perfiles de usuario móviles.
2. Si la cuenta de usuario ha iniciado sesión en el equipo previamente, abra una sesión de Windows PowerShell o una ventana del símbolo del sistema como administrador, escriba el siguiente comando y, a continuación, cierre la sesión cuando se le solicite para asegurarse de que se aplica la configuración de la directiva de grupo más reciente al equipo cliente:

    ```PowerShell
    Gpupdate /force
    ```

3. Abra el Explorador de archivos.
1. Haga clic con el botón secundario en una carpeta redirigida (por ejemplo, la carpeta Mis documentos de la biblioteca documentos) y, a continuación, seleccione **propiedades**.
1. Seleccione la pestaña **Ubicación** y confirme que la ruta de acceso muestra el recurso compartido de archivos especificado en lugar de una ruta de acceso local. Para confirmar que el perfil de usuario es móvil, abra el **Panel de control**, seleccione **sistema y seguridad**, seleccione **sistema**, **Configuración avanzada del sistema**, seleccione **configuración** en la sección perfiles de usuario y, a continuación, busque  **Itinerancia** en la columna de **tipo** .
1. Inicie sesión con la misma cuenta de usuario en un equipo que no esté designado como el equipo principal del usuario.
1. Repita los pasos 2 a 5, en lugar de buscar las rutas de acceso locales y un tipo de perfil **local** .

> [!NOTE]
> Si las carpetas se redirigieron en un equipo antes de habilitar la compatibilidad con el equipo principal, las carpetas se redirigirán a menos que se configure el siguiente valor en la configuración de la Directiva de redirección de carpetas de cada carpeta: **Redirija la carpeta de nuevo a la ubicación local de userprofile cuando se quite la Directiva**. Del mismo modo, los perfiles que estaban en itinerancia previamente en un equipo determinado mostrarán la **itinerancia** en las columnas de **tipo** ; sin embargo, la columna **Estado** mostrará **local**.

## <a name="more-information"></a>Más información

- [Implementar el redireccionamiento de carpetas con Archivos sin conexión](deploy-folder-redirection.md)
- [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)
- [Información general sobre el redireccionamiento de carpetas, los Archivos sin conexión y los perfiles de usuario móviles](folder-redirection-rup-overview.md)
- [Profundizar un poco más en el equipo principal de Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)