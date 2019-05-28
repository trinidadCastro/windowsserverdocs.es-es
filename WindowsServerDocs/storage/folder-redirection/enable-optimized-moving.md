---
title: Habilitar movimientos optimizadas de las carpetas redirigidas
description: Cómo realizar un movimiento rápida optimizada de las carpetas redirigidas para un nuevo recurso compartido de archivos.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7bdf30a4f721568add4e7902245da2a803b72db1
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475870"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Habilitar movimientos optimizadas de las carpetas redirigidas

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual)

Este tema describe cómo realizar un movimiento rápida optimizada de las carpetas redirigidas (redirección de carpetas) a un nuevo recurso compartido de archivos. Si habilita esta configuración de directiva, cuando un administrador mueve el recurso compartido de archivos hospeda las carpetas redirigidas y actualiza la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo, el contenido almacenado en caché simplemente cambia el nombre de la caché de archivos sin conexión local sin retrasos o posible pérdida de datos para el usuario.

Anteriormente, los administradores pueden cambiar la ruta de acceso de destino de las carpetas redirigidas en directiva de grupo y permita que los equipos cliente copie los archivos en el próximo inicio de sesión del usuario afectado, provocando un retraso de inicio de sesión. Como alternativa, los administradores se pudieran mover el recurso compartido de archivos y actualizar la ruta de acceso de destino de las carpetas redirigidas en la directiva de grupo. Sin embargo, se perderían los cambios realizados localmente en los equipos cliente entre el inicio de la operación de movimiento y la primera sincronización tras el movimiento.

## <a name="prerequisites"></a>Requisitos previos

Movimiento rápida optimizada tiene los siguientes requisitos:

- Redirección de carpetas debe ser el programa de instalación. Para obtener más información, consulte [implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server (canal semianual).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Paso 1: Habilitar movimiento rápida optimizada de directiva de grupo

Para optimizar la reubicación de datos de la redirección de carpetas, utilice la directiva de grupo para habilitar el **Enable optimizado para mover de contenido en caché de archivos sin conexión en el cambio de ruta de acceso de servidor de redirección de carpetas** configuración de directiva de la directiva de grupo adecuada Objeto (GPO). Esta configuración de directiva para **deshabilitado** o **no configurado** hace que el cliente copiar todo el contenido de la redirección de carpetas a la nueva ubicación y, a continuación, elimine el contenido de la ubicación anterior si la cambios de ruta de acceso del servidor.

Aquí le mostramos cómo permiten el traslado optimizada de las carpetas redirigidas:

1. En administración de directivas de grupo, haga clic en el GPO que creó para la configuración de redirección de carpetas (por ejemplo, **redirección de carpetas y la configuración de perfiles de usuario móviles**) y, a continuación, seleccione **editar**.
2. En **configuración de usuario**, vaya a **directivas**, a continuación, **plantillas administrativas**, a continuación, **sistema**, a continuación,  **Redirección de carpetas**.
3. Haga clic en **Enable optimizado para mover de contenido en caché de archivos sin conexión en el cambio de ruta de acceso de servidor de redirección de carpetas**y, a continuación, seleccione **editar**.
4. Seleccione **habilitado**y, a continuación, seleccione **Aceptar**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Paso 2: Cambiar la ubicación de recurso compartido de archivos para las carpetas redirigidas

Al mover el recurso compartido de archivos que contiene los usuarios las carpetas redirigidas, es importante tomar precauciones para asegurarse de que las carpetas se reubican correctamente.

>[!IMPORTANT]
>Si los archivos del usuario se encuentran en usar o si no se conserva el estado de archivo completo en el traslado, los usuarios podrían experimentar un rendimiento deficiente, como los archivos se copian a través de la red, conflictos de sincronización generados por los archivos sin conexión, o incluso la pérdida de datos.

1. Notificar a los usuarios de antemano que el servidor que hospeda sus carpetas redirigidas se cambia y se recomienda realicen las siguientes acciones:

      - Sincronizar el contenido de la memoria caché de archivos sin conexión y resuelva los conflictos.
      - Abra un símbolo del sistema con privilegios elevados, escriba **GpUpdate /Target:User /Force**y, a continuación, cierre la sesión e iníciela de nuevo para asegurarse de que la última configuración de directiva de grupo se aplica al equipo cliente

        >[!NOTE]
        >De forma predeterminada, los equipos cliente actualizarán directiva de grupo cada 90 minutos, por lo que si dejar tiempo suficiente para cliente de equipos recibir la directiva actualizada, no es necesario pedir a los usuarios para usar **GpUpdate**.
2. Quitar el recurso compartido de archivos del servidor para asegurarse de que no hay archivos en el recurso compartido de archivos están en uso. Para hacerlo en el administrador del servidor, en el **recursos compartidos** página del archivo y servicios de almacenamiento, haga clic en el recurso compartido de archivo adecuado y luego seleccione **quitar**.

    Los usuarios funcionará sin conexión mediante archivos sin conexión hasta que el movimiento es completando y que reciben la configuración actualizada de la redirección de carpetas de directiva de grupo.

3. Con una cuenta con privilegios de copia de seguridad, mover el contenido del recurso compartido de archivos a la nueva ubicación mediante un método que conserva la marca de tiempo, por ejemplo, una copia de seguridad y restauración de la utilidad. Para usar el **Robocopy** de comandos, abra un símbolo del sistema con privilegios elevados y, a continuación, escriba el comando siguiente, donde ```<Source>``` es la ubicación actual del recurso compartido de archivos, y ```<Destination>``` es la nueva ubicación:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >El **Xcopy** comando no conserva todo el estado de archivo.
4. Editar la configuración de directiva de redirección de carpetas, la ubicación de carpeta de destino para cada carpeta redirigida a la que desea cambiar la ubicación de la actualización. Para obtener más información, consulte el paso 4 de [implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md).
5. Notificar a los usuarios que ha cambiado el servidor que hospeda sus carpetas redirigidas y que deben usar el **GpUpdate /Target:User /Force** comando y, a continuación, cierre la sesión e iníciela de nuevo obtener la configuración actualizada y reanudar adecuada de los archivos sincronización.

    Los usuarios deben iniciar sesión en todas las máquinas al menos una vez para asegurarse de que los datos obtiene reubica correctamente en cada caché de archivos sin conexión.

## <a name="more-information"></a>Más información

* [Implementar la redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md)
* [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)
* [Información general sobre la redirección de carpetas, archivos sin conexión y perfiles de usuario móviles](folder-redirection-rup-overview.md)