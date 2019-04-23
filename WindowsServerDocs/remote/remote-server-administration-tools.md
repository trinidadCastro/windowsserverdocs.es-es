---
title: Herramientas de administración remota del servidor
description: Tema de nivel superior para herramientas de administración remota del servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: cc4b0eb51b477ec175040b46c9563f81955c0be3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846216"
---
# <a name="remote-server-administration-tools"></a>Herramientas de administración remota del servidor

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema admite remoto Server Administration Tools for Windows 10.

> [!IMPORTANT]
> Inicie con Windows 10 de octubre de 2018 Update, RSAT se incluye como un conjunto de **características a petición** en Windows 10 sí. Consulte **cuándo se debe usar la versión RSAT** a continuación para obtener instrucciones de instalación.

RSAT permite a los administradores de TI administrar roles y características Windows Server desde un equipo con Windows 10.

Herramientas de administración remota del servidor incluye el administrador del servidor, complementos Microsoft Management Console (mmc), consolas, cmdlets de Windows PowerShell y los proveedores y algunas herramientas de línea de comandos para administrar roles y características que se ejecutan en Windows Server.

Herramientas de administración remota del servidor incluye módulos de cmdlet de Windows PowerShell que pueden usarse para administrar roles y características que se ejecutan en servidores remotos. Aunque la administración remota de Windows PowerShell está habilitada de forma predeterminada en Windows Server 2016, no está habilitado de forma predeterminada en Windows 10. Para ejecutar los cmdlets que forman parte de herramientas de administración remota en un servidor remoto, ejecute `Enable-PSremoting` en una sesión de Windows PowerShell abierta con derechos de usuario elevados (es decir, ejecutar como administrador) en el equipo cliente de Windows después de instalar herramientas de administración remota del servidor.

## <a name="BKMK_Thresh"></a>Herramientas de administración remota del servidor para Windows 10
Utilice remoto Server Administration Tools for Windows 10 para administrar tecnologías específicas en equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, y en casos limitados, Windows Server 2012 o Windows Server 2008 R2.

Remoto Server Administration Tools for Windows 10 incluye compatibilidad para la administración remota de equipos que ejecutan la opción de instalación Server Core o la configuración de la interfaz de servidor mínima de Windows Server 2016, Windows Server 2012 R2 y limitada casos, las opciones de instalación Server Core de Windows Server 2012. Sin embargo, no se puede instalar remoto Server Administration Tools for Windows 10 en las versiones del sistema operativo Windows Server.

