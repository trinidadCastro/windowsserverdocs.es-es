---
title: Personalización de proveedores de autenticación multifactor y autenticación externa
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 8252244738d59f11a07c3bebadbbf2a5f4818845
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816238"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personalización de proveedores de autenticación multifactor y autenticación externa 



En AD FS, se proporciona compatibilidad con la autenticación multifactor\-de\-el cuadro de\-. Por ejemplo, puede configurar AD FS para usar\-compilados en la autenticación de certificado como autenticación de segundo factor. También puedes usar proveedores de autenticación externos. Este enfoque puede habilitar AD FS para integrarse con servicios adicionales, como Azure multi-factor Authentication, o puede desarrollar su propio proveedor. Consulte [Guía de soluciones: administración de riesgos con el factor multi\-Access Control](https://technet.microsoft.com/library/dn280937.aspx) para obtener más información sobre cómo registrar un proveedor de autenticación externo mediante AD FS.  
  
Se recomienda que un proveedor de autenticación externo utilice las clases definidas en el archivo. CSS que AD FS proporciona para crear la interfaz de usuario de autenticación. Puedes usar el siguiente cmdlet para exportar el tema web predeterminado e inspeccionar los elementos y las clases de la interfaz de usuario definidos en el archivo .css. El archivo. CSS se puede usar en el desarrollo del\-de inicio de sesión en la interfaz de usuario de un proveedor de autenticación externo.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
El siguiente es un ejemplo del signo\-en la interfaz de usuario, que está resaltado en rojo, por un proveedor de autenticación externo. La interfaz de usuario utiliza las clases de interfaz de usuario en el archivo AD FS. CSS.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de escribir un nuevo método de autenticación personalizado, le recomendamos que estudie el tema AD FS las definiciones de estilo para comprender los requisitos de creación de contenido.  
  
-   Un método de autenticación personalizado solo crea un segmento HTML en el\-de AD FS firmar en la página y no en la página completa. Debe usar la definición de estilo de AD FS para obtener una apariencia y un comportamiento coherentes.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenga en cuenta que los administradores de AD FS pueden personalizar los estilos de AD FS. . Te recomendamos que no escribas a mano tus propios estilos: En su lugar, se recomienda usar los estilos de AD FS siempre que sea posible.  
  
-   \-de\-el cuadro, los estilos de AD FS se crean con un\-a la izquierda\-derecha \(el estilo de\) LTR y uno\-a la izquierda\-derecha RTL \(.\) Los administradores pueden personalizar ambos y pueden proporcionar lenguajes\-estilos específicos a través de la definición del tema Web. Cada hoja de estilos contiene tres secciones con sus respectivos comentarios:  
  
    -   Los **estilos de tema** \- estos estilos no deberían y no se pueden usar. Sirven para definir el tema en todas las páginas. Están definidos deliberadamente por un identificador de elemento para que no se reutilicen.  
  
    -   los **estilos comunes** \- estos son los estilos que se deben usar para el contenido.  
  
    -   Los **estilos de factor de forma** \- son estilos para diferentes factores de forma. Debes entender esta sección para asegurarte de que tu contenido funcione con distintos factores de forma como, por ejemplo, teléfonos y tabletas.  
  
Para obtener más información, consulte Guía de la [Solución: administración de riesgos con multi\-factor Access Control](https://technet.microsoft.com/library/dn280937.aspx) y [Guía de la solución: administración de riesgos con autenticación multi\-factor adicional para aplicaciones confidenciales](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md) 
