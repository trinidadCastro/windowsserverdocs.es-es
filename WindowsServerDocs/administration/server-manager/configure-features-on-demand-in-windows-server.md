---
title: Configure Features on Demand in Windows Server
description: Obtenga información sobre cómo quitar archivos de características en una configuración de características a petición mediante el cmdlet Uninstall-WindowsFeature.
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8a2737aec1729bc30bba571a222cc659ff04d8af
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965100"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configure Features on Demand in Windows Server

>Se aplica a: Windows Server 2016

En este tema se describe cómo quitar archivos de características en una configuración de Características a petición mediante el cmdlet Uninstall-WindowsFeature.

Características a petición es una característica, introducida en Windows 8 y Windows Server 2012, que permite quitar archivos de roles y características (a veces denominados *carga* de características) del sistema operativo para conservar espacio en disco e instalar roles y características desde ubicaciones remotas o medios de instalación en lugar de desde equipos locales. Puede quitar archivos de características de equipos físicos o virtuales en ejecución. También puede agregar o quitar archivos de características desde archivos de imagen de Windows (WIM) o discos duros virtuales (VHD) sin conexión para crear una copia reproducible de las configuraciones de Características a petición.

En una configuración de características a petición, cuando los archivos de características no están disponibles en un equipo, si una instalación requiere esos archivos de características, se puede dirigir a Windows Server 2012 R2 o Windows Server 2012 para obtener los archivos de un almacén de características en paralelo (una carpeta compartida que contiene archivos de características y que está disponible para el equipo en la red), desde Windows Update o desde medios de instalación. De forma predeterminada, cuando los archivos de características no están disponibles en el servidor de destino, Características a petición busca los archivos de características que faltan realizando las tareas siguientes, en el orden mostrado.

1.  Buscar en una ubicación especificada por los usuarios del Asistente para agregar roles y características o comandos de instalación de DISM

2.  Evaluación de la configuración del directiva de grupo configuración, **configuración del Equipo\plantillas administrativas\sistema\especificar configuración de instalación de componentes opcionales y reparación de componentes**

3.  Buscar en Windows Update

Puede invalidar el comportamiento predeterminado de Características a petición si realiza cualquiera de las siguientes acciones.

-   Especificar una ruta de acceso de origen alternativa como parte del cmdlet `Install-WindowsFeature` mediante la adición del parámetro `Source`

-   Especificar una ruta de acceso de origen alternativa en la página **confirmar opciones de instalación** mientras se instalan las características mediante el Asistente para agregar roles y características

-   Configurar la directiva de grupo **Especificar configuración de instalación de componentes opcionales y reparación de componentes**

En este tema se incluyen las siguientes secciones.

