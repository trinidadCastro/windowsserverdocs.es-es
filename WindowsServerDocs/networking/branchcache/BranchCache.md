---
title: BranchCache
description: Este tema proporciona una visión general de BranchCache en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5540d89827b80a21bf23f6a2aa8f54f09dfde67a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache"></a>BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema, que está destinado a profesionales de tecnología de información (TI), proporciona información general sobre BranchCache, incluidos los modos BranchCache, características, capacidades y las funciones de BranchCache que está disponible en diferentes sistemas operativos.

> [!NOTE]
> Además de este tema, la siguiente documentación BranchCache está disponible.
> 
> - [El Shell de red BranchCache y comandos de Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guía de implementación de BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**¿Que pueden interesarte BranchCache?**

Si eres un administrador del sistema, red o arquitecto de la solución de almacenamiento u otros profesionales de TI, BranchCache interesantes en las siguientes circunstancias:

- Diseñar o admite la infraestructura de TI de una organización que tiene dos o más ubicaciones físicas y una conexión de área extensa (WAN) de la red de las sucursales a la oficina principal.

- Diseñar o admite la infraestructura de TI de una organización que ha implementado tecnologías de nube, y se usa una conexión WAN por los trabajadores para acceder a los datos y aplicaciones en ubicaciones remotas.

- Tendrás que optimizar el uso de ancho de banda WAN al reducir la cantidad de tráfico de red entre las sucursales y la oficina principal.

- Has implementado o planeación implementar servidores de contenido en la oficina principal que coincidan con las configuraciones que se describen en este tema.

- Los equipos cliente en las sucursales ejecutan Windows 10, Windows 8.1, Windows 8 o Windows 7.

Este tema incluye las siguientes secciones:

-   [¿Qué es BranchCache?](#bkmk_what)

-   [BranchCache modos](#BKMK_2)
  
-   [BranchCache habilitado servidores de contenido](#BKMK_3)
  
-   [BranchCache y la nube](#BKMK_3a)
  
-   [Versiones de la información de contenido](#bkmk_version)  
  
-   [Cómo BranchCache controla las actualizaciones de contenido en archivos](#bkmk_handles)  
  
-   [Guía de instalación de BranchCache](#BKMK_4)  
  
-   [Versiones de sistema operativo para BranchCache](#bkmk_os)  
  
-   [BranchCache seguridad](#bkmk_security)  
  
-   [Procesos y flujo de contenido](#bkmk_flow)  
  
-   [Seguridad de la memoria caché](#bkmk_cache)  
  
## <a name="bkmk_what"></a>¿Qué es BranchCache?

BranchCache es una tecnología de optimización de ancho de banda (WAN) de red de área extensa que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en algunas ediciones de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 y Windows 7. Para optimizar el ancho de banda WAN cuando los usuarios acceder a contenido en servidores remotos, BranchCache recupera el contenido de la oficina principal u hospeda servidores de contenido de la nube y almacena en caché el contenido en sucursales, permitir que los equipos en sucursales para acceder al contenido localmente en lugar de en la red WAN de cliente.
  
En las sucursales, el contenido se almacena en servidores que están configurados en la memoria caché de host o cuando no hay ningún servidor está disponible en las sucursales, en los equipos cliente que ejecutan Windows 10, Windows 8.1, Windows 8 o Windows 7. Después de un equipo cliente solicita y recibe contenido de la oficina principal y el contenido se almacena en caché en la sucursal, otros equipos en la misma sucursal pueden obtener el contenido de forma local en lugar de descargar el contenido del servidor de contenido a través del vínculo WAN.

Cuando las solicitudes posteriores para el mismo contenido se realizan los equipos cliente, los clientes descargan *contenido información* desde el servidor en lugar del contenido real. Información sobre el contenido consta de los hash que se calculan con fragmentos de contenido original y son muy pequeño en comparación con el contenido de los datos originales. Los equipos cliente, a continuación, usan la información de contenido para localizar el contenido de una memoria caché en las sucursales, si se encuentra en un equipo cliente o en un servidor de la caché. Servidores y equipos cliente también usan la información sobre el contenido para proteger el contenido almacenado en caché para que los usuarios no autorizados no puede acceder a.

BranchCache aumenta la productividad del usuario final mejora los tiempos de respuesta de la consulta de contenido para los clientes y servidores en sucursales y también puede ayudar a mejorar el rendimiento de la red reduciendo el tráfico en vínculos WAN.

## <a name="BKMK_2"></a>BranchCache modos
BranchCache tiene dos modos de operación: distribuye el modo de caché y el modo de la memoria caché hospedada.

Cuando implementas BranchCache en modo de caché distribuido, la memoria caché de contenido en una sucursal se distribuye entre los equipos cliente.

Cuando implementas BranchCache en modo de caché hospedada, la memoria caché de contenido en una sucursal es hospedada en uno o más equipos de servidor, que se denominan servidores de caché.

> [!NOTE]
> Puedes implementar BranchCache con ambos modos, el modo sin embargo solo puede usarse por sucursal. Por ejemplo, si tienes dos sucursales, que tiene un servidor y otro que no lo hace, puedes implementar BranchCache en modo de la memoria caché hospedada en la oficina que contiene un servidor, al implementar BranchCache en modo de caché distribuida en la oficina que contenga solo los equipos cliente.

En la ilustración siguiente, se implementa BranchCache en ambos modos.  

![BranchCache modos](../media/BranchCache/bc_modes.jpg)

Modo de caché distribuida es ideal para las sucursales pequeñas que no contienen un servidor local para usarlo como un servidor de la memoria caché hospedada. Modo de caché distribuida te permite implementar BranchCache sin hardware adicional en sucursales.

Si la sucursal donde quieres implementar BranchCache contiene infraestructura adicional, como uno o varios servidores que ejecutan otras cargas de trabajo, implementar BranchCache en modo de caché hospedada es útil para los siguientes motivos:

### <a name="increased-cache-availability"></a>Disponibilidad de más memoria caché

Modo de caché hospedada aumenta la eficacia de la memoria caché porque el contenido está disponible, incluso si el cliente que originalmente solicitado y almacena en caché los datos sin conexión. Como el servidor de la memoria caché hospedada siempre está disponible, se almacena en caché más contenido, proporcionar mayor ahorro de ancho de banda WAN, y mejora la eficacia de BranchCache.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Centraliza el almacenamiento en caché para las sucursales de subred de varios


El modo de caché distribuida funciona en una única subred. En una subred de varios sucursal que está configurada para el modo de caché distribuida, no se pueden compartir un archivo descargado en una subred con los equipos cliente de otras subredes. 

Por este motivo, los clientes de otras subredes, no puedes descubrir que ya se ha descargado el archivo, Obtén el archivo desde el servidor de contenido principal de office, el uso de ancho de banda WAN en el proceso.

Cuando implementas el modo de caché hospedada, sin embargo, esto no es el caso, todos los clientes en una sucursal varios subred pueden tener acceso a una memoria caché único, que se almacena en el servidor de la memoria caché hospedada, incluso si los clientes están en diferentes subredes. Además, BranchCache en Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016 ofrece la posibilidad de implementar más de un servidor de la memoria caché hospedada por sucursal.

> [!CAUTION]
> Si usas BranchCache para SMB almacenamiento en caché de archivos y carpetas, deshabilitar archivos sin conexión. Si deshabilitas archivos sin conexión, SMB BranchCache almacenamiento en caché no funciona correctamente.

## <a name="BKMK_3"></a>BranchCache habilitado servidores de contenido

Cuando implementas BranchCache, el contenido de origen se almacena en servidores de contenido BranchCache habilitados en la oficina principal o en un centro de datos de la nube. Se admiten los siguientes tipos de servidores de contenido por BranchCache:

> [!NOTE]
> Solo contenido de origen - es decir, contenido que los equipos cliente obtener inicialmente desde un servidor de contenido habilitado BranchCache - tiene aceleración por BranchCache. Contenido que los equipos cliente obtener directamente de otras fuentes, como los servidores Web en Internet o la actualización de Windows, no se almacena en caché los equipos cliente u hospedado servidores de caché y, a continuación, se comparte con otros equipos de la sucursal. Sin embargo, si quieres acelerar el contenido de la actualización de Windows, puede instalar a un servidor de aplicaciones de Windows Server Update Services (WSUS) en su oficina principal o el centro de datos de la nube y configurarla como un servidor de contenido BranchCache.

### <a name="web-servers"></a>Servidores Web

Servidores Web compatibles incluyen equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que tiene instalado el rol de servidor Web Server (IIS) y que usan el protocolo de transferencia de hipertexto (HTTP) o HTTP seguro (HTTPS).

Además, el servidor Web debe tener la característica BranchCache instalada.

### <a name="file-servers"></a>Servidores de archivos

Los servidores de archivo admitidos incluyen equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que tienen el rol de servidor de servicios de archivo y la BranchCache para archivos de red los servicios de rol instalados. 

Estos servidores de archivos usan el bloque de mensajes de servidor (SMB) para intercambiar información entre equipos. Después de completar la instalación del servidor de archivos, también debes compartir carpetas y habilitar la generación de hash para carpetas compartidas mediante Directiva de grupo o la directiva de equipo Local para habilitar BranchCache.

### <a name="application-servers"></a>Servidores de aplicaciones

Servidores de aplicaciones compatibles son equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con servicio de transferencia inteligente en segundo plano (BITS) instalado y habilitado. 

Además, el servidor de aplicaciones debe tener la característica BranchCache instalada. Como ejemplos de servidores de aplicaciones, puedes implementar los servidores de Microsoft Windows Server Update Services (WSUS) y Microsoft System Center Configuration Manager Branch Distribution Point como BranchCache servidores de contenido.

## <a name="BKMK_3a"></a>BranchCache y la nube

La nube tiene el enorme potencial para reducir los gastos operacionales y lograr nuevos niveles de escala, pero mueve cargas de trabajo fuera de las personas que dependen de ellos capaz de aumentar los costos de red y afectar negativamente a la productividad. Los usuarios esperan de alto rendimiento y no importa que están hospedados sus aplicaciones y datos. 

BranchCache puede mejorar el rendimiento de aplicaciones en red y reducir el consumo de ancho de banda con una caché de datos compartida.  Mejora la productividad en sucursales y en las oficinas centrales, donde los trabajadores utilizan los servidores que se implementan en la nube.

Ya no requiere BranchCache nuevo hardware o cambios de la topología de red, es una solución excelente para la mejora de la comunicación entre las ubicaciones de office y nubes públicas y privadas.

> [!NOTE]
> Dado que algunos servidores proxy Web no pueden procesar encabezados de codificación de contenido no estándares, se recomienda que uses BranchCache con Hyper texto transferencia protocolo seguro (HTTPS) y no HTTP.
  
=== Para obtener más información acerca de las tecnologías de nube en Windows Server 2016, consulta [definido redes Software & #40; SDN & #41; ](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versiones de la información de contenido

Existen dos versiones de información de contenido:

- Información sobre el contenido que sea compatible con equipos que ejecutan Windows Server 2008 R2 y Windows 7 se denomina versión 1, o V1. Con V1 BranchCache segmentación de archivo, los segmentos de archivo son más grandes que v2 y son de tamaño fijo. Debido a tamaños grandes segmento fijo, cuando un usuario realiza un cambio que modifica la longitud del archivo, no solo se invalida el segmento con el cambio, pero no se invalidan todos los segmentos hasta el final del archivo. La siguiente llamada para el archivo modificado por otro usuario en las sucursales ocasiona, por tanto, ahorro de ancho de banda WAN reducidos porque el contenido cambiado y todo el contenido después del cambio se envían a través del vínculo WAN.

- Información sobre el contenido que sea compatible con equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 y Windows 8 se llama a la versión 2 o V2. Información sobre el contenido v2 usa segmentos de tamaños variables, más pequeños que toleran mejor a los cambios en un archivo. Esto aumenta la probabilidad de que los segmentos desde una versión anterior del archivo puede volver a usarse cuando los usuarios acceder a una versión actualizada, provocan recuperar únicamente la parte modificada del archivo del servidor de contenido y usa menos ancho de banda WAN.

La siguiente tabla proporciona información sobre la versión de la información sobre el contenido que se usa según qué cliente, el servidor de contenido, y hospedado sistemas operativos de servidor de caché que usas en la implementación de BranchCache.

> [!NOTE]
> En la tabla siguiente, el acrónimo "SO" significa el sistema operativo.

|Sistema operativo cliente|Sistema operativo de servidor de contenido|Sistema operativo de servidor de memoria caché hospedada|Versión de la información de contenido|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 y Windows 7 |Windows Server 2012 o posterior|Windows Server 2012 o posterior; Ninguno para el modo de caché distribuida|V1|
|Windows Server 2012 o posterior; Windows 8 o versiones posteriores|Windows Server 2008 R2 |Windows Server 2012 o posterior; Ninguno para el modo de caché distribuida|V1|
|Windows Server 2012 o posterior; Windows 8 o versiones posteriores| Windows Server 2012 o posterior| Windows Server 2008 R2 |V1|
|Windows Server 2012 o posterior; Windows 8 o versiones posteriores|Windows Server 2012 o posterior|Windows Server 2012 o posterior; Ninguno para el modo de caché distribuida|V2|

Cuando tienes servidores de contenido y hospedado servidores de caché que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, usa la versión de información sobre el contenido que sea apropiada según el sistema operativo del cliente BranchCache que solicita información de. 

Cuando los equipos que ejecutan Windows Server 2012 y Windows 8 o sistemas operativos posteriores solicitan contenido, los servidores de la memoria caché de contenido y hospedadas usan información sobre el contenido V2; Cuando los equipos que ejecutan Windows Server 2008 R2 y Windows 7 solicitan contenido, los servidores de la memoria caché de contenido y hospedadas usan información sobre el contenido V1.

>[!IMPORTANT]
>Cuando implementas BranchCache en modo de caché distribuido, los clientes que usan la información de contenido diferentes versiones no compartan contenido entre sí. Por ejemplo, un equipo cliente que ejecuta Windows 7 y un equipo cliente que ejecutan Windows 10 que están instalados en la misma sucursal no compartir contenido entre sí.
  
## <a name="bkmk_handles"></a>Cómo BranchCache controla las actualizaciones de contenido en archivos

Cuando los usuarios de sucursales modificar o actualización el contenido de documentos, los cambios se escriben directamente en el servidor de contenido en la oficina principal sin la intervención del BranchCache. Esto sucede si el usuario descarga el documento desde el servidor de contenido o había obtenida de una caché hospedada o se distribuyen en las sucursales.

Cuando un cliente diferente en una sucursal solicita el archivo modificado, se descarga del servidor de office principal los segmentos del archivo de nuevo y se agregan a la caché distribuida u hospedada en ese rama. Por este motivo, los usuarios de sucursales siempre recibirán las versiones más recientes de contenido almacenado en caché.

## <a name="BKMK_4"></a>Guía de instalación de BranchCache

Puedes usar el administrador del servidor en Windows Server 2016 para instalar la característica BranchCache o la BranchCache para el servicio de rol de archivos de red del rol de servidor de servicios de archivo. Puedes usar la tabla siguiente para determinar si se debe instalar el servicio de rol o la característica.

|Funcionalidad|Ubicación del equipo|Instalar este elemento BranchCache|
|-----------------|---------------------|------------------------------------|
|Servidor de contenido \ (server\ aplicación basada en BITS)|Centro de datos principal de office o en la nube|Característica BranchCache|
|Servidor de contenido \(Web server\)|Centro de datos principal de office o en la nube|Característica BranchCache|
|Servidor de contenido \ (servidor de archivos usando el protocol\ SMB)|Centro de datos principal de office o en la nube|BranchCache para el servicio de rol de archivos de red del rol de servidor de servicios de archivos|
|Servidor de la memoria caché hospedada|Sucursal|Característica BranchCache con el modo de servidor de memoria caché hospedada habilitado|
|Equipo cliente BranchCache habilitado|Sucursal|No es necesaria; la instalación solo tienes que habilitar BranchCache y un modo BranchCache \(distributed or hosted\) en el cliente|

Para instalar el servicio de rol o la característica, abre el administrador del servidor y selecciona los equipos donde desea habilitar la funcionalidad de BranchCache. En el administrador del servidor, haz clic en **administrar**y, a continuación, haz clic en **agregar Roles y características**. La **agregar Roles y características** asistente abre. Como ejecutar al asistente, selecciona las opciones siguientes:

- En la página del asistente **seleccionar el tipo de instalación**, selecciona **instalación basada en rol o característica**.

- En la página del asistente **seleccionar Roles de servidor**, si vas a instalar un servidor de archivos habilitado BranchCache, expanda **File and Storage Services** y **servicios de iSCSI y archivo**y, a continuación, selecciona **BranchCache para archivos de red**.  Para ahorrar espacio en disco, también puedes seleccionar la **desduplicación datos** rol de servicio y, a continuación, continúe con el Asistente para la instalación y la finalización. Si no desea instalar a un servidor de archivos BranchCache habilitado, se instala el rol de servicios de archivos y almacenamiento con la BranchCache para el servicio de rol de archivos de red.

- En la página del asistente **Select features**, si vas a instalar un servidor de contenido que no es un servidor de archivos o vas a instalar un servidor de la memoria caché hospedada, seleccione **BranchCache**y, a continuación, continúe con el Asistente para la instalación y la finalización. Si no desea instalar a un servidor de contenido que no sea un servidor de archivos o un servidor de la memoria caché hospedada, instala la característica BranchCache.
  
## <a name="bkmk_os"></a>Versiones de sistema operativo para BranchCache

Siguiente es una lista de sistemas operativos que admiten diferentes tipos de BranchCache funcionalidad.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Sistemas operativos para la funcionalidad de equipo cliente BranchCache

Los siguientes sistemas operativos proporcionan BranchCache con compatibilidad para el servicio de transferencia inteligente en segundo plano (BITS), el protocolo de transferencia de hipertexto (HTTP) y el bloque de mensajes de servidor (SMB).

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- Windows 8 Enterprise

- Windows 7 Enterprise

- Windows 7 Ultimate

En los siguientes sistemas operativos, BranchCache no admite la funcionalidad HTTP y SMB, pero es compatible con la funcionalidad de BranchCache BITS.

-   Windows 10 Pro, BITS solo admiten

-   Windows 8.1 Pro, BITS solo admiten

-   Windows 8 Pro, BITS solo admiten

-   Windows 7 Pro BITS solo admiten

> [!NOTE]
> BranchCache no está disponible de forma predeterminada en los sistemas operativos Windows Server 2008 o Windows Vista. En estos sistemas operativos, sin embargo, si descarga e instala la actualización de Windows Management Framework BranchCache funcionalidad está disponible para el protocolo de servicio de transferencia inteligente en segundo plano (BITS) solo. Para obtener más información y para descargar Windows Management Framework, consulta [Windows Management Framework (Windows PowerShell 2.0, WinRM 2.0 y 4.0 de BITS)](https://go.microsoft.com/fwlink/?LinkId=188677) en https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Sistemas operativos para BranchCache funcionalidad del servidor de contenido

Puedes usar las familias de sistemas operativos de Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como BranchCache servidores de contenido.

Además, la familia de Windows Server 2008 R2 de sistemas operativos puede usarse como BranchCache servidores de contenido, con las siguientes excepciones:

- BranchCache no se admite en las instalaciones de Server Core de Windows Server 2008 R2 Enterprise con Hyper-V.

- BranchCache no se admite en las instalaciones de Server Core de Windows Server 2008 R2 Datacenter con Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Sistemas operativos para BranchCache hospedado funcionalidad del servidor de caché

Puedes usar las familias de sistemas operativos de Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como BranchCache había hospedado servidores de caché.

Además, los siguientes sistemas operativos de Windows Server 2008 R2 puede usarse como BranchCache había hospedado servidores de caché:

- Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 Enterprise con Hyper-V

- Instalación de Windows Server 2008 R2 Enterprise Server Core

- Instalación Windows Server 2008 R2 Enterprise Server Core con Hyper-V

- Windows Server 2008 R2 para sistemas basados en Itanium

- Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter con Hyper-V

- Instalación del núcleo de servidor de Windows Server 2008 R2 Datacenter con Hyper-V

## <a name="bkmk_security"></a>BranchCache seguridad

BranchCache implementa un método seguro por diseño que funcione perfectamente junto con las arquitecturas de seguridad de red existente, sin necesidad de otros equipos o configuración de seguridad adicional complejos.
  
BranchCache es no invasiva y no altera los procesos de autenticación o autorización de Windows. Después de implementar BranchCache, autenticación aún se realiza con las credenciales de dominio y la manera en que autorización con listas de Control de acceso (ACL) funciones se modifica. Además, otras configuraciones seguirán funcionando exactamente igual que antes de la implementación de BranchCache.

El modelo de seguridad BranchCache se basa en la creación de metadatos, que toman la forma de una serie de hash. Estos valores de hash también se denominan información sobre el contenido.

Después de crea la información de contenido, se usa en los intercambios de mensajes BranchCache en lugar de los datos reales y se intercambian mediante los protocolos compatibles (HTTP, HTTPS y SMB).

Se mantienen cifradas y no puede acceder a los clientes que no tienen permiso para acceder a contenido desde el origen de datos en caché.  Los clientes deben autenticar y autorizar el origen de contenido original antes de que pueden recuperar metadatos de contenido y deben tener metadatos de contenido para tener acceso a la memoria caché en la oficina local.

### <a name="how-branchcache-generates-content-information"></a>Cómo BranchCache genera la información de contenido

Dado que se crea la información sobre el contenido de varios elementos, el valor de la información de contenido siempre es único. Estos elementos son:

- El contenido real (como las páginas Web o archivos compartidos) desde el que se derivan los hash.  

- Parámetros de configuración, como el tamaño del algoritmo y bloque hash. Para generar información de contenido, el servidor de contenido el contenido divide en segmentos y, a continuación, subdivide los segmentos en bloques. BranchCache utiliza los hashes criptográficos seguros para identificar y comprueba cada bloque y un segmento, compatibilidad con el algoritmo de hash SHA256.

- Una clave secreta del servidor. Todos los servidores de contenido deben estar configurados con una clave secreta del servidor, que es un valor de longitud arbitraria binario.

> [!NOTE]
> El uso de un secreto de servidor garantiza que los equipos cliente que no se puede generar la información de contenido. Esto impide que los usuarios malintencionados mediante ataques de fuerza bruta con equipos cliente habilitados BranchCache para adivinar cambios menores en el contenido en las versiones en situaciones en que el cliente ha tenido acceso a una versión anterior, pero no tiene acceso a la versión actual.

### <a name="content-information-details"></a>Detalles de la información de contenido

BranchCache usa la clave secreta de servidor como clave para derivar un hash de contenido específico que se envía a los clientes autorizados. Aplicar un algoritmo hash a la clave secreta de servidor combinada y los Hash de datos, genera este valor de hash.

Este hash se denomina la clave secreta de segmento. BranchCache usa secretos segmento para proteger las comunicaciones. Además, BranchCache crea una lista de Hash de bloqueo, que es la lista de bloques de datos con hash, y el Hash de los datos, que es generado por la lista de Hash de bloque de hash.

La información de contenido incluye lo siguiente:

- La lista de Hash de bloqueo:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- El Hash de datos (HoD):

    `HoD = Hash(BlockHashList)`

- Clave secreta del segmento (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache usa el protocolo de almacenamiento en caché del contenido del mismo nivel y el protocolo de marco de recuperación para implementar los procesos que son necesarios para garantizar el almacenamiento en caché y la recuperación de datos entre las memorias caché de contenido seguro.

Además, BranchCache controla el contenido información con el mismo grado de seguridad que se usa para controlar y transmitir el propio contenido.

## <a name="bkmk_flow"></a>Procesos y flujo de contenido

El flujo de información sobre el contenido y el contenido real se divide en cuatro fases:

1.  [BranchCache procesos: solicitud de contenido](#BKMK_8)

2.  [BranchCache procesos: buscar contenido](#BKMK_9)

3.  [BranchCache procesos: recuperar el contenido](#BKMK_10)

4.  [BranchCache procesos: contenido en caché](#BKMK_11)

Las siguientes secciones describen estas fases.

## <a name="BKMK_8"></a>BranchCache procesos: solicitud de contenido

En la primera fase, el equipo cliente en la sucursal solicita contenido, como un archivo o una página Web, desde un servidor de contenido en una ubicación remota, como una oficina principal. El servidor de contenido se comprueba que el equipo cliente está autorizado para recibir el contenido solicitado. Si está autorizado el equipo cliente y servidor de contenido y el cliente están habilitadas para BranchCache\, el servidor de contenido generará información sobre el contenido.

El servidor de contenido, a continuación, envía la información de contenido en el equipo cliente con el mismo protocolo que se habrían usado para el contenido real. 

Por ejemplo, si el equipo cliente solicita una página Web a través de HTTP, el servidor de contenido envía la información de contenido mediante HTTP. Por este motivo, la seguridad de red garantiza el contenido y la información de contenido son idénticos.

Cuando se recibe la parte inicial de la información de contenido (datos de Hash + clave secreta del segmento), el equipo cliente realiza las siguientes acciones:

- Usa la clave secreta del segmento (Kp) como la clave de cifrado (Ke).

- Genera el identificador del segmento (HoHoDk) desde el HoD y Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

La amenaza principal en este nivel es el riesgo para la clave secreta de segmento, pero BranchCache cifra los bloques de datos de contenido para proteger la clave secreta de segmento. BranchCache hace esto mediante la clave de cifrado que se deriva de la clave secreta de segmento del segmento contenido dentro del cual se encuentran los bloques de contenido.

Este enfoque garantiza que una entidad no posee el secreto de servidor no puede detectar el contenido real en un bloque de datos. El secreto de segmento se trata con el mismo grado de seguridad como texto no cifrado segmento de sí mismo, como conocimiento de la clave secreta de segmento para un segmento determinado permite que una entidad para obtener el segmento de sistemas del mismo nivel y, a continuación, descifrarlo. Conocimiento de la clave secreta de servidor no produce ningún texto no cifrado particular inmediatamente, pero puede usarse para determinados tipos de datos se derivan del texto cifrado y, a continuación, posiblemente exponen sabe parcialmente algunos datos que un ataque de fuerza bruta averiguación. El secreto de servidor, por lo tanto, debe ser confidenciales.
  
## <a name="BKMK_9"></a>BranchCache procesos: buscar contenido

Después de que el equipo cliente recibe la información de contenido, el cliente usa el identificador de segmento para localizar el contenido solicitado en la caché de office rama local, si esa memoria caché se distribuye entre los equipos cliente o se encuentra en un servidor de la memoria caché hospedada.

Si el equipo cliente está configurado para el modo de la memoria caché hospedada, se configura con el nombre del equipo del servidor de la memoria caché hospedada y los contactos de ese servidor para recuperar el contenido.

Si el equipo cliente está configurado para el modo de caché distribuida, sin embargo, el contenido puede estar almacenado en varias cachés en varios equipos en las sucursales. El equipo cliente debe detectar dónde se encuentra el contenido antes de que se recupera el contenido.

Cuando están configurados para el modo de caché distribuida, los equipos cliente localizan contenido mediante un protocolo de detección que se basa en el protocolo de detección dinámica de los servicios Web (WS-Discovery). Los clientes envían detección WS multidifusión mensajes de sondeo para detectar el contenido almacenado en caché en la red. Mensajes de sondeo incluyen el identificador de segmento, que permite a los clientes comprobar si el contenido solicitado coincide con el contenido almacenado en la memoria caché. Clientes que reciban la respuesta de mensaje de sondeo inicial al cliente consultas con los mensajes de coincidencia de sondeo unidifusión si el ID de segmento coincide con contenido que se almacena en caché localmente.  

El éxito del proceso de detección WS depende en el hecho de que el cliente que realiza la detección tiene la información de contenido correcta, proporcionada por el servidor de contenido, para el contenido que está solicitando.

La amenaza a los datos durante la fase de solicitud de contenido principal es la divulgación de información, porque el acceso a la información de contenido implica el acceso autorizado a contenido. Para mitigar este riesgo, el proceso de detección no revelar la información de contenido que no sean el ID de segmento, que no muestra nada sobre el segmento de texto no cifrado que contiene el contenido.

Además, otro equipo cliente que ejecuta un usuario malintencionado en la misma subred de la red puede ver el tráfico del detección BranchCache con el origen de contenido que va a través del enrutador.

Si el contenido solicitado no se encuentra en las sucursales, el cliente solicita el contenido directamente desde el servidor de contenido a través del vínculo WAN.

Después de recibe el contenido, se agrega a la memoria caché local, en el equipo cliente o en un servidor de la memoria caché hospedada. En este caso, la información de contenido impide que a un cliente u hospeda el servidor de caché de agregar a la memoria caché local de cualquier contenido que no coincide con el hash. El proceso de comprobación de contenido comparando los hash garantiza que se agrega contenido válido solo en la memoria caché, y se protege la integridad de la memoria caché local.

## <a name="BKMK_10"></a>BranchCache procesos: recuperar el contenido

Después de un equipo cliente localiza el contenido deseado en el host de contenido, que es un servidor de la memoria caché hospedada o un equipo de cliente de modo de caché distribuida, el equipo cliente inicia el proceso de recuperar el contenido.

Primero, el equipo cliente envía una solicitud para el host de contenido para el primer bloque que esta requiere. La solicitud contiene el intervalo de Id. de segmento y el bloque que identifican el contenido deseado. Dado que se devuelve un solo bloque, el intervalo de bloque contiene solo un único bloque. (Las solicitudes de varios bloques actualmente no se admiten.) El cliente también almacena la solicitud en su lista de solicitudes pendientes local.  

Después de recibir un mensaje de solicitud válido de un cliente, el host de contenido comprueba si existe el bloque especificado en la solicitud en la memoria caché de contenido del host de contenido.

Si el host de contenido es posee el bloque de contenido, el host de contenido envía una respuesta que contiene el identificador del segmento, el identificador de bloque, el bloque de datos cifrados y el vector de inicialización que se usa para cifrar el bloque.

Si el host de contenido no está posee el bloque de contenido, el host de contenido envía un mensaje de respuesta vacía. Esto informa que el equipo cliente que el host de contenido no tiene el bloque solicitado. Un mensaje de respuesta vacío contiene el identificador del segmento y el identificador de bloques del bloque solicitado, junto con un bloque de datos de tamaño cero.

Cuando el equipo cliente recibe la respuesta desde el host de contenido, el cliente comprueba que el mensaje corresponde a un mensaje de solicitud en su lista de solicitudes pendientes. (El índice de Id. de segmento y bloque debe coincidir con de una solicitud pendiente.)

Si este proceso de verificación es incorrecta y el equipo cliente no tiene un mensaje de solicitud correspondiente en su lista de solicitudes pendientes, el equipo cliente descarta el mensaje.

Si este proceso de comprobación se realiza correctamente y el equipo cliente tiene un mensaje de solicitud correspondiente en su lista de solicitudes pendientes, el equipo cliente descifra el bloque. El cliente, a continuación, valida el bloque descifrado con el hash de bloque apropiado de la información de contenido que inicialmente Obtiene el cliente desde el servidor de contenido original.

Si la validación se realiza correctamente, el bloque de descifrado se almacena en la memoria caché.

Este proceso se repite hasta que el cliente tiene todos los bloques necesarios.

> [!NOTE]
> Si no existen los segmentos de contenido completos en un equipo, el protocolo de recuperación recupera y ensambla el contenido de una combinación de orígenes: un conjunto de equipos de cliente de modo de caché, un servidor de la memoria caché hospedada y distribuidas - si la sucursal almacena en caché no contienen el contenido completo: el servidor de contenido original en la oficina principal.

Antes de BranchCache envía información sobre el contenido o contenido, los datos se cifran. BranchCache cifra el bloque del mensaje de respuesta. En Windows 7, el algoritmo de cifrado predeterminado que usa BranchCache es AES-128, la clave de cifrado está Ke y el tamaño de la clave es de 128 bits, tal como dicta el algoritmo de cifrado. 

BranchCache genera un vector de inicialización que es adecuado para el algoritmo de cifrado y usa la clave de cifrado para cifrar el bloque. BranchCache, a continuación, registra el algoritmo de cifrado y el vector de inicialización en el mensaje. 

Los clientes y servidores nunca de exchange, compartirán o enviar la clave de cifrado. El cliente recibe la clave de cifrado desde el servidor que hospeda el contenido de origen de contenido. Con el vector de e inicialización de algoritmo de cifrado que reciben del servidor, a continuación, descifra el bloque. No hay ningún otro explícita autenticación o autorización integrado en el protocolo de descarga.

### <a name="security-threats"></a>Amenazas de seguridad

Las amenazas de seguridad principal en este nivel incluyen:

- Manipulación de datos:

  *Manipula los datos de un cliente de servidor de datos a un solicitante*. El modelo de seguridad BranchCache usa hash para confirmar que el cliente ni el servidor ha modificado los datos.  

- Divulgación de información:  

    *BranchCache envía el contenido cifrado a cualquier cliente que especifica el identificador de segmento correspondiente*. Los identificadores de segmento son públicos, para que cualquier cliente pueda recibir contenido cifrado. Sin embargo, si un usuario malintencionado obtiene el contenido cifrado, debe conocer la clave de cifrado para descifrar el contenido. El protocolo de nivel superior realiza la autenticación y luego da a la información de contenido en el cliente autenticado y autorizado. La seguridad de la información de contenido es equivalente a la seguridad proporcionada para el propio contenido y BranchCache nunca expone la información de contenido.  

    *Un atacante husmea el cable para obtener el contenido*.  BranchCache cifra a todas las transferencias entre clientes mediante el uso de AES128 donde la clave secreta es Ke, lo cual evitó que datos desde que se examinados por desde la red.  Información sobre el contenido que se descarga desde el servidor de contenido está protegido en la misma manera como los datos en Sí habría sido y, por tanto, está protegidos no más o menos de divulgación de información que si BranchCache hubiese no usado en absoluto.

-   Denegación de servicio:

    *Un cliente se sientan abrumado por las solicitudes de datos*. BranchCache protocolos incorporan contadores de administración de la cola y temporizadores para impedir que los clientes que se está sobrecargado.

## <a name="BKMK_11"></a>BranchCache procesos: contenido en caché

En los servidores de la memoria caché hospedada que se encuentran en las sucursales y equipos de cliente de modo de caché distribuida, memorias caché de contenido se crean con el tiempo que se recupera contenido sobre vínculos WAN.

Cuando los equipos cliente se configuran con el modo de la memoria caché hospedada, se agrega contenido a su propia memoria caché local y también ofrecen datos al servidor de la memoria caché hospedada. El protocolo de la memoria caché hospedada proporciona un mecanismo para que los clientes informar al servidor de la memoria caché hospedada sobre la disponibilidad de contenido y segmentarlo.

Para cargar contenido en el servidor de la memoria caché hospedada, el cliente informa al servidor que tiene un segmento que está disponible. A continuación, el servidor de la memoria caché hospedada recupera toda la información de contenido que está asociado con el segmento ofrecido y descargas de los bloques dentro del segmento que realmente necesita. Este proceso se repite hasta que el cliente tiene que no hay más segmentos para ofrecer el servidor de la memoria caché hospedada.

Para actualizar el servidor de la memoria caché hospedada mediante el protocolo de la memoria caché hospedada, deben cumplirse los siguientes requisitos:

- El equipo cliente debe tener un conjunto de bloques dentro de un segmento que puede ofrecer al servidor de la memoria caché hospedada. El cliente debe proporcionar información sobre el contenido del segmento ofrecida; Esto está formado por el ID de segmento, el segmento de datos de Hash, el secreto del segmento y una lista de todos los hash de bloque que están contenidos dentro del segmento.

- Para la memoria caché hospedada son necesarios los servidores que ejecutan Windows Server 2008 R2, un servidor de la memoria caché hospedada certificado y la clave privada asociada, y la entidad de certificación (CA) que emitió el certificado debe ser de confianza para los equipos cliente en las sucursales. Esto permite que el cliente y servidor participar correctamente en la autenticación de servidor HTTPS.

    > [!IMPORTANT]
    > Servidores de la memoria caché hospedada que ejecutan Windows Server 2012, Windows Server 2012 R2 o Windows Server 2016 no requieren un certificado de servidor de la memoria caché hospedada y la clave privada asociada.  

- El equipo cliente se configura con el nombre del equipo de la memoria caché hospedada servidor y el número de puerto de protocolo de Control de transmisión (TCP) en el que el servidor de la memoria caché hospedada escucha el tráfico de BranchCache. La memoria caché hospedada del certificado del servidor se enlaza a este puerto. El nombre de equipo del servidor de la memoria caché hospedada puede ser un nombre de dominio completo (FQDN), si el servidor de la memoria caché hospedada es un equipo miembro del dominio; o puede ser el nombre NetBIOS del equipo si la memoria caché hospedada server no es un miembro de dominio.

- El equipo cliente activamente escucha las solicitudes entrantes de bloque. El puerto en el que está escuchando se pasa como parte de los mensajes de la oferta desde el cliente al servidor de la memoria caché hospedada. Esto permite que el servidor de la memoria caché hospedada usen protocolos de BranchCache para conectar con el equipo cliente para recuperar los bloques de datos en el segmento.

- El servidor de la memoria caché hospedada empieza a escuchar solicitudes entrantes de HTTP cuando se inicializa.

- Si el servidor de la memoria caché hospedada está configurado para requerir la autenticación de equipo cliente, el cliente y el servidor de la memoria caché hospedada se requiere autenticación HTTPS de soporte técnico.

### <a name="hosted-cache-mode-cache-population"></a>Memoria caché hospedada modo caché población

El proceso de agregar contenido a la memoria caché hospedada caché del servidor en una sucursal comienza cuando el cliente envía una INITIAL_OFFER_MESSAGE, que incluye el Id. El ID de segmento en la solicitud INITIAL_OFFER_MESSAGE se usa para recuperar el Hash de los datos del segmento correspondiente, lista de hash de bloque y el secreto de segmento de la memoria caché hospedada caché del servidor bloque. Si el servidor de la memoria caché hospedada ya tiene toda la información de contenido de un segmento específico, la respuesta a la INITIAL_OFFER_MESSAGE será Aceptar y se produce ninguna solicitud de descarga de bloques.

Si el servidor de la memoria caché hospedada no tiene todos los bloques de datos ofrecida en el que se asocian con los valores de hash de bloque en el segmento, la respuesta a la INITIAL_OFFER_MESSAGE está INTERESADA. El cliente, a continuación, envía una SEGMENT_INFO_MESSAGE que describe el segmento único que se ofrece. El servidor de la memoria caché hospedada responde con un mensaje Aceptar e inicia la descarga de los bloques de faltantes de la oferta de equipo cliente.

El segmento de datos de Hash, lista de hash de bloque y el secreto de segmento se usan para asegurarte de que el contenido que se está descargando no se ha alterado o modificado de lo contrario. Los bloques descargados, a continuación, se agregan a la memoria caché hospedada caché del servidor bloque.

## <a name="bkmk_cache"></a>Seguridad de la memoria caché  
En esta sección se proporciona información sobre cómo BranchCache protege los datos almacenados en caché en los equipos cliente y en servidores de la memoria caché hospedada.

### <a name="client-computer-cache-security"></a>Seguridad de memoria caché del equipo cliente
La mayor amenaza para los datos almacenados en el BranchCache es alteraciones. Si un atacante puede alterar con información de contenido y de contenido que se almacena en la memoria caché, a continuación, es posible usar esto para prueba e iniciar un ataque contra los equipos que usan BranchCache. Los atacantes pueden iniciar un ataque insertando software malintencionado en lugar de otros datos. BranchCache reduce esta amenaza automáticamente con la validación de todo el contenido con el hash de bloque se encuentra en la información de contenido. Si un atacante intenta alterar estos datos, se descarta y se ha reemplazado por los datos válidos desde el código fuente original.

Una amenaza secundaria para los datos almacenados en el BranchCache es divulgación de información. En el modo de caché distribuida, el cliente almacena solo el contenido que ha solicitado. Sin embargo, estos datos se almacenan en un texto claro y pueden estar en peligro. Para ayudarte a restringir el acceso de la memoria caché para el BranchCache Service solo, la memoria caché local está protegida por permisos del sistema de archivos que se especifican en una ACL. 

Aunque la ACL es eficaz para evitar que los usuarios no autorizados tengan acceso a la memoria caché, es posible que un usuario con privilegios administrativos acceder a la memoria caché cambiando manualmente los permisos que se especifican en la ACL. BranchCache no protege contra el uso de una cuenta de administrador malintencionado.

No se cifran los datos que se almacenan en la memoria caché de contenido, por lo que si la pérdida de datos es un problema, puedes usar tecnologías de cifrado como BitLocker o el sistema de cifrado de archivos (EFS). La memoria caché local que se usa en BranchCache no aumentará la amenaza de divulgación de información a cargo de un equipo en la oficina sucursal; la memoria caché contiene solo copias de archivos que se encuentran sin cifrar en otro lugar en el disco. 

Cifrado de todo el disco es particularmente importante en entornos en los que es difícil garantizar la seguridad física de los clientes. Por ejemplo, el cifrado de todo el disco ayuda a proteger datos confidenciales en equipos portátiles que se podrían eliminar desde el entorno de sucursales.

### <a name="hosted-cache-server-cache-security"></a>Seguridad de memoria caché del servidor de memoria caché hospedada

En el modo de la memoria caché hospedada, la mayor amenaza para la seguridad del servidor de la memoria caché hospedada es divulgación de información. BranchCache en un entorno de la memoria caché hospedada se comporta de manera similar al modo de caché distribuida, con permiso del sistema de archivo protege los datos en caché. La diferencia es que el servidor de la memoria caché hospedada almacena todo el contenido que las solicitudes de cualquier equipo BranchCache habilitado en la sucursal, en lugar de solo los datos que solicita un solo cliente. Las consecuencias de intrusiones no autorizado a esta caché podrían ser mucho más graves, porque los datos mucho más en riesgo.  
  
En un entorno de la memoria caché hospedada donde el servidor de la memoria caché hospedada ejecuta Windows Server 2008 R2, es recomendable si alguno de los clientes en la sucursal puede acceder a datos confidenciales a través del vínculo WAN el uso de tecnologías de cifrado como BitLocker o EFS. También es necesario impedir el acceso físico a la memoria caché hospedada, porque el cifrado de disco funciona únicamente cuando el equipo está apagado cuando el atacante obtenga acceso físico.  Si el equipo está activado o está en modo de suspensión, el cifrado de disco ofrece poca protección.

> [!NOTE]
> Servidores de la memoria caché hospedada que ejecutan Windows Server 2012, Windows Server 2012 R2 o Windows Server 2016 cifran todos los datos en la memoria caché de forma predeterminada, por lo que no se requiere el uso de tecnologías de cifrado adicional.

Incluso si un cliente está configurado en modo de caché hospedada, seguirá caché localmente los datos y quieres tomar medidas para proteger la memoria caché local además de la memoria caché en el servidor de la memoria caché hospedada.
