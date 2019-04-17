---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: "Planeación de la ubicación del controlador de dominio Regional"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c5254560e9b1a10030fa37ec0966868986212a4a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="planning-regional-domain-controller-placement"></a>Planeación de la ubicación del controlador de dominio Regional

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para asegurarte de rentabilidad, planea colocar como algunos controladores de dominio regional como sea posible. Primero, revisa la hoja de cálculo de "Geográfica ubicaciones y vínculos de comunicación" (DSSTOPO_1.doc) usada en [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) para determinar si una ubicación es un concentrador.  
  
Plan para colocar los controladores de dominio regional para cada dominio que se representa en cada ubicación de concentrador. Después de colocar los controladores de dominio regional de todas las ubicaciones de concentrador, evaluar la necesidad de colocar los controladores de dominio regionales en ubicaciones de TV por satélite. La eliminación de controladores de dominio regional innecesarios desde ubicaciones de satélite reduce los costos de soporte técnico necesarios para mantener una infraestructura de servidor remoto.  
  
Además, garantizar la seguridad física de los controladores de dominio en ubicaciones de satélite y de concentrador para que los usuarios no autorizados no pueden acceder a ellos. No coloques controladores de dominio grabable en ubicaciones de satélite y de concentrador en el que no puede garantizar la seguridad física del controlador de dominio. Una persona que tenga acceso físico a un controlador de dominio grabable puede atacar el sistema:  
  
-   Tener acceso a discos físicos a partir de un sistema operativo alternativo en un controlador de dominio.  
  
-   Quitar (y posiblemente el reemplazo) discos físicos en un controlador de dominio.  
  
-   Obtener y manipular una copia de una copia de seguridad del estado del sistema de controlador de dominio.  
  
Agregar controladores de dominio regional de escritura solo a ubicaciones en el que puedes garantizar la seguridad física.  
  
En ubicaciones con la seguridad física inadecuada, la implementación de un controlador de dominio de solo lectura (RODC) es la solución recomendada. Excepto por las contraseñas de cuenta, un RODC dispone de todos los objetos de Active Directory y atributos que contiene un controlador de dominio grabable. Sin embargo, no se pueden realizar cambios a la base de datos que se almacena en el RODC. Deben ser realizados en un controlador de dominio de escritura y vuelven a replicar el RODC cambios.  
  
Para autenticar los inicios de sesión del cliente y el acceso a servidores de archivos local, la mayoría de las organizaciones colocar los controladores de dominio regionales para todos los dominios regionales que se representan en una ubicación determinada. Sin embargo, debes tener en cuenta todas las variables al evaluar si una ubicación de la empresa requiere que los clientes tengan la autenticación local o los clientes pueden basarse en la autenticación y consulta sobre un vínculo de área extensa (WAN). La siguiente ilustración muestra cómo determinar si se debe colocar los controladores de dominio en ubicaciones de TV por satélite.  
  
