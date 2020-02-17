---
title: Habilitación de los movimientos optimizados de carpetas redirigidas
description: Cómo realizar un movimiento optimizado de carpetas redirigidas a un nuevo recurso compartido de archivos.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: edf714bc0d6b39dbe7c5e800e953d7820fe9abc5
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75352609"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Habilitación de los movimientos optimizados de carpetas redirigidas

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual)

En este tema se describe cómo realizar un movimiento optimizado de carpetas redirigidas (redirección de carpetas) a un nuevo recurso compartido de archivos. Si habilitas esta configuración de directiva, cuando un administrador mueva el recurso compartido de archivos que hospeda las carpetas redirigidas y actualice la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo, el contenido almacenado en caché simplemente cambiará de nombre en la memoria caché local de los archivos sin conexión sin que se produzcan retrasos ni posibles pérdidas de datos para el usuario.

Anteriormente, los administradores podían cambiar la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo y permitir que los equipos cliente copiaran los archivos en el siguiente inicio de sesión del usuario afectado, lo que provocaba un retraso en el inicio de sesión. Los administradores también podían migrar el recurso compartido de archivos y actualizar la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo. Sin embargo, los cambios realizados localmente en los equipos cliente entre el inicio de la migración y la primera sincronización después del movimiento se perdían.

## <a name="prerequisites"></a>Requisitos previos

El movimiento optimizado tiene los siguientes requisitos:

- La redirección de carpetas debe estar configurada. Para obtener más información, consulta [Implementación de la redirección de carpetas con Archivos sin conexión](deploy-folder-redirection.md).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server (canal semianual).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Paso 1: habilita el movimiento optimizado en la directiva de grupo

Para optimizar la reubicación de los datos de la redirección de carpetas, usa una directiva de grupo para habilitar la opción de configuración **Habilitar el movimiento optimizado de los contenidos en caché de archivos sin conexión en el cambio de ruta de servidor de Redirección de carpetas** para el objeto directiva de grupo correspondiente (GPO). Al configurar esta directiva en **Deshabilitada** o **Sin configurar**, el cliente copia todo el contenido de Redirección de carpetas en la nueva ubicación y, a continuación, elimina el contenido de la ubicación anterior si se cambia la ruta de acceso del servidor.

A continuación se indica cómo habilitar el movimiento optimizado de carpetas redirigidas:

1. En la administración de directivas de grupo, haz clic con el botón secundario en el GPO que creaste para la configuración de Redirección de carpetas [por ejemplo, **Folder Redirection and Roaming User Profiles Settings** (Configuración de Redirección de carpetas y perfiles de usuario móvil)] y, a continuación, selecciona **Editar**.
2. En **Configuración de usuario**, ve a **Directivas**, **Plantillas administrativas**, **Sistema** y **Redirección de carpetas**.
3. Haz clic con el botón derecho en **Habilitar el movimiento optimizado de los contenidos en caché de archivos sin conexión en el cambio de ruta de servidor de Redirección de carpetas** y, a continuación, selecciona **Editar**.
4. Selecciona **Habilitado** y después selecciona **Aceptar**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Paso 2: reubicación del recurso compartido de archivos para carpetas redirigidas

Al mover el recurso compartido de archivos que contiene las carpetas redirigidas de los usuarios, es importante tomar las precauciones necesarias para asegurarte de que las carpetas se reubican correctamente.

>[!IMPORTANT]
>Si los archivos de los usuarios están en uso o si el estado del archivo completo no se conserva durante el traslado, es posible que los usuarios experimenten bajo rendimiento a medida que los archivos se copian a través de la red, conflictos de sincronización generados por Archivos sin conexión, o incluso pérdida de datos.

1. Notifica a los usuarios con antelación que el servidor que hospeda sus carpetas redirigidas cambiará y recomienda que realicen las siguientes acciones:

      - Sincronizar el contenido de la memoria caché de Archivos sin conexión y resolver los conflictos.
      - Abrir un símbolo del sistema con privilegios elevados, escribir **GpUpdate /Target:User /Force** y, a continuación, cerrar la sesión y volver a iniciarla para asegurarse de que la configuración de la directiva de grupo más reciente se aplicó al equipo cliente.

        >[!NOTE]
        >De forma predeterminada, los equipos cliente actualizan las directivas de grupo cada 90 minutos, por lo que si se espera el tiempo suficiente para que los equipos cliente reciban la directiva actualizada, no será necesario pedir a los usuarios que utilicen **GpUpdate**.
2. Quita el recurso compartido de archivos del servidor para asegurarte de que no se esté usando ningún archivo en el recurso compartido de archivos. Para ello, en el Administrador del servidor, en la página **Recursos compartidos** de Servicios de archivos y almacenamiento, haz clic con el botón derecho en el recurso compartido de archivos adecuado y selecciona **Quitar**.

    Los usuarios trabajarán sin conexión mediante Archivos sin conexión hasta que se complete el movimiento y reciban la configuración de Redirección de carpetas actualizada de la directiva de grupo.

3. Con una cuenta con privilegios de copia de seguridad, mueve el contenido del recurso compartido de archivos a la nueva ubicación mediante un método que conserva las marcas de tiempo de los archivos, como una utilidad para copias de seguridad y restauración. Para usar el comando **Robocopy**, abre un símbolo del sistema con privilegios elevados y, a continuación, escribe el siguiente comando, donde ```<Source>``` es la ubicación actual del recurso compartido de archivos y ```<Destination>``` es la nueva ubicación:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >El comando **Xcopy** no conserva todo el estado del archivo.
4. Edita la configuración de la directiva de Redirección de carpetas y actualiza la ubicación de la carpeta de destino para cada carpeta redirigida que quieras reubicar. Para obtener más información, consulta el Paso 4 de [Implementación de la redirección de carpetas con Archivos sin conexión](deploy-folder-redirection.md).
5. Notifica a los usuarios que el servidor que hospeda sus carpetas redirigidas ha cambiado, y que deben usar el comando **GpUpdate /Target:User /Force**, cerrar la sesión y volver a iniciarla para obtener la configuración actualizada y reanudar la sincronización de archivos correcta.

    Los usuarios deben iniciar sesión en todos los equipos al menos una vez para asegurarse de que los datos se reubican correctamente en la memoria caché de cada instancia de Archivos sin conexión.

## <a name="more-information"></a>Información adicional

* [Implementación de la redirección de carpetas con Archivos sin conexión](deploy-folder-redirection.md)
* [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)
* [Introducción a la redirección de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)