---
title: Herramientas de administración remota del servidor
description: Tema de nivel superior para Herramientas de administración remota del servidor
ms.prod: windows-server
ms.technology: manage-rsat
ms.topic: get-started-article
ms.assetid: d54a1f5e-af68-497e-99be-97775769a7a7
author: coreyp-at-msft
ms.author: coreyp
manager: dansimp
ms.openlocfilehash: 510ad2cb1449f161658684eeceec4dbbb7ce6699
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857098"
---
# <a name="remote-server-administration-tools"></a>Herramientas de administración remota del servidor

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema proporciona ayuda sobre las Herramientas de administración remota del servidor para Windows 10.

> [!IMPORTANT]
> A partir de la Actualización de octubre de 2018 de Windows 10, RSAT se incluye como un conjunto de **características a petición** en el propio sistema operativo Windows 10. Consulta **Cuándo usar cada versión de RSAT** a continuación para ver las instrucciones de instalación.

RSAT permite a los administradores de TI administrar roles y características de Windows Server desde un PC Windows 10.

La característica Herramientas de administración remota del servidor incluye el Administrador del servidor, los complementos de Microsoft Management Console (MMC), las consolas, los proveedores y los cmdlets de Windows PowerShell, y algunas herramientas de línea de comandos para administrar roles y características que se ejecutan en Windows Server.

Herramientas de administración remota del servidor incluye módulos de cmdlets de Windows PowerShell que se pueden usar para administrar roles y características que se ejecutan en servidores remotos. Aunque la administración remota de Windows PowerShell está habilitada de forma predeterminada en Windows Server 2016, no lo está en Windows 10. Para ejecutar cmdlets que forman parte de Herramientas de administración remota del servidor en un servidor remoto, ejecuta `Enable-PSremoting` en una sesión de Windows PowerShell que se haya abierto con derechos de usuario con privilegios elevados (es decir, Ejecutar como administrador) en el equipo cliente Windows después de instalar Herramientas de administración remota del servidor.

## <a name="remote-server-administration-tools-for-windows-10"></a><a name="BKMK_Thresh"></a>Herramientas de administración remota del servidor para Windows 10
Usa Herramientas de administración remota del servidor para Windows 10 para administrar tecnologías específicas en equipos que ejecutan Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y, en casos limitados, Windows Server 2012 o Windows Server 2008 R2.

Herramientas de administración remota del servidor para Windows 10 incluye compatibilidad con la administración remota de equipos que ejecutan la opción de instalación Server Core o la configuración de interfaz de servidor básica de Windows Server 2016, Windows Server 2012 R2 y, en casos limitados, las opciones de instalación Server Core de Windows Server 2012. No obstante, Herramientas de administración remota del servidor para Windows 10 no puede instalarse en ninguna versión del sistema operativo Windows Server.

