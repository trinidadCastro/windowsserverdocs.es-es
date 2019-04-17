---
title: "Crear un DVD de recuperación del servidor para la compatibilidad con múltiples idioma"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-server-recovery-dvd-for-multi-language-support"></a>Crear un DVD de recuperación del servidor para la compatibilidad con múltiples idioma

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_MLHeadedRecovery"></a>Crear una instalación del servidor y el DVD de recuperación de servidor para la compatibilidad con varios idiomas en servidores administrados de forma local  
  
> [!NOTE]
>  Primero debes crear una imagen de Windows multilingüe como se describe en la [Tutorial: creación de imágenes de Windows multilingües](https://technet.microsoft.com/library/jj126995) antes de agregar el paquete de idioma de Windows Server Essentials en install.wim.  
  
 Hay dos fases de configuración: el entorno de preinstalación de Windows (Windows PE) y la configuración inicial. De manera predeterminada, no se mostrará la página de selección de idioma en la configuración inicial.  
  
-   Para una instalación de OEM que administrados de forma remota o en un escenario de preinstalación de OEM, deberás agregar un registro clave con el siguiente comando para mostrar la página de selección de idioma en la configuración inicial.  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v ShowPreinstallPages /t REG_SZ /d true /f  
    ```  
  
    > [!IMPORTANT]
    >  Cuando los OEM crean una imagen en el laboratorio, deben elegir **inglés** como el idioma durante la fase de Windows PE de instalación.  
  
-   Para un escenario Kit para la opción de revendedor (ROK), los clientes reciben un DVD y quizás, cierto hardware. El cliente debe seleccionar el idioma durante la instalación de Windows PE y ya no se muestra la página de selección de idioma durante la configuración inicial.  
  
 Puedes enviar un único DVD de doble capa que contenga varios idiomas.  
  
 En esta sección se describe cómo agregar compatibilidad de idioma al programa de instalación de Windows. La herramienta principal para personalizar Windows PE 3.0 es el mantenimiento de imágenes de implementación y administración (DISM), una herramienta de línea de comandos. Esta solución permite los siguientes escenarios:  
  
1.  Crear instalaciones multilingües  
  
2.  Creación de medios distribuibles  
  
### <a name="prerequisites"></a>Requisitos previos  
 Para agregar compatibilidad multilingüe al programa de instalación de Windows, necesitas lo siguiente:  
  

-   Un equipo del técnico que proporciona todas las herramientas y los archivos de origen necesarios para crear una imagen de WinPE personalizada. Para obtener más información, consulta [preparar el equipo del técnico](Prepare-the-Technician-Computer.md).  

-   Un equipo del técnico que proporciona todas las herramientas y los archivos de origen necesarios para crear una imagen de WinPE personalizada. Para obtener más información, consulta [preparar el equipo del técnico](../install/Prepare-the-Technician-Computer.md).  

  
-   Un DVD de Essentials del servidor de Windows.  
  
-   Un Windows Server Essentials paquete de idioma DVD.  
  
###  <a name="BKMK_Steps"></a>Agregar compatibilidad con varios idiomas  
 Para agregar compatibilidad con varios idiomas al programa de instalación de Windows update Install.wim agregando el Windows Server 2012 y paquetes de idioma de Windows Server Essentials a ella.  
  
#### <a name="update-installwim"></a>Actualizar Install.wim  
 En este paso, agregarás Windows Server 2012 y paquetes de idioma de Windows Server Essentials en Install.wim.  
  
> [!NOTE]
>  Comprueba que instalación paquetes de idioma para Windows Server 2012. Esto garantiza que obtengas la personalización de marca adecuado. El Windows Server 2012 multilingües paquetes de idioma están disponibles en [Microsoft.com](https://www.microsoft.com/OEM/en/installation/downloads/Pages/technical-downloads.aspx). Sigue las instrucciones que aparecen como se describe en la [Tutorial: creación de imágenes de Windows multilingües sobre la creación de un multilingües](https://technet.microsoft.com/library/jj126995.aspx) sobre la creación de una imagen multilingüe de Windows antes de agregar el paquete de idioma de Windows Server Essentials en Install.wim.  
>   
>  Paquetes de idioma de Windows Server Essentials están disponibles en los medios de paquete de idioma en \Language Packs\\ < CultureName\ >.  
  
> [!NOTE]
>  No todos los paquetes de idioma podrán que no esté disponible antes del lanzamiento de Windows Server 2012.  
  
###### <a name="to-add-language-packs-to-installwim"></a>Para agregar paquetes de idioma a Install.wim  
  
1.  Agregar paquetes de idioma del sistema operativo y el producto en Install.wim de esta manera (en este ejemplo usa a francés):  
  
    ```  
    Dism /Mount-Wim /WimFile:C:\my_distribution\sources\install.wim /index:1 /MountDir:C:\InstallMount  
    Mkdir c:\temp_Scratch  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<path_to_OS_language_packs>\fr-fr\lp.cab  
    Dism /Image:C:\InstallMount /ScratchDir:C:\temp_Scratch /Add-Package /PackagePath:<OCD\Language Packs\FR-FR\lp.cab  
  
    ```  
  

2.  Agregar archivos específicos del idioma que admite la creación de USB de restaurar copia de seguridad de cliente unidad flash, mediante el procedimiento descrito en [medio de restauración de cliente de generar varios idiomas ](Build-Multi-Language-Client-Restore-Media.md).  

2.  Agregar archivos específicos del idioma que admite la creación de USB de restaurar copia de seguridad de cliente unidad flash, mediante el procedimiento descrito en [medio de restauración de cliente de generar varios idiomas ](../install/Build-Multi-Language-Client-Restore-Media.md).  

  
3.  Volver a crear el archivo Lang.ini en los medios dinámica para reflejar la compatibilidad de idioma adicional mediante el uso de la `DISM /Gen-LangINI`comando, por ejemplo:  
  
    ```  
    Dism /image:C:\InstallMount /Gen-LangINI /distribution:C:\my_distribution  
  
    ```  
  
4.  Guardar los cambios en la imagen mediante la `DISM /unmount /commit`comando, por ejemplo:  
  
    ```  
    Dism /Unmount-Wim /MountDir:C:\InstallMount /Commit  
    ```  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

