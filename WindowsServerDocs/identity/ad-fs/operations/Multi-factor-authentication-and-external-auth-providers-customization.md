---
title: "La autenticación multifactor y la personalización de proveedores de autenticación externa"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>La autenticación multifactor y la personalización de proveedores de autenticación externa 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En AD FS, la compatibilidad para la autenticación multifactor se proporciona out\ descripción\ the\ integrados. Por ejemplo, puede configurar AD FS para usar built\ en la autenticación de certificado como la autenticación de factor de segundo. También puedes usar los proveedores de autenticación externo. Este enfoque puede habilitar AD FS para integrarse con servicios adicionales, como la autenticación multifactor de Azure, o pueden desarrollar su propios proveedor. Consulta [Guía de solución: administrar riesgo con Control de acceso de factor de Multi\](https://technet.microsoft.com/library/dn280937.aspx) para obtener más información sobre cómo registrar el proveedor de autenticación externo mediante el uso de AD FS.  
  
Te recomendamos que un proveedor de autenticación externo usar las clases que se definen en el archivo .css que AD FS proporciona para crear la interfaz de usuario de autenticación. Puedes usar el cmdlet siguiente para exportar el tema de web predeterminado e inspeccionar las clases de interfaz de usuario y los elementos que se definen en el archivo .css. El archivo .css puede usarse en el desarrollo de la interfaz de usuario sign\ de un proveedor de autenticación externo.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
El siguiente es un ejemplo de la interfaz de usuario sign\, que está resaltado en color rojo, por un proveedor de autenticación externo. La interfaz de usuario utiliza las clases de interfaz de usuario en el archivo .css de AD FS.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de escribir un nuevo método de autenticación personalizado, te recomendamos que estudiar las definiciones de tema y estilo de AD FS para comprender lo requisitos de creación de contenido.  
  
-   Un método de autenticación personalizado solo autores de un segmento HTML en la página de sign\ AD FS y no a la página completa. Debes usar la definición de estilo de AD FS para obtener la apariencia y comportamiento uniformes.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Ten en cuenta que los administradores de AD FS pueden personalizar los estilos de AD FS. . No recomendamos codificar tus propios estilos. En su lugar, te recomendamos que uses estilos de AD FS siempre que sea posible.  
  
-   El Out\ descripción\ cuadro, estilos de AD FS que se haya creado con un estilo \(LTR\) left\ to\-derecha y un \(RTL\) right\ to\-izquierda. Los administradores pueden personalizar dos y pueden proporcionar estilos específicos del language\ a través de la definición del tema web. Cada hoja de estilos tiene tres secciones con comentarios de los respectivos:  
  
    -   **los estilos del tema** \-estos estilos no se debe y no se pueden usar. Estos estilos están diseñados para definir el tema de entre todas las páginas. Están definidos por un identificador de elemento intencionadamente para que no se vuelven.  
  
    -   **estilos comunes** \-estos son los estilos que deben usarse para el contenido.  
  
    -   **estilos de factor de forma** \-estos son los estilos para diferentes factores de forma. Debes comprender esta sección para asegurarse de que el contenido funciona con diferentes factores de forma, por ejemplo, teléfonos y tabletas.  
  
Para obtener más información, consulta [Guía de solución: administrar riesgo con Control de acceso de factor de Multi\](https://technet.microsoft.com/library/dn280937.aspx) y [Guía de solución: administrar riesgo con la autenticación de Factor de Multi\ adicionales para las aplicaciones con información confidencial](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión del usuario FS de anuncios](AD-FS-user-sign-in-customization.md) 
