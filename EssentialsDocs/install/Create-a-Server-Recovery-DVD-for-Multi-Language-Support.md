---
title: Crear un DVD de recuperación del servidor para compatibilidad con varios idiomas
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
author: daveba
ms.author: daveba
ms.openlocfilehash: 3c415155734515af004e25a07c4e61afabaa3359
ms.sourcegitcommit: 04637054de2bfbac66b9c78bad7bf3e7bae5ffb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2020
ms.locfileid: "87838014"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Crear un DVD de recuperación del servidor para compatibilidad con varios idiomas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="create-a-server-setup-and-server-recovery-dvd-for-multiple-language-support-on-locally-administered-servers"></a><a name="BKMK_MLHeadedRecovery"></a>Crear un DVD de configuración del servidor y de recuperación del servidor para compatibilidad con varios idiomas en servidores administrados localmente

> [!NOTE]
>  Primero debe crear una imagen de Windows multilingüe tal y como se describe en el [Tutorial: creación de imágenes de Windows multilingüe](/previous-versions/windows/it-pro/windows-8.1-and-8/jj126995(v=win.10)) antes de agregar el paquete de idioma de Windows Server Essentials a install. Wim.

 La instalación consta de dos fases: el entorno de preinstalación de Windows (Windows PE) y la configuración inicial. De forma predeterminada, la página de selección de idioma de la configuración inicial no se mostrará.

- Para una instalación administrada de forma remota por un OEM o un escenario de preinstalación de OEM, se deberá añadir una clave de registro con el comando siguiente para que se muestre la página de selección de idioma en la configuración inicial.

  ```
  %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f
  ```

  > [!IMPORTANT]
  >  Cuando los OEM crean una imagen en el laboratorio, deben elegir el idioma **Inglés** durante la fase de configuración de Windows PE.

- En un escenario del Kit de opciones de distribuidor (ROK), los clientes reciben un DVD y, quizás, hardware. El cliente debe poder seleccionar el idioma durante la instalación de Windows PE y la página de selección de idioma no se vuelve a mostrar durante la configuración inicial.

  Puede optar por enviar un único DVD de doble capa que contenga varios idiomas.

  Esta sección describe cómo agregar compatibilidad con idioma al programa de instalación de Windows. La herramienta principal para personalizar Windows PE 3.0 es Administración y mantenimiento de imágenes de implementación (DISM), una herramienta de línea de comandos. Esta solución habilita los siguientes escenarios:

1.  Creación de instalaciones multilingües

2.  Creación de medios distribuibles

### <a name="prerequisites"></a>Requisitos previos
 Para agregar compatibilidad multilingüe al programa de instalación de Windows, necesita lo siguiente:


-   El equipo del técnico con todas las herramientas y los archivos de origen necesarios para crear una imagen personalizada de WinPE. Para obtener más información, consulte [Preparar el equipo del técnico](Prepare-the-Technician-Computer.md).

-   El equipo del técnico con todas las herramientas y los archivos de origen necesarios para crear una imagen personalizada de WinPE. Para obtener más información, consulte [Preparar el equipo del técnico](../install/Prepare-the-Technician-Computer.md).


-   Un DVD de Windows Server Essentials.

-   Un DVD de paquete de idioma de Windows Server Essentials.

###  <a name="adding-multiple-language-support"></a><a name="BKMK_Steps"></a>Incorporación de compatibilidad con varios idiomas
 Para agregar compatibilidad con varios idiomas a instalación de Windows actualice el archivo install. Wim mediante la adición de los paquetes de idioma de Windows Server 2012 y Windows Server Essentials.

#### <a name="update-installwim"></a>Actualizar Install.wim
 En este paso, agregará los paquetes de idioma de Windows Server 2012 y Windows Server Essentials a install. Wim.

> [!NOTE]
>  Compruebe que ha instalado los paquetes de idioma para Windows Server 2012. Esto garantiza la personalización de marca adecuada. Los paquetes de idioma de la interfaz de usuario multilingüe de Windows Server 2012 están disponibles en [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Siga las instrucciones que se describen en el [Tutorial: creación de imágenes de Windows multilingües sobre la creación de un multilingüe](/previous-versions/windows/it-pro/windows-8.1-and-8/jj126995(v=win.10)) en la creación de una imagen de Windows multilingüe antes de agregar el paquete de idioma de Windows Server Essentials a install. Wim.
>
>  Los paquetes de idioma de Windows Server Essentials están disponibles en el medio del paquete de idioma en \Paquetes packs \\<CultureName \> .

> [!NOTE]
>  Es posible que no todos los paquetes de idioma estén disponibles antes del lanzamiento de Windows Server 2012.

###### <a name="to-add-language-packs-to-installwim"></a>Para agregar paquetes de idioma para Install.wim

1.  Agregue los paquetes de idiomas del sistema operativo y del producto a Install.wim tal como se indica a continuación (en este ejemplo se usa el francés):

    ```
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount
    Mkdir c:\temp_Scratch
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab

    ```


2.  Agregue archivos específicos de idioma para permitir la creación de la unidad flash USB de restauración de copia de seguridad de cliente, mediante el procedimiento descrito en [creación de medios de restauración de cliente multilingüe](Build-Multi-Language-Client-Restore-Media.md).

2.  Agregue archivos específicos de idioma para permitir la creación de la unidad flash USB de restauración de copia de seguridad de cliente, mediante el procedimiento descrito en [creación de medios de restauración de cliente multilingüe](../install/Build-Multi-Language-Client-Restore-Media.md).


3.  Vuelva a crear el archivo Lang.ini en el soporte flexible para reflejar la compatibilidad con idiomas adicionales mediante el comando `DISM /Gen-LangINI` , por ejemplo:

    ```
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution

    ```

4.  Vuelva a guardar los cambios en la imagen con el comando `DISM /unmount /commit` , por ejemplo:

    ```
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit
    ```

## <a name="see-also"></a>Consulte también

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)