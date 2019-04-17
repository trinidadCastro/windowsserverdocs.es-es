---
title: Implementar equipos principales de redirección de carpetas y perfiles de usuario móvil
description: Cómo habilitar la compatibilidad de equipos principal y designar equipos principales para los usuarios con redirección de carpetas y perfiles de usuario móvil.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831405"
---
# Implementar equipos principales de redirección de carpetas y perfiles de usuario móvil

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

En este tema se describe cómo habilitar la compatibilidad de equipos principal y designar equipos principales para los usuarios. Si lo haces, permite controlar qué equipos utilizar redirección de carpetas y perfiles de usuario móvil.

>[!IMPORTANT]
>Al habilitar la compatibilidad de equipos principal de perfiles de usuario móvil, siempre habilitar la compatibilidad de equipos principal redirección de carpetas también. Esto mantiene documentos y otros archivos de usuario fuera de los perfiles de usuario, lo que ayuda a perfiles siendo pequeños y tiempos de inicio de sesión más rápido mantenerse.

## Requisitos previos

## Requisitos de software

Compatibilidad de equipos principal tiene los siguientes requisitos:

- El esquema de Active Directory Domain Services (AD DS) debe actualizarse para incluir las adiciones de esquema de Windows Server 2012 (instalar un controlador de dominio de Windows Server 2012 actualizará automáticamente el esquema). Para obtener información sobre cómo actualizar el esquema de AD DS, consulte [la integración de Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) y [Ejecutar Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

>[!TIP]
>Aunque la compatibilidad con el equipo principal requiere la redirección de carpetas y perfiles de usuario móvil, si vas a implementar estas tecnologías por primera vez, es mejor establecer la compatibilidad con el equipo principal antes de habilitar el GPO que configurar la redirección de carpetas y Los perfiles de usuario móviles. Esto impide que los datos de usuario se copien en equipos que no principal antes de habilita la compatibilidad de equipos principal. Para obtener información de configuración, consulta [Implementar redirección de carpetas](deploy-folder-redirection.md) e [Implementar perfiles de usuario móvil](deploy-roaming-user-profiles.md).

## Paso 1: Designar equipos principales para los usuarios

El primer paso para implementar la compatibilidad de equipos principal es designar los equipos principales para cada usuario. Para ello, usa el centro de administración de Active Directory para obtener el nombre completo de los equipos relevantes y, a continuación, Establece el atributo **msDs-PrimaryComputer** .

>[!TIP]
>Para usar Windows PowerShell para trabajar con los equipos principales, consulta la entrada de blog [profundizar un poco en el equipo principal de Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Aquí te mostramos cómo especificar los equipos principales para los usuarios:

1. Abre el Administrador del servidor en un equipo que tenga instaladas las Herramientas de administración de Active Directory.
2. En el menú **Herramientas** , selecciona **El centro de administración de Active Directory**. Aparecerá el Centro de administración de Active Directory.
3. Navega hasta el contenedor de **equipos** en el dominio adecuado.
4. Haz clic en un equipo que desea designar como un equipo principal y, a continuación, selecciona **Propiedades**.
5. En el panel de navegación, selecciona **las extensiones**.
6. Selecciona la pestaña del **Editor de atributos** , desplázate hasta **distinguishedName**, selecciona **vista**, haz clic en el valor de la lista, seleccione **Copiar**, haga **clic en Aceptar**y, a continuación, selecciona **Cancelar**.
7. Navega hasta el contenedor **usuarios** en el dominio adecuado, haz clic en el usuario al que quieres asignar el equipo y, a continuación, selecciona **Propiedades**.
8. En el panel de navegación, selecciona **las extensiones**.
9. Selecciona la pestaña del **Editor de atributos** , **msDs-PrimaryComputer** y, a continuación, selecciona **Editar**. Aparecerá el cuadro de diálogo Editor de cadena multivalor.
10. Haz clic en el cuadro de texto, seleccionar **Pegar**, selecciona **Agregar**, haga **clic en Aceptar**y, a continuación, seleccione **Aceptar** de nuevo.

## Paso 2: La opción de habilitar equipos principales para el redireccionamiento de carpetas en la directiva de grupo

El siguiente paso es configurar, opcionalmente, la directiva de grupo para habilitar la compatibilidad de equipos principal redirección de carpetas. Si lo haces, permite que carpetas de un usuario que se redireccione en equipos designados como equipos principal del usuario, pero no en otros equipos. Puedes controlar los equipos principales redirección de carpetas en forma individual para cada equipo o por usuario.

Aquí te mostramos cómo permitir que los equipos principales redirección de carpetas:

1. En administración de directivas de grupo, haz clic en el GPO que creaste al realizar la configuración inicial de redirección de carpetas y perfiles de usuario móvil (por ejemplo, **Configuración de redirección de carpetas** o **Configuración de perfiles de usuario de itinerancia**) y, después, selecciona **Editar**.
2. Para habilitar la compatibilidad de equipos principal mediante la directiva de grupo basada en el equipo, ve a **Configuración del equipo**. Basado en usuario la directiva de grupo, ve a **Configuración del usuario**.
    - Basada en el equipo de la directiva de grupo se aplica el procesamiento de equipo principal a todos los equipos a los que se aplica el GPO, que afectan a todos los usuarios de los equipos.
    - Basado en usuario de directiva de grupo que aplica el procesamiento de equipo principal para todas las cuentas de usuario al que se aplica el GPO, que afectan a todos los equipos a los que los usuarios inicien sesión.
3. En **Configuración del equipo** o **Configuración de usuario**, ve a **las directivas**, a continuación, **Plantillas administrativas**, **sistema**a continuación, **Redirección de carpetas**.
4. Haz clic en **redirigir carpetas principales sólo en equipos**y, a continuación, seleccione **Editar**.
5. Seleccione **habilitada**y, a continuación, selecciona **Aceptar**.

## Paso 3: La opción de habilitar equipos principales de perfiles de usuario móvil en la directiva de grupo

El siguiente paso es configurar, opcionalmente, la directiva de grupo para habilitar la compatibilidad de equipos principal de perfiles de usuario móvil. Si lo haces, permite un perfil de usuario para que se mueva en equipos designados como equipos principal del usuario, pero no en otros equipos.

Aquí te mostramos cómo permitir que los equipos principales de perfiles de usuario móvil:

1. Habilitar compatibilidad de equipos principal para el redireccionamiento de carpetas, si no lo has hecho ya.
    * Esto mantiene documentos y otros archivos de usuario fuera de los perfiles de usuario, lo que ayuda a perfiles siendo pequeños y tiempos de inicio de sesión más rápido mantenerse.
2. En administración de directivas de grupo, haz clic en el GPO que creaste (por ejemplo, **redirección de carpetas y configuración de perfiles de usuario de itinerancia**) y, a continuación, selecciona **Editar**.
3. Ve a **Configuración del equipo**, **directivas**, a continuación, **plantillas administrativas**, a continuación, **sistema**y, a continuación, **perfiles de usuario**.
4. Haz clic en **descargar perfiles móviles principales sólo en equipos** y, a continuación, selecciona **Editar**.
5. Seleccione **habilitada**y, a continuación, selecciona **Aceptar**.

## Paso 4: Habilitar el GPO

Cuando haya completado la configuración de redirección de carpetas y perfiles de usuario móvil, habilita el GPO si aún no tienes. Si lo haces, permite que se aplique a los usuarios afectados y equipos.

Aquí te mostramos cómo habilitar la redirección de carpetas o los GPO de perfiles de usuario de itinerancia:

1. Administración de directivas de grupo de Open
2. Haz clic en el GPO que creaste y, a continuación, selecciona el **Vínculo habilitado**. Una casilla debe aparecer junto al elemento de menú.

## Paso 5: Probar la función principal del equipo

Para probar la compatibilidad de equipos principal, inicie sesión en un equipo principal, confirma que se redirigen las carpetas y perfiles, a continuación, inicia sesión en un equipo no principales y confirma que no se redirigen las carpetas y perfiles.

Aquí te mostramos cómo probar la funcionalidad principal del equipo:

1. Inicia sesión en un equipo principal designado con una cuenta de usuario para la que has habilitado la redirección de carpetas y perfiles de usuario móvil.
2. Si la cuenta de usuario ha iniciado sesión en el equipo anteriormente, abre una sesión de Windows PowerShell o una ventana de símbolo del sistema como administrador, escribe el siguiente comando y, a continuación, aprobar cuando aparezca el mensaje para garantizar que se aplica la configuración de directiva de grupo más reciente para el equipo cliente:
    ```PowerShell
    Gpupdate /force
    ```
3. Abre el Explorador de archivos.
4. Haz clic en una carpeta redirigida (por ejemplo, la carpeta Mis documentos en la biblioteca de documentos) y, a continuación, selecciona **Propiedades**.
5. Selecciona la pestaña de la **ubicación** y confirma que la ruta de acceso muestre el recurso compartido de archivos que especificaste en lugar de una ruta de acceso local. Para confirmar que el perfil de usuario esté en itinerancia, abre el **Panel de Control**, selecciona **sistema y seguridad**, selecciona el **sistema**, seleccionar la **Configuración avanzada de sistema**, selecciona la **configuración** en la sección de perfiles de usuario y, a continuación, busca ** Itinerancia** en la columna **tipo** .
6. Inicia sesión con la misma cuenta de usuario a un equipo que no se haya designado como el equipo del usuario principal.
7. Repite los pasos 2 a 5, en su lugar para buscar las rutas de acceso locales y un tipo de perfil **Local** .

>[!NOTE]
>Si se redirigen carpetas en un equipo antes de habilitar la compatibilidad de equipos principal, las carpetas permanecerá redirigidas a menos que la siguiente configuración está configurada en configuración de directiva de redirección de carpetas de cada carpeta: **devolver la carpeta local ubicación de perfil de usuario cuando se quita la directiva**. Del mismo modo, los perfiles que anteriormente estaban itinerancia en un equipo concreto mostrarán **Roaming** en las columnas de **tipo** . Sin embargo, la columna **estado** mostrará **Local**.

## Más información

- [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)
- [Implementar perfiles de usuario móvil](deploy-roaming-user-profiles.md)
- [Información general de la redirección de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)
- [Un poco profundizar en el equipo principal de Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)