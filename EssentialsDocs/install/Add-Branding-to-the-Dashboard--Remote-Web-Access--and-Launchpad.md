---
title: "Agregar personalización de marca en el panel, acceso Web remoto y del Launchpad"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 04/10/2014
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 02088b169e44cdcf87385425e1949232ffa408a6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Agregar personalización de marca en el panel, acceso Web remoto y del Launchpad

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_Branding"></a>Agregar personalización de marca en el panel, acceso Web remoto y del Launchpad  
 Puedes hacer muchas personalizaciones de marca agregando entradas en el registro. Todas las entradas de personalización de marca en el registro para el sistema operativo se encuentran en HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM.  
  
 Todos los personalización de marca conjunta debe cumplir los siguientes requisitos de logotipo:  
  
-   El logotipo de Windows Server Essentials debe tener un ancho mínimo de **170 píxeles**, mantener la relación de aspecto correcta, con **96 PPP**.  
  
#### <a name="to-add-branding-by-changing-the-registry"></a>Para agregar personalización de marca cambiando el registro  
  
1.  En el servidor, mueve el mouse a la esquina superior derecha de la pantalla y haz clic en **búsqueda**.  
  
2.  En el cuadro de búsqueda, escriba **regedit**y, a continuación, haz clic en el **Regedit** aplicación.  
  
3.  En el panel de navegación, expande **HKEY_LOCAL_MACHINE**, expanda **SOFTWARE**, expanda **Microsoft**, expanda **Windows Server**. Si la **OEM** clave no existe, completa los siguientes pasos para crearla:  
  
    1.  Haz clic en **Windows Server**, haz clic en **nueva**y, a continuación, haz clic en **clave**.  
  
    2.  Tipo **OEM** para el nombre de la clave.  
  
4.  (Opcional) Si vas a crear una entrada para un logotipo, puedes crear distintas claves para diferenciar las versiones de idioma del logotipo. Por ejemplo, si tienes versiones de inglés y alemán del logotipo, puedes crear una en-us clave y una clave de-de. Dado que todos los archivos de logotipo se almacenan en la misma carpeta, debes proporcionar instancias del archivo de imagen del logotipo con un nombre único para cada idioma. Por ejemplo, crear un archivo llamado DashboardLogo_en.png y otro llamado DashboardLogo_de.png.  
  
5.  Ya sea con el botón derecho **OEM** o haz clic en la clave de idioma apropiada, haz clic en **nueva**y, a continuación, haz clic en **valor de cadena**.  
  

6.  Escribe el nombre de la cadena y, a continuación, presione ENTRAR. Consulte la [las cadenas del registro y valores](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) tabla para los valores de nombres y los datos de cadena.  

6.  Escribe el nombre de la cadena y, a continuación, presione ENTRAR. Consulte la [las cadenas del registro y valores](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_RegStrings) tabla para los valores de nombres y los datos de cadena.  

  
7.  Haz clic en la nueva cadena y, a continuación, haz clic en **modificar**.  
  
8.  Escribe el valor de la tabla que está asociada con el nombre de cadena y, a continuación, haz clic en **Aceptar**.  
  
9. Si vas a crear una entrada para una imagen de logotipo o vínculos anexados, copia el archivo en %programFiles%\Windows Server\Bin\OEM. Si el directorio OEM no existe, Créalo.  
  
10. Si se realizan cambios que afecten al acceso Web remoto, una vez que el cliente tome posesión del servidor, tendrá que activar acceso Web remoto. Avisa al cliente para hacer lo siguiente:  
  
    1.  En el panel, haz clic en **configuración**y, a continuación, haz clic en el **acceso desde cualquier lugar** pestaña.  
  
    2.  Si acceso desde cualquier lugar está activado, haz clic en **configurar**y, a continuación, desactiva la casilla acceso Web remoto en el **características elige acceso desde cualquier lugar para habilitar** página de configurar el Asistente de acceso desde cualquier lugar.  
  
    3.  Haz clic en **configurar**.  
  
###  <a name="BKMK_RegStrings"></a>La siguiente tabla enumera la ubicación donde los cambios del registro afecta a personalización de marca, el nombre de cadena y el valor de datos.  
  
### <a name="registry-strings-and-values"></a>Cadenas de registro y valores  
  
