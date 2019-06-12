---
title: Instalar Windows Admin Center
description: Cómo instalar Windows Admin Center en un equipo con Windows o en un servidor para que varios usuarios puedan acceder a Windows Admin Center mediante un explorador web.
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a9eb7944cd35dfa68e3c36cdc6c016f483a9f1e1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811957"
---
# <a name="install-windows-admin-center"></a>Instalar Windows Admin Center

> Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Este tema describe cómo instalar Windows Admin Center en un equipo con Windows o en un servidor para que varios usuarios puedan acceder a Windows Admin Center mediante un explorador web.

> [!Tip]
> ¿Novedad en Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../understand/windows-admin-center.md) o [descárgalo ahora](https://aka.ms/windowsadmincenter).

## <a name="determine-your-installation-type"></a>Determinar el tipo de instalación

Revise el [opciones de instalación](../plan/installation-options.md) que incluye el [sistemas operativos compatibles con](../plan/installation-options.md#supported-operating-systems-installation). Para instalar Windows Admin Center en una máquina virtual en Azure, consulte [implementar Windows Admin Center en Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Instalar en Windows 10

Cuando instalas Windows Admin Center en Windows 10, utiliza el puerto 6516 de forma predeterminada, pero tiene la opción para especificar un puerto diferente. También puedes crear un acceso directo al escritorio y permitir que Windows Admin Center administre tus TrustedHosts.

> [!NOTE]
> Es necesaria modificar TrustedHosts en un entorno de grupo de trabajo o al usar credenciales de administrador local en un dominio. Si optas por pasar por alto esta configuración, debes [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Cuando inicias Windows Admin Center desde el menú **Inicio**, se abre en el explorador predeterminado.

Cuando inicias Windows Admin Center por primera vez, verás un icono en el área de notificación de tu escritorio. Haz clic con el botón derecho en este icono y elige **Abrir** para abrir la herramienta en el navegador predeterminado, o elige **Salir** para salir del proceso en segundo plano.

## <a name="install-on-windows-server-with-desktop-experience"></a>Instalar en Windows Server con experiencia de escritorio

En Windows Server, Windows Admin Center se instala como un servicio de red. Debes especificar el puerto en el que el servicio escucha y requiere un certificado para HTTPS. El instalador puede crear un certificado autofirmado para pruebas o puedes proporcionar la huella digital de un certificado ya instalado en el equipo. Si usas el certificado generado, coincidirá con el nombre DNS del servidor. Si usa su propio certificado, asegúrese de que el nombre proporcionado en el certificado coincide con el nombre del equipo (no se admiten certificados de comodín.) También tiene la opción para permitir que Windows Admin Center administrar su TrustedHosts.

> [!NOTE]
> Es necesaria modificar TrustedHosts en un entorno de grupo de trabajo o al usar credenciales de administrador local en un dominio. Si optas por pasar por alto esta configuración, debes [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Una vez completada la instalación, abra un explorador desde un equipo remoto y vaya a la dirección URL que se presentan en el último paso del instalador.

> [!WARNING]
> Los certificados generados automáticamente vencen 60 días después de la instalación.

## <a name="install-on-server-core"></a>Instalar en Server Core

Si tienes una instalación Server Core de Windows Server, puedes instalar Windows Admin Center desde el símbolo del sistema (ejecutando como Administrador). Especifica un puerto y el certificado SSL utilizando los argumentos `SME_PORT` y `SSL_CERTIFICATE_OPTION` respectivamente. Si vas a usar un certificado existente, usa la `SME_THUMBPRINT` para especificar su huella digital.

> [!WARNING]
> Instalar Windows Admin Center se reiniciará el servicio WinRM, que eliminará todas las sesiones remotas de PowerShell. Se recomienda que instale desde un local Cmd o PowerShell. Si va a instalar con una solución de automatización que se dividirá por el reinicio del servicio WinRM, puede agregar el parámetro ```RESTART_WINRM=0``` para la instalación de argumentos, pero WinRM debe reiniciarse para Windows Admin Center a la función.

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

## <a name="updating-windows-admin-center"></a>Actualizar Windows Admin Center

Puede actualizar versiones no preliminares de Windows Admin Center mediante Microsoft Update o instalando manualmente. 

La configuración se conservará al actualizar a una nueva versión de Windows Admin Center. No se admiten oficialmente actualizar versiones Insider Preview de Windows Admin Center: Creemos que es mejor hacer una instalación limpia - pero no bloquearlo.