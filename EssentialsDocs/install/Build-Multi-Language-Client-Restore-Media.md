---
title: "Crear el medio de restauración de cliente de varios idiomas"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fdbc016-d464-43cb-bd75-8a63e61588a2
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1ad934d297c3092050bd6adbb6bb0f50d1ec6f36
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="build-multi-language-client-restore-media"></a>Crear el medio de restauración de cliente de varios idiomas

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

> [!NOTE]
>  Primero debes crear una imagen de Windows multilingüe como se describe en la [Tutorial: creación de imágenes de Windows multilingües](https://technet.microsoft.com/library/jj126995) antes de agregar el paquete de idioma de Windows Server Essentials en install.wim.  
  
 Al crear el DVD de instalación del servidor de múltiples idiomas, los paquetes de idioma se instalará en install.wim del servidor. Como parte del paquete de idioma se instalarán los recursos localizados para el Asistente para la restauración.  
  
### <a name="to-build-a-multi-language-client-restore-media"></a>Crear el medio de restauración de un cliente de varios idioma  
  
1.  Montar install.wim en c:\mount, llamamos a la carpeta de archivos de programa\Windows Server\bin\ClientRestore c:\mount\Program como raíz del medio de restauración de cliente: [RestoreMediaRoot] a continuación:  
  
    ```  
    dism /mount-wim /wimfile:install.wim /index:1 /mountdir:c:\mount  
    ```  
  
2.  Monta el archivo WIM de restauración de cliente en [RestoreMediaRoot]\Sources\Boot.wim (los mismos pasos deben realizarse para boot_x86.wim demasiado). Es la línea de comandos:  
  
    ```  
    dism /mount-wim /wimfile:boot.wim /index:1 /mountdir:c:\mountRestore  
    ```  
  
3.  Agrega paquete WinPE-Setup.cab el medio de restauración, ejecutando:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:WinPE-Setup.cab  
    ```  
  
4.  Usa el Bloc de notas para editar c:\mountRestore\windows\system32\winpeshl.ini, rellenar con el siguiente contenido:  
  
    ```  
    [LaunchApp]  
    AppPath = %SYSTEMDRIVE%\sources\SelectLanguage.exe  
    ```  
  
5.  Agregar paquetes de idioma para el medio de restauración. Agregar paquetes de idioma puede realizarse mediante ejecutando el siguiente comando:  
  
    ```  
    dism /image:c:\mountRestore /add-package /packagepath:[language pack path]  
    ```  
  
     Deben agregarse siguientes paquetes de idioma:  
  
    1.  Paquete de idioma (lp.cab) de WinPE  
  
    2.  Paquete de idioma de WinPE-Setup (.cab WinPE-Setup_ [lang], por ejemplo, WinPE-Setup_en-us.cab)  
  
    3.  Para las fuentes de Asia, incluidos zh-cn, zh-tw, zh-hk, ko-kr, ja-jp, es necesario instalar el paquete de fuentes adicionales (winpe - fontsupport-[lang] .cab, por ejemplo, winpe-fontsupport-zh-cn.cab)  
  
6.  Generar nuevo archivo Lang.ini ejecutando:  
  
    ```  
    dism /image:c:\mountRestore /Gen-LangINI /distribution:mount  
    ```  
  
7.  Confirma y desmonta la imagen y ejecutando:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mountRestore /commit  
    ```  
  
8.  Repite el paso 2 en el paso 7 para [RestoreMediaroot]\Sources\Boot_x86.wim.  
  
9. Confirma y desmonta la imagen y ejecutando:  
  
    ```  
    dism /unmount-wim /mountdir:c:\mount /commit  
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

