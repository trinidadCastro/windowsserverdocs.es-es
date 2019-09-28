---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Planeación de la ubicación del controlador de dominio regional
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2508476f35462516f32877365cb15be919b5b6df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408739"
---
# <a name="planning-regional-domain-controller-placement"></a>Planeación de la ubicación del controlador de dominio regional

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para garantizar la eficacia de los costos, piense en colocar los controladores de dominio regionales que sea posible. En primer lugar, revise la hoja de cálculo "ubicaciones geográficas y vínculos de comunicación" (DSSTOPO_1. doc) que se usa en [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) para determinar si una ubicación es un concentrador.  
  
Planear la colocación de los controladores de dominio regionales para cada dominio que se representa en cada ubicación del concentrador. Después de colocar los controladores de dominio regionales en todas las ubicaciones del concentrador, evalúe la necesidad de colocar los controladores de dominio regionales en las ubicaciones de satélite. La eliminación de los controladores de dominio regionales innecesarios de ubicaciones satélite reduce los costos de soporte técnico necesarios para mantener una infraestructura de servidores remotos.  
  
Además, asegúrese de la seguridad física de los controladores de dominio en las ubicaciones del concentrador y el satélite para que el personal no autorizado no pueda acceder a ellos. No coloque controladores de dominio de escritura en ubicaciones de concentrador y satélite en las que no pueda garantizar la seguridad física del controlador de dominio. Una persona que tenga acceso físico a un controlador de dominio de escritura puede atacar al sistema mediante:  
  
- Obtener acceso a discos físicos iniciando un sistema operativo alternativo en un controlador de dominio.  
- Quitar (y posiblemente reemplazar) discos físicos en un controlador de dominio.  
- Obtener y manipular una copia de una copia de seguridad del estado del sistema del controlador de dominio.  
  
Agregue controladores de dominio regionales modificables solo a ubicaciones en las que pueda garantizar su seguridad física.  
  
En ubicaciones con seguridad física inadecuada, la implementación de un controlador de dominio de solo lectura (RODC) es la solución recomendada. A excepción de las contraseñas de cuenta, un RODC contiene todos los objetos Active Directory y atributos que contiene un controlador de dominio de escritura. Sin embargo, los cambios no se pueden realizar en la base de datos almacenada en el RODC. Los cambios se deben realizar en un controlador de dominio grabable y, a continuación, volver a replicarse en el RODC.  
  
Para autenticar los inicios de sesión de cliente y el acceso a servidores de archivos locales, la mayoría de las organizaciones colocan controladores de dominio regionales para todos los dominios regionales que se representan en una ubicación determinada. Sin embargo, debe tener en cuenta muchas variables al evaluar si una ubicación empresarial requiere que sus clientes tengan autenticación local o los clientes pueden basarse en la autenticación y la consulta en un vínculo de red de área extensa (WAN). En la ilustración siguiente se muestra cómo determinar si se deben colocar controladores de dominio en ubicaciones de satélite.  
  