-   [Crear un archivo de características o un almacén colateral](#BKMK_store)

-   [Métodos para quitar archivos de características](#BKMK_methods)

-   [Quitar archivos de características mediante Uninstall-WindowsFeature](#BKMK_remove)

## <a name="create-a-feature-file-or-side-by-side-store"></a><a name=BKMK_store></a>Crear un archivo de características o un almacén colateral
En esta sección se describe cómo configurar una carpeta compartida de archivos de características remota (también denominada tienda en paralelo) que almacena los archivos necesarios para instalar roles, servicios de rol y características en servidores que ejecutan Windows Server 2012 R2 o Windows Server 2012. Después de configurar un almacén de características, puede instalar roles, servicios de rol y características en los servidores que ejecutan esos sistemas operativos y especificar el almacén de características como la ubicación de los archivos de origen de instalación.

#### <a name="to-create-a-feature-file-store"></a>Para crear un almacén de archivos de características

1.  Cree una carpeta compartida en un servidor de la red. Por ejemplo, *\\ \network\share\sxs*.

2.  Compruebe que tiene los permisos correctos asignados al almacén de características. La ruta de acceso de origen o el recurso compartido de archivos deben conceder permisos de **lectura** al grupo **todos** (no se recomienda por motivos de seguridad) o a las cuentas de equipo (*dominio* \\ *SERverNAME*$) de los servidores en los que tiene previsto instalar características mediante este almacén de características; la concesión de acceso a la cuenta de usuario no es suficiente.

    Para obtener acceso a la configuración de permisos y uso compartido de archivos, realice cualquiera de las siguientes acciones en el escritorio de Windows.

    -   Haga clic con el botón secundario en la carpeta compartida, haga clic en **Propiedades** y, a continuación, cambie los usuarios permitidos y sus derechos de acceso para la carpeta en la pestaña **Seguridad**.

    -   Haga clic con el botón secundario en la carpeta compartida, elija **Compartir con** y, a continuación, haga clic en **Usuarios específicos**.

    > [!NOTE]
    > Los servidores que formen parte de grupos de trabajo no pueden obtener acceso a recursos compartidos de archivos externos, aunque la cuenta de equipo del servidor del grupo de trabajo tenga el permiso **Lectura** en el recurso compartido externo. Hay ubicaciones de origen alternativas que funcionan para servidores del grupo de trabajo, como medios de instalación, Windows Update y archivos VHD o WIM almacenados en el servidor del grupo de trabajo local.

3.  Copie la carpeta **Sources\SxS** de los medios de instalación de Windows Server en la carpeta compartida que creó en el paso 1.

## <a name="methods-of-removing-feature-files"></a><a name=BKMK_methods></a>Métodos para quitar archivos de características
Dispone de dos métodos para quitar archivos de características de Windows Server en una configuración de Características a petición.

-   El `remove` parámetro del `Uninstall-WindowsFeature` cmdlet permite eliminar archivos de características de un servidor o un disco duro virtual (VHD) sin conexión que ejecute windows Server 2012 R2 o windows Server 2012. Los valores válidos para el `remove` parámetro son los nombres de los roles, servicios de rol y características.

-   Los comandos de Administración y mantenimiento de imágenes de implementación (DISM) permiten crear archivos WIM personalizados que conservan espacio en disco mediante la omisión de archivos de características que no son necesarios o que se pueden obtener de otros orígenes remotos. Para obtener más información acerca del uso de DISM para preparar imágenes personalizadas, vea [Cómo habilitar o deshabilitar características de Windows](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824822(v=win.10)).

## <a name="remove-feature-files-by-using-uninstall-windowsfeature"></a><a name=BKMK_remove></a>Quitar archivos de características mediante Uninstall-WindowsFeature
Puede usar el cmdlet Uninstall-WindowsFeature para desinstalar roles, servicios de rol y características de servidores y VHD sin conexión que ejecutan Windows Server 2012 R2 o Windows Server 2012, y para eliminar archivos de características. Puede desinstalar y eliminar los mismos roles, servicios de rol y características en el mismo comando si lo desea.

> [!IMPORTANT]
> Al eliminar los archivos de características de un rol, servicio de función o característica, roles, servicios de función y características que dependen de los archivos que se están quitando también se eliminan. Si elimina archivos de características de un servicio de rol o característica secundaria y no permanecen instalados otros servicios de rol o características secundarias para el rol o característica primario, se eliminan los archivos de todo el rol o característica primario. Para ver todos los archivos de características que eliminaría el comando `Uninstall-WindowsFeature -remove`, agregue el parámetro `whatif` al comando para ejecutarlo y ver los resultados sin eliminar los archivos de características.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Para quitar archivos de roles y características mediante Uninstall-WindowsFeature

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    > [!NOTE]
    > Si está desinstalando roles y características de un servidor remoto, no necesita ejecutar Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.

    -   En la pantalla **Inicio** de Windows, haga clic con el botón derecho en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **Ejecutar como administrador**.

    -   En un servidor que ejecute la opción de instalación Server Core, escriba **PowerShell** en un símbolo del sistema y, a continuación, presione **entrar**.

2.  Escriba lo siguiente y, a continuación, presione **Entrar**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Ejemplo:** Administración de licencias de Escritorio remoto es el único servicio de rol de Servicios de Escritorio remoto que queda instalado. El comando desinstala Administración de licencias de Escritorio remoto y después elimina los archivos de características de todo el rol Servicios de Escritorio remoto del servidor especificado, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Ejemplo:** En el ejemplo siguiente, el comando quita los servicios de dominio de Active Directory y la administración de directiva de grupo de un VHD sin conexión. Primero se desinstala el rol y la característica y después sus archivos de características se quitan totalmente del VHD sin conexión, *Contoso.vhd*.

    > [!NOTE]
    > Debe agregar el `computerName` parámetro si está ejecutando el cmdlet desde un equipo que ejecuta Windows 8.1 o Windows 8.
    >
    > Si escribe el nombre de un archivo VHD de un recurso compartido de red, ese recurso compartido debe conceder permisos de **lectura** y **escritura** a la cuenta de equipo del servidor que ha seleccionado para montar el VHD. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Consulte también
[Instalar o desinstalar roles, servicios de rol o características](install-or-uninstall-roles-role-services-or-features.md) 
 Opciones de instalación de [Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831786(v=ws.11)) 
 [Cómo habilitar o deshabilitar características](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824822(v=win.10)) 
 de Windows [Información general de administración y mantenimiento de imágenes de implementación (DISM)](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825236(v=win.10))