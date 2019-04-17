---
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: "Delegar la administración de unidades organizativas de la cuenta y unidades organizativas de recursos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 56e69bffdf8ecc0f37f9f5159ae6bce7fcdc49f8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delegar la administración de unidades organizativas de la cuenta y unidades organizativas de recursos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Unidades organizativas (OU) de cuenta contienen los objetos de grupo, usuario y del equipo. Unidades organizativas de recursos contienen recursos y las cuentas que se encarga de administrar esos recursos. El propietario del bosque es responsable de crear una estructura de unidad organizativa para administrar estos objetos y los recursos y de delegación del control de dicha estructura al propietario de la unidad organizativa.  
  
## <a name="delegating-administration-of-account-ous"></a>Delegar la administración de unidades organizativas de la cuenta  
Delegado una estructura de unidad organizativa de la cuenta a los administradores de datos si se necesitan para crear y modificar objetos de grupo, usuario y del equipo. La estructura de unidad organizativa de la cuenta es un subárbol de unidades organizativas para cada tipo de cuenta que debe ser controlada por separado. Por ejemplo, el propietario de la unidad organizativa puede delegar control específico para los administradores de datos distintos en unidades organizativas secundarios en una cuenta de la unidad organizativa para los usuarios, equipos, grupos y cuentas de servicio.  
  
La siguiente ilustración muestra un ejemplo de una estructura de unidad organizativa de la cuenta.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)  
  
La siguiente tabla se enumera y describe las unidades organizativas secundario posible que se pueden crear en una estructura de unidad organizativa de la cuenta.  
  
|UNIDAD ORGANIZATIVA|Propósito|  
|------|-----------|  
|Usuarios|Contiene las cuentas de usuario para el personal no administrativos.|  
|Cuentas de servicio|Algunos servicios que requieren acceso a recursos de red que se ejecutan como cuentas de usuario. Esta unidad organizativa se crea a cuentas de usuario de servicios independiente de las cuentas de usuario incluidas en la unidad organizativa de los usuarios. Además, la colocación de los diferentes tipos de cuentas de usuario en unidades organizativas independientes permite administrarlos según sus necesidades administrativas específicas.|  
|Equipos|Contiene cuentas de equipos que no sean de controladores de dominio.|  
|Grupos|Contiene grupos de todos los tipos excepto grupos administrativos, que se administran por separado.|  
|Administradores|Contiene las cuentas de usuario y grupo para que los administradores de datos en la estructura de unidad organizativa de la cuenta para que puedan administrarse por separado de usuarios normales. Habilitar la auditoría de esta unidad organizativa, por lo que puede controlar los cambios a los grupos y los usuarios administrativos.|  
  
La siguiente ilustración muestra un ejemplo de un diseño de grupo administrativo de una estructura de unidad organizativa de la cuenta.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)  
  
Los grupos que administran las unidades organizativas secundarias se otorgan control total solamente a través de la clase específica de objetos que son responsables de administrar.  
  
Los tipos de grupos que usas para delegar control dentro de una estructura de unidad organizativa se basan en donde se encuentran en relación con la estructura de unidad organizativa que se pueden administrar las cuentas. Si la estructura de unidad organizativa y las cuentas de usuario de administrador existen dentro de un dominio único, los grupos que creas para usar para la delegación deben ser grupos globales. Si tu organización tiene un departamento que administre sus propias cuentas de usuario y que existe en más de una región geográfica, podría tener un grupo de administradores de datos que es responsable de administrar las unidades organizativas de más de un dominio de la cuenta. Si las cuentas de todos los administradores de datos existen en un solo dominio y tiene las estructuras de la unidad organizativa en varios dominios que necesites delegar control, hacer que los miembros de las cuentas administrativas de grupos globales y delegar el control de las estructuras de unidad organizativa en cada dominio a esos grupos globales. Si las cuentas de los administradores de datos a la que se delega el control de una estructura de unidad organizativa provienen de varios dominios, debes usar un grupo universal. Grupos universales pueden contener los usuarios de distintos dominios y, por lo tanto, pueden usarse para delegar el control de varios dominios.  
  
## <a name="delegating-administration-of-resource-ous"></a>Delegar la administración de unidades organizativas de recursos  
Unidades organizativas de recursos se usan para administrar el acceso a los recursos. El propietario de la unidad organizativa de recursos crea cuentas de equipo para los servidores que están unidos al dominio que incluyen recursos, como impresoras, bases de datos y recursos compartidos de archivos. El propietario de la unidad organizativa de recursos también crea grupos para controlar el acceso a esos recursos.  
  
La siguiente ilustración muestra las dos ubicaciones posibles para la unidad organizativa de recurso.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)  
  
El recurso de unidad organizativa puede encontrarse en la raíz del dominio o como una unidad organizativa secundaria de la unidad organizativa de cuenta correspondiente en la jerarquía de unidad organizativa administrativa. Unidades organizativas de recursos no tienen sus unidades organizativas secundarias estándar. Equipos y grupos se colocan directamente en el recurso de la unidad organizativa.  
  
El propietario de la unidad organizativa de recursos es el propietario de los objetos en la unidad organizativa, pero no posee el contenedor de la unidad organizativa. Los propietarios de unidad organizativa de recurso Administración solo equipo y objetos de grupo; no pueden crear otras clases de objetos dentro de la unidad organizativa y no pueden crear unidades organizativas secundarios.  
  
> [!NOTE]  
> El creador o el propietario de un objeto tiene la capacidad de establecer la lista de control de acceso (ACL) en el objeto independientemente de los permisos que se heredan desde el contenedor primario. Si el propietario de unidad organizativa de un recurso puede restablecer las ACL en una unidad organizativa, dicho propietario puede crear cualquier clase de objeto en la unidad organizativa, incluidos los usuarios. Por este motivo, los propietarios de unidad organizativa de recursos no tienen permiso para crear unidades organizativas.  
  
Para cada unidad organizativa en el dominio del recurso, crea un grupo global para representar los administradores de los datos que son responsables de administrar el contenido de la unidad organizativa. Este grupo tiene control total sobre los objetos de grupo y el equipo en la unidad organizativa, pero no en el propio contenedor de la unidad organizativa.  
  
La siguiente ilustración muestra el diseño de grupo administrativo de un recurso de unidad organizativa.  
  
![delegar la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)  
  
Al colocar las cuentas de equipo en un recurso de unidad organizativa te ofrece el control de propietario de la unidad organizativa sobre los objetos de la cuenta, pero no hace que el Administrador de los equipos el propietario de la unidad organizativa. En un dominio de Active Directory, el grupo de administradores de dominio, de manera predeterminada, se encuentra en el grupo de administradores local en todos los equipos. Es decir, los administradores de servicio tienen control sobre los equipos. Si los propietarios de unidad organizativa de recursos requieren control administrativo sobre los equipos de sus unidades organizativas, el propietario del bosque puede aplicar una directiva de grupo de grupos restringidos para hacer que el propietario de la unidad organizativa de recursos un miembro del grupo Administradores en los equipos en esa unidad organizativa.  
  


