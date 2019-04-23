---
title: Implementar equipos principales para redirección de carpetas y perfiles de usuario móviles
description: Cómo habilitar la compatibilidad con equipo principal y designar equipos principales para los usuarios con redirección de carpetas y perfiles de usuario móviles.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854016"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Implementar equipos principales para redirección de carpetas y perfiles de usuario móviles

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este tema describe cómo habilitar soporte de equipo principal y designar equipos principales para los usuarios. Si lo hace, le permite controlar qué equipos usan el redireccionamiento de carpetas y perfiles de usuario móviles.

>[!IMPORTANT]
>Al habilitar soporte de equipo principal para perfiles de usuario móviles, habilitar siempre el soporte de equipo principal para redirección de carpetas también. Esto mantiene los documentos y otros archivos de usuario fuera de los perfiles de usuario, lo que ayuda a los perfiles seguirán siendo pequeñas y tiempos de inicio de sesión rápido mantenerse.

## <a name="prerequisites"></a>Requisitos previos

## <a name="software-requirements"></a>Requisitos de software

Soporte de equipo principal tiene los siguientes requisitos:

- El esquema de Active Directory Domain Services (AD DS) debe actualizarse para incluir adiciones de esquema de Windows Server 2012 (al instalar un controlador de dominio de Windows Server 2012 actualizará automáticamente el esquema). Para obtener información acerca de cómo actualizar el esquema de AD DS, consulte [integración de Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) y [ejecutar Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

>[!TIP]
>Aunque el soporte de equipo principal requiere la redirección de carpetas y perfiles de usuario móviles, si va a implementar estas tecnologías por primera vez, es mejor establecer la compatibilidad con el equipo principal antes de habilitar los GPO que configuración la redirección de carpetas y Los perfiles de usuario móviles. Esto evita que se copien los datos de usuario a equipos no principales antes de habilitar el soporte de equipo principal. Para obtener información de configuración, consulte [implementar la redirección de carpetas](deploy-folder-redirection.md) y [implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Paso 1: Designar equipos principales para usuarios

El primer paso para implementar la compatibilidad con equipos principales es que designa los equipos principales para cada usuario. Para ello, use el centro de administración de Active Directory para obtener el nombre completo de los equipos pertinentes y, a continuación, establezca el **msDs-PrimaryComputer** atributo.

>[!TIP]
>Para usar Windows PowerShell para trabajar con equipos principales, consulte la entrada de blog [adentrarnos un poco más en el equipo principal de Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Aquí le mostramos cómo especificar los equipos principales para los usuarios:

1. Abra el Administrador del servidor en un equipo que tenga instaladas las Herramientas de administración de Active Directory.
2. En el **herramientas** menú, seleccione **centro de administración de Active Directory**. Aparece el Centro de administración de Active Directory.
3. Navegue hasta la **equipos** contenedor en el dominio correspondiente.
4. Haga clic en un equipo que desea designar como equipo principal y, a continuación, seleccione **propiedades**.
5. En el panel de navegación, seleccione **extensiones**.
6. Seleccione el **Editor de atributos** pestaña, desplácese a **distinguishedName**, seleccione **vista**, haga clic en el valor que aparece, seleccione **copia**, Seleccione **Aceptar**y, a continuación, seleccione **cancelar**.
7. Navegue hasta la **usuarios** contenedor en el dominio adecuado, haga clic en el usuario al que desea asignar el equipo y, a continuación, seleccione **propiedades**.
8. En el panel de navegación, seleccione **extensiones**.
9. Seleccione el **Editor de atributos** ficha, seleccione **msDs-PrimaryComputer** y, a continuación, seleccione **editar**. Aparece el cuadro de diálogo Editor de cadena multivalor.
10. Haga clic en el cuadro de texto, seleccione **pegar**, seleccione **agregar**, seleccione **Aceptar**y, a continuación, seleccione **Aceptar** nuevo.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Paso 2: Opcionalmente, habilitar equipos principales para redirección de carpetas en la directiva de grupo

El siguiente paso es configurar opcionalmente la directiva de grupo para habilitar la compatibilidad con equipo principal para redirección de carpetas. Esto permite que las carpetas de un usuario se redirijan en los equipos designados como equipos principales del usuario, pero no en otros equipos. Puede controlar los equipos principales para redirección de carpetas en el equipo por equipo o por el usuario.

Aquí le mostramos cómo habilitar equipos principales para redirección de carpetas:

1. En administración de directivas de grupo, haga clic en el GPO que creaste al realizar la configuración inicial de redirección de carpetas y perfiles de usuario móviles (por ejemplo, **configuración de redirección de carpeta** o **usuario móviles Configuración de perfiles**) y, a continuación, seleccione **editar**.
2. Para habilitar la compatibilidad de equipos principales mediante Directiva de grupo basada en equipo, vaya a **configuración del equipo**. Directiva de grupo basada en usuario, vaya a **configuración de usuario**.
    - Directiva de grupo basada en el equipo se aplica procesamiento principal del equipo a todos los equipos a los que el GPO se aplica, que afectan a todos los usuarios de los equipos.
    - Directiva de grupo basada en usuario para que se aplica el procesamiento del equipo principal a todas las cuentas de usuario al que se aplica el GPO, que afectan a todos los equipos a los que los usuarios iniciar sesión en.
3. En **configuración del equipo** o **configuración de usuario**, vaya a **directivas**, a continuación, **plantillas administrativas**, entonces  **Sistema**, a continuación, **redirección de carpetas**.
4. Haga clic en **redirigir carpetas en equipos principales solo**y, a continuación, seleccione **editar**.
5. Seleccione **habilitado**y, a continuación, seleccione **Aceptar**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Paso 3: Opcionalmente, habilitar equipos principales para perfiles de usuario móviles en la directiva de grupo

El siguiente paso es configurar opcionalmente la directiva de grupo para habilitar la compatibilidad con equipo principal para perfiles de usuario móviles. Esto permite que un perfil de usuario con posibilidad de movilidad en los equipos designados como equipos principales del usuario, pero no en otros equipos.

Aquí le mostramos cómo habilitar equipos principales para perfiles de usuario móviles:

1. Habilitar soporte de equipo principal para redirección de carpetas, si no lo ha hecho ya.
    * Esto mantiene los documentos y otros archivos de usuario fuera de los perfiles de usuario, lo que ayuda a los perfiles seguirán siendo pequeñas y tiempos de inicio de sesión rápido mantenerse.
2. En administración de directivas de grupo, haga clic en el GPO que creaste (por ejemplo, **redirección de carpetas y la configuración de perfiles de usuario móviles**) y, a continuación, seleccione **editar**.
3. Vaya a **configuración del equipo**, a continuación, **directivas**, a continuación, **plantillas administrativas**, a continuación, **sistema**y, a continuación, **Perfiles de usuario**.
4. Haga clic en **descargar perfiles móviles en equipos principales solo,** y, a continuación, seleccione **editar**.
5. Seleccione **habilitado**y, a continuación, seleccione **Aceptar**.

## <a name="step-4-enable-the-gpo"></a>Paso 4: Habilitar el GPO

Una vez haya completado la configuración de redirección de carpetas y perfiles de usuario móviles, habilitar el GPO, si aún no lo hizo. Esto permite que se aplique a equipos y usuarios afectados.

Aquí le mostramos cómo habilitar la redirección de carpetas o los GPO de perfiles de usuario móviles:

1. Administración de directivas de grupo abierto
2. Haga clic en el GPO que ha creado y, a continuación, seleccione **vínculo habilitado**. Debe aparecer una casilla de verificación situada junto al elemento de menú.

## <a name="step-5-test-primary-computer-function"></a>Paso 5: Probar la función de equipo principal

Para probar la compatibilidad de equipo principal, inicie sesión en un equipo principal, confirme que se redirigen las carpetas y perfiles, a continuación, inicie sesión un equipo no principal y confirme que no se redirigen las carpetas y perfiles.

Aquí le mostramos cómo probar la funcionalidad principal del equipo:

1. Inicie sesión en un equipo principal designado con una cuenta de usuario para el que ha habilitado la redirección de carpetas y perfiles de usuario móviles.
2. Si la cuenta de usuario ha iniciado la sesión en el equipo anteriormente, abra una sesión de Windows PowerShell o la ventana del símbolo del sistema como administrador, escriba el siguiente comando y, a continuación, cierre la sesión cuando se le solicite para asegurarse de que la última configuración de directiva de grupo se aplica a la equipo cliente:
    ```PowerShell
    Gpupdate /force
    ```
3. Abra el Explorador de archivos.
4. Haga clic en una carpeta redirigida (por ejemplo, la carpeta Mis documentos en la biblioteca de documentos) y, a continuación, seleccione **propiedades**.
5. Seleccione el **ubicación** pestaña y confirme que muestra la ruta de acceso del recurso compartido de archivos especificado en lugar de una ruta de acceso local. Para confirmar que el perfil de usuario está en itinerancia, abra **Panel de Control**, seleccione **sistema y seguridad**, seleccione **sistema**, seleccione **configuración avanzada del sistema** , seleccione **configuración** en los perfiles de usuario de la sección y, a continuación, busque **Roaming** en el **tipo** columna.
6. Inicie sesión con la misma cuenta de usuario en un equipo que no se designa como equipo principal del usuario.
7. Repita los pasos del 2 al 5, en su lugar, busca las rutas de acceso locales y un **Local** tipo de perfil.

>[!NOTE]
>Si se redirigieron las carpetas en un equipo antes de habilitar soporte de equipo principal, permanecerá las carpetas redirigidas a menos que se configuran las opciones siguientes en la configuración de directiva de redirección de carpeta de la carpeta: **Redirigir la carpeta a la ubicación del perfil de usuario local cuando se quita la directiva**. De forma similar, los perfiles que eran roaming previamente en un equipo determinado mostrará **Roaming** en el **tipo** columnas; sin embargo, el **estado** columna mostrará **Local**.

## <a name="more-information"></a>Más información

- [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)
- [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)
- [Información general sobre la redirección de carpetas, archivos sin conexión y perfiles de usuario móviles](folder-redirection-rup-overview.md)
- [Al profundizar un poco más en el equipo principal de Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)