---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Planear la ubicación del controlador de dominio Regional
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bec8595ab6eae8eb6cedaf9307ab97ac9c8316b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880446"
---
# <a name="planning-regional-domain-controller-placement"></a>Planear la ubicación del controlador de dominio Regional

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para garantizar la eficacia de costos, planea colocar tan pocos controladores de dominio regional como sea posible. En primer lugar, revise la hoja de cálculo "Geográfica ubicaciones y vínculos de comunicación" (DSSTOPO_1.doc) utilizado en [recopilar información de red](../../ad-ds/plan/Collecting-Network-Information.md) para determinar si una ubicación es un concentrador.  
  
Plan para colocar los controladores de dominio regional para cada dominio que se representa en cada ubicación del concentrador. Después de colocar los controladores de dominio regional en todas las ubicaciones de concentrador, evalúe la necesidad de colocar los controladores de dominio regional en ubicaciones de satélite. La eliminación de los controladores de dominio regional innecesarios desde ubicaciones de satélite reduce los costos de soporte técnico necesarios para mantener una infraestructura de servidor remoto.  
  
Además, garantizar la seguridad física de los controladores de dominio en ubicaciones de centro y satélite para que el personal no autorizado no puede tener acceso a ellos. No coloque los controladores de dominio de escritura en ubicaciones de centro y satélite en la que no puede garantizar la seguridad física del controlador de dominio. Una persona que tenga acceso físico a un controlador de dominio de escritura puede atacar el sistema por:  
  
- A partir de un sistema operativo alternativo en un controlador de dominio para tener acceso a discos físicos.  
- Quitar (y posiblemente reemplazando) discos físicos en un controlador de dominio.  
- Obtener y manipular una copia de una copia de seguridad del estado del sistema de controlador de dominio.  
  
Agregar controladores de dominio regional grabables solo a las ubicaciones en el que puede garantizar la seguridad física.  
  
En las ubicaciones con seguridad física inadecuada, implementar un controlador de dominio de solo lectura (RODC) es la solución recomendada. Excepto por las contraseñas de cuentas, un RODC dispone de todos los objetos de Active Directory y los atributos que contiene un controlador de dominio de escritura. Sin embargo, no se pueden realizar cambios a la base de datos que se almacena en el RODC. Los cambios deben ser realizados en un controlador de dominio de escritura y, a continuación, vuelven a replicar en el RODC.  
  
Para autenticar los inicios de sesión de cliente y el acceso a los servidores de archivos local, la mayoría de las organizaciones colocan los controladores de dominio regional para todos los dominios regionales que se representan en una ubicación determinada. Sin embargo, debe tener en cuenta muchas variables al evaluar si una ubicación de la empresa requiere que sus clientes para que la autenticación local o los clientes pueden basarse en la autenticación y la consulta a través de un vínculo de área extensa (WAN). La siguiente ilustración muestra cómo determinar si se debe colocar los controladores de dominio en ubicaciones de satélite.  
  
