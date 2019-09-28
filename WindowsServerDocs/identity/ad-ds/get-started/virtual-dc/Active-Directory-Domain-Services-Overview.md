---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Introducción a Active Directory Domain Services
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 84ce4986d27884f817eb5e632ac8dc1c5a22b922
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390482"
---
# <a name="active-directory-domain-services-overview"></a>Introducción a Active Directory Domain Services

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Un directorio es una estructura jerárquica que almacena información acerca de los objetos de la red. Un servicio de directorio, como Active Directory Domain Services (AD DS), proporciona los métodos para almacenar los datos de directorio y hacer que estos datos estén disponibles para los usuarios y administradores de la red. Por ejemplo, AD DS almacena información acerca de las cuentas de usuario, como nombres, contraseñas, números de teléfono, etc., y permite que otros usuarios autorizados de la misma red tengan acceso a esta información.

Active Directory almacena información acerca de los objetos de la red y facilita la búsqueda y el uso de esta información para los administradores y los usuarios. Active Directory usa un almacén de datos estructurado como base para una organización jerárquica lógica de información de directorio.

Este almacén de datos, también conocido como directorio, contiene información sobre los objetos de Active Directory. Estos objetos suelen incluir recursos compartidos como servidores, volúmenes, impresoras y las cuentas de equipo y usuario de red. Para obtener más información sobre el almacén de datos de Active Directory, vea [almacén de datos de directorio](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

La seguridad se integra con Active Directory a través de la autenticación de inicio de sesión y el control de acceso a los objetos del directorio. Con un único inicio de sesión de red, los administradores pueden administrar los datos de directorio y la organización a través de su red, y los usuarios de red autorizados pueden tener acceso a los recursos en cualquier parte de la red. La administración basada en directiva facilita la administración de incluso las redes más complejas. Para obtener más información sobre la seguridad de Active Directory, consulte [información general sobre seguridad](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

Active Directory también incluye:
* Conjunto de reglas, **el esquema**, que define las clases de objetos y atributos incluidos en el directorio, las restricciones y los límites de las instancias de estos objetos y el formato de sus nombres. Para obtener más información sobre el esquema, vea esquema.


* **Catálogo global** que contiene información sobre todos los objetos del directorio. Esto permite a los usuarios y administradores buscar información de directorio independientemente del dominio del directorio que contenga realmente los datos. Para obtener más información acerca del catálogo global, vea el rol del catálogo global.


* Un **mecanismo de consulta e índice**, de modo que los usuarios o las aplicaciones de red puedan publicar y encontrar los objetos y sus propiedades. Para obtener más información sobre cómo consultar el directorio, vea buscar información de directorio.


* Un **servicio de replicación** que distribuye los datos de directorio a través de una red. Todos los controladores de dominio de un dominio participan en la replicación y contienen una copia completa de toda la información de directorio de su dominio. Cualquier cambio en los datos del directorio se replica en todos los controladores de dominio del dominio. Para obtener más información acerca de la replicación de Active Directory, consulte información general sobre la replicación.

## <a name="understanding-active-directory"></a>Descripción de Active Directory
 En esta sección se proporcionan vínculos a conceptos básicos de Active Directory:
 
* [Tecnologías de almacenamiento y estructura de Active Directory](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Roles de controlador de dominio](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Esquema de Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771796(v=ws.10))
* [Descripción de las confianzas](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771568(v=ws.10)) 
* [Tecnologías de replicación Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory tecnologías de búsqueda y publicación](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* [Interoperar con DNS y directiva de grupo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197486(v=ws.10))
* [Descripción del esquema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Para obtener una lista detallada de los conceptos de Active Directory, vea [Descripción de Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx). 


