---
title: Información general de Active Directory Domain Services
description: Seguridad de Windows Server
ms.prod: windows-server
ms.technology: security-auditing
ms.topic: article
ms.assetid: 6cfe9479-5d17-41d5-939a-891e5233fdca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 9c2182139ee7f891cd026545fecc69610c0b00a6
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520281"
---
# <a name="overview-of-active-directory-domain-services"></a>Información general de Active Directory Domain Services

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Un directorio es una estructura jerárquica que almacena información acerca de los objetos de la red. Un servicio de directorio, como Active Directory Domain Services (AD DS), proporciona los métodos para almacenar los datos de directorio y hacer que estos datos estén disponibles para los usuarios y administradores de la red. Por ejemplo, AD DS almacena información acerca de las cuentas de usuario, como nombres, contraseñas, números de teléfono, etc., y permite que otros usuarios autorizados de la misma red tengan acceso a dicha información.

Active Directory almacena información acerca de los objetos de una red y facilita su búsqueda y uso por parte de los usuarios y administradores. Active Directory usa un almacén de datos estructurado como base para una organización jerárquica lógica de la información del directorio.

Este almacén de datos, también conocido como directorio, contiene información sobre los objetos de Active Directory. Estos objetos suelen incluir recursos compartidos como servidores, volúmenes, impresoras y las cuentas de equipo y usuario de red. Para obtener más información sobre el almacén de datos de Active Directory, vea [almacén de datos de directorio](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

La seguridad se integra con Active Directory a través de la autenticación de inicio de sesión y el control de acceso a los objetos del directorio. Con un único inicio de sesión de red, los administradores pueden administrar los datos del directorio y la organización a través de su red, y los usuarios de red autorizados pueden tener acceso a los recursos en cualquier parte de la red. La administración basada en directiva facilita la administración de incluso las redes más complejas. Para obtener más información sobre la seguridad de Active Directory, consulte información general sobre seguridad.

Active Directory también incluye:
* Conjunto de reglas, **el esquema**, que define las clases de objetos y atributos incluidos en el directorio, las restricciones y los límites de las instancias de estos objetos y el formato de sus nombres. Para obtener más información sobre el esquema, vea esquema.


* **Catálogo global** que contiene información sobre todos los objetos del directorio. Esto permite a los usuarios y administradores buscar información de directorio independientemente del dominio del directorio que contenga realmente los datos. Para obtener más información acerca del catálogo global, vea el rol del catálogo global.


* Un **mecanismo de consulta e índice**, de modo que los usuarios o las aplicaciones de red puedan publicar y encontrar los objetos y sus propiedades. Para obtener más información sobre cómo consultar el directorio, vea buscar información de directorio.


* Un **servicio de replicación** que distribuye los datos de directorio a través de una red. Todos los controladores de dominio de un dominio participan en la replicación y contienen una copia completa de toda la información de directorio de su dominio. Cualquier cambio en los datos del directorio se replica en todos los controladores de dominio del dominio. Para obtener más información acerca de la replicación de Active Directory, consulte información general sobre la replicación.

## <a name="understanding-active-directory"></a>Descripción de Active Directory
 En esta sección se proporcionan vínculos a conceptos básicos de Active Directory:

* [Tecnologías de almacenamiento y estructura de Active Directory](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Roles de controlador de dominio](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)
* Esquema de Active Directory
* [Descripción de confianzas](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx)
* [Tecnologías de replicación Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx)
* [Active Directory tecnologías de búsqueda y publicación](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx)
* Interoperar con DNS y directiva de grupo
* [Descripción del esquema](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx)

Para obtener una lista detallada de los conceptos de Active Directory, vea [Descripción de Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx).

