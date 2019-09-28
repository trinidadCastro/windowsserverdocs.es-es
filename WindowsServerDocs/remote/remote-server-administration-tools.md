---
title: Herramientas de administración remota del servidor
description: Tema de nivel superior para Herramientas de administración remota del servidor
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-rsat
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 121914f721cda7cbf0a117527b69568032d5541b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387088"
---
# <a name="remote-server-administration-tools"></a>Herramientas de administración remota del servidor

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se admite Herramientas de administración remota del servidor para Windows 10.

> [!IMPORTANT]
> A partir de la actualización 2018 de octubre de Windows 10, RSAT se incluye como un conjunto de **características a petición** en el propio Windows 10. Consulte **Cuándo se debe usar la versión de RSAT** siguiente para obtener instrucciones de instalación.

RSAT permite a los administradores de ti administrar roles y características de Windows Server desde un equipo con Windows 10.

Herramientas de administración remota del servidor incluye Administrador del servidor, Complementos de Microsoft Management Console (MMC), consolas, proveedores y cmdlets de Windows PowerShell, y algunas herramientas de línea de comandos para administrar roles y características que se ejecutan en Windows Server.

Herramientas de administración remota del servidor incluye módulos de cmdlet de Windows PowerShell que se pueden usar para administrar roles y características que se ejecutan en servidores remotos. Aunque la administración remota de Windows PowerShell está habilitada de forma predeterminada en Windows Server 2016, no está habilitada de forma predeterminada en Windows 10. Para ejecutar cmdlets que forman parte de Herramientas de administración remota del servidor en un servidor remoto, ejecute `Enable-PSremoting` en una sesión de Windows PowerShell que se haya abierto con permisos de usuario elevados (es decir, ejecutar como administrador) en el equipo cliente de Windows después de instalar Herramientas de administración remota del servidor.

## <a name="BKMK_Thresh"></a>Herramientas de administración remota del servidor para Windows 10
Use Herramientas de administración remota del servidor para Windows 10 para administrar tecnologías específicas en equipos que ejecutan Windows Server 2016, Windows Server 2012 R2 y, en casos limitados, Windows Server 2012 o Windows Server 2008 R2.

Herramientas de administración remota del servidor para Windows 10 incluye compatibilidad con la administración remota de equipos que ejecutan la opción de instalación Server Core o la configuración de interfaz de servidor básica de Windows Server 2016, Windows Server 2012 R2 y, en un límite casos, las opciones de instalación Server Core de Windows Server 2012. Sin embargo, Herramientas de administración remota del servidor para Windows 10 no se puede instalar en ninguna versión del sistema operativo Windows Server.

