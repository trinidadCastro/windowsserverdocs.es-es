---
title: "Crear el archivo Oobe.xml, incluido el logotipo y términos de licencia"
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: f8f99a2051e114b3c890f1cdac23aebf58689980
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="create-the-oobexml-file-including-logo-and-eula"></a>Crear el archivo Oobe.xml, incluido el logotipo y términos de licencia

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes agregar tus propios contrato de licencia de usuario final (CLUF) para la configuración inicial usando el archivo Oobe.xml. Oobe.xml es un archivo que se usan para proporcionar texto e imágenes para la configuración inicial, la bienvenida de Windows y otras páginas que se presentan al usuario final. Puedes agregar varios archivos Oobe.xml para personalizar el contenido en función de las selecciones de idioma y país o región del usuario final. Para obtener más información, consulta el [Windows Assessment and Deployment Kit para Windows 8](https://go.microsoft.com/fwlink/?LinkId=248694) documentación.  
  
 Además del EULA de Microsoft, se muestra el CLUF para tu empresa. La será de Microsoft EULA el CLUF primero se muestra durante la experiencia del usuario final de la configuración inicial y, a continuación, tu voluntad CLUF se muestre. El CLUF puede colocarse en cualquier lugar en el servidor y especifica la ubicación en el archivo Oobe.xml.  
  
#### <a name="to-add-your-company-eula-and-logo"></a>Para agregar tus términos de licencia de empresa y logotipo  
  
1.  Abre el archivo Oobe.xml en un editor de texto, como el Bloc de notas.  
  
2.  En el elemento < logopath\ >< / logopath\ > etiquetas, escribe la ruta de acceso absoluta al archivo de logotipo. Este archivo debe contener un archivo de (.png) de gráficos de red portátil de 32 bits que se 240 x 100 píxeles.  
  
3.  En el elemento < eulafilename\ >< / eulafilename\ > etiquetas, escriba la ruta de acceso absoluta al archivo de CLUF. El archivo de CLUF debe ser un archivo de (.rtf) de formato de texto enriquecido.  
  
4.  En el elemento < equipo\ >< / equipo\ > etiquetas, escribe el nombre de tu empresa.  
  
     El siguiente ejemplo muestra las etiquetas en un archivo Oobe.xml:  
  
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
  
5.  Guarda el archivo.  
  
6.  Coloca el archivo Oobe.xml en una de las siguientes ubicaciones:  
  
    |Ubicación de Oobe.xml|Condición para determinar la ubicación|  
    |-----------------------|----------------------------------------|  
    |%windir%\system32\oobe\info\|El servidor se incluirá en un solo país o región y un sistema de un solo idioma.|  
    |%windir%\system32\oobe\info\default\\ < language\ >|El servidor se incluirá en un solo país o región y el sistema de varios idiomas.|  
    |%windir%\system32\oobe\info\\ < país o región > \ y %windir%\system32\oobe\info\\ < país o región > \\ < language\ > \|El servidor se incluirá en más de un país o región y las opciones de configuración requieren personalizaciones para una base por-país o región, cada uno con un solo idioma. Donde < país o región > es la versión decimal del identificador de ubicación geográfica (GeoID) del país o región donde se está implementando el servidor y < language\ > es la versión decimal del identificador de configuración regional (LCID).|  
  
 Si tienes un logotipo de la empresa alternativa con texto blanco, puede mostrar mejor en el flujo de la instalación debido al fondo azul.  Opcionalmente, puedes especificar este logotipo estableciendo un valor y la clave del registro.  
  
#### <a name="to-specify-a-company-logo-by-setting-the-oem-registry-key"></a>Para especificar un logotipo de compañía estableciendo la clave del registro de OEM  
  
1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla y haz clic en **búsqueda**.  
  
2.  En el cuadro de búsqueda, escriba **regedit**y, a continuación, haz clic en la aplicación Regedit.  
  
3.  En el panel de navegación, vaya a **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Si no existe la clave de OEM, crear la clave de la siguiente manera:  
  
    1.  Haz clic en **Windows Server**, haz clic en **nueva**y, a continuación, haz clic en **clave**.  
  
    2.  Nombre de clave, escriba **OEM**.  
  
4.  (Opcional) Si vas a crear una entrada para un logotipo, puedes crear distintas claves para diferenciar las versiones de idioma del logotipo. Por ejemplo, si tienes versiones de inglés y alemán del logotipo, puedes crear una en-us clave y una clave de-de. Dado que todos los archivos de logotipo se almacenan en la misma carpeta, debes proporcionar instancias del archivo de imagen del logotipo con un nombre único para cada idioma. Por ejemplo, puedes crear un archivo llamado LogoWithWhiteText_en.png y LogoWithWhiteText_de.png.  
  
5.  Ya sea con el botón derecho **OEM** o haz clic en la clave de idioma apropiada, haz clic en **nueva**y, a continuación, haz clic en **valor de cadena**.  
  
6.  Escribe LogoWithWhiteText como la cadena y, a continuación, presione ENTRAR.  
  
7.  Haz clic en la nueva cadena y, a continuación, haz clic en **modificar**.  
  
8.  Escribe la ruta de acceso que contiene la imagen de logotipo y, a continuación, haz clic en Aceptar.  
  
## <a name="see-also"></a>Consulta también  
 [Tareas iniciales con el ADK de Windows Server Essentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)