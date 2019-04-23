---
title: Crear un DVD de recuperación del servidor para compatibilidad con varios idiomas
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c7da0f6c-9732-4784-9c28-7dad72c4071d
4author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ac547f97b48e4cd0ebf87e0935cadc2c539b4d0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855006"
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Crear un DVD de recuperación del servidor para compatibilidad con varios idiomas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a> Crear un programa de instalación de servidor y el DVD de recuperación del servidor para compatibilidad con varios idiomas en servidores administrados localmente  
  
> [!NOTE]
>  En primer lugar debe crear una imagen de Windows multilingüe tal y como se describe en el [Tutorial: Creación de imágenes de Windows multilingües](https://technet.microsoft.com/library/jj126995) antes de agregar el paquete de idioma de Windows Server Essentials a install.wim.  
  
 La instalación consta de dos fases: el entorno de preinstalación de Windows (Windows PE) y la configuración inicial. De forma predeterminada, la página de selección de idioma de la configuración inicial no se mostrará.  
  
-   Para una instalación administrada de forma remota por un OEM o un escenario de preinstalación de OEM, se deberá añadir una clave de registro con el comando siguiente para que se muestre la página de selección de idioma en la configuración inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Cuando los OEM crean una imagen en el laboratorio, deben elegir el idioma **Inglés** durante la fase de configuración de Windows PE.  
  
-   En un escenario del Kit de opciones de distribuidor (ROK), los clientes reciben un DVD y, quizás, hardware. El cliente debe poder seleccionar el idioma durante la instalación de Windows PE y la página de selección de idioma no se vuelve a mostrar durante la configuración inicial.  
  
 Puede optar por enviar un único DVD de doble capa que contenga varios idiomas.  
  
 Esta sección describe cómo agregar compatibilidad con idioma al programa de instalación de Windows. La herramienta principal para personalizar Windows PE 3.0 es Administración y mantenimiento de imágenes de implementación (DISM), una herramienta de línea de comandos. Esta solución habilita los siguientes escenarios:  
  
1.  Creación de instalaciones multilingües  
  
2.  Creación de medios distribuibles  
  
### <a name="prerequisites"></a>Requisitos previos  
 Para agregar compatibilidad multilingüe al programa de instalación de Windows, necesita lo siguiente:  
  

-   El equipo del técnico con todas las herramientas y los archivos de origen necesarios para crear una imagen personalizada de WinPE. Para obtener más información, consulte [Prepare the Technician Computer](Prepare-the-Technician-Computer.md).  

-   El equipo del técnico con todas las herramientas y los archivos de origen necesarios para crear una imagen personalizada de WinPE. Para obtener más información, consulte [Prepare the Technician Computer](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD de Windows Server Essentials.  
  
-   Un Windows Server Essentials paquete de idioma de DVD.  
  
###  <a name="BKMK_Steps"></a> Agregar compatibilidad con varios idiomas  
 Para agregar compatibilidad con varios idiomas al programa de instalación de Windows, actualice Install.wim mediante la adición de Windows Server 2012 y el idioma de Windows Server Essentials Pack a él.  
  
#### <a name="update-installwim"></a>Actualizar Install.wim  
 En este paso, agregue Windows Server 2012 y paquetes de idioma de Windows Server Essentials a Install.wim.  
  
> [!NOTE]
>  Compruebe que está instalando paquetes de idioma para Windows Server 2012. Esto garantiza la personalización de marca adecuada. Están disponibles en el Windows Server 2012 Multilingual User Interface Language Packs [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Siga las instrucciones tal como se describe en el [Tutorial: Creación de imágenes de Windows multilingüe sobre la creación de un multilingüe](https://technet.microsoft.com/library/jj126995.aspx) sobre la creación de una imagen de Windows multilingüe antes de agregar el paquete de idioma de Windows Server Essentials a install.wim.  
>   
>  Paquetes de idioma de Windows Server Essentials están disponibles en el medio del paquete de idioma en módulos \Language\\< CultureName\>.  
  
> [!NOTE]
>  Es posible que no todos los paquetes de idioma no está disponible antes del lanzamiento de Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Para agregar paquetes de idioma para Install.wim  
  
1.  Agregue los paquetes de idiomas del sistema operativo y del producto a Install.wim tal como se indica a continuación (en este ejemplo se usa el francés):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Agregue archivos específicos de idiomas para permitir la creación de USB de restauración de copias de seguridad de cliente unidad flash mediante el procedimiento descrito en [medios de restauración de cliente de compilación multilingüe](Build-Multi-Language-Client-Restore-Media.md).  

2.  Agregue archivos específicos de idiomas para permitir la creación de USB de restauración de copias de seguridad de cliente unidad flash mediante el procedimiento descrito en [medios de restauración de cliente de compilación multilingüe](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Vuelva a crear el archivo Lang.ini en el soporte flexible para reflejar la compatibilidad con idiomas adicionales mediante el comando `DISM /Gen-LangINI` , por ejemplo:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Vuelva a guardar los cambios en la imagen con el comando `DISM /unmount /commit` , por ejemplo:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
    ```  
  
## <a name="see-also"></a>Vea también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparar la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

