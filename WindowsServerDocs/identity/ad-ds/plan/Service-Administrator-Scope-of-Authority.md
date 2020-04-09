---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Ámbito de autoridad del administrador de servicios
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3a891ade46fdee1dffc35df31a11c6d138e71e8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821898"
---
# <a name="service-administrator-scope-of-authority"></a>Ámbito de autoridad del administrador de servicios

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si decide participar en un bosque de Active Directory, debe confiar en el propietario del bosque y en los administradores de servicios. Los propietarios del bosque son responsables de seleccionar y administrar los administradores de servicios; por lo tanto, cuando confía en el propietario de un bosque, también confía en los administradores de servicios que administra el propietario del bosque. Estos administradores de servicios tienen acceso a todos los recursos del bosque. Antes de tomar la decisión de participar en un bosque, es importante comprender que el propietario del bosque y los administradores de servicios tendrán acceso completo a los datos. No se puede evitar este acceso.  
  
Todos los administradores de servicios de un bosque tienen control total sobre todos los datos y servicios de todos los equipos del bosque. Los administradores de servicios tienen la capacidad de hacer lo siguiente:  
  
-   Corrija los errores en las listas de control de acceso (ACL) de los objetos. Esto permite al administrador de servicios leer, modificar o eliminar objetos, independientemente de las ACL que se establezcan en esos objetos.  
  
-   Modifique el software del sistema en un controlador de dominio para omitir las comprobaciones de seguridad normales. Esto permite al administrador de servicios ver o manipular cualquier objeto del dominio, independientemente de la ACL en el objeto.  
  
-   Use la Directiva de seguridad grupos restringidos para conceder a cualquier usuario o grupo acceso administrativo a cualquier equipo unido al dominio. De este modo, los administradores de servicios pueden obtener el control de cualquier equipo unido al dominio, independientemente de las intenciones del propietario del equipo.  
  
-   Restablecer contraseñas o cambiar la pertenencia a grupos para los usuarios.  
  
-   Obtener acceso a otros dominios del bosque modificando el software del sistema en un controlador de dominio. Los administradores de servicios pueden afectar al funcionamiento de cualquier dominio del bosque, ver o manipular los datos de configuración del bosque, ver o manipular los datos almacenados en cualquier dominio y ver o manipular los datos almacenados en cualquier equipo unido al bosque.  
  
Por esta razón, los grupos que almacenan datos en unidades organizativas (OU) en el bosque y que unen equipos a un bosque deben confiar en los administradores de servicios. Para que un grupo se una a un bosque, debe elegir confiar en todos los administradores de servicios en el bosque. Esto implica asegurarse de que:  
  
-   Se puede confiar en el propietario del bosque para que actúe en interés del grupo y no tenga razón para que actúe de forma malintencionada con respecto al grupo.  
  
-   El propietario del bosque restringe de forma adecuada el acceso físico a los controladores de dominio. Los controladores de dominio dentro de un bosque no se pueden aislar entre sí. Es posible que un atacante que tenga acceso físico a un solo controlador de dominio realice cambios sin conexión en la base de datos de directorio y, al hacerlo, interfiera con el funcionamiento de cualquier dominio del bosque, vea o manipule los datos almacenados en cualquier lugar del bosque y vea o manipule los datos almacenados en cualquier equipo unido al bosque. Por esta razón, el acceso físico a los controladores de dominio debe estar restringido a personal de confianza.  
  
-   Comprende y acepta el riesgo potencial de que los administradores de servicios de confianza se puedan convertir en poner en peligro la seguridad del sistema.  
  
Algunos grupos pueden determinar que las ventajas de colaboración y de ahorro de costos de participar en una infraestructura compartida superan los riesgos que los administradores de servicio van a usar o se convertirán en un mal uso de su autoridad. Estos grupos pueden compartir un bosque y usar unidades organizativas para delegar la autoridad. Sin embargo, es posible que otros grupos no acepten este riesgo porque las consecuencias de un riesgo en la seguridad son demasiado graves. Estos grupos requieren bosques independientes.  
  


