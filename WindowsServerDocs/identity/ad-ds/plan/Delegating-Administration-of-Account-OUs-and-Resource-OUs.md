---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delegar la administración de unidades organizativas de cuentas y unidades organizativas de recursos
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70900b44de85774e8e595f9691885d67682fa753
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834266"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delegar la administración de unidades organizativas de cuentas y unidades organizativas de recursos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unidades organizativas (OU) de cuenta contienen objetos de usuario, grupo y equipo. Las unidades organizativas de recursos contienen recursos y las cuentas que son responsables de administrar esos recursos. El propietario del bosque es responsable de crear una estructura de unidades Organizativas para administrar estos recursos y los objetos y para delegar el control de esa estructura al propietario de la unidad organizativa.  
  
## <a name="delegating-administration-of-account-ous"></a>Delegar la administración de unidades organizativas de cuenta  
Delegar una estructura de unidades Organizativas de cuenta a los administradores de datos si necesita crear y modificar objetos de usuario, grupo y equipo. La estructura de unidades Organizativas de cuenta es un subárbol de unidades organizativas para cada tipo de cuenta que debe controlarse de forma independiente. Por ejemplo, el propietario de la unidad organizativa puede delegar el control específico a los administradores de datos distintos a través de unidades organizativas secundarias en una cuenta de la unidad organizativa para los usuarios, equipos, grupos y cuentas de servicio.  
  
La siguiente ilustración muestra un ejemplo de una estructura de unidades Organizativas de cuenta.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
En la tabla siguiente enumera y describe las unidades organizativas secundarios posibles que se pueden crear en una estructura de unidades Organizativas de cuenta.  
  
|OU|Finalidad|  
|------|-----------|  
|Usuarios|Contiene cuentas de usuario para el personal no administrativo.|  
|Cuentas de servicio|Algunos servicios que requieren acceso a los recursos de red se ejecutan como cuentas de usuario. Esta unidad organizativa se crea para cuentas de usuario de servicio independiente de las cuentas de usuario contenidas en la unidad organizativa de usuarios. Además, la colocación de los distintos tipos de cuentas de usuario en unidades organizativas independientes permite administrándolos según sus requisitos administrativos específicos.|  
|Equipos|Contiene cuentas de equipos que no sean controladores de dominio.|  
|Grupos|Contiene grupos de todos los tipos excepto los grupos administrativos, que se administran por separado.|  
|Administradores|Contiene cuentas de usuario y grupo de administradores de datos en la estructura de unidades Organizativas de cuenta para que puedan administrarse por separado de los usuarios normales. Habilitar la auditoría de esta unidad organizativa, por lo que puede realizar un seguimiento de cambios a los usuarios administrativos y grupos.|  
  
La siguiente ilustración muestra un ejemplo de un diseño de grupo administrativo para una estructura de unidades Organizativas de cuenta.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Los grupos que administran las OU secundarias se conceden control total únicamente a través de la clase específica de los objetos que son responsables de administrar.  
  
Los tipos de grupos que se usan para delegar el control dentro de una estructura de unidades Organizativas se basan en donde se encuentran en relación con la estructura de OU que va a administrar las cuentas. Si las cuentas de usuario administrador y la estructura de OU existen dentro de un único dominio, los grupos que se creen para usarlos para la delegación deben ser grupos globales. Si su organización tiene un departamento que administra sus propias cuentas de usuario y existe en más de una región geográfica, podría tener un grupo de administradores de datos que son responsables de administrar unidades organizativas en más de un dominio de cuenta. Si las cuentas de todos los administradores de datos existen en un único dominio y tienen estructuras de unidades Organizativas en varios dominios a los que necesita delegar el control, asegúrese de esos miembros las cuentas administrativas de los grupos globales y delegar el control de las estructuras de unidad organizativa en cada uno dominio a esos grupos globales. Si las cuentas de administradores de datos a la que se delega el control de una estructura de OU proceden de varios dominios, debe usar un grupo universal. Grupos universales pueden contener usuarios de dominios diferentes y, por lo tanto, puede utilizarse para delegar el control en varios dominios.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delegar la administración de unidades organizativas de recursos  
Las unidades organizativas de recursos se usan para administrar el acceso a los recursos. El propietario del recurso de unidad organizativa crea cuentas de equipo para servidores que están unidos al dominio que incluyen recursos como recursos compartidos de archivos, bases de datos y las impresoras. El propietario de la unidad organizativa de recursos también crea grupos para controlar el acceso a esos recursos.  
  
La siguiente ilustración muestra dos posibles ubicaciones para el recurso de unidad organizativa.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
El recurso de unidad organizativa puede encontrarse en la raíz del dominio o como una unidad organizativa secundaria de la cuenta correspondiente unidad organizativa en la jerarquía de unidades Organizativas administrativa. Las unidades organizativas de recursos no tienen todas las unidades organizativas de secundario estándar. Equipos y grupos se colocan directamente en el recurso de unidad organizativa.  
  
El propietario de la unidad organizativa recursos propietario de los objetos dentro de la unidad organizativa pero no posee el propio contenedor de la unidad organizativa. Los propietarios de unidad organizativa recursos administración sólo equipo y los objetos de grupo; no pueden crear otras clases de objetos dentro de la unidad organizativa, y no pueden crear unidades organizativas secundarias.  
  
> [!NOTE]  
> El creador o propietario de un objeto tiene la capacidad para establecer la lista de control de acceso (ACL) en el objeto independientemente de los permisos que se heredan desde el contenedor primario. Si un propietario de la unidad organizativa de recursos puede restablecer la ACL en una unidad organizativa, ese propietario puede crear cualquier clase de objeto en la unidad organizativa, incluidos los usuarios. Por este motivo, no se permiten los propietarios de unidad organizativa de recursos para crear unidades organizativas.  
  
Para cada recurso de unidad organizativa del dominio, cree un grupo global para representar los administradores de datos que son responsables de administrar el contenido de la unidad organizativa. Este grupo tiene control total sobre los objetos de equipo y de grupo en la unidad organizativa pero no sobre el propio contenedor de la unidad organizativa.  
  
La siguiente ilustración muestra el diseño de grupo administrativo para un recurso de unidad organizativa.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Colocar las cuentas de equipo en un recurso de unidad organizativa ofrece el control de propietario de la unidad organizativa a través de los objetos de cuenta, pero no realizar al propietario de la unidad organizativa de un administrador de los equipos. En un dominio de Active Directory, el grupo Admins. del dominio, de forma predeterminada, se encuentra en el grupo de administradores local en todos los equipos. Es decir, los administradores de servicios tienen control sobre los equipos. Si los propietarios de unidad organizativa de recursos requieren control administrativo sobre los equipos de sus unidades organizativas, el propietario del bosque puede aplicar una directiva de grupo de grupos restringidos para realizar al propietario del recurso de unidad organizativa en un miembro del grupo Administradores en los equipos de esa unidad organizativa.  
  


