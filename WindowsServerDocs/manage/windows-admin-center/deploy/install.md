---
title: Instalación de Windows Admin Center
description: Cómo instalar Windows Admin Center en un PC Windows o en un servidor para que varios usuarios puedan acceder a este desde un explorador web.
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 07/17/2019
ms.localizationpriority: medium
ms.openlocfilehash: cf403094ec7b9fb91b3337680c49538425e0fc2e
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766498"
---
# <a name="install-windows-admin-center"></a>Instalación de Windows Admin Center

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En este tema se describe cómo instalar Windows Admin Center en un PC Windows o en un servidor para que varios usuarios puedan acceder a este desde un explorador web.

> [!Tip]
> ¿No estás familiarizado con Windows Admin Center?
> [Obtén más información acerca de Windows Admin Center](../overview.md) o [descárgalo ahora](../overview.md).

## <a name="determine-your-installation-type"></a>Determinación del tipo de instalación

Revisa las [opciones de instalación ](../plan/installation-options.md), que incluyen los [sistemas operativos compatibles](../plan/installation-options.md#installation-supported-operating-systems). Para instalar Windows Admin Center en una VM en Azure, consulta [Implementación de Windows Admin Center en Azure](../azure/deploy-wac-in-azure.md).

## <a name="install-on-windows-10"></a>Instalar en Windows 10

Cuando instalas Windows Admin Center en Windows 10, usa el puerto 6516 de forma predeterminada, pero puedes especificar otro. También puedes crear un acceso directo al escritorio y dejar que Windows Admin Center administre TrustedHosts.

> [!NOTE]
> Es necesario modificar TrustedHosts en un entorno de grupo de trabajo o al usar credenciales de administrador locales en un dominio. Si eliges pasar por alto esta configuración, debes [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Cuando inicias Windows Admin Center desde el menú **Inicio**, se abre en el explorador predeterminado.

Cuando inicies Windows Admin Center por primera vez, verás un icono en el área de notificación del escritorio. Haz clic con el botón derecho en este icono y elige **Abrir** para abrir la herramienta en el navegador predeterminado, o elige **Salir** para salir del proceso en segundo plano.

## <a name="install-on-windows-server-with-desktop-experience"></a>Instalar en Windows Server con experiencia de escritorio

En Windows Server, Windows Admin Center se instala como servicio de red. Debes especificar el puerto en el que escucha el servicio. Se requiere un certificado para HTTPS. El instalador puede crear un certificado autofirmado para pruebas o puedes proporcionar la huella digital de un certificado ya instalado en el equipo. Si usas el certificado generado, coincidirá con el nombre DNS del servidor. Si usas tu propio certificado, asegúrate de que el nombre proporcionado en el certificado coincide con el nombre de la máquina (no se admiten los certificados comodín). También tienes la opción de permitir que Windows Admin Center administre TrustedHosts.

> [!NOTE]
> Es necesario modificar TrustedHosts en un entorno de grupo de trabajo o al usar credenciales de administrador locales en un dominio. Si optas por pasar por alto esta configuración, debes [configurar TrustedHosts manualmente](../support/troubleshooting.md#configure-trustedhosts).

Una vez completada la instalación, abre un explorador desde un equipo remoto y navega a la dirección URL presentada en el último paso del instalador.

> [!WARNING]
> Los certificados generados automáticamente vencen 60 días después de la instalación.

## <a name="install-on-server-core"></a>Instalar en Server Core

Si tienes una instalación de Server Core de Windows Server, puedes instalar Windows Admin Center desde el símbolo del sistema (como Administrador). Especifica un puerto y el certificado SSL mediante los argumentos `SME_PORT` y `SSL_CERTIFICATE_OPTION`, respectivamente. Si vas a usar un certificado existente, usa `SME_THUMBPRINT` para especificar su huella digital.

> [!WARNING]
> Al instalar Windows Admin Center, se reiniciará el servicio WinRM, lo que desconectará todas las sesiones remotas de PowerShell. Es recomendable realizar la instalación desde PowerShell o un cmd local. Si vas a realizar la instalación con una solución de automatización que se interrumpiría si se reinicia el servicio WinRM, agrega el parámetro ```RESTART_WINRM=0``` a los argumentos de instalación, pero es necesario reiniciar WinRM para que Windows Admin Center funcione.

Ejecuta el siguiente comando para instalar Windows Admin Center y generar automáticamente un certificado autofirmado:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SSL_CERTIFICATE_OPTION=generate
```

Ejecuta el siguiente comando para instalar Windows Admin Center con un certificado existente:

```
msiexec /i <WindowsAdminCenterInstallerName>.msi /qn /L*v log.txt SME_PORT=<port> SME_THUMBPRINT=<thumbprint> SSL_CERTIFICATE_OPTION=installed
```

> [!WARNING]
> No invoques `msiexec` desde PowerShell usando la notación de ruta de acceso relativa del punto con una barra diagonal (por ejemplo, `.\<WindowsAdminCenterInstallerName>.msi`). Esta notación no se admite y provocará un error en la instalación. Quita el prefijo `.\` o especifica la ruta de acceso completa al MSI.

## <a name="upgrading-to-a-new-version-of-windows-admin-center"></a>Actualización a una nueva versión de Windows Admin Center

Puede actualizar versiones no preliminares de Windows Admin Center mediante Microsoft Update o mediante la instalación manual.

La configuración se conserva al actualizar a una nueva versión de Windows Admin Center. Oficialmente, no admitimos la actualización de versiones Insider Preview de Windows Admin Center, ya que consideramos que es mejor realizar una instalación limpia, pero tampoco la bloqueamos.

## <a name="updating-the-certificate-used-by-windows-admin-center"></a>Actualización del certificado que usa Windows Admin Center

Si tienes Windows Admin Center implementado como servicio, debes proporcionar un certificado para HTTPS. Para actualizar este certificado en otro momento, vuelve a ejecutar el instalador y elige ```change```.