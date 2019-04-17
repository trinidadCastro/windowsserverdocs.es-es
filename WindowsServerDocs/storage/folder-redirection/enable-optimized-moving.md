---
title: Habilitar optimizado se mueve de las carpetas redirigidas
description: Cómo realizar un movimiento optimizado de las carpetas redirigidas a un nuevo recurso compartido de archivos.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 98fd5d50645ad454204dcf9dabf58e97c246ab1a
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831385"
---
# Habilitar optimizado se mueve de las carpetas redirigidas

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

En este tema se describe cómo realizar un movimiento optimizado de carpetas redirigidas (redirección de carpetas) a un nuevo recurso compartido de archivos. Si habilitas esta configuración de directiva, cuando un administrador mueve el recurso compartido de archivos que aloja carpetas redirigidas y actualiza la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo, el contenido almacenado en caché simplemente se cambia el nombre de la caché de archivos sin conexión local sin retrasos o posible pérdida de datos para el usuario.

Anteriormente, los administradores de TI podrían cambiar la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo y dejar que los equipos cliente copiar los archivos en el siguiente inicio de sesión del usuario afectado, lo que provoca un inicio de sesión retrasada. Como alternativa, los administradores de TI podrían mover el recurso compartido de archivos y actualizar la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo. Sin embargo, los cambios realizados localmente en los equipos cliente entre el inicio de la operación de movimiento y la primera sincronización después del movimiento sería perdidos.

## Requisitos previos

Movimiento optimizado tiene los siguientes requisitos:

- Redirección de carpetas debe ser el programa de instalación. Para obtener más información, consulta [Implementar redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

## Paso 1: Habilitar optimizado transición en la directiva de grupo

Para optimizar el traslado de los datos de redirección de carpetas, usa la directiva de grupo para habilitar a la configuración de directiva de **Habilitar había optimizado el movimiento de contenido en la memoria caché de archivos sin conexión en el cambio de ruta de acceso de servidor de redirección de carpetas** para el objeto de directiva de grupo (GPO) apropiado. Esta configuración de directiva en **deshabilitado** o **No configurado** , hace que el cliente puede copiar todo el contenido de redirección en la nueva ubicación y, a continuación, elimina el contenido de la ubicación anterior si cambia la ruta de acceso de servidor.

Aquí te mostramos cómo habilitar mover optimizada de carpetas redirigidas:

1. En administración de directivas de grupo, haz clic en el GPO que creaste para la configuración de redirección de carpetas (por ejemplo, **redirección de carpetas y configuración de perfiles de usuario de itinerancia**) y, a continuación, selecciona **Editar**.
2. En **Configuración de usuario**, ve a **las directivas**, a continuación, **Plantillas administrativas**, **sistema**a continuación, **Redirección de carpetas**.
3. Haz clic en **que Enable optimizado el movimiento de contenido en la memoria caché de archivos sin conexión en el cambio de ruta de acceso de servidor de redirección de carpetas**y, a continuación, seleccione **Editar**.
4. Seleccione **habilitada**y, a continuación, selecciona **Aceptar**.

## Paso 2: Cambiar la ubicación del recurso compartido de archivos para las carpetas redirigidas

Al mover el recurso compartido de archivo que contiene los usuarios las carpetas redirigidas, es importar tomar precauciones para garantizar que las carpetas se reubican correctamente.

>[!IMPORTANT]
>Si los archivos de los usuarios están en usar o si no se conserva el estado de archivo completo al migrar, los usuarios pueden experimentar un rendimiento deficiente tal como se copian los archivos a través de la red, conflictos de sincronización generados por archivos sin conexión, o incluso la pérdida de datos.

1. Notificar a los usuarios de antemano que el servidor que aloja sus carpetas redirigidas cambiará y recomendamos que realizan las siguientes acciones:

      - Sincronizar el contenido de su caché de archivos sin conexión y resolver los conflictos.
      - Abre un símbolo del sistema con privilegios elevados, escribe **/force/Force GpUpdate /Target:User**y luego cierra la sesión y volver a iniciarla garantizar que se aplica la configuración de directiva de grupo más reciente en el equipo cliente

        >[!NOTE]
        >De manera predeterminada, los equipos cliente actualizan la directiva de grupo cada 90 minutos, por lo que si permites que el tiempo suficiente para el cliente de los equipos a recibir la directiva actualizada, no es necesario pedir a los usuarios usar **GpUpdate**.
2. Quitar el recurso compartido de archivos desde el servidor para asegurarse de que no hay archivos en el recurso compartido de archivos están en uso. Para hacer esto en el Administrador de servidores, en la página de **recursos compartidos** de archivos y los servicios de almacenamiento, haz clic en el recurso compartido de archivo apropiado y luego selecciona **Quitar**.

    Los usuarios lo funcione sin conexión mediante archivos sin conexión hasta que finalice el movimiento y reciben la configuración de redirección de carpetas actualizada de la directiva de grupo.

3. Con una cuenta con privilegios de copia de seguridad, mover el contenido del recurso compartido de archivo a la nueva ubicación mediante un método que conserva marcas de tiempo del archivo, por ejemplo, una copia de seguridad y restauración de utilidad. Para usar los comandos de **Robocopy** , abre un símbolo del sistema con privilegios elevados y, a continuación, escribe el comando siguiente, donde ```<Source>``` es la ubicación actual del recurso compartido de archivos, y ```<Destination>``` es la nueva ubicación:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >El comando **Xcopy** no conserva todo el estado de archivo.
4. Editar la configuración de directiva de redirección de carpetas, actualizar la ubicación de la carpeta de destino para todas las carpetas redirigidas que quieres cambiar la ubicación. Para obtener más información, consulte el paso 4 de [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md).
5. Notificar a los usuarios que ha cambiado el servidor que aloja sus carpetas redirigidas y que deben usar el **comando/Force GpUpdate /Target:User** y luego cierra la sesión y vuelva iniciar sesión en para obtener la configuración actualizada y reanudar la sincronización de archivo apropiado.

    Los usuarios deben iniciar sesión todas las máquinas al menos una vez garantizar que los datos obtienen reubican correctamente en cada caché de archivos sin conexión.

## Más información

* [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)
* [Implementar perfiles de usuario móvil](deploy-roaming-user-profiles.md)
* [Información general de la redirección de carpetas, archivos sin conexión y perfiles de usuario móvil](folder-redirection-rup-overview.md)