### <a name="tools-available-in-this-release"></a>Herramientas disponibles en esta versión
Para obtener una lista de las herramientas disponibles en Herramientas de administración remota del servidor para Windows 10, consulta la tabla que aparece en el tema sobre [Herramientas de administración remota del servidor (RSAT) para sistemas operativos Windows](https://support.microsoft.com/help/2693643/remote-server-administration-tools-rsat-for-windows-operating-systems).

### <a name="system-requirements"></a>Requisitos del sistema
Herramientas de administración remota del servidor para Windows 10 solo se puede instalar en equipos que ejecutan Windows 10. Herramientas de administración remota del servidor no se puede instalar en equipos que ejecutan Windows RT 8.1 u otros dispositivos de sistema en un chip.

Herramientas de administración remota del servidor para Windows 10 se ejecuta en ediciones basadas en x86 y x64 de Windows 10.

> [!IMPORTANT]
> Herramientas de administración remota del servidor para Windows 10 no debe instalarse en un equipo que ejecute paquetes de herramientas de administración para Windows 8.1, Windows 8, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 o Windows 2000 Server. Quita todas las versiones anteriores del Paquete de herramientas de administración o de Herramientas de administración remota del servidor (incluidas las versiones preliminares anteriores y las de las herramientas para diferentes idiomas o configuraciones regionales) del equipo antes de instalar Herramientas de administración remota del servidor para Windows 10.

Para usar esta versión del Administrador del servidor para tener acceso a los servidores remotos que ejecutan Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2, así como administrarlos, debes instalar varias actualizaciones para hacer que los sistemas operativos anteriores de Windows Server sean fáciles de administrar mediante el Administrador del servidor. Para obtener información detallada sobre cómo preparar Windows Server 2012 R2, Windows Server 2012 y Windows Server 2008 R2 para la administración con el Administrador del servidor en Herramientas de administración remota del servidor para Windows 10, consulta [Administración de varios servidores remotos con el Administrador del servidor](https://technet.microsoft.com/library/hh831456.aspx).
        
La administración remota de Windows PowerShell y el Administrador del servidor debe habilitarse en los servidores remotos a fin de administrarlos mediante las herramientas que forman parte de Herramientas de administración remota del servidor para Windows 10. La administración remota está habilitada de forma predeterminada en los servidores que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012. Para obtener más información sobre cómo deshabilitar la administración remota, vea [Administración de varios servidores remotos con el Administrador del servidor](https://go.microsoft.com/fwlink/p/?LinkId=241358).
        
## <a name="install-uninstall-and-turn-offon-rsat-tools"></a>Instalación, desinstalación y desactivación o activación de las herramientas de RSAT        

### <a name="use-features-on-demand-fod-to-install-specific-rsat-tools-on-windows-10-october-2018-update-----r-l----ter"></a>Uso de características a petición (FoD) para instalar herramientas de RSAT específicas en la Actualización de octubre de 2018 de Windows 10 o posterior

A partir de la Actualización de octubre de 2018 de Windows 10, RSAT se incluye como un conjunto de **características a petición** directamente en Windows 10. Ahora, en lugar de descargar un paquete de RSAT, solo tienes que ir a **Administrar características opcionales** en **Configuración** y hacer clic en **Agregar una característica** para ver la lista de herramientas de RSAT disponibles. Selecciona e instala las herramientas de RSAT específicas que necesites. Para ver el progreso de la instalación, haz clic en el botón **Atrás** para ver el estado en la página **Administrar características opcionales**.
        
Consulta la [lista de herramientas de RSAT disponibles a través de **características a petición**](https://docs.microsoft.co    /wi    dows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Además de la instalación a través de la aplicación gráfica **Configuración**, también puedes instalar herramientas de RSAT específicas mediante la línea de comandos o la automatización con [**DISM /Add-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

Una ventaja de las características a petición es que las características instaladas se mantienen entre las actualizaciones de la versión de Windows 10.        
        
#### <a name="to-uninstall-specific-rsat-tools-on-windows-10-october-2018-update-or-later-after-installing-with-fod"></a>Para desinstalar herramientas de RSAT específicas en la Actualización de octubre de 2018 de Windows 10 o posterior (después de instalarlas con FoD)        

En Windows 10, abre la aplicación **Configuración**, ve a **Administrar características opcionales**, y selecciona y desinstala las herramientas de RSAT específicas que quieras quitar. Ten en cuenta que, en algunos casos, tendrás que desinstalar las dependencias manualmente. En concreto, si la herramienta de RSAT A es necesaria para la herramienta de RSAT B, al elegir la opción para desinstalar la herramienta de RSAT A, se producirá un error si la herramienta de RSAT B todavía está instalada. En este caso, desinstala primero la herramienta de RSAT B y, a continuación, desinstala la herramienta de RSAT A. Ten en cuenta también que, en algunos casos, puede parecer que la desinstalación de una herramienta de RSAT se realiza correctamente aunque la herramienta todavía esté instalada. En este caso, al reiniciar el equipo se completará la eliminación de la herramienta.

Consulta la [lista de herramientas de RSAT que incluyen dependencias](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-non-language-fod#remote-server-administration-tools-rsat). Además de la desinstalación a través de la aplicación gráfica Configuración, también puedes desinstalar herramientas de RSAT específicas mediante la línea de comandos o la automatización con [**DISM /Remove-Capability**](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities#using-dism-add-capability-to-add-or-remove-fods).

### <a name="when-to-use-which-rsat-version"></a>Cuándo usar cada versión de RSAT

Si tienes una versión de Windows 10 anterior a la Actualización de octubre de 2018 (1809), no podrás usar las **características a petición**. Deberás descargar e instalar el paquete de RSAT.

- **Instala las características a petición de RSAT directamente desde Windows 10, tal como se describe anteriormente**: al instalarlas en la Actualización de octubre de 2018 de Windows 10 (1809) o posterior, para administrar Windows Server 2019 o versiones anteriores.

- **Descarga e instala el paquete de RSAT WS_1803, tal y como se describe a continuación**: al instalarlo en la Actualización de abril de 2018 de Windows 10 (1803) o anterior, para administrar la versión 1803 de Windows Server o la versión 1709 de Windows Server.

- **Descarga e instala el paquete de RSAT WS2016, tal y como se describe a continuación**: al instalarlo en la Actualización de abril de 2018 de Windows 10 (1803) o anterior, para administrar Windows Server 2016 o versiones anteriores.

#### <a name="download-the-rsat-package-to-install-remote-server-administration-tools-for-windows-10"></a><a name="BKMK_installthresh"></a>Descarga del paquete de RSAT para instalar Herramientas de administración remota del servidor para Windows 10

1.  Descarga el paquete Herramientas de administración remota del servidor para Windows 10 desde el [Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?LinkID=404281). Puede ejecutar el instalador desde el sitio web del Centro de descarga o guardar el paquete de descarga en un recurso compartido o equipo local.

    > [!IMPORTANT]
    > La característica Herramientas de administración remota del servidor para Windows 10 solo se puede instalar en equipos que ejecutan Windows 10. Herramientas de administración remota del servidor no se puede instalar en equipos que ejecutan Windows RT 8.1 u otros dispositivos de sistema en un chip.

2.  Si guarda el paquete de descarga en un recurso compartido o equipo local, haga doble clic en el programa de instalación, **WindowsTH-KB2693643-x64.msu** o **WindowsTH-KB2693643-x86.msu**, en función de la arquitectura del equipo en el que desea instalar las herramientas.

3.  Cuando aparezca el cuadro de diálogo **Instalador independiente de Windows Update** y le pregunte si desea instalar la actualización, haga clic en **Sí**.

4.  Lea los términos de licencia y acéptelos. Haga clic en **Acepto**.

5.  La instalación tardará unos minutos en completarse.    
        
##### <a name="to-uninstall-remote-server-administration-tools-for-windows-10-----aft----r-rsat-package-install"></a>Para desinstalar Herramientas de administración remota del servidor para Windows 10 (después de instalar el paquete de RSAT)
        
1. En el escritorio, haga clic en **Inicio**, **Todas las aplicaciones**, **Sistema de Windows**y **Panel de control**.

2. En **Programas**, haga clic en **Desinstalar un programa**.

3. Haga clic en **Ver actualizaciones instaladas**.

4. Haga clic con el botón secundario en **Actualización para Microsoft Windows (KB2693643)** y, a continuación, haga clic en **Desinstalar**.

5. Cuando se le pregunte si está seguro de que desea desinstalar la actualización, haga clic en **Sí**.
   S
   ##### <a name="to-turn----off-specific-tools-after-rsat-package-in----tall"></a>Para desactivar herramientas específicas (después de instalar el paquete de RSAT)
        
6. En el escritorio, haga clic en **Inicio**, **Todas las aplicaciones**, **Sistema de Windows** y, a continuación, en **Panel de control**.

7. Haga clic en **Programas**y, a continuación, en **Programas y características** ; haga clic en **Activar o desactivar las características de Windows**.

8. En el cuadro de diálogo **Características de Windows** , expanda **Herramientas de administración remota del servidor**y, a continuación, expanda **Herramientas de administración de roles** o **Herramientas de administración de características**.

9. Desactive las casillas de las herramientas que desee desactivar.

   > [!NOTE]
   > Si desactivas el Administrador del servidor, el equipo deberá reiniciarse, y las herramientas a las que se obtenía acceso desde el menú **Herramientas** del Administrador del servidor deberán abrirse desde la carpeta **Herramientas administrativas**.
        
10. Cuando termine de desactivar las herramientas que no desea usar, haga clic en **Aceptar**.

### <a name="run-remote-server-administration-tools"></a>Ejecutar Herramientas de administración remota del servidor

> [!NOTE]
> Después de instalar Herramientas de administración remota del servidor para Windows 10, la carpeta **Herramientas administrativas** se muestra en el menú **Inicio**. Puede tener acceso a las herramientas desde las ubicaciones siguientes.
>
> -   El menú **Herramientas** de la consola del Administrador del servidor.
> -   **Panel de control/Sistema y seguridad/Herramientas administrativas**.
> -   Un acceso directo que se guarda en el escritorio de la carpeta **Herramientas administrativas** (para ello, haga clic con el vínculo **Panel de control\Sistema y herramientas de Security\Herramientas administrativas** y luego haga clic en **Crear acceso directo**).

Las herramientas instaladas como parte de Herramientas de administración remota del servidor para Windows 10 no se pueden usar para administrar el equipo cliente local. Independientemente de la herramienta que ejecutes, debes especificar uno o varios servidores remotos en los que se ejecutará la herramienta. Puesto que la mayoría de las herramientas están integradas con el Administrador del servidor, debes agregar los servidores remotos que quieras administrar al grupo de servidores del Administrador del servidor para poder administrarlos mediante las herramientas del menú **Herramientas**. Para obtener más información sobre cómo agregar servidores al grupo de servidores y crear grupos de servidores personalizados, vea el tema sobre la [adición de servidores al Administrador del servidor](https://go.microsoft.com/fwlink/p/?LinkId=241353) y la [creación y administración de grupos de servidores](https://go.microsoft.com/fwlink/?LinkId=247328).

En Herramientas de administración remota del servidor para Windows 10, se obtiene acceso a todas las herramientas de administración del servidor basadas en GUI, como cuadros de diálogo y complementos de MMC, desde el menú **Herramientas** de la consola del Administrador del servidor. Aunque el equipo que ejecuta Herramientas de administración remota del servidor para Windows 10 ejecute un sistema operativo basado en cliente, después de instalar las herramientas, el Administrador del servidor (incluido con Herramientas de administración remota del servidor para Windows 10) se abrirá automáticamente de manera predeterminada en el equipo cliente. Ten en cuenta que no se ejecuta ninguna página **Servidor local** de la consola del Administrador del servidor en el equipo cliente.

##### <a name="to-start-server-manager-on-a-clien-----co----puter"></a>Para iniciar el Administrador del servidor en un equipo cliente

1.  En el menú **Inicio** , haga clic en **Todas las aplicaciones**y en **Herramientas administrativas**.

2.  En la carpeta **Herramientas administrativas** , haga clic en **Administrador del servidor**.

Aunque no aparezcan enumerados en el menú **Herramientas** de la consola del Administrador del servidor, las herramientas de administración del símbolo del sistema y los cmdlets de Windows PowerShell también se instalan para roles y características como parte de Herramientas de administración remota del servidor. Por ejemplo, si abres una sesión de Windows PowerShell con derechos de usuario con privilegios elevados (Ejecutar como administrador) y ejecutas el cmdlet `Get-Command -Module RDManagement`, los resultados incluyen una lista de cmdlets de Servicios de Escritorio remoto que están disponibles para ejecutarse en el equipo local después de instalar Herramientas de administración remota del servidor, siempre y cuando los cmdlets estén destinados a un servidor remoto que ejecute todo el rol Servicios de Escritorio remoto, o parte de él.

##### <a name="to-start-windows-powershell-with-elevated-user-rights-run-as-administrator"></a>Para iniciar Windows PowerShell con permisos del usuario elevados (Ejecutar como administrador)

1.  En el menú **Inicio** , haga clic en **Todas las aplicaciones**, **Sistema de Windows**y **Windows PowerShell**.

2.  Para ejecutar Windows PowerShell como administrador desde el escritorio, haz clic con el botón derecho en el acceso directo de **Windows PowerShell** y, luego, haz clic en **Ejecutar como administrador**.

> [!NOTE]
> También puedes iniciar una sesión de Windows PowerShell destinada a un servidor específico. Para ello, haz clic con el botón derecho en un servidor administrado en una página de grupo o rol del Administrador del servidor y, a continuación, haz clic en **Windows PowerShell**.
        

## <a name="known-issues"></a>Problemas conocidos

### <a name="issue-rsat-fod-installation-fails-with-error-code-0x800f0954"></a>**Problema**: error de instalación de una característica a petición de RSAT con el código de error 0x800f0954

> **Impacto**: la característica a petición de RSAT en Windows 10 1809 (Actualización de octubre de 2018) en entornos de WSUS/Configuration Manager
> 
> **Solución**: para instalar características a petición en un equipo unido a un dominio que recibe actualizaciones a través de WSUS o Configuration Manager, deberás cambiar una configuración de directiva de grupo a fin de habilitar la descarga de las características a petición de RSAT directamente desde Windows Update o un recurso compartido local. Para obtener más información e instrucciones sobre cómo cambiar esta configuración, consulta [Cómo hacer que las características a petición y los paquetes de idioma estén disponibles cuando usas WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).

---

### <a name="issue-rsat-fod-installation-via-settings-app-does-not-show-statusprogress"></a>**Problema**: la instalación de una característica a petición de RSAT a través de la aplicación Configuración no muestra el estado o el progreso.

> **Impacto**: la característica a petición de RSAT en Windows 10 1809 (Actualización de octubre de 2018)
> 
> **Solución**: para ver el progreso de la instalación, haz clic en el botón **Atrás** a fin de ver el estado en la página **Administrar características opcionales**.

---

### <a name="issue-rsat-fod-uninstallation-via-settings-app-may-fail"></a>**Problema**: a veces no se puede realizar la desinstalación de la característica a petición de RSAT a través de la aplicación Configuración.

> **Impacto**: la característica a petición de RSAT en Windows 10 1809 (Actualización de octubre de 2018)
> 
> **Solución**: en algunos casos, los errores en la desinstalación se deben a la necesidad de desinstalar manualmente las dependencias. En concreto, si la herramienta de RSAT B requiere la herramienta de RSAT A, al elegir la opción para desinstalar la herramienta de RSAT A, se producirá un error si la herramienta de RSAT B todavía está instalada. En este caso, desinstala primero la herramienta de RSAT B y, a continuación, desinstala la herramienta de RSAT A. Consulta la lista de características a petición de RSAT, incluidas las dependencias.

---

### <a name="issue-rsat-fod-uninstallation-appears-to-succeed-but-the-tool-is-still-installed"></a>**Problema**: la desinstalación de la característica a petición de RSAT parece haberse realizado correctamente, pero la herramienta todavía está instalada.

> **Impacto**: la característica a petición de RSAT en Windows 10 1809 (Actualización de octubre de 2018)
> 
> **Solución**: al reiniciar el equipo se completará la eliminación de la herramienta.

---

### <a name="issue-rsat-missing-after-windows-10-upgrade"></a>**Problema**: falta RSAT después de la actualización de Windows 10.

> **Impacto**: cualquier instalación de un paquete .MSU de RSAT (anterior a las características a petición de RSAT) no se vuelve a instalar automáticamente.
> 
> **Solución**: una instalación de RSAT no puede persistir entre las actualizaciones del sistema operativo debido a que el paquete .MSU de RSAT se entrega como un paquete de Windows Update. Vuelve a instalar RSAT después de actualizar Windows 10. Ten en cuenta que esta limitación es uno de los motivos por los que hemos pasado a usar las características a petición a partir de Windows 10 1809. Las características a petición de RSAT instaladas se conservarán en futuras actualizaciones de versión de Windows 10.

## <a name="see-also"></a>Consulte también
>- [Herramientas de administración remota del servidor para Windows 10](https://go.microsoft.com/fwlink/?LinkID=404281)
>- [Herramientas de administración remota del servidor (RSAT) para Windows Vista, Windows 7, Windows 8, Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012 y Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?LinkID=221055) '                                                                                                                                                                                                                                                                                                                                                                                    