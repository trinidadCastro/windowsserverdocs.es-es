---
title: Personalización de proveedores de autenticación multifactor y autenticación externa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 8271e65e05a8da1d397f087242308f04a51a6249
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357933"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personalización de proveedores de autenticación multifactor y autenticación externa 



En AD FS, la compatibilidad con la autenticación multifactor se proporciona\-\-de\-forma integrada. Por ejemplo, puede configurar AD FS para usar la autenticación\-de certificado integrada como autenticación de segundo factor. También puedes usar proveedores de autenticación externos. Este enfoque puede habilitar AD FS para integrarse con servicios adicionales, como Azure multi-factor Authentication, o puede desarrollar su propio proveedor. Vea [la guía de soluciones: Administrar el riesgo con\-Access control](https://technet.microsoft.com/library/dn280937.aspx) multifactor para obtener más información sobre cómo registrar un proveedor de autenticación externo mediante AD FS.  
  
Se recomienda que un proveedor de autenticación externo utilice las clases definidas en el archivo. CSS que AD FS proporciona para crear la interfaz de usuario de autenticación. Puedes usar el siguiente cmdlet para exportar el tema web predeterminado e inspeccionar los elementos y las clases de la interfaz de usuario definidos en el archivo .css. El archivo. CSS se puede usar en el desarrollo de la interfaz\-de usuario de inicio de sesión de un proveedor de autenticación externo.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
El siguiente es un ejemplo de la interfaz\-de usuario de inicio de sesión, que está resaltada en rojo, por un proveedor de autenticación externo. La interfaz de usuario utiliza las clases de interfaz de usuario en el archivo AD FS. CSS.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de escribir un nuevo método de autenticación personalizado, le recomendamos que estudie el tema AD FS las definiciones de estilo para comprender los requisitos de creación de contenido.  
  
-   Un método de autenticación personalizado solo crea un segmento HTML en la página\-de inicio de sesión AD FS y no en la página completa. Debe usar la definición de estilo de AD FS para obtener una apariencia y un comportamiento coherentes.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenga en cuenta que los administradores de AD FS pueden personalizar los estilos de AD FS. . Te recomendamos que no escribas a mano tus propios estilos: En su lugar, se recomienda usar los estilos de AD FS siempre que sea posible.  
  
-   \(\-\- \(\-\-\) \-De un lado a otro, los estilos de AD FS se crean con un estilo de izquierda a derecha LTR y un de derecha a izquierda RTL\-\). Los administradores pueden personalizar ambos y pueden proporcionar estilos específicos\-del lenguaje a través de la definición del tema Web. Cada hoja de estilos contiene tres secciones con sus respectivos comentarios:  
  
    -   **estilos de tema** \- Estos estilos no deben y no se pueden usar. Sirven para definir el tema en todas las páginas. Están definidos deliberadamente por un identificador de elemento para que no se reutilicen.  
  
    -   **estilos comunes** \- Estos son los estilos que se deben usar para el contenido.  
  
    -   **estilos de factor de forma** \- Son estilos para diferentes factores de forma. Debes entender esta sección para asegurarte de que tu contenido funcione con distintos factores de forma como, por ejemplo, teléfonos y tabletas.  
  
Para obtener más información, [consulte la guía de soluciones: Administración de riesgos con\-multi-](https://technet.microsoft.com/library/dn280937.aspx) factor [Access Control y guía de soluciones: Administración de riesgos con autenticación\-multifactor adicional para aplicaciones](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx)confidenciales.  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md) 
