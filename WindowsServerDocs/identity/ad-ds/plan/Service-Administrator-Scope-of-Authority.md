---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: "Ámbito del Administrador de servicios de autoridad"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba682067b9c7a7b4bf583482abe6470b60b25450
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="service-administrator-scope-of-authority"></a>Ámbito del Administrador de servicios de autoridad

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si decides participar en un bosque de Active Directory, debe confiar en el propietario de bosque y los administradores de servicio. Los propietarios de bosque son responsables de seleccionar y administrar los administradores de servicio; por lo tanto, cuando es el propietario de un bosque de confianza, también confiar los administradores de servicio que administra el propietario del bosque. Estos administradores de servicio tienen acceso a todos los recursos en el bosque. Antes de decidir participar en un bosque, es importante comprender que el propietario de bosque y los administradores de servicio tendrá acceso completo a los datos. No puede evitar este acceso.  
  
Todos los administradores de servicio en un bosque tienen control total sobre todos los datos y servicios en todos los equipos en el bosque. Los administradores de servicio tienen la capacidad de hacer lo siguiente:  
  
-   Corregir los errores en las listas de control de acceso (ACL) de objetos. Esto permite al administrador de servicio leer, modificar o eliminar objetos, independientemente de las ACL que se establecen en esos objetos.  
  
-   Modifica el software del sistema en un controlador de dominio para omitir las comprobaciones de seguridad normales. Esto permite al administrador de servicio ver o manipular los objetos en el dominio, independientemente de las ACL en el objeto.  
  
-   Usar la directiva de seguridad de grupos restringidos para conceder a cualquier acceso administrativo de usuario o grupo de cualquier equipo unido al dominio. De este modo, los administradores de servicio pueden obtener un control de cualquier equipo unido al dominio independientemente de las intenciones del propietario del equipo.  
  
-   Restablecer las contraseñas o cambiar la pertenencia a grupos de usuarios.  
  
-   Obtener acceso a otros dominios del bosque modificando el software del sistema en un controlador de dominio. Los administradores de servicios pueden afectar al funcionamiento de cualquier dominio en el bosque, vista o manipular los datos de configuración del bosque, ver o manipular los datos almacenados en cualquier dominio y ver o manipular los datos almacenados en cualquier equipo unido al bosque.  
  
Por este motivo, los grupos que almacena datos en unidades organizativas (OU en el bosque) y que los equipos de unión a un bosque deben confiar en los administradores de servicio. Para que un grupo para unirse a un bosque, debe elegir todos los administradores de servicio en el bosque de confianza. Esto implica asegurarse de que:  
  
-   El propietario del bosque puede ser de confianza para que actúe en los intereses del grupo y no tiene el motivo para que actúe de forma malintencionada en el grupo.  
  
-   El propietario del bosque correctamente restringe el acceso físico a controladores de dominio. Controladores de dominio dentro de un bosque no pueden estar aislados entre sí. Es posible que un atacante que tenga acceso físico a un controlador de dominio para realizar cambios sin conexión para la base de datos del directorio y, al hacerlo, interferir con el funcionamiento de cualquier dominio del bosque, ver o manipular los datos almacenados en cualquier parte del bosque y ver o manipular los datos almacenados en cualquier equipo unido al bosque. Por este motivo, el acceso físico a controladores de dominio debe restringirse al personal de confianza.  
  
-   Comprender y acepta el riesgo potencial que los administradores de servicio pueden convertirse en peligro la seguridad del sistema de confianza.  
  
Algunos grupos que podría determinar que los beneficios de colaboración y ahorro de participar en una infraestructura compartida superen los riesgos que los administradores de servicio se uso inadecuado o se convertirán en uso indebido de su autoridad. Estos grupos pueden compartir un bosque y se usa unidades organizativas para delegar la autoridad. Sin embargo, es posible que otros grupos no aceptará este riesgo porque las consecuencias de una amenaza de seguridad son demasiado graves. Estos grupos requieren bosques independientes.  
  