### <a name="tools-available-in-this-release"></a>Herramientas disponibles en esta versión
para obtener una lista de las herramientas disponibles en Herramientas de administración remota del servidor para Windows 10, consulte la tabla en [herramientas de administración remota del servidor (RSAT) para los sistemas operativos Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisitos del sistema
Herramientas de administración remota del servidor para Windows 10 solo se puede instalar en equipos que ejecutan Windows 10. Herramientas de administración remota del servidor no se pueden instalar en equipos que ejecutan Windows RT 8,1 u otros dispositivos de sistema en chip.

Herramientas de administración remota del servidor para Windows 10 se ejecuta en ediciones basadas en x86 y x64 de Windows 10.

> [!IMPORTANT]
> Herramientas de administración remota del servidor para Windows 10 no debe instalarse en un equipo que ejecute paquetes de herramientas de administración para Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o Windows 2000 Server. Quite todas las versiones anteriores del paquete de herramientas de administración o Herramientas de administración remota del servidor, incluidas versiones preliminares anteriores, y versiones de las herramientas para diferentes idiomas o configuraciones regionales del equipo antes de instalar la administración remota del servidor. Herramientas para Windows 10.

Para usar esta versión de Administrador del servidor para tener acceso y administrar servidores remotos que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, debe instalar varias actualizaciones para hacer que los sistemas operativos anteriores de Windows Server sean fáciles de administrar mediante el uso de Administrador de RVer. Para obtener información detallada sobre cómo preparar Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 para la administración mediante Administrador del servidor en Herramientas de administración remota del servidor para Windows 10, consulte [Administración de varios servidores remotos con servidor. Administrador](https://technet.microsoft.com/library/hh831456.aspx)de.

La administración remota de Windows PowerShell y Administrador del servidor debe estar habilitada en los servidores remotos para administrarlos mediante herramientas que forman parte de Herramientas de administración remota del servidor para Windows 10. La administración remota está habilitada de forma predeterminada en los servidores que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012. Para obtener más información sobre cómo deshabilitar la administración remota, vea [Administración de varios servidores remotos con el Administrador del servidor](https://go.microsoft.com/fwlink/p/?LinkId=241358).

## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Instalar, desinstalar y desactivar/en herramientas de RSAT

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-or-later"></a>Usar características a petición (du) para instalar herramientas de RSAT específicas en la actualización 2018 de octubre de Windows 10 o posterior

A partir de la actualización 2018 de octubre de Windows 10, RSAT se incluye como un conjunto de **características a petición** directamente desde Windows 10. Ahora, en lugar de descargar un paquete de RSAT, solo tiene que ir a **administrar características opcionales** en **configuración** y hacer clic en **Agregar una característica** para ver la lista de herramientas de RSAT disponibles. Seleccione e instale las herramientas de RSAT específicas que necesite. Para ver el progreso de la instalación, haga clic en el botón **atrás** para ver el estado en la página **administrar características opcionales** .

Consulte la [lista de herramientas de RSAT disponibles a través **de características a petición**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Además de instalar a través de la aplicación de **configuración** gráfica, también puede instalar herramientas de RSAT específicas mediante la línea de comandos o la automatización mediante [**DISM/Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Una ventaja de las características a petición es que las características instaladas se mantienen en las actualizaciones de la versión de Windows 10.

#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Para desinstalar herramientas de RSAT específicas en la actualización 2018 de octubre de Windows 10 o posterior (después de instalar con du)

En Windows 10, abra la aplicación de **configuración** , vaya a **administrar características opcionales**, seleccione y desinstale las herramientas de RSAT específicas que desea quitar. Tenga en cuenta que, en algunos casos, tendrá que desinstalar las dependencias manualmente. En concreto, si la herramienta de RSAT es necesaria para la herramienta de RSAT B, al elegir desinstalar la herramienta de RSAT a, se producirá un error si la herramienta de RSAT B todavía está instalada. En este caso, desinstale primero la herramienta de RSAT B y, a continuación, desinstale la herramienta de RSAT A. Tenga en cuenta también que en algunos casos, puede parecer que la desinstalación de una herramienta de RSAT se realiza correctamente aunque la herramienta todavía esté instalada. En este caso, al reiniciar el equipo se completará la eliminación de la herramienta.

Consulte la [lista de herramientas de RSAT, incluidas las dependencias](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Además de desinstalar a través de la aplicación de configuración gráfica, también puede desinstalar herramientas de RSAT específicas mediante la línea de comandos o la automatización mediante [**DISM/Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Cuándo se debe usar la versión de RSAT

Si tiene una versión de Windows 10 anterior a la actualización 2018 de octubre (1809), no podrá usar **las características a petición**. Deberá descargar e instalar el paquete de RSAT.

- **Instale RSAT FODs directamente desde Windows 10, tal como se describe anteriormente**: Al instalar en Windows 10 de octubre de 2018 Update (1809) o posterior, para administrar Windows Server 2019 o versiones anteriores.

- **Descargue e instale el paquete de RSAT de WS_1803, como se describe a continuación**: Al instalar en Windows 10 de abril de 2018 (1803) o versiones anteriores, para administrar Windows Server, versión 1803 o Windows Server, versión 1709.

- **Descargue e instale el paquete de RSAT de WS2016, como se describe a continuación**: Al instalar en Windows 10 de abril de 2018 (1803) o versiones anteriores, para administrar Windows Server 2016 o versiones anteriores.

#### <a name="BKMK_installthresh"></a>Descargar el paquete de RSAT para instalar Herramientas de administración remota del servidor para Windows 10

1.  Descargue el paquete de Herramientas de administración remota del servidor para Windows 10 del [centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?LinkID=404281). Puede ejecutar el instalador desde el sitio web del Centro de descarga o guardar el paquete de descarga en un recurso compartido o equipo local.

    > [!IMPORTANT]
    > Solo puede instalar Herramientas de administración remota del servidor para Windows 10 en equipos que ejecutan Windows 10. Herramientas de administración remota del servidor no se pueden instalar en equipos que ejecutan Windows RT 8,1 u otros dispositivos de sistema en chip.

2.  Si guarda el paquete de descarga en un recurso compartido o equipo local, haga doble clic en el programa de instalación, **WindowsTH-KB2693643-x64.msu** o **WindowsTH-KB2693643-x86.msu**, en función de la arquitectura del equipo en el que desea instalar las herramientas.

3.  Cuando aparezca el cuadro de diálogo **Instalador independiente de Windows Update** y le pregunte si desea instalar la actualización, haga clic en **Sí**.

4.  Lea los términos de licencia y acéptelos. Haga clic en **Acepto**.

5.  La instalación tardará unos minutos en completarse.

##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-after-rsat-package-install"></a>Para desinstalar Herramientas de administración remota del servidor para Windows 10 (después de instalar el paquete de RSAT)

1. En el escritorio, haga clic en **Inicio**, **Todas las aplicaciones**, **Sistema de Windows** y **Panel de control**.

2. En **Programas**, haga clic en **Desinstalar un programa**.

3. Haga clic en **Ver actualizaciones instaladas**.

4. Haga clic con el botón secundario en **Actualización para Microsoft Windows (KB2693643)** y, a continuación, haga clic en **Desinstalar**.

5. Cuando se le pregunte si está seguro de que desea desinstalar la actualización, haga clic en **Sí**.
   S
   ##### <a name="to-turn-off-specific-tools-after-rsat-package-install"></a>Para desactivar herramientas específicas (después de instalar el paquete de RSAT)

6. En el escritorio, haga clic en **Inicio**, **Todas las aplicaciones**, **Sistema de Windows** y **Panel de control**.

7. Haga clic en **Programas**y, a continuación, en **Programas y características** ; haga clic en **Activar o desactivar las características de Windows**.

8. En el cuadro de diálogo **Características de Windows** , expanda **Herramientas de administración remota del servidor**y, a continuación, expanda **Herramientas de administración de roles** o **Herramientas de administración de características**.

9. Desactive las casillas de las herramientas que desee desactivar.

   > [!NOTE]
   > Si desactiva Administrador del servidor, el equipo debe reiniciarse y las herramientas a las que se podía tener acceso desde el menú **herramientas** de administrador del servidor deben abrirse desde la carpeta **herramientas administrativas** .

10. Cuando termine de desactivar las herramientas que no desea usar, haga clic en **Aceptar**.

### <a name="run-remote-server-administration-tools"></a>Ejecutar Herramientas de administración remota del servidor

> [!NOTE]
> Después de instalar Herramientas de administración remota del servidor para Windows 10, se muestra la carpeta **herramientas administrativas** en el menú **Inicio** . Puede tener acceso a las herramientas desde las ubicaciones siguientes.
>
> -   El menú **herramientas** de la consola de administrador del servidor.
> -   **Panel de control\Sistema y seguridad\Herramientas administrativas**.
> -   Un acceso directo que se guarda en el escritorio de la carpeta **Herramientas administrativas** (para ello, haga clic con el vínculo **Panel de control\Sistema y herramientas de Security\Herramientas administrativas** y luego haga clic en **Crear acceso directo**).

Las herramientas instaladas como parte de Herramientas de administración remota del servidor para Windows 10 no se pueden usar para administrar el equipo cliente local. Independientemente de la herramienta que ejecute, debe especificar un servidor remoto, o varios servidores remotos, en el que se ejecutará la herramienta. Dado que la mayoría de las herramientas están integradas con Administrador del servidor, agregue los servidores remotos que desea administrar al grupo de servidores de Administrador del servidor antes de administrar el servidor mediante el uso de las herramientas del menú **herramientas** . Para obtener más información sobre cómo agregar servidores al grupo de servidores y crear grupos de servidores personalizados, vea el tema sobre la [adición de servidores al Administrador del servidor](https://go.microsoft.com/fwlink/p/?LinkId=241353) y la [creación y administración de grupos de servidores](https://go.microsoft.com/fwlink/?LinkId=247328).

En Herramientas de administración remota del servidor para Windows 10, se obtiene acceso a todas las herramientas de administración de servidor basadas en GUI, como los cuadros de diálogo y los complementos de MMC, desde el menú **herramientas** de la consola de administrador del servidor. Aunque el equipo que ejecuta Herramientas de administración remota del servidor para Windows 10 ejecuta un sistema operativo basado en cliente, después de instalar las herramientas, Administrador del servidor incluido con Herramientas de administración remota del servidor para Windows 10, se abre automáticamente de forma predeterminada. en el equipo cliente. Tenga en cuenta que no hay ninguna página del **servidor local** en la consola de administrador del servidor que se ejecuta en un equipo cliente.

##### <a name="to-start-server-manager-on-a-client-computer"></a>Para iniciar el Administrador del servidor en un equipo cliente

1.  En el menú **Inicio**, haga clic en **Todas las aplicaciones** y en **Herramientas administrativas**.

2.  En la carpeta **Herramientas administrativas**, haga clic en **Administrador del servidor**.

Aunque no se muestran en el menú **herramientas** de la consola de administrador del servidor, también se instalan los cmdlets de Windows PowerShell y las herramientas de administración del símbolo del sistema para roles y características como parte de herramientas de administración remota del servidor. Por ejemplo, si abre una sesión de Windows PowerShell con derechos de usuario elevados (ejecutar como administrador) y ejecuta el cmdlet `Get-Command -Module RDManagement`, los resultados incluyen una lista de cmdlets de servicios de escritorio remoto que ahora están disponibles para ejecutarse en el equipo local después de instalar Herramientas de administración remota del servidor, siempre y cuando los cmdlets se destinen a un servidor remoto que ejecute todo o parte del rol servicios de escritorio remoto.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Para iniciar Windows PowerShell con permisos del usuario elevados (Ejecutar como administrador)

1.  En el menú **Inicio**, haga clic en **Todas las aplicaciones**, **Sistema de Windows** y **Windows PowerShell**.

2.  Para ejecutar Windows PowerShell como administrador desde el escritorio, haga clic con el botón derecho en el acceso directo de **Windows PowerShell** y, a continuación, haga clic en **Ejecutar como administrador**.

> [!NOTE]
> También puede iniciar una sesión de Windows PowerShell que esté destinada a un servidor específico; para ello, haga clic con el botón secundario en un servidor administrado en una página de rol o grupo de Administrador del servidor y, a continuación, haga clic en **Windows PowerShell**.


## <a name="known-issues"></a>Problemas conocidos

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: Error de instalación de RSAT du con el código de error 0x800f0954

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018) en entornos de WSUS/SCCM
> 
> **Solución**: Para instalar FODs en un equipo unido a un dominio que recibe actualizaciones a través de WSUS o SCCM, deberá cambiar una configuración de directiva de grupo para habilitar la descarga de FODs directamente desde Windows Update o un recurso compartido local. Para obtener más información e instrucciones sobre cómo cambiar esta configuración, consulte [Cómo hacer que las características a petición y los paquetes de idioma estén disponibles cuando se usa WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: Instalación de RSAT du a través de la aplicación de configuración no muestra el estado o el progreso

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018)
> 
> **Solución**: Para ver el progreso de la instalación, haga clic en el botón **atrás** para ver el estado en la página **administrar características opcionales** .

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: No se puede realizar la desinstalación de RSAT du a través de la aplicación de configuración

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018)
> 
> **Solución**: En algunos casos, los errores de desinstalación se deben a la necesidad de desinstalar manualmente las dependencias. En concreto, si la herramienta de RSAT es necesaria para la herramienta de RSAT B, al elegir desinstalar la herramienta de RSAT a, se producirá un error si la herramienta de RSAT B todavía está instalada. En este caso, desinstale primero la herramienta de RSAT B y, a continuación, desinstale la herramienta de RSAT A. Consulte la lista de RSAT FODs, incluidas las dependencias.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: La desinstalación de RSAT du parece ser correcta, pero la herramienta todavía está instalada

> **Impacto**: RSAT FODs en Windows 10 1809 (actualización de octubre de 2018)
> 
> **Solución**: Al reiniciar el equipo, se completará la eliminación de la herramienta.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: RSAT que faltan después de la actualización de Windows 10

> **Impacto**: Cualquier RSAT. La instalación del paquete MSU (anterior a RSAT FODs) no se reinstala automáticamente.
> 
> **Solución**: Una instalación de RSAT no puede persistir entre las actualizaciones del sistema operativo debido a las RSAT. MSU que se entrega como un paquete de Windows Update. Vuelva a instalar RSAT después de actualizar Windows 10. Tenga en cuenta que esta limitación es uno de los motivos por los que hemos pasado a FODs a partir de Windows 10 1809. Las FODs de RSAT instaladas se conservarán en futuras actualizaciones de versiones de Windows 10.

## <a name="see-also"></a>Vea también
>- [Herramientas de administración remota del servidor para Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Herramientas de administración remota del servidor (RSAT) para Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 y Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055)
