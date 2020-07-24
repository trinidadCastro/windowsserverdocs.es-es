---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: Cómo se controlan las cookies de servidor LDAP
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: aa7d30d17d6e7a44daf2c5a65a4e173f25160456
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86960777"
---
# <a name="how-ldap-server-cookies-are-handled"></a>Cómo se controlan las cookies de servidor LDAP

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En LDAP, algunas consultas producen un resultado de gran tamaño. Dichas consultas presentan algunos desafíos para Windows Server.  
  
La recopilación y la creación de estos grandes conjuntos de resultados suponen mucho trabajo. Muchos de los atributos deben convertirse a partir de una representación interna a la representación de conexión LDAP. Para muchos atributos, una conversión de un formato interno, a menudo binario, debe producirse en un formato UTF-8 basado en texto en el marco de respuesta LDAP.  
  
Otro desafío es que el tamaño de los conjuntos de resultados con decenas de miles de objetos se vuelve enorme, llegando a tener fácilmente varios cientos de megabytes. Después, estos requieren de mucho espacio de dirección virtual y también la transferencia por red tiene problemas ya que se pierde todo el esfuerzo cuando la sesión TCP se desglosa en tránsito.  
  
Estos problemas logísticos y de capacidad han llevado a los desarrolladores de Microsoft LDAP a crear una extensión LDAP conocida como "consulta paginada". Está implementando un control LDAP para separar una consulta enorme en fragmentos de conjuntos de resultados más pequeños. Se ha convertido en un estándar RFC como [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Administración de cookies en el cliente  
El método de consulta paginada usa el tamaño de página establecido por el cliente o a través de una [Directiva LDAP](https://support.microsoft.com/kb/315071/en-us) ("MaxPageSize"). El cliente siempre necesita habilitar la paginación enviando un control LDAP.  

  
Al trabajar en una consulta con muchos resultados, en algún momento se alcanza el número máximo de objetos permitidos. El servidor LDAP empaqueta el mensaje de respuesta y agrega una cookie que contiene la información que necesita para continuar después la búsqueda.  
  
La aplicación cliente debe tratar la cookie como blob opaco. Puede recuperar el recuento de objetos en la respuesta y continuar la búsqueda en función de la presencia de la cookie. El cliente continúa la búsqueda enviando la consulta al servidor LDAP de nuevo con los mismos parámetros como filtro y objeto base e incluye el valor de cookie devuelto en la respuesta anterior.  
  
Si el número de objetos no llena una página, la consulta LDAP se completa y la respuesta no contiene ninguna cookie de página. Si el servidor no devuelve ninguna cookie, el cliente considera la búsqueda paginada como correctamente completada.  
  
Si el servidor devuelve un error, el cliente debe considerar que la búsqueda paginada es incorrecta. Volver a intentar la búsqueda dará como resultado que se reinicie la búsqueda desde la primera página.  
  
## <a name="server-side-cookie-handling"></a>Administración de cookies del lado servidor  
Windows Server devuelve la cookie al cliente y a veces almacena información relacionada con la cookie en el servidor. Esta información se almacena en el servidor en una caché y está sujeta a determinadas limitaciones.  
  
En este caso, la cookie enviada al cliente por el servidor también la usa el servidor para buscar la información de la memoria caché en el servidor. Cuando el cliente continúa la búsqueda paginada, Windows Server usará la cookie del cliente, así como cualquier información relacionada desde la caché de cookies de servidor para continuar la búsqueda. Si el servidor no encuentra la información de cookie relacionada de la caché del servidor por cualquier motivo, la búsqueda será descontinuada y se devolverá un error al cliente.  
  
## <a name="how-the-cookie-pool-is-managed"></a>Cómo se administra el grupo de cookies  
Obviamente, el servidor LDAP está atendiendo a más de un cliente al mismo tiempo y también más de un cliente cada vez puede iniciar consultas que requieren el uso de la caché de cookies de servidor. Por lo tanto, la implementación de Windows Server ahí es un seguimiento del uso del grupo de cookies y se establecen límites para que el grupo de cookies no tome demasiados recursos. El Administrador puede establecer los límites mediante la siguiente configuración de la directiva LDAP. Los valores predeterminados y las explicaciones son:  
  
**MinResultSets: 4**  
  
El servidor LDAP no examinará el tamaño máximo del grupo que se describe a continuación, si hay menos de MinResultSets entradas en la caché de cookies del servidor.  
  
**MaxResultSetSize:: 262.144 bytes**  
  
El tamaño total de caché de cookies en el servidor no puede superar el valor máximo de MaxResultSetSize en bytes. Si lo hace, se eliminan las cookies empezando por la más antigua hasta que el grupo es menor que MaxResultSetSize bytes o hay menos de MinResultSets cookies en el grupo. Esto significa que al usar la configuración predeterminada, el servidor LDAP considera que un grupo de 450 KB será correcto si solo hay 3 cookies almacenadas.  
  
**MaxResultSetsPerConn: 10**  
  
El servidor LDAP no permite más de MaxResultSetsPerConn cookies por conexión LDAP en el grupo.  
  
## <a name="handling-deleted-cookies"></a>Administración de cookies eliminadas  
La eliminación de la información de cookies de la caché del servidor LDAP no produce un error inmediato para las aplicaciones en todos los casos. Las aplicaciones pueden reiniciar la búsqueda paginada desde el principio y completarla en otro intento. Algunas aplicaciones tienen este tipo de un mecanismo de reintento para agregar solidez.  
  
Algunas aplicaciones pueden pasar por una búsqueda de página y no completarla nunca. Esto puede dejar entradas en la caché de cookies del servidor LDAP, que se controla mediante el mecanismo de la sección 4. Esto es esencial para liberar memoria en el servidor para búsquedas LDAP activas.  
  
¿Qué ocurre cuando se elimina una cookie de este tipo en el servidor y el cliente continúa la búsqueda con este identificador de cookie? El servidor LDAP no encontrará la cookie en la caché de cookies del servidor y devolverá un error para la consulta; la respuesta de error será similar a la siguiente:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> El valor hexadecimal que se encuentra detrás de "DSID" variará en función de la versión de compilación de los archivos binarios del servidor LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Informes sobre el grupo de cookies  
El servidor LDAP tiene la capacidad de registrar eventos a través de la categoría "16 la interfaz LDAP" en la [clave de diagnóstico NTDS](https://support.microsoft.com/kb/314980/en-us). Si establece esta categoría en "2", puede obtener los siguientes eventos:  
  
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
  
Los eventos señalan que se ha quitado una cookie almacenada. No significa que un cliente ha visto el error LDAP, sino únicamente que el servidor LDAP ha alcanzado los límites de administración para la caché.  En algunos casos, un cliente LDAP puede haber abandonado la búsqueda paginada y puede que no haya visto nunca el error.  
  
## <a name="monitoring-the-cookie-pool"></a>Supervisión del grupo de cookies  
Si nunca experimenta errores de búsqueda LDAP en su dominio, puede que no necesite nunca supervisar el grupo de cookies de búsqueda de página de servidor LDAP. En caso de que vea los errores relacionados con la búsqueda de página LDAP en su entorno, puede que tenga un problema con los límites del administrador del grupo de cookies.  
  
Los eventos 2898 y 2899 son las únicas maneras de saber que el servidor LDAP ha alcanzado los límites de administrador. Cuando tenga esa experiencia, las consultas LDAP generarán error debido al error de procesamiento de control anterior; debe considerar aumentar los límites en uno o varios valores de la configuración de la directiva LDAP mencionados en la sección 4, según el evento que obtenga.  
  
Si ve el evento 2898 en el servidor DC/LDAP, se recomienda establecer MaxResultSetsPerConn en 25. No es habitual que haya más de 25 búsquedas paginadas paralelas en una sola conexión LDAP. Si continúa viendo el evento 2898, considere la posibilidad de investigar la aplicación de cliente LDAP que encuentra el error. La conjetura sería que de algún modo se queda detenido al recuperar resultados paginados adicionales, deja la cookie pendiente y reinicia una nueva consulta. Por tanto, vea si la aplicación tendría en algún momento cookies suficientes para su finalidad; también puede aumentar el valor de MaxResultSetsPerConn más allá de 25. Cuando vea eventos 2899 registrados en los controladores de dominio, el plan sería diferente. Si su servidor DC/LDAP se ejecuta en una máquina con memoria suficiente (varios gigabytes de memoria libre), se recomienda establecer el MaxResultSetSize en el servidor LDAP en >= 250 MB Este límite es lo suficientemente grande como para dar cabida a grandes volúmenes de búsquedas de página LDAP incluso en directorios muy grandes.  
  
Si todavía está viendo eventos 2899 con un grupo de 250 MB o más, probablemente tenga muchos clientes con un gran número de objetos devueltos, consultados con mucha frecuencia. Los datos que puede recopilar con el [Conjunto de recopiladores de datos de Active Directory](/archive/blogs/askds/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond) pueden ayudarle a encontrar consultas paginadas repetitivas que mantienen los servidores LDAP ocupados. Todas estas consultas se mostrarán con un número de "entradas devueltas" que coincida con el tamaño de la página usada.  
  
Si es posible, debe revisar el diseño de la aplicación e implementar un enfoque diferente con una frecuencia inferior, un volumen de datos y/o menos instancias de cliente que consultan estos datos. En el caso de las aplicaciones para las que tiene acceso al código fuente, esta guía para [crear aplicaciones eficaces habilitadas para ad](/previous-versions/ms808539(v=msdn.10)) puede ayudarle a entender la forma óptima para que las aplicaciones tengan acceso a ad.  
  
Si no se puede cambiar el comportamiento de la consulta, un enfoque también agrega más instancias replicadas de los contextos de nomenclatura necesarios y redistribuye los clientes y, finalmente, reduce la carga en los servidores LDAP individuales.  
  