![selección de ubicación de controlador de dominio regional de plan](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilidad de experiencia técnica en el sitio

Los controladores de dominio deben administrarse continuamente por diversos motivos. Colocar un controlador de dominio regional solo en ubicaciones que incluyen a personal que puede administrar el controlador de dominio o asegúrese de que el controlador de dominio se puede administrar de forma remota.  
  
En entornos de sucursales con seguridad física normalmente deficiente y el personal con conocimientos de tecnología de información poco, la implementación de un RODC a menudo es la solución recomendada. Para cualquier usuario del dominio, se pueden delegar permisos administrativos locales para un RODC sin concederle ningún derecho de usuario para el dominio o en otros controladores de dominio. Esto permite que un usuario local de sucursal para iniciar sesión en un RODC y realizar el trabajo de mantenimiento en el servidor, como actualizar un controlador. Sin embargo, el usuario de rama no puede iniciar sesión en cualquier otro controlador de dominio o realizar cualquier otra tarea administrativa en el dominio. De este modo, el usuario de sucursal puede ser delegado la capacidad para administrar eficazmente el RODC en la sucursal sin poner en peligro la seguridad del resto del dominio o bosque.  
  
## <a name="wan-link-availability"></a>Disponibilidad del vínculo WAN

Vínculos WAN interrupciones frecuentes pueden provocar la pérdida de productividad importantes a los usuarios si la ubicación no incluye un controlador de dominio que se puede autenticar a los usuarios. Si la disponibilidad del vínculo WAN no es 100% y los sitios remotos no pueden tolerar una interrupción del servicio, coloque un controlador de dominio regional en ubicaciones donde los usuarios necesitan la capacidad de inicio de sesión o el acceso al servidor de exchange cuando el vínculo WAN está inactivo.  
  
## <a name="authentication-availability"></a>Disponibilidad de autenticación

Algunas organizaciones como bancos, requieren que los usuarios se autentiquen en todo momento. Colocar un controlador de dominio regional en una ubicación en la disponibilidad del vínculo WAN no está al 100 por ciento, pero los usuarios requieren autenticación en todo momento.  
  
## <a name="logon-performance-over-wan-links"></a>Rendimiento de inicio de sesión a través de vínculos WAN

Si la disponibilidad del vínculo WAN es altamente confiable, colocar un controlador de dominio en la ubicación depende de los requisitos de rendimiento de inicio de sesión a través del vínculo WAN. Los factores que influyen en el rendimiento de inicio de sesión a través de la WAN incluyen velocidad de vínculo y el ancho de banda disponible, número de usuarios y perfiles de uso y la cantidad de tráfico de red de inicio de sesión frente al tráfico de replicación.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Uso de ancho de banda y la velocidad del vínculo WAN

Las actividades de un único usuario pueden congestionar un vínculo WAN lento. Colocar un controlador de dominio en una ubicación si el rendimiento de inicio de sesión a través del vínculo WAN es inaceptable.  
  
El porcentaje promedio de utilización de ancho de banda indica cómo congestionada un vínculo de red. Si un vínculo de red tiene un uso medio de ancho de banda que es mayor que un valor aceptable, colocar un controlador de dominio en esa ubicación.  
  
### <a name="number-of-users-and-usage-profiles"></a>Número de usuarios y perfiles de uso

El número de usuarios y sus perfiles de uso en una ubicación determinada puede ayudar a determinar si necesita colocar los controladores de dominio regional en esa ubicación. Para evitar la pérdida de productividad si se produce un error en un vínculo WAN, colocar un controlador de dominio regional en una ubicación que tenga 100 o más usuarios.  
  
Los perfiles de uso indican cómo los usuarios usar los recursos de red. No es necesario colocar un controlador de dominio en una ubicación que contiene solo unos pocos usuarios que no tienen acceso con frecuencia los recursos de red.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Tráfico de red de inicio de sesión frente al tráfico de replicación

Si un controlador de dominio no está disponible en la misma ubicación que el cliente de Active Directory, el cliente crea tráfico de inicio de sesión en la red. La cantidad de tráfico de red de inicio de sesión que se crea en la red física se ve afectada por varios factores, como las pertenencias a grupos; número y tamaño de los objetos de directiva de grupo (GPO); scripts de inicio de sesión; y características como las carpetas sin conexión, redirección de carpetas y perfiles móviles.  
  
Por otro lado, un controlador de dominio que se coloca en una ubicación determinada genera tráfico de replicación en la red. La frecuencia y el importe de las actualizaciones realizadas en las particiones hospedadas en los controladores de dominio influyen en la cantidad de tráfico de replicación que se crea en la red. Los distintos tipos de actualizaciones que se pueden realizar en las particiones hospedadas en los controladores de dominio incluyen agregar o cambiar los usuarios y los atributos de usuario, cambiar las contraseñas y agregar o cambiar grupos globales, impresoras o volúmenes.  
  
Para determinar si se debe colocar un controlador de dominio regional en una ubicación, compare el coste del tráfico de inicio de sesión creado por una ubicación sin un controlador de dominio frente al costo del tráfico de replicación que creó mediante la colocación de un controlador de dominio en la ubicación.  
  
Por ejemplo, considere la posibilidad de una red que tenga las sucursales que están conectadas a través de vínculos lentos a la oficina central y en qué controladores de dominio pueden agregarse fácilmente. Si el tráfico de búsqueda de inicio de sesión y directorio diario de unos pocos usuarios del sitio remoto hace más tráfico de red que replicar todos los datos de empresa a la rama, considere la posibilidad de agregar un controlador de dominio a la rama.  
  
Si reduce el costo de mantener los controladores de dominio es más importante que el tráfico de red, centralizar los controladores de dominio para ese dominio y no coloque los controladores de dominio regional en la ubicación o considere la posibilidad de colocar los RODC en la ubicación.  
  
Para que una hoja de cálculo que le ayudarán a documentar la colocación de los controladores de dominio regional y el número de usuarios para cada dominio que se representa en cada ubicación, consulte [trabajo ayudas para Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), descargue Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abra "Domain Controller Placement" (DSSTOPO_4.doc).  
  
Deberá consultar la información acerca de las ubicaciones en el que tiene que colocar los controladores de dominio regional al implementar dominios regionales. Para obtener más información sobre cómo implementar dominios regionales, vea [implementar Windows Server 2008 dominios regionales](https://technet.microsoft.com/library/cc755118.aspx).  
