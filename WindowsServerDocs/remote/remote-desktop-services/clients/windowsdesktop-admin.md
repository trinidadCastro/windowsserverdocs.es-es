---
title: Cliente de escritorio de Windows para administradores
description: Información sobre el cliente de escritorio de Windows principalmente útil para los administradores.
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 12/11/2020
ms.localizationpriority: medium
ms.openlocfilehash: b4085868c136f2f8653f623e5a167a283f463eee
ms.sourcegitcommit: 7c0794e257f602bd71af5eb9a11b8a03d2b9adfd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97390323"
---
# <a name="windows-desktop-client-for-admins"></a>Cliente de escritorio de Windows para administradores

>Se aplica a: Windows 10 y Windows 7

En este tema se incluye información adicional sobre el cliente de escritorio de Windows que resultará útil para los administradores. Para obtener información de uso básica, consulta [Introducción al cliente de escritorio de Windows](windowsdesktop.md).

## <a name="installation-options"></a>Opciones de instalación

Aunque los usuarios pueden instalar el cliente directamente después de descargarlo, si vas a realizar la implementación en varios dispositivos, también puede que quieras implementar el cliente a través de otros medios. La implementación mediante directivas de grupo o Microsoft Endpoint Configuration Manager te permite ejecutar el instalador silenciosamente mediante la línea de comandos. Ejecuta los siguientes comandos para implementar el cliente por dispositivo o por usuario.

### <a name="per-device-installation"></a>Instalación por dispositivo

```
msiexec.exe /I <path to the MSI> /qn ALLUSERS=1
```

### <a name="per-user-installation"></a>Instalación por usuario

```
msiexec.exe /i `<path to the MSI>` /qn ALLUSERS=2 MSIINSTALLPERUSER=1
```

## <a name="configuration-options"></a>Opciones de configuración

En esta sección se describen las nuevas opciones de configuración para este cliente.

### <a name="configure-update-notifications"></a>Configuración de las notificaciones de actualización

De forma predeterminada, el cliente le envía una notificación cada vez que hay una actualización, y la realiza automáticamente cuando el cliente está cerrado y no tiene conexiones activas. Incluso sin conexiones activas, el proceso msrdc.exe se ejecuta en segundo plano para que pueda volver a conectarse rápidamente cuando vuelva a abrir el cliente. Para detener msrdc.exe, haga clic con el botón derecho en el icono de Windows Virtual Desktop que está en el área de la bandeja del sistema; a continuación, seleccione **Desconectar todas las sesiones** en el menú desplegable.

Para desactivar las notificaciones, configura la siguiente información del registro:

- **Clave:** HKLM\Software\Microsoft\MSRDC\Policies
- **Tipo:** REG_DWORD
- **Nombre:** AutomaticUpdates
- **Datos:** 0 = deshabilitar las notificaciones y desactivar la actualización automática. 1 = mostrar las notificaciones y desactivar la actualización automática. 2 = mostrar notificaciones y actualizar automáticamente al cerrar.

### <a name="configure-user-groups"></a>Configuración de grupos de usuarios

Puedes configurar el cliente para uno de los siguientes tipos de grupos de usuarios, que determinan cuándo recibe actualizaciones el cliente.

#### <a name="insider-group"></a>Grupo de Insider

El grupo de Insider es para las primeras comprobaciones y consta de administradores y sus usuarios seleccionados. El grupo de Insider actúa como una serie de pruebas para detectar cualquier problema en la actualización que pueda afectar al rendimiento antes de que se lance al grupo público.

> [!NOTE]
> Se recomienda que cada organización tenga algunos usuarios en el grupo de Insider para probar las actualizaciones y detectar los problemas pronto.

En el grupo de Insider, se publica una nueva versión del cliente para los usuarios el segundo martes de cada mes a fin de realizar las primeras validaciones. Si la actualización no tiene problemas, se lanza al grupo público dos semanas más tarde. Los usuarios del grupo de Insider recibirán notificaciones de actualización automáticamente cada vez que las actualizaciones estén listas. Puedes obtener más información detallada sobre los cambios del cliente en [Novedades del cliente de escritorio de Windows](windowsdesktop-whatsnew.md).

Para configurar el cliente para el grupo de Insider, establece la siguiente información de registro:

- **Clave:** HKLM\Software\Microsoft\MSRDC\Policies
- **Tipo:** REG_SZ
- **Nombre:** ReleaseRing
- **Datos:** insider

#### <a name="public-group"></a>Grupo público

Este grupo es para todos los usuarios y es la versión más estable. No es necesario realizar ninguna acción para configurar este grupo.

El grupo público recibe el cuarto martes de cada mes la versión del cliente que el grupo de Insider probó. Todos los usuarios del grupo público recibirán una notificación de actualización si esa opción está habilitada.
