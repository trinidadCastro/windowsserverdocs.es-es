---
title: Consideraciones de red y cuentas de usuario
description: Proporciona información de planeación para diferentes escenarios de redes y usuarios con Multipoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef4859fc-b7ae-4827-ab9c-b1dc07ab6c16
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 5369776a0341bf1f4d4d1d13569cf0964fdf11f1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405035"
---
# <a name="network-considerations-and-user-accounts"></a>Consideraciones de red y cuentas de usuario
Multipoint Services se puede implementar en varios entornos de red y puede admitir cuentas de usuario locales y cuentas de usuario de dominio. Por lo general, las cuentas de usuario de Multipoint Services se administran en uno de los siguientes entornos de red:  
  
-   Un solo equipo que ejecuta Multipoint Services con cuentas de usuario locales  
  
-   Varios equipos que ejecutan Multipoint Services, cada uno con una cuenta de usuario local  
  
-   Varios equipos que ejecutan Multipoint Services y que usan cuentas de usuario de dominio

Por definición, solo se puede tener acceso a *las cuentas de usuario locales* desde el equipo en el que se crearon. Las cuentas de usuario locales son cuentas de usuario que se crean en un equipo específico que ejecuta Multipoint Services. Por el contrario, *las cuentas de usuario de dominio* son cuentas de usuario que residen en un controlador de dominio y se puede tener acceso a ellas desde cualquier equipo que esté conectado al dominio. Cuando decida qué tipo de entorno de red usar, tenga en cuenta lo siguiente:  
  
-   ¿Se compartirán los recursos entre los servidores?  
  
-   ¿Los usuarios cambiarán entre servidores?  
  
-   ¿Accederán los usuarios a los servidores de bases de datos que requieren autenticación?  
  
-   ¿Accederán los usuarios a los servidores Web internos que requieran autenticación?  
  
-   ¿Existe alguna infraestructura de dominio de Active Directory existente?  
  
-   ¿Quién va a usar la consola de Multipoint Manager para administrar escritorios de usuario, ver miniaturas, agregar usuarios, limitar sitios web, etc.? ¿Esta persona va a administrar más de un servidor? Esta persona debe tener privilegios administrativos en los servidores.  
  
En las secciones siguientes se aborda la administración de cuentas de usuario en estos entornos de red.  
  
## <a name="single-multipoint-server-with-local-user-accounts"></a>Un solo servidor multipoint con cuentas de usuario locales  
En entornos con un solo equipo que ejecuta Multipoint Services, no es necesario tener una red. Sin embargo, para aprovechar los recursos de Internet, los requisitos de red pueden ser tan básicos como un enrutador y una conexión a un proveedor de servicios Internet (ISP). Las conexiones de red que están asociadas a un adaptador de red en Multipoint Services están configuradas de forma predeterminada para obtener una dirección IP y una dirección de servidor DNS automáticamente a través de DHCP. Los enrutadores de Internet normalmente se configuran como servidores DHCP y proporcionan direcciones IP privadas a los equipos que se conectan a ellos en la red interna. Por lo tanto, es posible que un único equipo que ejecute Multipoint Services pueda conectarse a la interfaz interna del enrutador, obtener información de IP automática y conectarse a Internet sin que un administrador tenga un esfuerzo o una configuración importantes.  
  
Una forma habitual de administrar usuarios en este tipo de entorno es crear una cuenta de usuario local para cada persona que vaya a tener acceso al sistema. Cualquier persona que tenga una cuenta de usuario local en ese equipo puede iniciar sesión en Multipoint Services desde cualquier estación que esté asociada con el sistema. Las cuentas de usuario locales se pueden crear y administrar desde Multipoint Manager.  
  
## <a name="multiple-multipoint-server-systems-with-local-user-accounts"></a>Varios sistemas MultiPoint Server con cuentas de usuario locales  
Dado que las cuentas de usuario locales solo son accesibles desde el equipo en el que se crearon, al implementar varios sistemas Multipoint Services en un entorno, puede administrar cuentas de usuario locales de una de estas dos maneras:  
  