### <a name="tools-available-in-this-release"></a>Herramientas disponibles en esta versión
Para obtener una lista de las herramientas disponibles en remoto Server Administration Tools for Windows 10, vea la tabla en [remoto las herramientas de administración de servidor (RSAT) para los sistemas operativos de Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisitos del sistema
Remoto Server Administration Tools for Windows 10 puede instalarse solo en equipos que ejecutan Windows 10. Herramientas de administración remota del servidor no puede instalarse en equipos que ejecutan Windows RT 8.1 u otros dispositivos del sistema del procesador.

Remoto Server Administration Tools for Windows 10 se ejecuta en ediciones tanto x86 y x64 64 de Windows 10.

> [!IMPORTANT]
> Remoto Server Administration Tools for Windows 10 no debe instalarse en un equipo que ejecute paquetes de herramientas de administración para Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o Windows 2000 Server. Quite todas las versiones anteriores del paquete de herramientas de administración o herramientas de administración remota, incluidas versiones preliminares anteriores y versiones de las herramientas para diferentes idiomas o configuraciones regionales desde el equipo antes de instalar Administración remota del servidor Herramientas para Windows 10.

Para usar esta versión del administrador del servidor para obtener acceso y administrar servidores remotos que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, debe instalar varias actualizaciones para hacer que los sistemas operativos anteriores de Windows Server puedan administrar mediante el uso de Se Administrador de ase. Para obtener información detallada acerca de cómo preparar Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 para la administración mediante el Administrador de servidor en remoto Server Administration Tools for Windows 10, consulte [Manage Multiple, Remote Servers con el administrador del servidor](https://technet.microsoft.com/library/hh831456.aspx).

Administración remota de Windows PowerShell y el administrador del servidor debe habilitarse en los servidores remotos para administrarlos mediante las herramientas que forman parte de remoto Server Administration Tools for Windows 10. Administración remota está habilitada de forma predeterminada en los servidores que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012. Para obtener más información sobre cómo deshabilitar la administración remota, vea [Administración de varios servidores remotos con el Administrador del servidor](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Instalar, desinstalar y activar y desactivar las herramientas RSAT

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Actualización las características de uso a petición (du) para instalar las herramientas RSAT específicas en Windows 10 de octubre de 2018, o una versión posterior

Inicie con Windows 10 de octubre de 2018 Update, RSAT se incluye como un conjunto de **características a petición** directamente desde Windows 10. Ahora, en lugar de descargar un paquete de RSAT simplemente vaya a **administrar características opcionales** en **configuración** y haga clic en **agregar una característica** para ver la lista de las herramientas RSAT disponibles. Seleccione e instale las herramientas RSAT específicas que necesita. Para ver el progreso de la instalación, haga clic en el **Atrás** botón para ver el estado de la **administrar características opcionales** página.

Consulte la [herramientas disponibles a través de la lista de RSAT **características a petición**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Además de instalar a través de la gráfica **configuración** aplicación, también puede instalar las herramientas RSAT específicas a través de la línea de comandos o la automatización con [ **DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Una ventaja de las características a petición es que las características instaladas conservan entre las actualizaciones de versión de Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Para desinstalar RSAT específico de herramientas en Windows 10 de octubre de 2018 Update o posterior (después de instalar con Demand, FoD)

En Windows 10, abra el **configuración** aplicación, vaya a **administrar características opcionales**, seleccione y desinstalar las herramientas RSAT específicas que desea quitar. Tenga en cuenta que en algunos casos, necesitará desinstalar manualmente las dependencias. En concreto, si se necesita una herramienta de RSAT herramienta RSAT B, a continuación, elegir desinstalar una herramienta RSAT se producirá un error si todavía está instalada la herramienta RSAT B. En este caso, desinstale la herramienta de RSAT B primero y, a continuación, desinstale RSAT herramienta a Tenga en cuenta también que en algunos casos, al desinstalar una herramienta RSAT puede parecer que se realice correctamente, aunque todavía está instalada la herramienta. En este caso, reiniciar el equipo completará la eliminación de la herramienta.

Consulte la [lista de las herramientas RSAT, incluidas las dependencias](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Además de desinstalación a través de la aplicación de configuración gráfica, también puede desinstalar las herramientas RSAT específicas a través de la línea de comandos o la automatización con [ **DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Cuándo usar la versión RSAT

Si tiene una versión de Windows 10 antes de la de octubre de 2018 actualizar (1809), no podrá usar **características a petición**. Deberá descargar e instalar el paquete de RSAT.

- **Instalar RSAT FODs directamente desde Windows 10, tal como se describe anteriormente**: Cuando se instala en Windows 10 de octubre de 2018 actualizar (1809) o posterior, para la administración de Windows Server 2019 o versiones anteriores.

- **Descargue e instale el paquete de RSAT WS_1803, tal como se describe a continuación**: Cuando se instala en Windows 10 de abril de 2018 actualizar (1803) o una versión anterior, para la administración de Windows Server, versión 1803 o Windows Server, versión 1709.

- **Descargue e instale el paquete de RSAT WS2016, tal como se describe a continuación**: Cuando se instala en Windows 10 de abril de 2018 actualizar (1803) o una versión anterior, para administrar Windows Server 2016 o versiones anteriores.

#### <a name="BKMK_installthresh"></a>Descargue el paquete RSAT para instalar remoto Server Administration Tools for Windows 10

1.  Descargue el paquete remoto Server Administration Tools for Windows 10 desde la [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkID=404281). Puede ejecutar el instalador desde el sitio web del Centro de descarga o guardar el paquete de descarga en un recurso compartido o equipo local.

    > [!IMPORTANT]
    > Remoto Server Administration Tools for Windows 10 solo se puede instalar en equipos que ejecutan Windows 10. Herramientas de administración remota del servidor no puede instalarse en equipos que ejecutan Windows RT 8.1 u otros dispositivos de sistema del procesador.

2.  Si guarda el paquete de descarga en un recurso compartido o equipo local, haga doble clic en el programa de instalación, **WindowsTH-KB2693643-x64.msu** o **WindowsTH-KB2693643-x86.msu**, en función de la arquitectura del equipo en el que desea instalar las herramientas.

3.  Cuando aparezca el cuadro de diálogo **Instalador independiente de Windows Update** y le pregunte si desea instalar la actualización, haga clic en **Sí**.

4.  Lea los términos de licencia y acéptelos. Haga clic en **Acepto**.

5.  La instalación tardará unos minutos en completarse.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Para desinstalar herramientas de administración remota del servidor para Windows 10 (después de la instalación del paquete RSAT)

1.  En el escritorio, haga clic en **Inicio**, **Todas las aplicaciones**, **Sistema de Windows** y **Panel de control**.

2.  En **Programas**, haga clic en **Desinstalar un programa**.

3.  Haga clic en **Ver actualizaciones instaladas**.

4.  Haga clic con el botón secundario en **Actualización para Microsoft Windows (KB2693643)** y, a continuación, haga clic en **Desinstalar**.

5.  Cuando se le pregunte si está seguro de que desea desinstalar la actualización, haga clic en **Sí**.
S
##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Para desactivar herramientas específicas (después de instalar el paquete de RSAT)

1.  En el escritorio, haga clic en **Inicio**, **Todas las aplicaciones**, **Sistema de Windows** y **Panel de control**.

2.  Haga clic en **Programas**y, a continuación, en **Programas y características** ; haga clic en **Activar o desactivar las características de Windows**.

3.  En el cuadro de diálogo **Características de Windows** , expanda **Herramientas de administración remota del servidor**y, a continuación, expanda **Herramientas de administración de roles** o **Herramientas de administración de características**.

4.  Desactive las casillas de las herramientas que desee desactivar.

    > [!NOTE]
    > Si desactivar el Administrador de servidor, debe reiniciarse el equipo, y herramientas que se puede acceder desde el **herramientas** deberá abrirse desde el menú del administrador del servidor el **herramientas administrativas** carpeta.

5.  Cuando termine de desactivar las herramientas que no desea usar, haga clic en **Aceptar**.

### <a name="run-remote-server-administration-tools"></a>Ejecutar Herramientas de administración remota del servidor

> [!NOTE]
> Después de instalar remoto Server Administration Tools para Windows 10, la **herramientas administrativas** carpeta se muestra en el **iniciar** menú. Puede tener acceso a las herramientas desde las ubicaciones siguientes.
>
> -   El **herramientas** menú de la consola de administrador del servidor.
> -   **Panel de control\Sistema y seguridad\Herramientas administrativas**.
> -   Un acceso directo que se guarda en el escritorio de la carpeta **Herramientas administrativas** (para ello, haga clic con el vínculo **Panel de control\Sistema y herramientas de Security\Herramientas administrativas** y luego haga clic en **Crear acceso directo**).

No se puede usar las herramientas instaladas como parte de remoto Server Administration Tools for Windows 10 para administrar el equipo cliente local. Independientemente de la herramienta que se ejecuta, debe especificar un servidor remoto o en varios servidores remotos, en el que se va a ejecutar la herramienta. Puesto que la mayoría de las herramientas están integradas con el administrador del servidor, agregue servidores remotos que desea administrar al grupo de servidores de administrador del servidor antes de administrar el servidor mediante las herramientas en el **herramientas** menú. Para obtener más información sobre cómo agregar servidores al grupo de servidores y crear grupos de servidores personalizados, vea el tema sobre la [adición de servidores al Administrador del servidor](https://go.microsoft.com/fwlink/p/?LinkId=241353) y la [creación y administración de grupos de servidores](https://go.microsoft.com/fwlink/?LinkId=247328).

En herramientas de administración remota del servidor para Windows 10, todas las herramientas de administración de servidor basada en GUI, como el cuadro de diálogo y complementos mmc cuadros, se obtiene acceso desde el **herramientas** menú de la consola de administrador del servidor. Aunque el equipo que ejecuta remoto Server Administration Tools for Windows 10 ejecuta un sistema operativo basado en cliente, después de instalar las herramientas, administrador del servidor, incluido con remoto Server Administration Tools para Windows 10, se abrirá automáticamente de forma predeterminada en el equipo cliente. Tenga en cuenta que no hay ningún **servidor Local** página en la consola de administrador del servidor que se ejecuta en un equipo cliente.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar el Administrador del servidor en un equipo cliente

1.  En el menú **Inicio**, haga clic en **Todas las aplicaciones** y en **Herramientas administrativas**.

2.  En la carpeta **Herramientas administrativas**, haga clic en **Administrador del servidor**.

Aunque no aparezcan en la consola de administrador del servidor **herramientas** menú, los cmdlets de Windows PowerShell y herramientas de administración de línea de comandos también se instalan para roles y características como parte de herramientas de administración remota del servidor. Por ejemplo, si abre una sesión de Windows PowerShell con derechos de usuario elevados (ejecutar como administrador) y ejecute el cmdlet `Get-Command -Module RDManagement`, los resultados incluyen una lista de cmdlets de servicios de escritorio remoto que ahora están disponibles para ejecutarse en el equipo local después de instalar herramientas de administración remota, siempre y cuando los cmdlets están dirigidos a un servidor remoto que se está ejecutando todo o parte del rol Servicios de escritorio remoto.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Para iniciar Windows PowerShell con permisos del usuario elevados (Ejecutar como administrador)

1.  En el menú **Inicio**, haga clic en **Todas las aplicaciones**, **Sistema de Windows** y **Windows PowerShell**.

2.  Para ejecutar Windows PowerShell como administrador desde el escritorio, haga clic en el **Windows PowerShell** acceso directo y, a continuación, haga clic en **ejecutar como administrador**.

> [!NOTE]
> También puede iniciar una sesión de Windows PowerShell que está dirigida a un servidor específico haciendo clic en un servidor administrado en una página de rol o grupo en Administrador del servidor y, a continuación, haga clic en **Windows PowerShell**.


## <a name="known-issues"></a>Problemas conocidos

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: Se produce un error en la instalación de RSAT FOD con código de error 0x800f0954

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018) en entornos de WSUS/SCCM

> **Resolución**: Para instalar FODs en un equipo unido al dominio que recibe las actualizaciones a través de WSUS o SCCM, deberá cambiar una configuración de directiva de grupo para habilitar la descarga FODs directamente desde Windows Update o un recurso compartido local. Para obtener más detalles e instrucciones sobre cómo cambiar la configuración, consulte [para que las características de demanda y paquetes de idiomas estén disponibles cuando se usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: Instalación RSAT FOD a través de la aplicación de configuración no muestra el estado y progreso

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018)

> **Resolución**: Para ver el progreso de la instalación, haga clic en el **Atrás** botón para ver el estado de la **administrar características opcionales** página.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: Desinstalación de RSAT FOD a través de la configuración de la aplicación puede producir un error

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018)

> **Resolución**: En algunos casos, los errores de desinstalación son debido a la necesidad de desinstalar manualmente las dependencias. En concreto, si se necesita una herramienta de RSAT herramienta RSAT B, a continuación, elegir desinstalar una herramienta RSAT se producirá un error si todavía está instalada la herramienta RSAT B. En este caso, desinstale la herramienta de RSAT B primero y, a continuación, desinstale RSAT herramienta a Ver la lista de FODs RSAT, incluidas las dependencias.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: Desinstalación de RSAT FOD parece realizarse correctamente, pero todavía se instala la herramienta

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018)

> **Resolución**: Reiniciar el equipo completará la eliminación de la herramienta.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: RSAT falta después de actualizar Windows 10

> **Impacto**: Cualquier RSAT. Instalación del paquete de MSU (antes FODs RSAT) se vuelve a instalar automáticamente

> **Resolución**: Una instalación de RSAT no puede persistir a través de actualizaciones del sistema operativo debido a las RSAT. MSU se entregan como un paquete de actualización de Windows. Instale RSAT nuevo después de actualizar Windows 10. Tenga en cuenta que esta limitación es uno de los motivos por qué tuvimos que explicar a FODs a partir de Windows 10 1809. Se conservará FODs RSAT que se instalan a través de actualizaciones de versiones futuras de Windows 10.

## <a name="see-also"></a>Vea también
>- [Herramientas de administración remota del servidor para Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Herramientas de administración remota del servidor (RSAT) para Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 y Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)


