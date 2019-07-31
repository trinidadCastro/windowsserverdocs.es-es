---
title: Instalar Windows Admin Center
description: Cómo instalar el centro de administración de Windows en un equipo Windows o en un servidor para que varios usuarios puedan tener acceso al centro de administración de Windows mediante un explorador Web.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e67102d1fa8b35d90e97df64cb8bd2991b205ad5
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68658876"
---
# <a name="install-windows-admin-center"></a>Instalar Windows Admin Center

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En este tema se describe cómo instalar el centro de administración de Windows en un equipo Windows o en un servidor para que varios usuarios puedan tener acceso al centro de administración de Windows mediante un explorador Web.

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinar el tipo de instalación

Revise las [Opciones de instalación](../plan/installation-options.md) que incluye los [sistemas operativos compatibles](https://docs.microsoft.com/windows-server/manage/windows-admin-center/plan/installation-options#installation-supported-operating-systems). Para instalar el centro de administración de Windows en una máquina virtual de Azure, consulte [implementación del centro de administración de Windows en Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Instalar en Windows 10

Cuando instalas Windows Admin Center en Windows 10, utiliza el puerto 6516 de forma predeterminada, pero tiene la opción para especificar un puerto diferente. También puedes crear un acceso directo al escritorio y permitir que Windows Admin Center administre tus TrustedHosts.

> [!NOTE]
> Es necesaria modificar TrustedHosts en un entorno de grupo de trabajo o al usar credenciales de administrador local en un dominio. Si optas por pasar por alto esta configuración, debes [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Cuando inicias Windows Admin Center desde el menú **Inicio**, se abre en el explorador predeterminado.

Cuando inicias Windows Admin Center por primera vez, verás un icono en el área de notificación de tu escritorio. Haz clic con el botón derecho en este icono y elige **Abrir** para abrir la herramienta en el navegador predeterminado, o elige **Salir** para salir del proceso en segundo plano.

## <a name="install-on-windows-server-with-desktop-experience"></a>Instalar en Windows Server con experiencia de escritorio

En Windows Server, Windows Admin Center se instala como un servicio de red. Debes especificar el puerto en el que el servicio escucha y requiere un certificado para HTTPS. El instalador puede crear un certificado autofirmado para pruebas o puedes proporcionar la huella digital de un certificado ya instalado en el equipo. Si usas el certificado generado, coincidirá con el nombre DNS del servidor. Si usa su propio certificado, asegúrese de que el nombre proporcionado en el certificado coincide con el nombre de la máquina (no se admiten certificados comodín). También se le ofrece la opción de dejar que el centro de administración de Windows administre su TrustedHosts.

> [!NOTE]
> Es necesaria modificar TrustedHosts en un entorno de grupo de trabajo o al usar credenciales de administrador local en un dominio. Si optas por pasar por alto esta configuración, debes [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Una vez completada la instalación, abra un explorador desde un equipo remoto y navegue a la dirección URL presentada en el último paso del instalador.

> [!WARNING]
> Los certificados generados automáticamente vencen 60 días después de la instalación.

## <a name="install-on-server-core"></a>Instalar en Server Core

Si tienes una instalación Server Core de Windows Server, puedes instalar Windows Admin Center desde el símbolo del sistema (ejecutando como Administrador). Especifica un puerto y el certificado SSL utilizando los argumentos `SME_PORT` y `SSL_CERTIFICATE_OPTION` respectivamente. Si vas a usar un certificado existente, usa la `SME_THUMBPRINT` para especificar su huella digital.

> [!WARNING]
> Al instalar el centro de administración de Windows, se reiniciará el servicio WinRM, que dará como servidor todas las sesiones remotas de PowerShell. Se recomienda instalar desde un cmd local o PowerShell. Si va a instalar con una solución de automatización que se interrumpirá al reiniciar el servicio WinRM, puede Agregar el parámetro ```RESTART_WINRM=0``` a los argumentos de instalación, pero WinRM debe reiniciarse para que el centro de administración de Windows funcione.

Ejecuta el siguiente comando para instalar Windows Admin Center y generar automáticamente un certificado autofirmado:

```   
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Ejecuta el siguiente comando para instalar Windows Admin Center con un certificado existente:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> No invoques `msiexec` desde PowerShell utilizando la notación de ruta de acceso relativa del punto con una barra diagonal (como, `.\<WindowsAdminCenterInstallerName>.msi`). No se admite esa notación, se producirá un error en la instalación. Quita el prefijo `.\` o especifica la ruta de acceso completa al MSI.

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Actualizar a una nueva versión del centro de administración de Windows

Puede actualizar versiones no preliminares de Windows Admin Center mediante Microsoft Update o mediante la instalación manual.

La configuración se conserva al actualizar a una nueva versión del centro de administración de Windows. No se admite oficialmente la actualización de las versiones de Insider Preview del centro de administración de Windows. creemos que es mejor realizar una instalación limpia, pero no la bloqueamos.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Actualizar el certificado usado por el centro de administración de Windows

Cuando el centro de administración de Windows se implementa como un servicio, debe proporcionar un certificado para HTTPS. Para actualizar este certificado en otro momento, vuelva a ejecutar el instalador y elija ```change```.
