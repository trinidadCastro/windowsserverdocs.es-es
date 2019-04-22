---
title: Configure Features on Demand in Windows Server
description: Administrador de servidores
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e663bbea-d025-41fa-b16c-c2bff00a88e8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c77bd088a02d405b17a4f7decd6093785296d5e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818936"
---
# <a name="configure-features-on-demand-in-windows-server"></a>Configure Features on Demand in Windows Server

>Se aplica a: Windows Server 2016

En este tema se describe cómo quitar archivos de características en una configuración de Características a petición mediante el cmdlet Uninstall-WindowsFeature.

Las características a petición es una nueva característica de Windows 8 y Windows Server 2012, que le permite quitar archivos de roles y características (a veces denominados *carga*) desde el sistema operativo para ahorrar espacio en disco e instalar roles y características desde ubicaciones remotas o medios de instalación en lugar de desde equipos locales. Puede quitar archivos de características de equipos físicos o virtuales en ejecución. También puede agregar o quitar archivos de características desde archivos de imagen de Windows (WIM) o discos duros virtuales (VHD) sin conexión para crear una copia reproducible de las configuraciones de Características a petición.

En una configuración de la demanda de características, cuando los archivos de características no están disponibles en un equipo, si una instalación necesita esos archivos de características, Windows Server 2012 R2 o Windows Server 2012 se puede dirigir a obtener los archivos de un almacén de características de side-by-side (una carpeta compartida que contiene los archivos de características y está disponible en el equipo en la red), desde Windows Update o desde medios de instalación. De forma predeterminada, cuando los archivos de características no están disponibles en el servidor de destino, Características a petición busca los archivos de características que faltan realizando las tareas siguientes, en el orden mostrado.

1.  Buscar en una ubicación que se ha especificado por los usuarios de agregar Roles y características Asistente o comandos de instalación de DISM

2.  Evaluar la configuración de la configuración de directiva de grupo **del equipo\Plantillas administrativas\sistema\especificar configuración de equipo para la instalación de componentes opcionales y reparación de componentes**

3.  Buscar en Windows Update

Puede invalidar el comportamiento predeterminado de Características a petición si realiza cualquiera de las siguientes acciones.

-   Especificar una ruta de acceso de origen alternativa como parte del cmdlet `Install-WindowsFeature` mediante la adición del parámetro `Source`

-   Especificar una ruta de acceso de origen alternativa en el **Confirmar opciones de instalación** página durante la instalación de características mediante el agregar Roles y características de Asistente

-   Configurar la directiva de grupo **Especificar configuración de instalación de componentes opcionales y reparación de componentes**

En este tema se incluyen las siguientes secciones.

