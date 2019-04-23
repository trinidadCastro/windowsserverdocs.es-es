---
title: Autenticación multifactor y personalización de los proveedores de autenticación externa
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 08724d45-9be4-4c56-a5f1-2cf40864e136
ms.technology: identity-adfs
ms.openlocfilehash: 6d06c017601003e3b93df32f5fa50190ce54541d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864806"
---
# <a name="multi-factor-authentication-and-external-authentication-providers-customization"></a>Autenticación multifactor y personalización de los proveedores de autenticación externa 

>Se aplica a: Windows Server 2016, Windows Server 2012 R2

En AD FS, se proporciona la compatibilidad con la autenticación multifactor out\-de\-el\-cuadro. Por ejemplo, puede configurar AD FS para uso integrada\-en la autenticación de certificado como el segundo factor de autenticación. También puedes usar proveedores de autenticación externos. Este enfoque puede habilitar AD FS para integrarse con servicios adicionales, como Azure Multi-factor Authentication, o puede desarrollar su propio proveedor. Consulte [Guía de soluciones: Administración de riesgos con múltiples\-factorizar el Control de acceso](https://technet.microsoft.com/library/dn280937.aspx) para obtener más información sobre cómo registrar el proveedor de autenticación externo con AD FS.  
  
Se recomienda que el proveedor de autenticación externo utilice las clases que se definen en el archivo .css que proporciona AD FS para crear el interfaz de usuario de autenticación. Puedes usar el siguiente cmdlet para exportar el tema web predeterminado e inspeccionar los elementos y las clases de la interfaz de usuario definidos en el archivo .css. El archivo .css se puede usar en el desarrollo de la sesión\-en la interfaz de usuario del proveedor de autenticación externo.  
  

    Export-AdfsWebTheme -Name default -DirectoryPath C:\theme  
 
  
El siguiente es un ejemplo de la sesión\-de interfaz de usuario que está resaltado en rojo, por un proveedor de autenticación externo. La interfaz de usuario utiliza las clases de interfaz de usuario en el archivo .css de AD FS.  
  
![MFA y AD FS](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom8.png)  
  
Antes de escribir un nuevo método de autenticación personalizada, se recomienda que revise atentamente las definiciones de estilos y el tema de AD FS para comprender los requisitos de la creación de contenido.  
  
-   Métodos de autenticación personalizados solo crean un segmento HTML en el inicio de sesión de AD FS\-en la página y no en la página completa. Debe usar la definición de estilo de AD FS para obtener una apariencia coherente y comportamiento.  
  
![MFA y AD FS](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom9.png)  
  
-   Tenga en cuenta que los administradores de AD FS pueden personalizar los estilos de AD FS. . Te recomendamos que no escribas a mano tus propios estilos: En su lugar, se recomienda utilizar estilos de AD FS, siempre que sea posible.  
  
-   Out\-de\-el cuadro, estilos de AD FS se crean con una a la izquierda\-a\-derecho \(LTR\) estilo y un derecho\-a\-izquierdo \(RTL\). Los administradores pueden personalizar estos dos y puede proporcionar el lenguaje\-estilos específicos a través de la definición del tema web. Cada hoja de estilos contiene tres secciones con sus respectivos comentarios:  
  
    -   **estilos de tema** \- estos estilos no se debe y no se puede usar. Sirven para definir el tema en todas las páginas. Están definidos deliberadamente por un identificador de elemento para que no se reutilicen.  
  
    -   **estilos comunes** \- son los estilos que se deben usar para su contenido.  
  
    -   **estilos de factor de forma** \- son estilos para diferentes factores de forma. Debes entender esta sección para asegurarte de que tu contenido funcione con distintos factores de forma como, por ejemplo, teléfonos y tabletas.  
  
Para obtener más información, consulte [Guía de soluciones: Administración de riesgos con múltiples\-factorizar el Control de acceso](https://technet.microsoft.com/library/dn280937.aspx) y [Guía de soluciones: Administración de riesgos con múltiples adicionales\-Factor de autenticación para aplicaciones confidenciales](https://tnstage.redmond.corp.microsoft.com/library/dn280949.aspx).  

## <a name="additional-references"></a>Referencias adicionales 
[AD FS Sign-personalización de usuario](AD-FS-user-sign-in-customization.md) 