|Ubicación de personalización de marca|Descripción|Nombre de cadena|Valor de datos|  
|--------------------------|-----------------|-----------------|----------------|  
|Logotipo del panel|Agrega la imagen del logotipo al panel. El logotipo del panel debe tener formato .png y no debe superar los 350 píxeles de ancho por 38 píxeles de alto.<br /><br /> **Importante:** para CO-el panel con tu logotipo, debes editar el icono del gráfico que se proporciona en el DVD del OPK y anexa el logotipo de tu empresa a la imagen cumpliendo los requisitos adecuados de espacio en blanco. Para más información, consulta el icono de ejemplo que se proporciona.|DashboardLogo|Nombre del archivo de imagen de logotipo|  
|DashboardClientLogo|Agrega la imagen de logotipo a la pantalla de inicio de sesión del cliente de panel.|DashboardClientLogo|Nombre del archivo de imagen de logotipo|  
|Imagen de fondo del sitio Web|Cambia la imagen de fondo que se muestra en la página de inicio de sesión de acceso Web remoto. Resoluciones típicas aparecerán de la siguiente manera:<br /><br /> -la resolución de 1024 x 768 píxeles rellenará de forma precisa la página de inicio de sesión<br /><br /> -800 x la resolución de 600 píxeles estará centrada en la página y aparecerá con un borde negro<br /><br /> -la resolución de 1280 x 720 píxeles estará centrada y los píxeles que superen los 1024 x 768 no aparecerán|LogonBackground|Nombre de archivo de imagen de fondo|  
|Título del sitio Web|Reemplaza el título del sitio acceso Web remoto de Windows Server Essentials para un título que elijas.|WebsiteName|Título del nuevo sitio de acceso Web remoto|  
|Logotipo de sitio Web|Cambia el logotipo predeterminado en el sitio de acceso Web remoto. El tamaño previsto del logotipo es de 32 x 32 píxeles. Si tu logotipo es menor o mayor, se estirará o se reducirá para ajustarse a esas dimensiones.|WebsiteLogo|Nombre del archivo de imagen de logotipo|  
|Logotipo de sitio Web anexado|Tu logotipo de asociado se mostrará debajo del logotipo de Microsoft que se muestra en el sitio de acceso Web remoto. El tamaño previsto del logotipo es de 200 píxeles de alto por 50 píxeles de ancho. Si tu logotipo es más grande, se reducirá para que quepa, manteniendo la relación de aspecto original. Si tu logotipo es más pequeño, se centrará en el espacio de 200 x 50 píxeles y se cambiará el tamaño ni relación de aspecto.|OEMLogo|Nombre del archivo de imagen de logotipo|  

| Vínculos en la página de inicio de sesión y la página principal del sitio Web | Anexa vínculos a la página de inicio de sesión y la página principal del sitio acceso Web remoto. El .xml que contiene la información de vínculo debe encontrarse en %programFiles%\Windows Server\Bin\OEM. El siguiente ejemplo muestra el formato del archivo. XML:<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < vínculo equipo\ = LogonLinkName ><br /> LogonLinkDescription < Text\ > < / Text\ ><br /> LogonLinkURL < Url\ > < / Url\ ><br /> LinkIcon < Icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < vínculo equipo\ = HomepageLinkName ><br /> HomepageLinkDescription < Text\ > < / Text\ ><br /> HomepageLinkURL < Url\ > < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > | LinksXML | Consulte la [elementos LinksXML](Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) tabla para obtener una lista de los elementos y descripciones. |  

| Vínculos en la página de inicio de sesión y la página principal del sitio Web | Anexa vínculos a la página de inicio de sesión y la página principal del sitio acceso Web remoto. El .xml que contiene la información de vínculo debe encontrarse en %programFiles%\Windows Server\Bin\OEM. El siguiente ejemplo muestra el formato del archivo. XML:<br /><br /> < OemLinks\ ><br /> < LogonLinks\ ><br /> < vínculo equipo\ = LogonLinkName ><br /> LogonLinkDescription < Text\ > < / Text\ ><br /> LogonLinkURL < Url\ > < / Url\ ><br /> LinkIcon < Icon\ > < / Icon\ ><br /> < / Link\ ><br /> < / LogonLinks\ ><br /> < HomepageLinks\ ><br /> < vínculo equipo\ = HomepageLinkName ><br /> HomepageLinkDescription < Text\ > < / Text\ ><br /> HomepageLinkURL < Url\ > < / Url\ ><br /> < / Link\ ><br /> < / HomepageLinks\ ><br /> < / OemLinks\ > | LinksXML | Consulte la [elementos LinksXML](../install/Add-Branding-to-the-Dashboard--Remote-Web-Access--and-Launchpad.md#BKMK_Links) tabla para obtener una lista de los elementos y descripciones. |  

| Logotipo del LaunchPad | Agrega la imagen del logotipo al Launchpad. El logotipo del Launchpad debe tener formato .png y no superar los 64 píxeles debe. | LaunchpadLogo | Nombre del archivo de imagen de logotipo |  
  
###  <a name="BKMK_Links"></a>La siguiente tabla se enumera y describe los elementos de nombre de cadena LinksXML.  
  
### <a name="linksxml-elements"></a>Elementos LinksXML  
  
|Elemento LinksXML|Descripción|  
|----------------------|-----------------|  
|**LogonLinks**|  
|LogonLinkName|El nombre del vínculo de inicio de sesión.|  
|LogonLinkDescription|El texto que se muestra como vínculo a la página de inicio de sesión.|  
|LogonLinkURL|La dirección url que resuelve el vínculo de la página de inicio de sesión.|  
|LinkIcon|El nombre del archivo de icono para el vínculo de inicio de sesión. Este archivo debe estar en la misma ubicación de carpeta que el archivo .xml. Las imágenes de icono de vínculo deben ser de 16 x 16 píxeles y tener formato .png. Si no proporcionas un LinkIcon, se usa la imagen del icono de vínculo predeterminada.|  
|**HomepageLinks**|  
|HomepageLinkName|El nombre del vínculo página principal.|  
|HomepageLinkDescription|El texto que aparece como vínculo a la página principal.|  
|HomepageLinkURL|La dirección URL que resuelve el vínculo a página principal.|  
|HomepageLinkIcon|El nombre del archivo de icono para el vínculo de la página principal. Este archivo debe estar en la misma ubicación de carpeta que el archivo .xml. Las imágenes HomepageLinkIcon deberían ser 16 x 16 píxeles y tener formato .png. Si no proporcionas un HomepageLinkIcon, se usa la imagen de icono del vínculo de página principal predeterminada.|  
  
## <a name="see-also"></a>Consulta también  

 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)

 [Crear y personalizar la imagen](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](../install/Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](../install/Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](../install/Testing-the-Customer-Experience.md)

