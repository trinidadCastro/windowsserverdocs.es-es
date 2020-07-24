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
ms.openlocfilehash: b7e94264511d1f05871a893b5a62dae4bda16d58
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963817"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Personalización de proveedores de autenticación multifactor y autenticación externa 



En AD FS, la compatibilidad con la autenticación multifactor se proporciona \- de forma \- integrada \- . Por ejemplo, puede configurar AD FS para usar \- la autenticación de certificado integrada como autenticación de segundo factor. También puedes usar proveedores de autenticación externos. Este enfoque puede habilitar AD FS para integrarse con servicios adicionales, como Azure multi-factor Authentication, o puede desarrollar su propio proveedor. Consulte [Guía de soluciones: administración de riesgos con multi- \- factor Access Control](./manage-risk-with-conditional-access-control.md) para obtener más información sobre cómo registrar un proveedor de autenticación externo mediante AD FS.  
  
Se recomienda que un proveedor de autenticación externo utilice las clases definidas en el archivo. CSS que AD FS proporciona para crear la interfaz de usuario de autenticación. Puedes usar el siguiente cmdlet para exportar el tema web predeterminado e inspeccionar los elementos y las clases de la interfaz de usuario definidos en el archivo .css. El archivo. CSS se puede usar en el desarrollo de la interfaz de usuario de inicio de sesión \- de un proveedor de autenticación externo.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
El siguiente es un ejemplo de la interfaz de usuario de inicio de sesión \- , que está resaltada en rojo, por un proveedor de autenticación externo. La interfaz de usuario utiliza las clases de interfaz de usuario en el archivo AD FS. CSS.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de escribir un nuevo método de autenticación personalizado, le recomendamos que estudie el tema AD FS las definiciones de estilo para comprender los requisitos de creación de contenido.  
  
-   Un método de autenticación personalizado solo crea un segmento HTML en la \- Página de inicio de sesión AD FS y no en la página completa. Debe usar la definición de estilo de AD FS para obtener una apariencia y un comportamiento coherentes.  
  
![AD FS y MFA](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenga en cuenta que los administradores de AD FS pueden personalizar los estilos de AD FS. . Te recomendamos que no escribas a mano tus propios estilos: En su lugar, se recomienda usar los estilos de AD FS siempre que sea posible.  
  
-   \-De \- un lado a otro, los estilos de AD FS se crean con un estilo de izquierda \- a \- derecha \( LTR \) y un de derecha \- a \- izquierda \( RTL \) . Los administradores pueden personalizar ambos y pueden proporcionar \- estilos específicos del lenguaje a través de la definición del tema Web. Cada hoja de estilos contiene tres secciones con sus respectivos comentarios:  
  
    -   **estilos** \- de tema Estos estilos no deben y no se pueden usar. Sirven para definir el tema en todas las páginas. Están definidos deliberadamente por un identificador de elemento para que no se reutilicen.  
  
    -   **estilos comunes** \- Estos son los estilos que se deben usar para el contenido.  
  
    -   estilos de factor de **forma** \- Son estilos para diferentes factores de forma. Debes entender esta sección para asegurarte de que tu contenido funcione con distintos factores de forma como, por ejemplo, teléfonos y tabletas.  
  
Para obtener más información, consulte Guía de la [Solución: administración de riesgos con multi- \- factor Access Control](./manage-risk-with-conditional-access-control.md) y [Guía de soluciones: administración de riesgos con \- autenticación multifactor adicional para aplicaciones confidenciales](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[Personalización de inicio de sesión de AD FS usuario](AD-FS-user-sign-in-customization.md) 
