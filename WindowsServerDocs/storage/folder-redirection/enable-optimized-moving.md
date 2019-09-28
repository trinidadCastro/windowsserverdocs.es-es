---
title: Habilitar movimientos optimizados de carpetas redirigidas
description: Cómo realizar un movimiento optimizado de carpetas redirigidas a un nuevo recurso compartido de archivos.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6c54fee98247b1ce0aa3ef3a2502cf18f314e763
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394365"
---
# <a name="enable-optimized-moves-of-redirected-folders"></a>Habilitar movimientos optimizados de carpetas redirigidas

>Se aplica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semianual)

En este tema se describe cómo realizar un movimiento optimizado de carpetas redirigidas (redirección de carpetas) a un nuevo recurso compartido de archivos. Si habilita esta configuración de Directiva, cuando un administrador mueva el recurso compartido de archivos que hospeda carpetas redirigidas y actualice la ruta de acceso de destino de las carpetas redirigidas en directiva de grupo, el contenido almacenado en caché simplemente cambiará de nombre en la memoria caché de Archivos sin conexión local sin que se produzcan retrasos. posible pérdida de datos para el usuario.

Anteriormente, los administradores podían cambiar la ruta de acceso de destino de las carpetas redirigidas en directiva de grupo y permitir que los equipos cliente copien los archivos en el siguiente inicio de sesión del usuario afectado, lo que provocaría un inicio de sesión retrasado. Como alternativa, los administradores pueden trasladar el recurso compartido de archivos y actualizar la ruta de acceso de destino de las carpetas redirigidas en directiva de grupo. Sin embargo, los cambios realizados localmente en los equipos cliente entre el inicio del movimiento y la primera sincronización después del movimiento se perderían.

## <a name="prerequisites"></a>Requisitos previos

El movimiento optimizado tiene los siguientes requisitos:

- La redirección de carpetas debe estar configurada. Para obtener más información, consulte implementación de la [redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md).
- Los equipos cliente deben ejecutar Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server (canal semianual).

## <a name="step-1-enable-optimized-move-in-group-policy"></a>Paso 1: Habilitar el movimiento optimizado en directiva de grupo

Para optimizar la reubicación de los datos de redireccionamiento de carpetas, use directiva de grupo para habilitar la configuración de directiva **Habilitar el movimiento optimizado de contenido en la caché de archivos sin conexión en la ruta de acceso del servidor de redireccionamiento de carpetas** para el objeto de directiva de grupo (GPO) adecuado. La configuración de esta directiva como **deshabilitada** o **no configurada** hace que el cliente Copie todo el contenido de redirección de carpetas en la nueva ubicación y, a continuación, elimine el contenido de la ubicación anterior si cambia la ruta de acceso del servidor.

Aquí se muestra cómo habilitar el movimiento optimizado de carpetas redirigidas:

1. En administración de directiva de grupo, haga clic con el botón secundario en el GPO que creó para la configuración de redirección de carpetas (por ejemplo, configuración de redirección de **carpetas y perfiles de usuario móvil**) y, a continuación, seleccione **Editar**.
2. En **configuración de usuario**, vaya a **directivas**, **plantillas administrativas**, **sistema**y **redirección de carpetas**.
3. Haga clic con el botón derecho en **Habilitar el movimiento de contenido optimizado en la caché de archivos sin conexión en la redirección de carpetas cambiar ruta de acceso del servidor**y seleccione **Editar**.
4. Seleccione **habilitado**y haga clic en **Aceptar**.

## <a name="step-2-relocate-the-file-share-for-redirected-folders"></a>Paso 2: Reubicar el recurso compartido de archivos para carpetas redirigidas

Al mover el recurso compartido de archivos que contiene las carpetas redirigidas de los usuarios, se importa para tomar las precauciones necesarias para asegurarse de que las carpetas se reubican correctamente.

>[!IMPORTANT]
>Si los archivos de los usuarios están en uso o si el estado del archivo completo no se conserva en el traslado, es posible que los usuarios experimenten un bajo rendimiento a medida que los archivos se copian a través de la red, los conflictos de sincronización generados por Archivos sin conexión o incluso la pérdida de datos.

1. Notifique a los usuarios con antelación que el servidor que hospeda sus carpetas redirigidas cambiará y le recomendará que realice las siguientes acciones:

      - Sincronizar el contenido de la memoria caché de Archivos sin conexión y resolver los conflictos.
      - Abra un símbolo del sistema con privilegios elevados, escriba **gpupdate/target: usuario/Force**y, a continuación, cierre la sesión y vuelva a iniciarla para asegurarse de que se aplica la configuración de directiva de grupo más reciente al equipo cliente.

        >[!NOTE]
        >De forma predeterminada, los equipos cliente se actualizan directiva de grupo cada 90 minutos, por lo que si se permite el tiempo suficiente para que los equipos cliente reciban la directiva actualizada, no es necesario pedir a los usuarios que utilicen **gpupdate**.
2. Quite el recurso compartido de archivos del servidor para asegurarse de que no se esté usando ningún archivo en el recurso compartido de archivos. Para ello, en Administrador del servidor, en la página **recursos compartidos** de servicios de archivos y almacenamiento, haga clic con el botón derecho en el recurso compartido de archivos adecuado y seleccione **quitar**.

    Los usuarios trabajarán sin conexión mediante Archivos sin conexión hasta que se complete el traslado y reciban la configuración de redirección de carpetas actualizada de directiva de grupo.

3. Mediante una cuenta con privilegios de copia de seguridad, mueva el contenido del recurso compartido de archivos a la nueva ubicación mediante un método que conserva las marcas de tiempo de los archivos, como una utilidad de copia de seguridad y restauración. Para usar el comando **Robocopy** , abra un símbolo del sistema con privilegios elevados y, a continuación, ```<Source>``` escriba el siguiente comando, donde es la ubicación actual ```<Destination>``` del recurso compartido de archivos y es la nueva ubicación:

    ```PowerShell
    Robocopy /B <Source> <Destination> /Copyall /MIR /EFSRAW
    ```

    >[!NOTE]
    >El comando **xcopy** no conserva todo el estado del archivo.
4. Edite la configuración de la Directiva de redireccionamiento de carpetas y actualice la ubicación de la carpeta de destino para cada carpeta redirigida que desee reubicar. Para obtener más información, consulte el paso 4 de implementación de la [redirección de carpetas con archivos sin conexión](deploy-folder-redirection.md).
5. Notifique a los usuarios que el servidor que hospeda sus carpetas redirigidas ha cambiado y que deben usar el comando **gpupdate/target: usuario/Force** y, a continuación, cierre la sesión y vuelva a iniciarla para obtener la configuración actualizada y reanudar la sincronización de archivos correcta.

    Los usuarios deben iniciar sesión en todos los equipos al menos una vez para asegurarse de que los datos se reubican correctamente en cada Archivos sin conexión caché.

## <a name="more-information"></a>Más información

* [Implementar el redireccionamiento de carpetas con Archivos sin conexión](deploy-folder-redirection.md)
* [Implementar perfiles de usuario móviles](deploy-roaming-user-profiles.md)
* [Información general sobre el redireccionamiento de carpetas, los Archivos sin conexión y los perfiles de usuario móviles](folder-redirection-rup-overview.md)