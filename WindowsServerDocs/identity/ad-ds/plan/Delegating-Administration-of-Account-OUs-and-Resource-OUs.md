---
description: Más información acerca de cómo delegar la administración de las unidades organizativas de cuentas y las unidades organizativas de recursos
ms.assetid: 19feca0e-a6d0-4d27-93b0-cb44f8c26484
title: Delegar la administración de unidades organizativas de cuentas y unidades organizativas de recursos
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 432f68a113055f9e68ca4c18ed8aa54b02cad8b0
ms.sourcegitcommit: d2224cf55c5d4a653c18908da4becf94fb01819e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97711710"
---
# <a name="delegating-administration-of-account-ous-and-resource-ous"></a>Delegar la administración de unidades organizativas de cuentas y unidades organizativas de recursos

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Las unidades organizativas (OU) de cuenta contienen objetos de usuario, grupo y equipo. Las unidades organizativas de recursos contienen recursos y las cuentas que son responsables de administrar esos recursos. El propietario del bosque es responsable de crear una estructura de unidades organizativas para administrar estos objetos y recursos, así como para delegar el control de esa estructura en el propietario de la unidad organizativa.

## <a name="delegating-administration-of-account-ous"></a>Delegación de la administración de unidades organizativas de cuentas
Delegue una estructura de unidad organizativa de cuentas en administradores de datos si necesitan crear y modificar objetos de usuario, grupo y equipo. La estructura de la unidad organizativa cuenta es un subárbol de unidades organizativas para cada tipo de cuenta que se debe controlar de forma independiente. Por ejemplo, el propietario de la unidad organizativa puede delegar el control específico a varios administradores de datos a través de unidades organizativas secundarias en una unidad organizativa de cuentas para usuarios, equipos, grupos y cuentas de servicio.

En la ilustración siguiente se muestra un ejemplo de una estructura de unidad organizativa de cuentas.

![Ilustración en la que se muestra un ejemplo de una estructura de unidad organizativa.](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/66d38fbe-e8eb-42d7-abab-9526243bf6d9.gif)

En la tabla siguiente se enumeran y describen las posibles unidades organizativas secundarias que se pueden crear en una estructura de unidad organizativa de cuentas.

|OU|Propósito|
|------|-----------|
|Usuarios|Contiene cuentas de usuario para personal no administrativo.|
|Cuentas de servicio|Algunos servicios que requieren acceso a los recursos de red se ejecutan como cuentas de usuario. Esta unidad organizativa se crea para separar las cuentas de usuario de servicio de las cuentas de usuario contenidas en la unidad organizativa usuarios. Además, la colocación de los distintos tipos de cuentas de usuario en unidades organizativas independientes le permite administrarlos de acuerdo con sus requisitos administrativos específicos.|
|Computers|Contiene cuentas para equipos que no son controladores de dominio.|
|Grupos|Contiene grupos de todos los tipos, excepto los grupos administrativos, que se administran por separado.|
|Administradores|Contiene cuentas de usuario y de grupo para los administradores de datos de la estructura de la unidad organizativa de cuentas para permitir que se administren de forma independiente de los usuarios normales. Habilite la auditoría de esta unidad organizativa para que pueda realizar un seguimiento de los cambios en los usuarios y grupos administrativos.|

En la ilustración siguiente se muestra un ejemplo de un diseño de grupo administrativo para una estructura de unidad organizativa.

![Ilustración en la que se muestra un ejemplo de un diseño de grupo administrativo para una estructura de unidad organizativa.](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/be2cd2d2-6956-429c-a53a-369e6fe40b2b.gif)

A los grupos que administran las unidades organizativas secundarias solo se les concede control total sobre la clase específica de objetos que son responsables de la administración.

