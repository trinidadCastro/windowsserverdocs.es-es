---
title: Agregar personalización de marca al panel, acceso Web remoto y Launchpad
description: Cómo agregar materiales de personalización de marca al panel, acceso Web remoto y pantallas de Launchpad.
ms.date: 04/10/2014
ms.topic: article
ms.assetid: 166262f8-b2a5-4b1c-a4a7-a141e1c54f10
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4d1b3ada8888de9885bfe88af56e5b0656fc92b7
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181611"
---
# <a name="add-branding-to-the-dashboard-remote-web-access-and-launchpad"></a>Agregar personalizaciones de marca al Panel, el acceso web remoto y Launchpad

> Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para agregar marcas, añada más entradas al registro. Todas las entradas de personalización de marca del registro del sistema operativo se encuentran en `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OEM` .

En lo que respecta a la personalización de marca, el logotipo de Windows Server Essentials debe tener un ancho mínimo de **170 píxeles**, manteniendo la relación de aspecto correcta, con **96 PPP**.

## <a name="to-add-branding-by-changing-the-registry"></a>Para cambiar la personalización de marca en el registro

1. En el servidor, mueva el ratón a la esquina superior derecha de la pantalla y haga clic en **Buscar**.

2. En el cuadro de búsqueda, escriba **regedit** y después haga clic en la aplicación **Regedit**.

3. En el panel de navegación, expanda **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft** y finalmente **Windows Server**. Si la clave **OEM** no existe, puede crearla:

    1. Haga clic con el botón secundario en **Windows Server**, en **Nuevo** y, a continuación, en **Clave**.

    2. Escriba **OEM** como nombre de la clave.

4. Opta Si va a crear una entrada para un logotipo, puede crear diferentes claves para diferenciar las versiones de idioma del logotipo. Por ejemplo, si tiene versiones en inglés y en alemán de su logotipo, puede crear una clave en-us y otra de-de. Ya que todos los archivos de logotipo se almacenan en la misma carpeta, deberá proporcionar instancias de archivo de imagen de logotipo con nombres únicos para cada idioma. Por ejemplo, puede crear un archivo llamado DashboardLogo_en.png y otro DashboardLogo_de.png.

5. Haga clic con el botón secundario en **OEM** o haga clic con el botón secundario en la clave de idioma correspondiente, haga clic en **nuevo**y, a continuación, en **valor de cadena**.

6. Escriba el nombre de la cadena y, a continuación, presione ENTRAR. Para obtener más información sobre los nombres de cadena y los valores de datos, consulte la sección **cadenas y valores del registro** de este artículo.

7. Escriba el nombre de la cadena y, a continuación, presione ENTRAR. Para obtener más información sobre los nombres de cadena y los valores de datos, consulte la sección **cadenas y valores del registro** de este artículo.

8. Haga clic con el botón secundario en la cadena nueva y, a continuación, haga clic en **Modificar**.

9. Introduzca el valor de la tabla asociada con el nombre de la cadena y haga clic en **Aceptar**.

10. Si va a crear una entrada para una imagen de logotipo o vínculos anexados, copie el archivo en `%programFiles%\Windows Server\Bin\OEM` . Si la clave RemoteUserPortal no existe, créela.

11. Si realiza cambios en el acceso Web remoto después de que el cliente tome posesión del servidor, debe activar el acceso Web remoto:

    1. En el Panel, haga clic en **Settings** y, a continuación, haga clic en la ficha **Acceso desde cualquier lugar**.

    2. Si está activado el **acceso desde cualquier lugar** , haga clic en **configurar**y, a continuación, desactive la casilla acceso Web remoto en la página **elegir las características de acceso desde cualquier lugar que desea habilitar** del Asistente para **configurar el acceso desde cualquier lugar** .

    3. Haga clic en **Configurar**.

### <a name="registry-strings-and-values"></a>Cadenas y valores del registro

En la tabla siguiente se proporciona información sobre los nombres de cadena y los valores de datos disponibles.