![Colocación de dc regional de plan](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilidad de experiencia técnica in situ  
Controladores de dominio necesitan administrarse continuamente por varios motivos. Coloca un controlador de dominio regional solo en ubicaciones que incluyen a personal que puede administrar el controlador de dominio o Asegúrate de que el controlador de dominio puede administrar de forma remota.  
  
En entornos de sucursales con la seguridad física normalmente deficiente y personal con poco conocimientos de tecnología de información, la implementación de un RODC a menudo es la solución recomendada. Permisos administrativos locales para un RODC pueden delegarse a cualquier usuario de dominio sin conceder los derechos de usuario para el dominio o a otros controladores de dominio de ese usuario. Esto permite que un usuario rama local para iniciar sesión en un RODC y realizar tareas de mantenimiento en el servidor, como actualizar un controlador. Sin embargo, el usuario rama no puede iniciar sesión en cualquier otro controlador de dominio o realizar cualquier otra tarea administrativa en el dominio. De este modo, el usuario rama pueden delegar la capacidad para administrar eficazmente el RODC en las sucursales sin comprometer la seguridad del resto del dominio o bosque.  
  
## <a name="wan-link-availability"></a>Disponibilidad de los vínculos WAN  
Los vínculos WAN interrupciones frecuentes pueden perder productividad importantes a los usuarios si la ubicación no incluye un controlador de dominio que puede autenticar a los usuarios. Si la disponibilidad del vínculo WAN no es 100 por ciento y los sitios remotos no pueden tolerar una interrupción del servicio, coloca un controlador de dominio regionales en ubicaciones donde los usuarios requieren la capacidad de iniciar sesión o acceso al servidor de exchange cuando el vínculo WAN está inactivo.  
  
## <a name="authentication-availability"></a>Disponibilidad de autenticación  
Ciertas organizaciones, como bancos, requieran la autenticación de usuarios en todo momento. Coloca un controlador de dominio regionales en una ubicación donde la disponibilidad del vínculo WAN no es 100%, pero los usuarios requieren autenticación en todo momento.  
  
## <a name="logon-performance-over-wan-links"></a>Rendimiento de inicio de sesión a través de vínculos WAN  
Si la disponibilidad del vínculo WAN es altamente confiable, colocar un controlador de dominio en la ubicación depende de los requisitos de rendimiento de inicio de sesión a través del vínculo WAN. Factores que influyen en el rendimiento de inicio de sesión a través de la WAN incluyen la velocidad del vínculo y el ancho de banda disponible, número de usuarios y perfiles de uso y la cantidad de tráfico de red de inicio de sesión en comparación con el tráfico de replicación.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilización de ancho de banda y la velocidad del vínculo WAN  
Las actividades de un usuario solo pueden congestionar un vínculo WAN lento. Coloca un controlador de dominio en una ubicación si el rendimiento de inicio de sesión a través del vínculo WAN es inaceptable.  
  
El porcentaje de promedio de uso de ancho de banda indica cómo congestionado un vínculo de red es. Si un vínculo de red tiene el uso de ancho de banda promedio que es mayor que los valores aceptables, coloca un controlador de dominio en esa ubicación.  
  
### <a name="number-of-users-and-usage-profiles"></a>Número de usuarios y perfiles de uso  
El número de usuarios y los perfiles de su uso en una ubicación determinada puede ayudar a determinar si es necesario colocar los controladores de dominio regionales en esa ubicación. Para evitar la pérdida de productividad si se produce un error en un vínculo WAN, coloca un controlador de dominio regionales en una ubicación de 100 o más usuarios.  
  
Los perfiles de uso indican cómo los usuarios utilizan los recursos de red. No es necesario colocar un controlador de dominio en una ubicación que contenga solo unos pocos los usuarios que no tienen acceso con frecuencia recursos de red.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Tráfico de red de inicio de sesión frente a tráfico de replicación  
Si un controlador de dominio no está disponible dentro de la misma ubicación que el cliente de Active Directory, el cliente crea el tráfico de inicio de sesión en la red. La cantidad de tráfico de red de inicio de sesión que se crea en la red física influye en varios factores, como las pertenencias a grupos; número y el tamaño de los objetos de directiva de grupo (GPO); scripts de inicio de sesión; y características como carpetas sin conexión, redirección de carpetas y perfiles móviles.  
  
Por otro lado, un controlador de dominio que se coloca en una ubicación determinada genera el tráfico de replicación de la red. La frecuencia y la cantidad de actualizaciones realizadas en las particiones hospedadas en los controladores de dominio influyen en la cantidad de tráfico de replicación que se crea en la red. Los distintos tipos de actualizaciones que se pueden realizar en las particiones hospedadas en los controladores de dominio incluyen agregar o cambiar los atributos de usuario y los usuarios, cambiar las contraseñas y agregar o cambiar grupos globales, impresoras o volúmenes.  
  
Para determinar si debes colocar un controlador de dominio regional en una ubicación, comparar el costo del tráfico de inicio de sesión creado por una ubicación sin un controlador de dominio frente al costo del tráfico de replicación creado mediante la colocación de un controlador de dominio en la ubicación.  
  
Por ejemplo, considera la posibilidad de una red que tiene las sucursales que están conectadas a través de vínculos lentos a la oficina y en qué controladores de dominio pueden agregarse fácilmente. Si el tráfico de la búsqueda de inicio de sesión y el directorio diario de algunos usuarios del sitio remoto hace más tráfico de red que replicar todos los datos de empresa a la rama, considera la posibilidad de agregar un controlador de dominio a la rama.  
  
Si lo que reduce el costo de mantenimiento de controladores de dominio es más importante que el tráfico de red, centralizar los controladores de dominio para ese dominio y no colocar los controladores de dominio regional en la ubicación o considera la posibilidad de colocar los RODC en la ubicación.  
  
Para que una hoja de cálculo que le ayudarán a documentar la colocación de los controladores de dominio regional y el número de usuarios para cada dominio que se representa en cada ubicación, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), descargar < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and Abre (DSSTOPO_4.doc) "Ubicación del controlador de dominio".  
  
Tendrás que hacer referencia a la información sobre ubicaciones en el que deberás colocar los controladores de dominio regional cuando implementas dominios regionales. Para obtener más información acerca de la implementación de dominios regionales, consulta [Regional dominios de implementación de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
  