Los tipos de grupos que se usan para delegar el control dentro de una estructura de unidad organizativa se basan en el lugar en el que se encuentran las cuentas en relación con la estructura de la unidad organizativa que se va a administrar. Si todas las cuentas de usuario administrador y la estructura de la unidad organizativa existen en un solo dominio, los grupos que cree para usar para la delegación deben ser grupos globales. Si su organización tiene un departamento que administra sus propias cuentas de usuario y existe en más de una región geográfica, es posible que tenga un grupo de administradores de datos que sean responsables de administrar las unidades organizativas de la cuenta en más de un dominio. Si todas las cuentas de los administradores de datos existen en un solo dominio y tiene estructuras de unidad organizativa en varios dominios en los que es necesario delegar el control, haga que dichos miembros de las cuentas administrativas de los grupos globales y dedeleguen el control de las estructuras de las unidades organizativas de cada dominio en esos grupos globales. Si las cuentas de administradores de datos en las que delega el control de una estructura de unidad organizativa proceden de varios dominios, debe usar un grupo universal. Los grupos universales pueden contener usuarios de distintos dominios y, por lo tanto, se pueden usar para delegar el control en varios dominios.

## <a name="delegating-administration-of-resource-ous"></a>Delegación de la administración de las unidades organizativas de recursos
Las unidades organizativas de recursos se usan para administrar el acceso a los recursos. El propietario de la unidad organizativa de recursos crea cuentas de equipo para los servidores que están Unidos al dominio que incluyen recursos como recursos compartidos de archivos, bases de datos e impresoras. El propietario de la unidad organizativa de recursos también crea grupos para controlar el acceso a esos recursos.

En la ilustración siguiente se muestran las dos ubicaciones posibles para la unidad organizativa de recursos.

![Ilustración que muestra las dos ubicaciones posibles para la unidad organizativa de recursos.](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/6667a5ce-34d6-48a9-9974-b823ba70e2af.gif)

La unidad organizativa de recursos se puede encontrar en la raíz del dominio o como una unidad organizativa secundaria de la unidad organizativa de la cuenta correspondiente en la jerarquía administrativa de la unidad organizativa. Las unidades organizativas de recursos no tienen unidades organizativas secundarias estándar. Los equipos y grupos se colocan directamente en la unidad organizativa de recursos.

El propietario de la unidad organizativa de recursos posee los objetos dentro de la unidad organizativa, pero no posee el propio contenedor de la unidad organizativa. Los propietarios de las unidades organizativas de recursos solo administran objetos de equipo y grupo; no pueden crear otras clases de objetos dentro de la unidad organizativa y no pueden crear unidades organizativas secundarias.

> [!NOTE]
> El creador o propietario de un objeto tiene la capacidad de establecer la lista de control de acceso (ACL) en el objeto, independientemente de los permisos que se heredan del contenedor principal. Si el propietario de una unidad organizativa de recursos puede restablecer la ACL en una unidad organizativa, ese propietario puede crear cualquier clase de objeto en la unidad organizativa, incluidos los usuarios. Por esta razón, no se permite a los propietarios de la OU de recursos crear unidades organizativas.

Para cada unidad organizativa de recursos del dominio, cree un grupo global que represente a los administradores de datos responsables de administrar el contenido de la unidad organizativa. Este grupo tiene control total sobre los objetos de grupo y equipo en la unidad organizativa, pero no sobre el propio contenedor de la unidad organizativa.

En la ilustración siguiente se muestra el diseño de grupo administrativo de una unidad organizativa de recursos.

![Delegación de la administración](media/Delegating-Administration-of-Account-OUs-and-Resource-OUs/8a3f7714-a3bf-43f7-b999-6070543248b0.gif)

La colocación de las cuentas de equipo en una unidad organizativa de recursos proporciona al propietario de la unidad organizativa el control sobre los objetos de cuenta, pero no hace que el propietario de la unidad organizativa sea administrador de los equipos. En un dominio de Active Directory, el grupo Admins. del dominio, de forma predeterminada, se coloca en el grupo de administradores locales en todos los equipos. Es decir, los administradores de servicios tienen control sobre esos equipos. Si los propietarios de las unidades organizativas de recursos requieren un control administrativo sobre los equipos de sus unidades organizativas, el propietario del bosque puede aplicar un directiva de grupo de grupos restringidos para convertir el propietario de la unidad organizativa de recursos en miembro del grupo administradores en los equipos de esa unidad organizativa.



