---
title: Consideraciones de red y cuentas de usuario
description: Proporciona información de planeación para diferentes escenarios de usuario y la red con MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9133f28d2c3b36b18a2b6bc81d238835156bf447
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880806"
---
# <a name="network-considerations-and-user-accounts"></a>Consideraciones de red y cuentas de usuario
MultiPoint Services pueden implementarse en una variedad de entornos de red y sea compatible con cuentas de usuario locales y cuentas de usuario de dominio. Por lo general, las cuentas de usuario de MultiPoint Services se administrarán en uno de los siguientes entornos de red:  
  
-   Un equipo que ejecuta MultiPoint Services con cuentas de usuario local  
  
-   Varios equipos que ejecuten MultiPoint Services, cada uno con una cuenta de usuario local  
  
-   Varios equipos que ejecuten MultiPoint Services y están usando las cuentas de usuario de dominio

Por definición, *cuentas de usuario locales* sólo se puede acceder desde el equipo en el que se crearon. Las cuentas de usuario local son cuentas de usuario que se crean en un equipo específico que está ejecutando MultiPoint Services. En cambio, *cuentas de usuario de dominio* son cuentas de usuario que residen en un controlador de dominio y se pueden acceder desde cualquier equipo que está conectado al dominio. Cuando decida qué tipo de entorno de red, tenga en cuenta lo siguiente:  
  
-   ¿Se compartirá los recursos entre servidores?  
  
-   ¿Los usuarios se se conmutación entre servidores?  
  
-   ¿Tendrá acceso a los usuarios a los servidores de base de datos que requieren autenticación?  
  
-   ¿Tendrá acceso a los usuarios a los servidores web internos que requieren autenticación?  
  
-   ¿Hay una infraestructura de dominio de Active Directory existente en su lugar?  
  
-   Que va a utilizar el Administrador de MultiPoint de la consola para administrar escritorios de usuario, vista de miniaturas, agregar usuarios, limitar los sitios Web etc.? ¿A esta persona administrar más de un servidor? Esta persona debe tener privilegios administrativos en los servidores.  
  
Las secciones siguientes tratan la administración de cuentas de usuario en estos entornos de red.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Solo servidor MultiPoint con cuentas de usuario local  
En entornos con un único equipo que está ejecutando MultiPoint Services, no hay ningún requisito que una red. Sin embargo, para aprovechar las ventajas de los recursos de Internet, los requisitos de red pueden ser tan básicos como un enrutador y una conexión a un proveedor de servicios Internet (ISP). Se configuran las conexiones de red que están asociadas con un adaptador de red en MultiPoint Services, de forma predeterminada, para obtener una dirección IP y la dirección del servidor DNS automáticamente a través de DHCP. Los enrutadores de Internet normalmente se configuran como servidores DHCP y proporcionan direcciones IP privadas a equipos que se conectan a ellos en la red interna. Por lo tanto, un único equipo que ejecuta MultiPoint Services puede ser capaz de conectarse a la interfaz interna del enrutador, obtener información de IP automática y conectarse a Internet sin requerir un esfuerzo significativo o la configuración por un administrador.  
  
Es una manera común de administración de usuarios en este tipo de entorno crear una cuenta de usuario local para cada persona que tendrá acceso a del sistema. Cualquier persona que tenga una cuenta de usuario local en ese equipo puede iniciar sesión en MultiPoint Services desde cualquier estación que está asociado con el sistema. Cuentas de usuario locales pueden crearse y administrarse desde el Administrador de MultiPoint.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Varios sistemas MultiPoint Server con las cuentas de usuario local  
Dado que las cuentas de usuario local solo son accesibles desde el equipo en el que fueron creadas, al implementar varios sistemas MultiPoint Services en un entorno, puede administrar cuentas de usuario locales en uno de dos maneras:  
  
-   Puede crear cuentas de usuario para usuarios específicos en equipos específicos que ejecuta MultiPoint Services.  
  
-   Puede usar MultiPoint Manager para crear cuentas para cada usuario en cada equipo que ejecuta MultiPoint Services.  
  
Por ejemplo, si va a asignar a usuarios a un equipo específico que ejecuta MultiPoint Services, podría crear cuatro cuentas de usuario local en el equipo (usuario01, user02, user03 y user04) y cuatro de las cuentas de usuario local en el equipo B (user05, user06, user07 y user08). En este escenario, los usuarios 01\-04 puede iniciar sesión en un equipo desde cualquier estación que esté conectada a ella; sin embargo, no se puede iniciar sesión en el equipo B. Lo mismo puede decirse de los usuarios 05\-08, que se puede iniciar sesión solo al equipo B, pero no al equipo A. según el entorno de implementación específicos, puede ser aceptable o incluso deseable.  
  
Sin embargo, si cada usuario debe ser capaz de iniciar sesión en cualquiera de los equipos que ejecutan MultiPoint Services, debe crearse una cuenta de usuario local para cada usuario en cada equipo que está ejecutando MultiPoint Services. Decidir administrar los usuarios de esta manera presenta ciertas complejidades. Por ejemplo, si usuario01 inicia sesión en el equipo A lunes y guarda un archivo en la carpeta documentos y, a continuación, el usuario inicia sesión en equipo B el martes, el archivo que se guardó en la carpeta de documentos en un equipo no esté accesible en el equipo B.  
  
Además, si un usuario tiene una cuenta en los equipos A y B, no hay ninguna manera de sincronizar automáticamente las contraseñas de las cuentas. Esto puede dar lugar a que los usuarios que tienen dificultades para iniciar sesión debe cambiarse la contraseña de cuenta en un equipo, pero no el otro. Puede simplificar la administración de cuentas de usuario en este tipo de entorno de red mediante la asignación de cada usuario a un único equipo que está ejecutando MultiPoint Services. De este modo, el usuario puede iniciar sesión en cualquiera de las estaciones que están asociados a ese equipo y tener acceso a los archivos adecuados.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Varios sistemas MultiPoint Services con cuentas de dominio  
Entornos de dominio son comunes en grandes entornos de red que incluyan varios servidores. Por ejemplo, podría unir uno o varios de los equipos que ejecutan el rol Servicios MultiPoint a un dominio y, a continuación, usar Microsoft Active Directory para administrar cuentas de usuario que se pueden acceder desde cualquier equipo en el dominio. Esto permite que las cuentas de usuario de dominio individuales crear y tener acceso desde cualquier estación en cualquier sistema MultiPoint Services que se ha unido al dominio.  
 
Cuando implemente servicios MultiPoint en un entorno de dominio, hay varios factores a tener en cuenta:  
  
-   Si se utilizan cuentas de dominio, no puede administrar desde el Administrador de MultiPoint.  
  
-   De forma predeterminada, MultiPoint Services está configurado para dar a cada usuario permiso para iniciar sesión en solo una estación a la vez. Si decide permitir que los usuarios inicien sesión en varias estaciones al mismo tiempo con una única cuenta, puede usar el **Editar configuración del servidor** opción en MultiPoint Manager.  
  
-   La ubicación de los controladores de dominio puede afectar a la velocidad y confiabilidad con el que los usuarios podrán para autenticarse con el dominio y busque los recursos.  
  
## <a name="single-user-account-for-multiple-stations"></a>Cuenta de usuario único para varias estaciones  
MultiPoint Services tiene la capacidad para iniciar sesión en varias estaciones en el mismo equipo simultáneamente con una cuenta de usuario único. Esta característica es útil en entornos donde los usuarios no tienen nombres de usuario único, y con una cuenta de usuario único puede simplificar la administración del sistema MultiPoint Services.  
  
