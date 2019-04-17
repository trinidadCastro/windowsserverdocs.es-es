---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: "¿Cómo se administran las Cookies de servidor LDAP"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 89369cb1e52a315520062ca5ecc96b66ac3e2bfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="how-ldap-server-cookies-are-handled"></a>¿Cómo se administran las Cookies de servidor LDAP

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En LDAP, Establece algún resultado consultas en un resultado de gran tamaño. Estas consultas suponen algunos retos en el servidor de Windows.  
  
Recopilación y la creación de estos conjuntos grandes de resultado es mucho trabajo. Muchos de los atributos que debas convertir a partir de una representación interna para la representación de hilo LDAP. Para muchos de los atributos, una conversión de un formato interno, a menudo binario, debe ocurrir en un formato UTF-8 basados en texto en el marco de la respuesta LDAP.  
  
Otro desafío es que el resultado se establece con decenas de miles de objetos se convierten en enormes fácilmente varios cientos de megabytes. Espacio de direcciones de estos, a continuación, requerir mucha virtual y también la transferencia en red tiene problemas como todo el esfuerzo se pierden cuando la sesión TCP se divide en tránsito.  
  
Estos capacidad y logísticas problemas nos han permitido los desarrolladores de Microsoft LDAP creación de una extensión LDAP que se conoce como "Consulta paginada". Que implementa un control LDAP para separar una consulta enorme en fragmentos de conjuntos más pequeños de resultado. Se ha convertido en un estándar RFC como [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Administración de cliente de cookies  
El método de consulta paginada usa el tamaño de página bien establecido por el cliente o a través un [directiva LDAP](https://support.microsoft.com/kb/315071/en-us) ("MaxPageSize"). El cliente siempre debe habilitar la paginación mediante el envío de un control LDAP.  

  
Cuando se trabaja en una consulta con varios resultados, en algún momento se alcanza el número máximo de objetos permitidos. El servidor LDAP empaqueta el mensaje de respuesta y agrega una cookie que contiene información necesaria continuar más tarde de la búsqueda.  
  
La aplicación cliente debe tratar la cookie como un blob opaco. Se puede recuperar el recuento de objetos en la respuesta y puede continuar con la búsqueda en función de la presencia de la cookie. El cliente continúa la búsqueda mediante el envío de la consulta al servidor LDAP con los mismos parámetros, como el objeto base y filtrar e incluye el valor de la cookie que se devolvió en la respuesta anterior.  
  
Si el número de objetos no rellena una página, la consulta LDAP está completa y la respuesta no contiene ninguna cookie de página. Si no se devuelve ninguna cookie por el servidor, el cliente debe tener en cuenta la búsqueda paginada que se completen correctamente.  
  
Si el servidor devuelve un error, el cliente debe tener en cuenta la búsqueda paginada no se realice correctamente. Reintentar la búsqueda se tenga que reiniciar la búsqueda de la primera página.  
  
## <a name="server-side-cookie-handling"></a>Administración de cookies del lado servidor  
El servidor de Windows devuelve la cookie al cliente y a veces almacena información relacionada con la cookie en el servidor. Esta información se almacena en el servidor en una memoria caché y está sujeta a ciertos límites.  
  
En este caso, la cookie enviada al cliente por el servidor también se usa el servidor para buscar la información de la memoria caché en el servidor. Cuando el cliente continúa la búsqueda paginada, el servidor de Windows usará la cookie de cliente, así como cualquier información relacionada de la memoria caché de cookie de servidor para continuar la búsqueda. Si el servidor no puede encontrar información relacionada de la caché del servidor por cualquier motivo, la búsqueda se suspende y error se devuelve al cliente.  
  
## <a name="how-the-cookie-pool-is-managed"></a>¿Cómo se administra el grupo de cookie  
Obviamente, el servidor LDAP ofrece a más de un cliente a la vez, y también más de un cliente a la vez puede iniciar las consultas que requieren el uso de la caché de cookies del servidor. Por lo tanto, existe la implementación de Windows Server es un seguimiento del uso de cookies grupo y límites se llevan a cabo para que el grupo de cookie no está tardando demasiado recursos. Los límites pueden establecerse por el Administrador con la siguiente configuración de directiva LDAP. Los valores predeterminados y explicaciones son:  
  
**MinResultSets: 4**  
  
El servidor LDAP no tendrá un aspecto en el tamaño máximo de la agrupación que se explica a continuación, si hay menos de MinResultSets entradas en la memoria caché de cookie de servidor.  
  
**MaxResultSetSize:: 262.144 bytes**  
  
El tamaño de caché de cookie total en el servidor no debe superar el máximo de MaxResultSetSize: en bytes. Si es así, se eliminan las cookies desde la más antigua hasta que el grupo es menor que MaxResultSetSize: bytes o inferior MinResultSets cookies en el grupo. Esto significa que con la configuración predeterminada, el servidor LDAP considera un grupo de 450KB a estar bien, si solo hay 3 cookies almacenadas.  
  
**MaxResultSetsPerConn: 10**  
  
Permite que el servidor LDAP no más de las cookies de MaxResultSetsPerConn por conexión LDAP en el grupo.  
  
## <a name="handling-deleted-cookies"></a>Control de elimina las Cookies  
La eliminación de la información de la cookie de caché de servidor LDAP no producirá un error inmediato para aplicaciones en todos los casos. Las aplicaciones pueden reiniciar la búsqueda paginada desde el principio y completarlo en otro intento. Algunas aplicaciones tienen este tipo de un mecanismo de reintento para agregar solidez.  
  
Algunas aplicaciones pueden ir a través de una búsqueda de la página y nunca completarlo. Esto puede dejar entradas en el servidor LDAP caché de cookies, que se controla mediante el mecanismo de en la sección 4. Esto es esencial para liberar memoria en el servidor para búsquedas LDAP activas.  
  
¿Qué sucede cuando se elimina este tipo de cookie en el servidor y el cliente continúa la búsqueda con el identificador de cookie? El servidor LDAP no encontrará la cookie en el servidor de caché de cookies y devolver un error de la consulta, será similar a la respuesta de error:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> El valor hexadecimal subyacente "DSID" varía según la versión de compilación de los binarios de servidor LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Informar sobre el grupo de cookie  
El servidor LDAP tiene la capacidad para registrar eventos a través de la categoría "interfaz Ldap 16" la [clave de diagnósticos NTDS](https://support.microsoft.com/kb/314980/en-us). Si estableces esta categoría "2", puedes obtener los siguientes eventos:  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2898  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has reached the limit of the number of Result Sets it will maintain for a single connection.  A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.  
Maximum number of Result Sets allowed per LDAP connection:  
10  
Current number of Result Sets for this LDAP connection:  
11  
  
User Action  
The client should consider a more efficient search filter.  The limit for Maximum Result Sets per Connection may also be increased.  
  
```  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2899  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has exceeded the limit of the LDAP Maximum Result Set Size. A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.   
  
Number of result sets currently stored:   
4   
Current Result Set Size:   
263504   
Maximum Result Set Size:   
262144   
Size of single Result Set being discarded:   
40876   
User Action   
The client should consider a more efficient search filter.  The limit for Maximum Result Set Size may also be increased.  
  
```  
  
Los eventos de señal que se ha quitado una cookie almacenada. No significa que un cliente ha visto el error LDAP, pero solo que ha llegado a los límites de administración de la memoria caché del servidor LDAP.  En algunos casos, un cliente LDAP puede ha abandonado la búsqueda paginada y nunca puede ver el error.  
  
## <a name="monitoring-the-cookie-pool"></a>Supervisar el grupo de cookie  
Si nunca se producen errores de búsqueda LDAP en tu dominio, no necesitarás nunca supervisar el grupo de cookie de búsqueda LDAP server página. En caso de ver la página de LDAP buscar errores relacionados en el entorno, tienes un problema con los límites de administrador del grupo de cookie.  
  
Eventos 2898 y 2899 son las únicas maneras saber que el servidor LDAP ha llegado a los límites de administrador. Cuando se produce ese error consultas LDAP debido a procesamiento error anterior del control, debes mirar aumenta límites en uno o más de la configuración de directiva de LDAP mencionada en la sección 4, según el evento que recibes.  
  
Si ves evento 2898 en el servidor de DC/LDAP, te recomendamos que establezcas MaxResultSetsPerConn en 25. Más de 25 búsquedas paginadas paralelas en una sola conexión LDAP no es lo habitual. Si sigues viendo el evento 2898, considera la posibilidad de investigar la aplicación de cliente LDAP que encuentre el error. La sospecha sería que de alguna forma queda bloqueado recuperar más resultados paginadas, deja la cookie pendientes y se reinicia una nueva consulta. Por lo tanto, consulta si la aplicación en algún momento tendría cookies suficientes para sus objetivos, también puede aumentar el valor de MaxResultSetsPerConn más allá de 25. cuando lo veas eventos 2899 registrados en los controladores de dominio, el plan sería diferente. Si tu servidor LDAP/DC se ejecuta en una máquina con suficiente memoria (varios GB de memoria libre), te recomendamos que establezcas el MaxResultSetSize: en el servidor LDAP > = 250MB. Este límite es lo suficientemente grande como para dar cabida a grandes volúmenes de búsquedas de la página LDAP incluso en directorios muy grandes.  
  
Si tienes que seguir viendo eventos 2899 con un grupo de 250MB o más, es probable que de tener muchos clientes con gran cantidad de objetos devueltos, consultan de una manera muy frecuente. Los datos que se puede recopilar con la [conjunto de recopiladores de datos de Active Directory](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) puede ayudarle a encontrar repetitivas consultas paginadas que mantienen los servidores LDAP ocupado. Estas consultas se mostrarán con un número de "Entradas devuelven" que coincida con el tamaño de la página que utiliza.  
  
Si es posible, debes revisar el diseño de la aplicación y, implementa un enfoque diferente con una frecuencia más baja, el volumen de datos o menos instancias de cliente de consultar estos datos. En el caso de las aplicaciones que tienes acceso al código de origen, esta guía para [crear aplicaciones eficaces de AD-Enabled](https://msdn.microsoft.com/en-us/library/ms808539.aspx) puede ayudarte a comprender la mejor forma para aplicaciones para acceder a AD.  
  
Si no se puede cambiar el comportamiento de la consulta, un enfoque también agrega más replicadas instancias de los contextos de nombres necesitados así como para redistribuir a los clientes y finalmente reducir la carga de los servidores LDAP individuales.  
  