-   [Crear un archivo de característica o almacén en paralelo](#BKMK_store)

-   [Métodos para quitar los archivos de características](#BKMK_methods)

-   [Quitar archivos de características mediante Uninstall-WindowsFeature](#BKMK_remove)

## <a name="BKMK_store"></a>Crear un archivo de característica o almacén en paralelo
En esta sección se describe cómo configurar una carpeta de archivos compartidos de características (conocida también como almacén side-by-side) que almacena los archivos necesarios para instalar roles, servicios de rol y características en los servidores que ejecutan Windows Server 2012 R2 o Windows Server 2012. Una vez haya configurado un almacén de características, puede instalar roles, servicios de rol y características en servidores que ejecutan esos sistemas operativos y especificar el almacén de características como la ubicación de archivos de origen de instalación.

#### <a name="to-create-a-feature-file-store"></a>Para crear un almacén de archivos de características

1.  Cree una carpeta compartida en un servidor de la red. Por ejemplo,  *\\\network\share\sxs*.

2.  Compruebe que tiene los permisos correctos asignados al almacén de características. El recurso compartido de archivo o ruta de acceso de origen debe conceder **lectura** permisos a la **todo el mundo** grupo (no recomendado por motivos de seguridad), o para las cuentas de equipo (*dominio* \\ *SERverNAME*$) de servidores en el que tiene previsto instalar características mediante el uso de este almacén de características; no es suficiente conceder acceso de cuenta de usuario.

    Para obtener acceso a la configuración de permisos y uso compartido de archivos, realice cualquiera de las siguientes acciones en el escritorio de Windows.

    -   Haga clic con el botón secundario en la carpeta compartida, haga clic en **Propiedades**y, a continuación, cambie los usuarios permitidos y sus derechos de acceso para la carpeta en la pestaña **Seguridad** .

    -   Haga clic con el botón secundario en la carpeta compartida, elija **Compartir con**y, a continuación, haga clic en **Usuarios específicos**.

    > [!NOTE]
    > Los servidores que formen parte de grupos de trabajo no pueden obtener acceso a recursos compartidos de archivos externos, aunque la cuenta de equipo del servidor del grupo de trabajo tenga el permiso **Lectura** en el recurso compartido externo. Hay ubicaciones de origen alternativas que funcionan para servidores del grupo de trabajo, como medios de instalación, Windows Update y archivos VHD o WIM almacenados en el servidor del grupo de trabajo local.

3.  Copie la carpeta **Sources\SxS** de los medios de instalación de Windows Server en la carpeta compartida que creó en el paso 1.

## <a name="BKMK_methods"></a>Métodos para quitar los archivos de características
Dispone de dos métodos para quitar archivos de características de Windows Server en una configuración de Características a petición.

-   El `remove` parámetro de la `Uninstall-WindowsFeature` cmdlet le permite eliminar los archivos de características desde un servidor o sin conexión disco duro virtual (VHD) que se está ejecutando Windows Server 2012 R2 o Windows Server 2012. Los valores válidos para el `remove` parámetro son los nombres de roles, servicios de rol y características.

-   Los comandos de Administración y mantenimiento de imágenes de implementación (DISM) permiten crear archivos WIM personalizados que conservan espacio en disco mediante la omisión de archivos de características que no son necesarios o que se pueden obtener de otros orígenes remotos. Para obtener más información acerca del uso de DISM para preparar imágenes personalizadas, vea [Cómo habilitar o deshabilitar características de Windows](https://technet.microsoft.com/library/hh824822.aspx).

## <a name="BKMK_remove"></a>Quitar archivos de características mediante Uninstall-WindowsFeature
Puede usar el cmdlet Uninstall-WindowsFeature para desinstalar los roles, servicios de rol y características de los servidores y VHD sin conexión que ejecutan Windows Server 2012 R2 o Windows Server 2012 y para eliminar los archivos de características. Puede desinstalar y eliminar el mismo roles, servicios de rol y características en el mismo comando si lo desea.

> [!IMPORTANT]
> Al eliminar los archivos de características para un rol, también se eliminan el servicio de rol o características, roles, servicios de rol y características que dependen de los archivos que se va a quitar. Si elimina archivos de características de un servicio de rol o característica secundaria y no permanecen instalados otros servicios de rol o características secundarias para el rol o característica primario, se eliminan los archivos de todo el rol o característica primario. Para ver todos los archivos de características que eliminaría el comando `Uninstall-WindowsFeature -remove`, agregue el parámetro `whatif` al comando para ejecutarlo y ver los resultados sin eliminar los archivos de características.

#### <a name="to-remove-role-and-feature-files-by-using-uninstall-windowsfeature"></a>Para quitar archivos de roles y características mediante Uninstall-WindowsFeature

1.  Realice una de las siguientes acciones para abrir una sesión de Windows PowerShell con derechos de usuario elevados.

    > [!NOTE]
    > Si está desinstalando roles y características de un servidor remoto, no es necesario ejecutar Windows PowerShell con derechos de usuario elevados.

    -   En el escritorio de Windows, haga clic con el botón secundario en **Windows PowerShell** en la barra de tareas y, a continuación, haga clic en **Ejecutar como administrador**.

    -   En el Windows **iniciar** pantalla, haga clic en el icono de Windows PowerShell y, a continuación, en la barra de la aplicación, haga clic en **ejecutar como administrador**.

    -   En un servidor que ejecuta la opción de instalación Server Core, escriba **PowerShell** en un símbolo del sistema y, a continuación, presione **ENTRAR**.

2.  Escriba lo siguiente y, a continuación, presione **Entrar**.

    ```
    Uninstall-WindowsFeature -Name <feature_name> -computerName <computer_name> -remove
    ```

    **Ejemplo:** Administración de licencias de Escritorio remoto es el único servicio de rol de Servicios de Escritorio remoto que queda instalado. El comando desinstala Administración de licencias de Escritorio remoto y después elimina los archivos de características de todo el rol Servicios de Escritorio remoto del servidor especificado, *contoso_1*.

    ```
    Uninstall-WindowsFeature -Name rdS-Licensing -computerName contoso_1 -remove
    ```

    **Ejemplo:** En el ejemplo siguiente, el comando quita servicios de dominio de Active Directory y administración de directivas de grupo de un VHD sin conexión. Primero se desinstala el rol y la característica y después sus archivos de características se quitan totalmente del VHD sin conexión, *Contoso.vhd*.

    > [!NOTE]
    > Debe agregar el `computerName` parámetro si está ejecutando el cmdlet desde un equipo que se está ejecutando Windows 8.1 o Windows 8.
    > 
    > Si escribe el nombre de un archivo VHD desde un recurso compartido de red, el recurso compartido debe conceder **lectura** y **escribir** permisos para la cuenta de equipo del servidor que ha seleccionado para montar el disco duro virtual. El acceso de cuenta de solo usuario no es suficiente. El recurso compartido puede otorgar permisos de **lectura** y **escritura** a **todos** los integrantes del grupo para darles acceso al VHD, pero por razones de seguridad, no se recomienda hacer esto.

    ```
    Uninstall-WindowsFeature -Name AD-Domain-Services,GPMC -VHD C:\WS2012VHDs\Contoso.vhd -computerName ContosoDC1
    ```

## <a name="see-also"></a>Vea también
[Instalar o desinstalar Roles, servicios de rol o características](install-or-uninstall-roles-role-services-or-features.md)
[opciones de instalación de Windows Server](https://technet.microsoft.com/library/hh831786.aspx)
[cómo habilitar o deshabilitar características de Windows](https://technet.microsoft.com/library/hh824822.aspx) 
 [Implementación información general de administración (DISM) y el mantenimiento de imágenes](https://technet.microsoft.com/library/hh825236.aspx)