-   Puede crear cuentas de usuario para usuarios específicos en equipos específicos que ejecutan Multipoint Services.  
  
-   Puede usar Multipoint Manager para crear cuentas para cada usuario en cada equipo que ejecute Multipoint Services.  
  
Por ejemplo, si planea asignar usuarios a un equipo específico que ejecuta Multipoint Services, puede crear cuatro cuentas de usuario locales en el equipo A (User01, user02, user03 y user04) y cuatro cuentas de usuario locales en el equipo B (user05, user06, user07 y user08). En este escenario, los usuarios 01 @ no__t-004 pueden iniciar sesión en el equipo a desde cualquier estación que esté conectada a él. sin embargo, no pueden iniciar sesión en el equipo B. Lo mismo se aplica a los usuarios 05 @ no__t-108, que solo podrán iniciar sesión en el equipo B, pero no en el equipo A. en función del entorno de implementación específico, esto puede ser aceptable o incluso deseable.  
  
Sin embargo, si todos los usuarios deben poder iniciar sesión en cualquiera de los equipos que ejecutan Multipoint Services, se debe crear una cuenta de usuario local para cada usuario en cada equipo que ejecute Multipoint Services. La elección de administrar usuarios de esta manera presenta ciertas complejidades. Por ejemplo, si User01 inicia sesión en el equipo A el lunes y guarda un archivo en la carpeta documentos y, a continuación, el usuario inicia sesión en el equipo b el martes, el archivo que se guardó en la carpeta documentos del equipo A no será accesible en el equipo B.  
  
Además, si un usuario tiene cuentas en los equipos A y B, no hay forma de sincronizar automáticamente las contraseñas de las cuentas. Esto puede dar lugar a que los usuarios tengan dificultades para iniciar sesión en el caso de que se cambie la contraseña de la cuenta en un equipo, pero no en el otro. Puede simplificar la administración de cuentas de usuario en este tipo de entorno de red asignando cada usuario a un solo equipo que ejecute Multipoint Services. De este modo, el usuario puede iniciar sesión en cualquiera de las estaciones asociadas a ese equipo y acceder a los archivos adecuados.  
  
## <a name="multiple-multipoint-services-systems-with-domain-accounts"></a>Varios sistemas Multipoint Services con cuentas de dominio  
Los entornos de dominio son comunes en entornos de red de gran tamaño que incluyen varios servidores. Por ejemplo, puede unir uno o más equipos que ejecutan el rol Multipoint Services a un dominio y, a continuación, usar Microsoft Active Directory para administrar cuentas de usuario a las que se puede tener acceso desde cualquier equipo del dominio. Esto permite crear cuentas de usuario de dominio individuales y acceder a ellas desde cualquier estación de cualquier sistema Multipoint Services que esté unido al dominio.  
 
Al implementar Multipoint Services en un entorno de dominio, se deben tener en cuenta varios factores:  
  
-   Si se usan cuentas de dominio, no se pueden administrar desde Multipoint Manager.  
  
-   De forma predeterminada, Multipoint Services está configurado para conceder permiso a cada usuario para que inicie sesión en una sola estación a la vez. Si decide permitir que los usuarios inicien sesión en varias estaciones al mismo tiempo mediante una sola cuenta, puede usar la opción **Editar configuración del servidor** en Multipoint Manager.  
  
-   La ubicación de los controladores de dominio puede afectar a la velocidad y la confiabilidad con las que los usuarios podrán autenticarse con el dominio y buscar recursos.  
  
## <a name="single-user-account-for-multiple-stations"></a>Cuenta de usuario único para varias estaciones  
Multipoint Services tiene la capacidad de iniciar sesión en varias estaciones del mismo equipo simultáneamente con una sola cuenta de usuario. Esta característica es útil en entornos en los que los usuarios no tienen nombres de usuario únicos y en los que el uso de una cuenta de usuario única puede simplificar la administración del sistema Multipoint Services.  
  
