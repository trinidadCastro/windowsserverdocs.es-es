---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Ámbito de autoridad del administrador de servicios
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b5bf2fb3b06a47d730b9dd124b2b66a0a4c9c691
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864856"
---
# <a name="service-administrator-scope-of-authority"></a>Ámbito de autoridad del administrador de servicios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si opta por participar en un bosque de Active Directory, debe confiar en el propietario del bosque y los administradores de servicios. Los propietarios de bosque son responsables de seleccionar y administrar los administradores de servicios; por lo tanto, si confía en el propietario de un bosque, también confiar los administradores de servicios que administra el propietario del bosque. Estos administradores de servicios tienen acceso a todos los recursos del bosque. Antes de tomar la decisión para participar en un bosque, es importante comprender que el propietario del bosque y los administradores de servicios tendrán acceso total a los datos. No se puede evitar que este acceso.  
  
Todos los administradores de servicios en un bosque tienen control total sobre todos los servicios y datos en todos los equipos en el bosque. Los administradores de servicios tienen la capacidad de hacer lo siguiente:  
  
-   Corrija los errores en las listas de control de acceso (ACL) de objetos. Esto permite al administrador de servicio leer, modificar o eliminar objetos independientemente de las ACL que se establecen en esos objetos.  
  
-   Modificar el software del sistema en un controlador de dominio para omitir las comprobaciones de seguridad normal. Esto permite que el Administrador de servicios ver o manipular cualquier objeto en el dominio, independientemente de la ACL en el objeto.  
  
-   Use la directiva de seguridad de grupos restringidos para conceder a cualquier usuario o grupo acceso administrativo a cualquier equipo unido al dominio. De este modo, los administradores de servicios pueden obtener el control de cualquier equipo unido al dominio, independientemente de las intenciones del propietario del equipo.  
  
-   Restablecer las contraseñas o cambiar la pertenencia a grupo para los usuarios.  
  
-   Obtener acceso a otros dominios del bosque modificando el software del sistema en un controlador de dominio. Los administradores de servicios pueden afectar al funcionamiento de cualquier dominio del bosque, vista o manipular los datos de configuración del bosque, ver o manipular los datos almacenados en cualquier dominio y ver o manipular los datos almacenados en cualquier equipo unido al bosque.  
  
Por este motivo, los grupos que almacenan datos en unidades organizativas (OU) en el bosque y unir equipos a un bosque deben confiar en los administradores de servicios. Para que un grupo para unirse a un bosque, debe elegir todos los administradores de servicios en el bosque de confianza. Esto implica asegurarse de:  
  
-   El propietario del bosque puede ser de confianza para que actúe en aras del grupo y no tiene razón para las operaciones de forma malintencionada con el grupo.  
  
-   El propietario del bosque adecuadamente restringe el acceso físico a controladores de dominio. Los controladores de dominio dentro de un bosque no pueden ser completamente aislados entre sí. Es posible que un atacante que tenga acceso físico a un controlador de dominio para realizar cambios sin conexión para la base de datos de directorio y, al hacerlo, interferir con el funcionamiento de cualquier dominio del bosque, ver o manipular los datos almacenados en cualquier lugar en el bosque y ver o manipular los datos almacenados en cualquier equipo unido al bosque. Por este motivo, el acceso físico a controladores de dominio debe limitarse al personal de confianza.  
  
-   Comprenda y acepte el posible riesgo de que los administradores de servicios pueden convertirse en poner en peligro la seguridad del sistema de confianza.  
  
Algunos grupos podrían determinar que las ventajas de colaboración y ahorro de costos de participar en una infraestructura compartida compensan los riesgos que los administradores de servicios se hacer un uso incorrecto o se convertirán en haciendo un uso indebido su autoridad. Estos grupos pueden compartir un bosque y usar unidades organizativas para delegar la autoridad. Sin embargo, otros grupos podrían aceptar este riesgo porque las consecuencias de un riesgo de seguridad son demasiado graves. Estos grupos requieren bosques separados.  
  


