---
title: Crear el archivo Oobe.xml, incluyendo el logotipo y los términos de licencia
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a7b3cc1-21bb-4344-8110-f5d5959b370d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5d7dac41ba6d6f73b0d3d65d3481fe45ff99a6bc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433612"
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Crear el archivo Oobe.xml, incluyendo el logotipo y los términos de licencia

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puede agregar su propio Contrato de licencia para el usuario final (CLUF) a la Configuración inicial mediante el archivo Oobe.xml. El archivo Oobe.xml se utiliza para insertar texto e imágenes en la Configuración inicial, Bienvenida de Windows y otras páginas presentadas al usuario final. Puede agregar múltiples archivos Oobe.xml para personalizar el contenido en función de las selecciones de idioma y de configuración regional del usuario final. Para obtener más información, consulte la documentación de [Windows Assessment and Deployment Kit para Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) .  
  
 Además del CLUF de su empresa, se mostrará el CLUF de Microsoft. El CLUF de Microsoft será lo primero que se mostrará durante la configuración inicial del usuario final y a continuación aparecerá el CLUF personalizado. Puede colocar su CLUF en cualquier lugar del servidor y especificar la ubicación en el archivo Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Para agregar el CLUF y el logotipo de la empresa  
  
1. Abra el archivo Oobe.xml en un editor de textos como el Bloc de notas.  
  
2. En el < logopath\>< / logopath\> etiquetas, escriba la ruta de acceso absoluta al archivo de logotipo. El archivo debe contener un archivo .png (portable network graphics) de 32 bits de 240 x 100 píxeles.  
  
3. En el < eulafilename\>< / eulafilename\> etiquetas, escriba la ruta de acceso absoluta al archivo del CLUF. El archivo del CLUF debe estar en formato .rtf (texto enriquecido).  
  
4. En el < nombre\>< / nombre de\> etiquetas, escriba el nombre de su empresa.  
  
    En el ejemplo siguiente se muestra el formato del archivo Oobe.xml:  
  
   ```  
  
   <FirstExperience>  
      <oobe>  
         <oem>  
            <name>Fabrikam</name>  
            <logopath>c:\fabrikam\fabrikam.png</logopath>  
            <eulafilename>c:\fabrikam\fabrikam_eula.rtf</eulafilename>  
         </oem>  
      </oobe>  
   </FirstExperience>  
  
   ```  
  
5. Guarde el archivo.  
  
6. Copie el archivo Oobe.xml en una de las ubicaciones siguientes:  
  
   |Ubicación de Oobe.xml|Condición para determinar la ubicación|  
   |-----------------------|----------------------------------------|  
   |%WINDIR%\System32\Oobe\Info\|el servidor se envía en un solo país/región y un sistema de idioma único.|  
   |%windir%\system32\oobe\info\default\\<language\>|El servidor se envía a un solo país/región y para sistemas de varios idiomas.|  
   |%WINDIR%\System32\Oobe\Info\\< país/región > \ y %windir%\system32\oobe\info\\< país/región >\\< idioma\>\|el servidor se envía a más de un país / región y la configuración requiere personalizaciones distintas para una base por país/región, cada uno con un solo idioma. Donde < país/región > es la versión decimal del identificador de ubicación geográfica (GeoID) del país o región donde se implementa el servidor, y < idioma\> es la versión decimal del identificador de configuración regional (LCID).|  
  
   Si tiene un logotipo de empresa alternativo con texto de color blanco, puede que se visualice mejor en el flujo de instalación debido al azul del fondo.  Opcionalmente puede especificar este logotipo configurando una clave del Registro y un valor.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Para especificar un logotipo de empresa mediante la configuración de la clave de Registro del OEM  
  
1.  En el servidor, mueva el ratón a la esquina superior derecha de la pantalla y haga clic en **Buscar**.  
  
2.  En el cuadro de búsqueda, escriba **regedit** y después haga clic en la aplicación Regedit.  
  
3.  En el panel de navegación, vaya a  **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, **Microsoft**y, finalmente, **Windows Server**. Si la clave del OEM no existe, créela de la manera siguiente:  
  
    1.  Haga clic con el botón secundario en **Windows Server**, en **Nuevo**y, a continuación, en **Clave**.  
  
    2.  Como nombre de clave, escriba **OEM**.  
  
4.  (Opcional) Si va a crear una entrada para un logotipo, puede crear diferentes claves para diferenciar las versiones de idioma. Por ejemplo, si tiene versiones en inglés y en alemán de su logotipo, puede crear una clave en-us y otra de-de. Ya que todos los archivos de logotipo se almacenan en la misma carpeta, deberá proporcionar instancias de archivo de imagen de logotipo con nombres únicos para cada idioma. Por ejemplo, puede crear un archivo llamado LogoWithWhiteText_en.png y LogoWithWhiteText_de.png.  
  
5.  Haga clic con el botón secundario en **OEM** o en la clave de idioma correspondiente, haga clic en **Nuevo** y, a continuación, en **Valor de cadena**.  
  
6.  Escriba LogoWithWhiteText como cadena y, a continuación, presione Entrar.  
  
7.  Haga clic con el botón secundario en la cadena nueva y, a continuación, haga clic en **Modificar**.  
  
8.  Escriba la ruta de acceso que contiene la imagen del logotipo y, a continuación, haga clic en Aceptar.  
  
## <a name="see-also"></a>Vea también  
 [Introducción al ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)