![Planeación de la ubicación del DC regional](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilidad de la experiencia técnica en el sitio

Los controladores de dominio deben administrarse continuamente por varias razones. Coloque un controlador de dominio regional solo en ubicaciones que incluyan personal que pueda administrar el controlador de dominio o asegúrese de que el controlador de dominio se puede administrar de forma remota.  
  
En entornos de sucursales con una seguridad física deficiente y personal con pocos conocimientos sobre tecnología de la información, la implementación de un RODC suele ser la solución recomendada. Los permisos administrativos locales para un RODC se pueden delegar a cualquier usuario del dominio sin conceder a ese usuario derechos de usuario para el dominio u otros controladores de dominio. Esto permite a un usuario de una sucursal local iniciar sesión en un RODC y realizar el trabajo de mantenimiento en el servidor, como actualizar un controlador. Sin embargo, el usuario de la rama no puede iniciar sesión en ningún otro controlador de dominio ni realizar ninguna otra tarea administrativa en el dominio. De esta manera, se puede delegar a los usuarios de la sucursal la capacidad de administrar de forma eficaz el RODC en la sucursal sin poner en peligro la seguridad del resto del dominio o del bosque.  
  
## <a name="wan-link-availability"></a>Disponibilidad de vínculo WAN

Los vínculos WAN que experimentan interrupciones frecuentes pueden provocar una pérdida de productividad significativa para los usuarios si la ubicación no incluye un controlador de dominio que pueda autenticar a los usuarios. Si la disponibilidad de los vínculos WAN no es del 100 por ciento y los sitios remotos no pueden tolerar una interrupción del servicio, coloque un controlador de dominio regional en ubicaciones donde los usuarios requieran la posibilidad de iniciar sesión o el acceso a Exchange Server cuando el vínculo WAN esté inactivo.  
  
## <a name="authentication-availability"></a>Disponibilidad de autenticación

Ciertas organizaciones, como los bancos, requieren que los usuarios se autentiquen en todo momento. Coloque un controlador de dominio regional en una ubicación en la que la disponibilidad del vínculo WAN no sea del 100 por ciento pero los usuarios requieran autenticación en todo momento.  
  
## <a name="logon-performance-over-wan-links"></a>Rendimiento de inicio de sesión a través de vínculos WAN

Si la disponibilidad de los vínculos WAN es muy confiable, la colocación de un controlador de dominio en la ubicación depende de los requisitos de rendimiento de inicio de sesión a través del vínculo WAN. Los factores que influyen en el rendimiento de inicio de sesión a través de WAN incluyen la velocidad de los vínculos y el ancho de banda disponible, el número de usuarios y los perfiles de uso, y la cantidad de tráfico de red de inicio de sesión  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Velocidad de vínculo WAN y uso de ancho de banda

Las actividades de un solo usuario pueden congestionar un vínculo WAN lento. Coloque un controlador de dominio en una ubicación si el rendimiento de inicio de sesión a través del vínculo WAN no es aceptable.  
  
El porcentaje medio de uso de ancho de banda indica la congestión de un vínculo de red. Si un vínculo de red tiene un uso de ancho de banda promedio mayor que un valor aceptable, coloque un controlador de dominio en esa ubicación.  
  
### <a name="number-of-users-and-usage-profiles"></a>Número de usuarios y perfiles de uso

El número de usuarios y sus perfiles de uso en una ubicación determinada pueden ayudar a determinar si es necesario colocar los controladores de dominio regionales en esa ubicación. Para evitar la pérdida de productividad si se produce un error en una conexión WAN, coloque un controlador de dominio regional en una ubicación que tenga 100 o más usuarios.  
  
Los perfiles de uso indican cómo los usuarios usan los recursos de red. No es necesario colocar un controlador de dominio en una ubicación que contenga solo unos pocos usuarios que no tengan acceso a los recursos de red con frecuencia.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Tráfico de red de inicio de sesión frente a tráfico de replicación

Si un controlador de dominio no está disponible en la misma ubicación que el cliente de Active Directory, el cliente crea el tráfico de inicio de sesión en la red. La cantidad de tráfico de red de inicio de sesión que se crea en la red física se ve afectada por varios factores, como la pertenencia a grupos. número y tamaño de los objetos de directiva de grupo (GPO); scripts de inicio de sesión; y características como carpetas sin conexión, redirección de carpetas y perfiles móviles.  
  
Por otro lado, un controlador de dominio que se coloca en una ubicación determinada genera tráfico de replicación en la red. La frecuencia y la cantidad de actualizaciones realizadas en las particiones hospedadas en los controladores de dominio influyen en la cantidad de tráfico de replicación que se crea en la red. Los diferentes tipos de actualizaciones que se pueden realizar en las particiones hospedadas en los controladores de dominio incluyen agregar o cambiar usuarios y atributos de usuario, cambiar contraseñas y agregar o cambiar grupos, impresoras o volúmenes globales.  
  
Para determinar si es necesario colocar un controlador de dominio regional en una ubicación, compare el costo del tráfico de inicio de sesión creado por una ubicación sin un controlador de dominio frente al costo del tráfico de replicación creado mediante la colocación de un controlador de dominio en la ubicación.  
  
Por ejemplo, considere una red que tiene sucursales que se conectan a través de vínculos lentos a la oficina central y en los que se pueden agregar fácilmente controladores de dominio. Si el tráfico diario de inicio de sesión y búsqueda de directorios de algunos usuarios del sitio remoto provoca más tráfico de red que la replicación de todos los datos de la compañía en la rama, considere la posibilidad de agregar un controlador de dominio a la rama.  
  
Si reducir el costo de mantenimiento de los controladores de dominio es más importante que el tráfico de red, Centralice los controladores de dominio para ese dominio y no coloque ningún controlador de dominio regional en la ubicación o considere la posibilidad de colocar RODC en la ubicación.  
  
Para obtener una hoja de cálculo que le ayude a documentar la ubicación de los controladores de dominio regionales y el número de usuarios para cada dominio que se representa en cada ubicación, consulte el [material de trabajo para el kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), descargar Job_Aids_Designing_and_ Deploying_Directory_and_Security_Services. zip y abra "Ubicación del controlador de dominio" (DSSTOPO_4. doc).  
  
Tendrá que consultar la información acerca de las ubicaciones en las que necesita colocar controladores de dominio regionales al implementar dominios regionales. Para obtener más información acerca de la implementación de dominios regionales, consulte [implementación de dominios regionales de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