| Ubicación de personalización de marca | Description | Nombre de la cadena | Valor de los datos |
|--|--|--|--|
| Logotipo del Panel | Agrega la imagen de logotipo al Panel. El logotipo del Panel debe estar en formato .png y el tamaño no debe superar los 350 píxeles de ancho por 38 de alto.<p>**Importante:** Para incorporar la marca del panel con su logotipo, debe editar el icono de la ilustración que se proporciona en el DVD del OPK y anexar el logotipo de la empresa a la imagen a la vez que sigue los requisitos de espacio en blanco apropiados. | DashboardLogo | Escriba el nombre del archivo de imagen del logotipo. |
| DashboardClientLogo | Agrega la imagen del logotipo a la pantalla de inicio de sesión del cliente del panel. | DashboardClientLogo | Escriba el nombre del archivo de imagen del logotipo. |
| Imagen de fondo del sitio web | Cambia la imagen de fondo que se muestra en la página de inicio de sesión de acceso Web remoto. Las resoluciones típicas son las siguientes:<ul><li>**1024x768 píxeles** : esta resolución rellena con precisión la página de inicio de sesión.</li><li>**800x600 píxeles** : esta resolución centra la imagen en la página y aparece con un borde negro.</li><li>**1280x720 píxeles** : esta resolución centra la imagen. Los píxeles que superen 1024x720 no aparecerán. | LogonBackground | Escriba el nombre del archivo de imagen de fondo. |
| Título del sitio web | Reemplaza el título del sitio de acceso Web remoto de Windows Server Essentials a su título especificado. | WebsiteName | Escriba el nuevo título del sitio de acceso Web remoto. |
| Logotipo del sitio web | Cambia el logotipo predeterminado del sitio de Acceso web remoto. El tamaño esperado del logotipo es de 32 píxeles por 32 píxeles. Si el logotipo es menor o mayor que este, se expandirá o reducirá para que coincida con estas dimensiones. | WebsiteLogo | Escriba el nombre del archivo de imagen del logotipo |
| Logotipo del sitio web anexo | El logotipo de su socio comercial se mostrará justo debajo del logotipo de Microsoft que aparece en el sitio de acceso Web remoto. El tamaño esperado del logotipo es de 200 píxeles de alto por 50 píxeles de ancho. Si el tamaño de su logotipo es mayor, se reducirá para ajustarlo pero se mantendrá la relación de aspecto original. Si el logotipo es más pequeño, se centrará en el espacio de 200 por 50 píxeles y no se cambiarán ni el relación de aspecto ni la relación de aspecto. | OEMLogo | Escriba el nombre del archivo de imagen del logotipo. |
| Vínculos en la Página principal del sitio web y página de inicio de sesión | Anexa vínculos a la página de inicio de sesión y a la Página principal del sitio de acceso Web remoto. El archivo XML que contiene la información del vínculo debe encontrarse en la `%programFiles%\Windows Server\Bin\OEM` carpeta. | LinksXML | Para ver una lista de los elementos y sus descripciones, consulte la tabla Elementos de LinksXML. |
| Logotipo de Launchpad | Agrega la imagen de logotipo al Launchpad. El logotipo de Launchpad debe tener el formato .png y no debe tener una altura superior a 64 píxeles. | LaunchpadLogo | Escriba el nombre del archivo de imagen del logotipo. |

#### <a name="xml-linking-format"></a>Formato de vinculación XML

Debe dar formato a los vínculos de la página **principal** del sitio web y la página de inicio de sesión como:

```xml
<OemLinks>
    <LogonLinks>
        <Link Name=LogonLinkName>
        <Text>LogonLinkDescription</Text>
        <Url>LogonLinkURL</Url>
        <Icon>LinkIcon</Icon>
        </Link>
    </LogonLinks>

    <HomepageLinks>
        <Link Name=HomepageLinkName>
        <Text>HomepageLinkDescription</Text>
        <Url>HomepageLinkURL</Url>
        <Icon>HomepageLinkIcon</Icon>
        </Link>
    </HomepageLinks>
</OemLinks>
```

### <a name="linksxml-elements"></a>Elementos de LinksXML

| Elemento de LinksXML | Description |
|--|--|
| LogonLinks | Entrada primaria para los vínculos de inicio de sesión. |
| Nombre del vínculo | Nombre del vínculo de inicio de sesión. |
| Texto | Texto que se muestra como el vínculo de la página de inicio de sesión. |
| URL | La dirección URL que se resuelve en el vínculo de la página de inicio de sesión. |
| Icono | Nombre del archivo de icono para el vínculo de inicio de sesión. El archivo debe encontrarse en la misma carpeta que el archivo .xml. Las imágenes de icono deben ser de 16x16 píxeles y estar en formato. png. Si no proporciona un icono, se usa la imagen del icono de vínculo predeterminado. |
| HomepageLinks | Entrada primaria de la página **principal** . |
| Nombre del vínculo | Nombre del vínculo de la página **principal** . |
| Texto | El texto que aparece como el vínculo de la página **principal** . |
| URL | La dirección URL que se resuelve en el vínculo de la página **principal** . |
| Icono | Nombre del archivo de icono para el vínculo de la página **principal** . El archivo debe encontrarse en la misma carpeta que el archivo .xml. Las imágenes de icono deben ser de 16x16 píxeles y estar en formato. png. Si no proporciona un icono, se usa la imagen de icono de vínculo de página **principal** predeterminada. |

## <a name="additional-references"></a>Referencias adicionales

- [Creación y personalización de la imagen](Creating-and-Customizing-the-Image.md)

- [Personalizaciones adicionales](Additional-Customizations.md)

- [Preparación de la imagen para su implementación](Preparing-the-Image-for-Deployment.md)

- [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)