---
title: BranchCache
description: En este tema se proporciona información general sobre BranchCache en Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-bc
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a4587cff-c086-49f1-a0bf-cd74b8a44440
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7fe8d948a5f43fdab394490f543f3583167bdfe9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406768"
---
# <a name="branchcache"></a>BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, destinado a los profesionales de tecnología de la información (TI), se proporciona información general sobre BranchCache, incluidos los modos, las características, las capacidades y la funcionalidad de BranchCache que se encuentran disponibles en diferentes sistemas operativos.

> [!NOTE]
> Además de este tema, está disponible la siguiente documentación sobre BranchCache.
> 
> - [Shell de red de BranchCache y comandos de Windows PowerShell](../branchcache/BranchCache-Network-Shell-and-Windows-PowerShell-Commands.md)
> -   [Guía de implementación de BranchCache](../branchcache/deploy/BranchCache-Deployment-Guide.md)

**¿A quién le interesará BranchCache?**

Si es administrador de un sistema, arquitecto de soluciones de red o almacenamiento, u otro profesional de TI, BranchCache podría interesarle bajo las siguientes circunstancias:

- Diseña u ofrece asistencia técnica a una infraestructura de TI de una organización que tiene dos o más ubicaciones físicas y una conexión de red de área extensa (WAN) desde las sucursales a la oficina central.

- Diseña u ofrece asistencia técnica a una infraestructura de TI para una organización que implementó tecnologías de nube y los trabajadores usan una conexión WAN para acceder a los datos y las aplicaciones en ubicaciones remotas.

- Desea optimizar el uso del ancho de banda WAN reduciendo la cantidad de tráfico de red entre las sucursales y la oficina central.

- Ha implementado o planea implementar servidores de contenido en la oficina central que coinciden con las configuraciones que se describen en este tema.

- Los equipos cliente de sus sucursales ejecutan Windows 10, Windows 8.1, Windows 8 o Windows 7.

Este tema incluye las siguientes secciones:

-   [¿Qué es BranchCache?](#bkmk_what)

-   [Modos de BranchCache](#BKMK_2)
  
-   [Servidores de contenido habilitados para BranchCache](#BKMK_3)
  
-   [BranchCache y la nube](#BKMK_3a)
  
-   [Versiones de información de contenido](#bkmk_version)  
  
-   [Cómo administra BranchCache las actualizaciones de contenido en archivos](#bkmk_handles)  
  
-   [Guía de instalación de BranchCache](#BKMK_4)  
  
-   [Versiones de sistema operativo para BranchCache](#bkmk_os)  
  
-   [Seguridad de BranchCache](#bkmk_security)  
  
-   [Flujo de contenido y procesos](#bkmk_flow)  
  
-   [Seguridad de caché](#bkmk_cache)  
  
## <a name="bkmk_what"></a>¿Qué es BranchCache?

BranchCache es una tecnología de optimización del ancho de banda de la red de área extensa (WAN) que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en algunas ediciones de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 y Windows 7. Para mejorar el ancho de banda de la WAN cuando los usuarios acceden a contenido de servidores remotos, BranchCache obtiene el contenido de los servidores de contenido de la oficina central o de la nube hospedada y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales accedan al contenido de forma local, y no a través de la WAN.
  
En las sucursales, el contenido se almacena en servidores configurados para hospedar la memoria caché o, si no hay ningún servidor disponible en la sucursal, en los equipos cliente que ejecutan Windows 10, Windows 8.1, Windows 8 o Windows 7. Cuando un equipo cliente haya solicitado y recibido un determinado contenido de la oficina central y este se haya almacenado en caché en la sucursal, el resto de equipos de la sucursal podrá disponer de este contenido de forma local y sin necesidad de descargarlo del servidor de contenido a través de un vínculo WAN.

Cuando los equipos cliente posteriormente realizan solicitudes para el mismo contenido, los clientes descargan *información del contenido* del servidor en lugar del contenido en sí. La información de contenido se compone de hashes que se calculan mediante fragmentos del contenido original y que son sumamente pequeños en comparación con el contenido de los datos originales. Después, los equipos cliente usan la información de contenido para ubicar el contenido de una memoria caché en la sucursal, ya sea que la memoria caché se encuentre en un equipo cliente o en un servidor. Los equipos y servidores cliente también usan información de contenido para proteger el contenido almacenado en caché de manera que usuarios no autorizados no puedan acceder a él.

BranchCache aumenta la productividad del usuario final al mejorar tanto el tiempo de respuesta de las consultas de contenido en los clientes y servidores de las sucursales como el rendimiento de la red, ya que reduce el tráfico en los vínculos WAN.

## <a name="BKMK_2"></a>Modos de BranchCache
BranchCache cuenta con dos modos de operación: modo Caché distribuida y modo Caché hospedada.

Cuando se implementa BranchCache en modo Caché distribuida, la caché de contenido en la sucursal se distribuye entre los equipos cliente.

Cuando implementa BranchCache en modo Caché hospedada, la memoria caché de contenido en la sucursal se hospeda en uno o más equipos servidores que se denominan servidores de caché hospedada.

> [!NOTE]
> Se puede implementar BranchCache con ambos modos; sin embargo, solo se puede usar un modo por sucursal. Por ejemplo, si tiene dos sucursales, una que tiene un servidor y una que no, puede implementar BranchCache en modo Caché hospedada en la oficina que tiene el servidor, e implementar BranchCache en modo Caché distribuida en la oficina que solo tiene equipos cliente.

En la siguiente ilustración, se implementa BranchCache en ambos modos.  

![Modos de BranchCache](../media/BranchCache/bc_modes.jpg)

El modo Caché distribuida es el más apropiado para sucursales pequeñas que no tienen un servidor local para usar como servidor de caché hospedada. El modo Caché distribuida le permite implementar BranchCache sin hardware adicional en las sucursales.

Si la sucursal donde desea implementar BranchCache contiene infraestructura adicional, como uno o más servidores que ejecutan otras cargas de trabajo, implementar BranchCache en modo Caché hospedada resulta beneficioso por los siguientes motivos:

### <a name="increased-cache-availability"></a>Mayor disponibilidad de la memoria caché

El modo Caché hospedada aumenta la eficacia del almacenamiento en caché, ya que el contenido se encuentra disponible incluso si el cliente que originalmente solicitó los datos almacenados en caché está desconectado. Dado que el servidor de caché hospedada está siempre disponible, se almacena más contenido en caché, lo cual ofrece más ahorro de ancho de banda WAN y se mejora la eficiencia de BranchCache.

### <a name="centralized-caching-for-multiple-subnet-branch-offices"></a>Almacenamiento en caché centralizado para sucursales de varias subredes


El modo Caché distribuida funciona en una subred única. En una sucursal de varias subredes que está configurada para el modo Caché distribuida no se puede compartir un archivo descargado a una subred con equipos cliente de otras subredes. 

Por este motivo, los clientes de otras subredes, que no pueden saber si el archivo ya se descargó, obtienen el archivo del servidor de contenido de la oficina central, usando ancho de banda WAN en el proceso.

Sin embargo, cuando implementan un modo Caché hospedada (este no es el caso), todos los clientes de la sucursal de varias subredes pueden acceder a una memoria caché única, que se almacena en el servidor de caché hospedada, aun si los clientes se encuentran en subredes diferentes. Además, BranchCache en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 proporcionan la capacidad de implementar más de un servidor de caché hospedada por cada sucursal.

> [!CAUTION]
> Si usa BranchCache para realizar almacenamiento en caché SMB de archivos y carpetas, no deshabilite Archivos sin conexión. Si deshabilita Archivos sin conexión, el almacenamiento en caché SMB no funcionará correctamente.

## <a name="BKMK_3"></a>Servidores de contenido habilitados para BranchCache

Cuando se implementa BranchCache, el contenido de origen se almacena en servidores de contenido habilitados para BranchCache en la oficina central o en un centro de datos en la nube. BranchCache admite los siguientes tipos de servidores de contenido:

> [!NOTE]
> Solo el contenido de origen, es decir, el contenido que los equipos cliente obtienen inicialmente del servidor de contenido habilitado para BranchCache, se acelera con BranchCache. Los equipos cliente no almacenan en la memoria caché el contenido que los equipos cliente obtienen directamente de otros orígenes, como servidores web de Internet o de Windows Update ni en servidores de caché hospedada y después lo comparten con otros equipos de la sucursal. Sin embargo, si desea acelerar el contenido de Windows Update, puede instalar un servidor de aplicaciones Windows Server Update Services (WSUS) en el centro de datos de la oficina central o en la nube y configurarlo como servidor de contenido de BranchCache.

### <a name="web-servers"></a>Servidores web

Los servidores web admitidos son equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 y que tienen instalado el rol de servidor servidor Web (IIS) y que usan el protocolo de transferencia de hipertexto (HTTP) o HTTP seguro ( HTTPS).

Además, el servidor web debe tener la característica BranchCache instalada.

### <a name="file-servers"></a>Servidores de archivos

Los servidores de archivos admitidos son equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que tienen instalado el rol de servidor servicios de archivo y el servicio de rol BranchCache para archivos de red. 

Estos servidores de archivos usan el Bloque de mensajes del servidor (SMB) para intercambiar información entre los equipos. Después de completar la instalación del servidor de archivos, también debe compartir carpetas y habilitar la generación de hash para carpetas compartidas usando la directiva de grupo o la directiva de equipo local para habilitar BranchCache.

### <a name="application-servers"></a>Servidores de aplicaciones

Los servidores de aplicaciones admitidos son equipos que ejecutan Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 con Servicio de transferencia inteligente en segundo plano (BITS) instalados y habilitados. 

Además, el servidor web debe tener la característica BranchCache instalada. Como ejemplos de servidores de aplicaciones, puede implementar servidores de Microsoft Windows Server Update Services (WSUS) y de punto de distribución de sucursal de Microsoft System Center Configuration Manager como servidores de contenido de BranchCache.

## <a name="BKMK_3a"></a>BranchCache y la nube

La nube ofrece grandes posibilidades de reducir los gastos operativos y alcanzar nuevos niveles de escala, pero quitarle las cargas de trabajo a las personas que dependen de ellas puede aumentar los costos de las redes y afectar a la productividad. Los usuarios esperan un alto rendimiento y no les importa dónde se hospedan sus aplicaciones y datos. 

BranchCache puede mejorar el rendimiento de aplicaciones en red y reducir el consumo de ancho de banda con una memoria caché de datos compartida.  Mejora la productividad en las sucursales y en las oficinas principales donde los trabajadores usan servidores que están implementados en la nube.

Dado que BranchCache no requiere hardware nuevo ni cambios en la topología de red, es una excelente solución para mejorar la comunicación entre las ubicaciones de la oficina y las nubes pública y privada.

> [!NOTE]
> Dado que algunos proxies web no pueden procesar encabezados de codificación de contenido no estándar, se recomienda usar BranchCache con el protocolo de transferencia de hipertexto seguro (HTTPS) y no HTTP.
  
= = = = = = = Para obtener más información sobre las tecnologías en la nube en Windows Server 2016, consulte [software Defined Networking &#40;&#41;Sdn](../sdn/Software-Defined-Networking--SDN-.md).
  
## <a name="bkmk_version"></a>Versiones de información de contenido

Existen dos versiones de la información de contenido:

- La información de contenido compatible con los equipos que ejecutan Windows Server 2008 R2 y Windows 7 se denomina versión 1 o v1. Con la segmentación de archivos de BranchCache V1, los segmentos de archivo son más grandes que los de V2 y tienen un tamaño fijo. Debido a estos tamaños fijos de segmento tan voluminosos, cuando un usuario realiza un cambio que altera la longitud del archivo, se invalida no solo el segmento con el cambio, sino todos los segmentos del archivo por completo. En consecuencia, la siguiente llamada que efectúe otro usuario de la sucursal en relación con el archivo modificado supondrá un escaso ahorro de ancho de banda de WAN, dado que tanto el contenido modificado como todo el contenido posterior al cambio se envía a través del vínculo WAN.

- La información de contenido compatible con los equipos que ejecutan Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012 y Windows 8 se denomina versión 2 o V2. La información de contenido V2 emplea segmentos más pequeños y de tamaño variable que son más tolerantes a los cambios en un archivo. Esto aumenta la posibilidad de poder reutilizar los segmentos procedentes de una versión más antigua del archivo cuando los usuarios acceden a una versión actualizada, lo que hace que obtengan del servidor de contenido únicamente la parte modificada del archivo y, por lo tanto, se use menos ancho de banda de WAN.

En la siguiente tabla encontrará información sobre la versión de información de contenido que se usa en función de los sistemas operativos de cliente, de servidor de contenido y de servidor de caché hospedada que use en su implementación de BranchCache.

> [!NOTE]
> En la tabla siguiente, el acrónimo "So" significa sistema operativo.

|SO de cliente|SO de servidor de contenido|SO de servidor de caché hospedada|Versión de información de contenido|
|-------------|---------------------|--------------------------|-------------------------------|
|Windows Server 2008 R2 y Windows 7 |Windows Server 2012 o posterior|Windows Server 2012 o posterior; ninguno para el modo caché distribuida|V1|
|Windows Server 2012 o posterior; Windows 8 o posterior|Windows Server 2008 R2 |Windows Server 2012 o posterior; ninguno para el modo caché distribuida|V1|
|Windows Server 2012 o posterior; Windows 8 o posterior| Windows Server 2012 o posterior| Windows Server 2008 R2 |V1|
|Windows Server 2012 o posterior; Windows 8 o posterior|Windows Server 2012 o posterior|Windows Server 2012 o posterior; ninguno para el modo caché distribuida|V2|

Cuando se tienen servidores de contenido y servidores de caché hospedada que ejecutan Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, usan la versión de información de contenido adecuada según el sistema operativo del cliente de BranchCache que solicita información. 

Cuando los equipos que ejecutan Windows Server 2012 y Windows 8 o sistemas operativos posteriores solicitan contenido, el contenido y los servidores de caché hospedada usan la información de contenido de V2. Cuando los equipos que ejecutan Windows Server 2008 R2 y el contenido de la solicitud de Windows 7, el contenido y los servidores de caché hospedada usan la información de contenido v1.

>[!IMPORTANT]
>Cuando BranchCache se implementa en modo de caché distribuida, los clientes que usen otras versiones de información de contenido no compartirán contenido entre ellos. Por ejemplo, un equipo cliente que ejecuta Windows 7 y un equipo cliente que ejecuta Windows 10 y que están instalados en la misma sucursal no comparten contenido entre sí.
  
## <a name="bkmk_handles"></a>Cómo administra BranchCache las actualizaciones de contenido en archivos

Cuando los usuarios de la sucursal modifican o actualizan el contenido de los documentos, los cambios se escriben directamente en el servidor de contenido de la oficina central sin la participación de BranchCache. Esto es así tanto si un usuario descarga el documento del servidor de contenidos como si lo obtiene de una caché hospedada o distribuida en la sucursal.

Cuando un cliente distinto de una sucursal solicita el archivo modificado, los nuevos segmentos del archivo se descargan desde el servidor de la oficina central y se agregan a la caché distribuida u hospedada de dicha sucursal. Debido a esto, los usuarios de las sucursales siempre disponen de las versiones más recientes del contenido almacenado en caché.

## <a name="BKMK_4"></a>Guía de instalación de BranchCache

Puede usar Administrador del servidor en Windows Server 2016 para instalar la característica BranchCache o el servicio de rol BranchCache para archivos de red del rol de servidor servicios de archivo. Puede usar la siguiente tabla para determinar si debe instalar el servicio de rol o la característica.

|Funcionalidad|Ubicación del equipo|Instalar este elemento de BranchCache|
|-----------------|---------------------|------------------------------------|
|Servidor de contenido \(servidor de aplicaciones basado en BITS\)|Centro de datos en la nube o en oficina central|Característica BranchCache|
|Servidor Web \(servidor de contenido\)|Centro de datos en la nube o en oficina central|Característica BranchCache|
|Servidor de contenido \(servidor de archivos mediante el protocolo SMB\)|Centro de datos en la nube o en oficina central|Servicio de rol BranchCache para archivos de red del rol de servidor Servicios de archivo|
|Servidor de caché hospedada|Sucursal|Característica de BranchCache con modo de servidor de caché hospedada habilitado|
|Equipo cliente habilitado para BranchCache|Sucursal|No se necesita ninguna instalación; simplemente habilite BranchCache y un modo de BranchCache \(\) distribuido o hospedado en el cliente.|

Para instalar el servicio de rol o la característica, abra el Administrador del servidor y seleccione los equipos en los que desea habilitar la funcionalidad de BranchCache. En el Administrador del servidor, haga clic en **Administrar** y en **Agregar roles y características**. Se abre el asistente para **Agregar roles y características** . Cuando ejecute el asistente, realice las siguientes selecciones:

- En la página del asistente **Seleccionar tipo de instalación**, seleccione **Instalación basada en características o en roles**.

- En la página del asistente **Seleccionar roles de servidor**, si va a instalar un servidor de archivos habilitado para BranchCache, expanda **servicios de archivos y almacenamiento** y servicios de **archivos e iSCSI**y, a continuación, seleccione **BranchCache para archivos de red**.  Para ahorrar espacio en disco, también puede seleccionar el servicio de rol **desduplicación de datos** y, a continuación, continuar con el Asistente para la instalación y la finalización. Si no desea instalar un servidor de archivos habilitado para BranchCache, no instale el rol servicios de archivos y almacenamiento con el servicio de rol BranchCache para archivos de red.

- En la página del asistente **seleccionar características**, si va a instalar un servidor de contenido que no es un servidor de archivos o va a instalar un servidor de caché hospedada, seleccione **BranchCache**y, a continuación, continúe con el Asistente para la instalación y finalización. Si no desea instalar un servidor de contenido que no sea un servidor de archivos o un servidor de caché hospedada, no instale la característica BranchCache.
  
## <a name="bkmk_os"></a>Versiones de sistema operativo para BranchCache

La siguiente es una lista de los sistemas operativos compatibles con diferentes tipos de funcionalidad de BranchCache.

### <a name="operating-systems-for-branchcache-client-computer-functionality"></a>Sistemas operativos para la funcionalidad de equipo cliente de BranchCache

Los sistemas operativos siguientes proporcionan a BranchCache compatibilidad con Servicio de transferencia inteligente en segundo plano (BITS), el protocolo de transferencia de hipertexto (HTTP) y el bloque de mensajes del servidor (SMB).

- Windows 10 Enterprise

- Windows 10 Education

- Windows 8.1 Enterprise

- Windows 8 Enterprise

- Windows 7 Enterprise

- Windows 7 Ultimate

En los siguientes sistemas operativos, BranchCache no admite la funcionalidad de HTTP y SMB, pero admite la funcionalidad de BITS de BranchCache.

-   Windows 10 Pro, solo compatible con BITS

-   Windows 8.1 Pro, solo compatible con BITS

-   Windows 8 Pro, solo compatible con BITS

-   Windows 7 Pro, solo compatible con BITS

> [!NOTE]
> BranchCache no está disponible de forma predeterminada en los sistemas operativos Windows Server 2008 o Windows Vista. Sin embargo, en estos sistemas operativos, si descarga e instala la actualización de Windows Management Framework, la funcionalidad de BranchCache solo está disponible para el protocolo Servicio de transferencia inteligente en segundo plano (BITS). Para obtener más información y para descargar Windows Management Framework, vea [Windows Management Framework (Windows PowerShell 2,0, WinRM 2,0 y BITS 4,0)](https://go.microsoft.com/fwlink/?LinkId=188677) en https://go.microsoft.com/fwlink/?LinkId=188677.
  
### <a name="operating-systems-for-branchcache-content-server-functionality"></a>Sistemas operativos para la funcionalidad de servidor de contenido de BranchCache

Puede usar las familias de sistemas operativos Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como servidores de contenido de BranchCache.

Además, la familia de sistemas operativos Windows Server 2008 R2 se puede usar como servidores de contenido de BranchCache, con las siguientes excepciones:

- BranchCache no se admite en las instalaciones Server Core de Windows Server 2008 R2 Enterprise con Hyper-V.

- BranchCache no se admite en las instalaciones Server Core de Windows Server 2008 R2 Datacenter con Hyper-V.

### <a name="operating-systems-for-branchcache-hosted-cache-server-functionality"></a>Sistemas operativos para la funcionalidad de servidor de caché hospedada de BranchCache

Puede usar las familias de sistemas operativos Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 como servidores de caché hospedada de BranchCache.

Además, los siguientes sistemas operativos Windows Server 2008 R2 se pueden usar como servidores de caché hospedada de BranchCache:

- Windows Server 2008 R2 Enterprise

- Windows Server 2008 R2 Enterprise con Hyper-V

- Instalación de Windows Server 2008 R2 Enterprise Server Core

- Instalación de Windows Server 2008 R2 Enterprise Server Core con Hyper-V

- Windows Server 2008 R2 for Itanium-Based Systems

- Windows Server 2008 R2 Datacenter

- Windows Server 2008 R2 Datacenter con Hyper-V

- Instalación de Windows Server 2008 R2 Datacenter Server Core con Hyper-V

## <a name="bkmk_security"></a>Seguridad de BranchCache

BranchCache implementa un enfoque de diseño seguro que funciona a la perfección junto con las arquitecturas de seguridad existentes, sin necesidad de contar con otros equipos o configuraciones de seguridad complejas.
  
BranchCache no es invasivo y no modifica ningún proceso de autenticación o autorización de Windows. Después de implementar BranchCache, la autenticación se lleva a cabo de todas formas con las credenciales de dominio sin modificaciones en la forma en que funciona la autorización con las listas de control de acceso (ACL). Además, el resto de las configuraciones continúa funcionando como lo venía haciendo antes de la implementación de BranchCache.

El modelo de seguridad de BranchCache se basa en la creación de metadatos que adoptan la forma de una serie de hashes. Estos hashes también se denominan información de contenido.

Una vez creada la información de contenido, se usa en intercambios de mensajes de BranchCache en lugar de los datos reales, y se intercambia mediante los protocolos admitidos (HTTP, HTTPS y SMB).

Los datos almacenados en la memoria caché se mantienen cifrados y los clientes que no cuentan con permiso para acceder al contenido de la fuente original no pueden acceder a ellos.  Los clientes deben estar autenticados y autorizados por el origen de contenido original antes de poder recuperar metadatos de contenido y deben poseer metadatos de contenido para poder acceder a la memoria caché de la oficina local.

### <a name="how-branchcache-generates-content-information"></a>Cómo BranchCache genera información de contenido

Dado que la información de contenido se crea a partir de varios elementos, el valor de la información del contenido es siempre única. Estos elementos son:

- El contenido real (como páginas web o archivos compartidos) de los cuales derivan los hashes.  

- Parámetros de configuración como el algoritmo hash y el tamaño de bloque. Para generar información de contenido, el servidor de contenido divide el contenido en segmentos y después subdivide esos segmentos en bloques. BranchCache usa hashes criptográficos seguros para identificar y comprobar cada bloque y cada segmento, al admitir el algoritmo hash SHA256.

- Un secreto de servidor Todos los servidores de contenido deben estar configurados con un secreto de servidor, que es un valor binario de longitud arbitraria.

> [!NOTE]
> El uso de un secreto de servidor garantiza que los equipos cliente no puedan generar la información de contenido por sí mismos. Esto impide que usuarios malintencionados usen ataques de fuerza bruta con equipos cliente habilitados para BranchCache para adivinar cambios menores en el contenido entre las versiones en situaciones en las que el cliente tuvo acceso a una versión anterior pero no a la versión actual.

### <a name="content-information-details"></a>Detalles de la información de contenido

BranchCache usa el secreto de servidor como clave para derivar un hash de contenido específico que se envía a los clientes autorizados. Al aplicar un algoritmo hash al secreto de servidor combinado y al hash de datos se genera este hash.

Este hash se denomina secreto de segmento. BranchCache usa secretos de segmento para proteger las comunicaciones. Además, BranchCache crea una lista de hashes de bloque, que es una lista de los bloques de datos con hash y el hash de datos, que se genera al aplicar el hash a la lista de hashes de bloque.

La información de contenido incluye lo siguiente:

- La lista de hashes de bloque:

    `BlockHashi = Hash(dataBlocki)   1<=i<=n`

- El hash de datos (HoD):

    `HoD = Hash(BlockHashList)`

- Secreto de segmento (Kp):

    `Kp = HMAC(Ks, HoD)`

BranchCache usa el protocolo de almacenamiento en caché de contenido y el protocolo de marco de recuperación para implementar los procesos necesarios para garantizar el almacenamiento en caché y la recuperación de datos seguros entre cachés de contenido.

Por otra parte, BranchCache maneja información de contenido con el mismo grado de seguridad que emplea al manejar y transmitir el contenido real.

## <a name="bkmk_flow"></a>Flujo de contenido y procesos

El flujo de información de contenido y el contenido real se divide en cuatro fases:

1.  [Procesos de BranchCache: contenido de la solicitud](#BKMK_8)

2.  [Procesos de BranchCache: buscar contenido](#BKMK_9)

3.  [Procesos de BranchCache: recuperación de contenido](#BKMK_10)

4.  [Procesos de BranchCache: contenido de la caché](#BKMK_11)

Estas fases se describen en las siguientes secciones.

## <a name="BKMK_8"></a>Procesos de BranchCache: contenido de la solicitud

En la primera fase, el equipo cliente de la sucursal solicita contenido, por ejemplo, un archivo o una página web, de un servidor de contenido en una ubicación remota, como una oficina central. El servidor de contenido comprueba que el equipo cliente esté autorizado a recuperar el contenido solicitado. Si el equipo cliente está autorizado y el servidor de contenido y el cliente están\-habilitados para BranchCache, el servidor de contenido genera información de contenido.

Después, el servidor de contenido envía información de contenido al equipo cliente utilizando el mismo protocolo que hubiese usado para el contenido real. 

Por ejemplo, si el equipo cliente solicitó una página web a través de HTTP, el servidor de contenido envía la información de contenido con HTTP. Debido a ello, las garantías de seguridad a nivel conexión del contenido y la información de contenido son idénticas.

Después de que se recibe la porción inicial de información de contenido (hash de datos + secreto de segmento), el equipo cliente realiza las siguientes acciones:

- Usa el secreto de segmento (Kp) como clave de cifrado (Ke).

- Genera el identificador de segmento (HoHoDk) a partir de HoD y Kp:

    `HoHoDk = HMAC(Kp, HoD + C), where C is the ASCII string "MS_P2P_CACHING" with NUL terminator.`

La principal amenaza en este nivel es el riesgo del secreto de segmento; sin embargo, BranchCache cifra los bloques de datos de contenido para proteger el secreto de segmento. BranchCache lo hace al utilizar la clave de cifrado que deriva del secreto de segmento del segmento de contenido dentro del cual se encuentran los bloques de contenido.

Este enfoque garantiza que una entidad que posee el secreto de servidor no pueda descubrir el contenido real en el bloque de datos. El secreto de segmento se trata con el mismo grado de seguridad que el segmento de texto sin formato mismo, ya que conocer el secreto de segmento de un segmento determinado permite a una entidad obtener el segmento de sus pares y después, descifrarlo. Conocer el secreto de servidor no produce de inmediato ningún texto sin formato en particular, pero se puede usar para derivar ciertos tipos de datos del texto cifrado y después, posiblemente exponer algunos datos parcialmente conocidos a un ataque por adivinar mediante la fuerza bruta. Por lo tanto, el secreto de servidor debe mantenerse en absoluta confidencialidad.
  
## <a name="BKMK_9"></a>Procesos de BranchCache: buscar contenido

Una vez que el equipo cliente recibe información de contenido, el cliente usa el identificador de segmento para ubicar el contenido solicitado en la memoria caché de la sucursal, ya sea que esa memoria caché esté distribuida entre los equipos cliente o se encuentre en un servidor de caché hospedada.

Si el equipo cliente está configurado para el modo Caché hospedada, se configura con el nombre de equipo del servidor de caché hospedada y se pone en contacto con ese servidor para recuperar el contenido.

Sin embargo, si el equipo cliente está configurado para el modo Caché distribuida, el contenido podría almacenarse entre varias cachés en varios equipos de la sucursal. El equipo cliente debe descubrir la ubicación del contenido antes de que se recupere el contenido.

Cuando están configurados para el modo Caché distribuida, los equipos cliente ubican el contenido mediante un protocolo de descubrimiento que se basa en el protocolo de detección dinámica de servicios web (WS-Discovery). Los clientes envían mensajes de sondeo de multidifusión de WS-Discovery para descubrir contenido almacenado en caché a través de la red. Los mensajes de sondeo incluyen el identificador de segmento, que permite a los clientes comprobar si el contenido solicitado coincide con el contenido almacenado en la memoria caché. Los clientes que reciben el mensaje de sondeo inicial responden ala cliente que realiza la consulta con mensajes que coincidan con el sondeo de unidifusión si el identificador de segmento coincide con el contenido almacenado localmente en la memoria caché.  

El éxito del proceso de WS-Discovery depende de si el cliente que está realizando el descubrimiento cuenta con la información de contenido correcta, que le proporcionó el servidor de contenido, para el contenido que está solicitando.

La principal amenaza a los datos durante la fase de solicitud de contenido es la divulgación de información, ya que el acceso a la información de contenido implica acceso no autorizado al contenido. Para mitigar este riesgo, el proceso de descubrimiento no revela la información de contenido a excepción del identificador de segmento, que no revela nada sobre el segmento de texto sin formato que contiene el contenido.

Además, otro equipo cliente en manos de un usuario malintencionado en la misma subred de red puede ver el tráfico de descubrimiento de BranchCache hacia el origen de contenido original que pasa por el enrutador.

Si el contenido solicitado no se encuentra en la sucursal, el cliente solicita el contenido directamente del servidor de contenido a través del vínculo WAN:

Después de que se recibe el contenido, se agrega a la caché local ya sea en el equipo cliente o en el servidor de caché hospedada. En este caso, la información de contenido impide que un cliente o servidor de caché hospedada agregue a la caché local ningún contenido que no coincida con los hashes. El proceso de comprobación de contenido mediante hashes coincidentes garantiza que solo se agregue a la caché contenido válido y se protege la integridad de la caché local.

## <a name="BKMK_10"></a>Procesos de BranchCache: recuperación de contenido

Una vez que un equipo cliente ubica el contenido deseado en el host de contenido, que es un servidor de caché hospedada o un equipo cliente en modo Caché distribuida, el equipo cliente inicia el proceso de recuperación de contenido.

En primer lugar, el equipo cliente envía una solicitud al host de contenido para el primer bloque que requiere. La solicitud contiene el identificador de segmento y el rango de bloques que identifican el contenido deseado. Ya que solo se devuelve un bloque, el rango de bloques contiene un solo bloque. (Actualmente no se admiten solicitudes de varios bloques). El cliente también almacena la solicitud en su lista de solicitudes pendientes local.  

Al recibir un mensaje de solicitud válido de un cliente, el host de contenido comprueba si el bloque especificado en la solicitud existe en la caché de contenido del host de contenido.

Si el host de contenido está en posesión del bloque de contenido, entonces el host de contenido envía una respuesta que contiene el identificador de segmento, el identificador de bloque, el bloque de datos cifrados y el vector de inicialización que se usa para cifrar el bloque.

Si el host de contenido no se encuentra en posesión del bloque de contenido, el host de contenido envía un mensaje de respuesta vacío. Esto informa al equipo cliente de que el host de contenido no cuenta con el bloque solicitado. Un mensaje de respuesta vacío contiene el identificador de segmento y el identificador de bloque del bloque solicitado, junto con un bloque de datos de tamaño cero.

Cuando el equipo cliente recibe la respuesta del host de contenido, el cliente comprueba que el mensaje corresponda a un mensaje de solicitud en su lista de solicitudes pendientes. (El identificador de segmento y el índice de bloque deben coincidir con los de una solicitud pendiente).

Si el proceso de comprobación es incorrecto y el equipo cliente no cuenta con un mensaje de solicitud correspondiente en su lista de solicitudes pendientes, el equipo cliente descarta el mensaje.

Si este proceso de comprobación es correcto y el equipo cliente cuenta con un mensaje de solicitud correspondiente en su lista de solicitudes pendientes, el equipo cliente descifra el bloque. A continuación, el cliente valida el bloque descifrado con el hash de bloque adecuado de la información de contenido que el cliente obtuvo inicialmente del servidor de contenido original.

Si la validación de bloque es correcta, el bloque descifrado se almacena en la memoria caché.

Este proceso se repite hasta que el cliente cuente con todos los bloques requeridos.

> [!NOTE]
> Si los segmentos de contenido completos no existen en un equipo, el protocolo de recuperación recupera y ensambla el contenido de una combinación de orígenes: un conjunto de equipos cliente en modo Caché distribuida, un servidor de caché hospedada y, si las memorias caché de la sucursal no tienen el contenido completo, el servidor de contenido original en la oficina central.

Antes de que BranchCache envíe información de contenido o el contenido, se cifran los datos. BranchCache cifra el bloque en el mensaje de respuesta. En Windows 7, el algoritmo de cifrado predeterminado que BranchCache usa es AES-128, la clave de cifrado es ke y el tamaño de la clave es de 128 bits, como indica el algoritmo de cifrado. 

BranchCache genera un vector de inicialización adecuado para el algoritmo de cifrado y usa la clave de cifrado para cifrar el bloque. Después, BranchCache registra el algoritmo de cifrado y el vector de inicialización en el mensaje. 

Los servidores y los clientes nunca intercambian, comparten, ni se envían entre sí la clave de cifrado. El cliente recibe la clave de cifrado del servidor de contenido que alberga el contenido de origen. A continuación, descifra el bloque con el algoritmo de cifrado y el vector de inicialización que recibió del servidor. En el protocolo de descarga no hay ninguna otra autenticación o autorización explicita.

### <a name="security-threats"></a>Amenazas para la seguridad

Las principales amenazas para la seguridad en este nivel incluyen:

- Alteración de los datos:

  *Un cliente que sirve datos a un solicitante altera los datos*. El modelo de seguridad de BranchCache usa hashes para confirmar que ni el cliente ni el servidor alteraron los datos.  

- Divulgación de información:  

    *BranchCache envía contenido cifrado a cualquier cliente que especifique un identificador de segmento apropiado*. Los identificadores de segmento son públicos, por lo tanto cualquier cliente puede recibir contenido cifrado. Sin embargo, si un usuario malintencionado obtiene contenido cifrado, debe conocer la clave de cifrado para descifrar el contenido. El protocolo de nivel superior realiza la autenticación y, a continuación, proporciona la información de contenido al cliente autenticado y autorizado. La seguridad de la información de contenido equivale a la seguridad que se proporciona al contenido mismo, y BranchCache nunca expone la información de contenido.  

    *Un atacante examina los datos de la conexión para obtener el contenido*.  BranchCache cifra todas las transferencias entre los clientes mediante AES128, con la clave secreta Ke, lo que impide que se examinen los datos de la conexión.  La información de contenido que se descarga del servidor de contenido se protege exactamente de la misma manera que los datos en sí; por lo tanto, no está ni más ni menos protegida de la divulgación de información que si no se hubiese utilizado BranchCache en absoluto.

-   Denegación de servicio:

    *Un cliente se desborda con solicitudes de datos*. Los protocolos de BranchCache incorporan contadores y temporizadores de administración de colas para impedir que los clientes se sobrecarguen.

## <a name="BKMK_11"></a>Procesos de BranchCache: contenido de la caché

En los equipos cliente en modo Caché distribuida y los servidores de caché hospedada que se encuentran en las sucursales, las caché de contenido se acumulan con el paso del tiempo a medida que se recupera el contenido a través de los vínculos WAN.

Cuando los equipos cliente están configurados con modo Caché hospedada, agregan contenido a su propia caché local y también ofrecen datos al servidor de caché hospedada. El protocolo de caché hospedada ofrece un mecanismo a los clientes que informan al servidor de caché hospedada acerca de la disponibilidad del contenido y los segmentos.

Para cargar contenido al servidor de caché hospedada, el cliente informa al servidor de que tiene un segmento que está disponible. A continuación, el servidor de caché hospedada recupera toda la información de contenido que está relacionada con el segmento ofrecido y descarga los bloques dentro del segmento que realmente se necesita. Este proceso se repite hasta que el cliente ya no tenga más segmentos que ofrecer al servidor de caché hospedada.

Para actualizar el servidor de caché hospedada utilizando el protocolo de caché hospedada, se deben cumplir los siguientes requisitos:

- El equipo cliente debe contar con un conjunto de bloques dentro de un segmento que pueda ofrecer al servidor de caché hospedada. El cliente debe suministrar información de contenido para el segmento ofrecido; esta se compone del identificador de segmento, el hash de datos del segmento, el secreto del segmento y una lista de todos los hashes de bloque que contiene el segmento.

- En el caso de los servidores de caché hospedada que ejecutan Windows Server 2008 R2, se requiere un certificado de servidor de caché hospedada y una clave privada asociada, y la entidad de certificación (CA) que emitió el certificado debe ser de confianza para los equipos cliente de la sucursal. Esto permite que el cliente y el servidor participen correctamente en la autenticación del servidor HTTPS.

    > [!IMPORTANT]
    > Los servidores de caché hospedada que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 no necesitan un certificado de servidor de caché hospedada ni una clave privada asociada.  

- El equipo cliente está configurado con el nombre de equipo del servidor de caché hospedada y el número de puerto del Protocolo de control de transmisión (TCP) en el cual el servidor de caché hospedada está escuchando el tráfico de BranchCache. El certificado del servidor de caché hospedada está enlazado a este puerto. El nombre de equipo del servidor de caché hospedada puede ser un nombre de dominio completo (FQDN), si el servidor de caché hospedada es un equipo miembro del dominio, o puede ser el nombre NetBIOS del equipo si el servidor de caché hospedada no es un miembro del dominio.

- El equipo cliente escucha activamente las solicitudes de bloque entrantes. El puerto en el que se escucha se pasa como parte de los mensajes de oferta desde el cliente al servidor de caché hospedada. Esto permite que el servidor de caché hospedada use protocolos de BranchCache para conectarse al equipo cliente para recuperar bloques de datos en el segmento.

- Al iniciarse, el servidor de caché hospedada comienza a escuchar las solicitudes de HTTP entrantes.

- Si el servidor de caché hospedada está configurado para exigir la autenticación del equipo cliente, se requiere que el cliente y el servidor de caché hospedada admitan autenticación HTTPS.

### <a name="hosted-cache-mode-cache-population"></a>Relleno de la memoria caché en modo de caché hospedada

El proceso de agregar contenido a la caché del servidor de caché hospedada en una sucursal comienza cuando el cliente envía una INITIAL_OFFER_MESSAGE, que incluye el identificador de segmento. El identificador de segmento en la solicitud de INITIAL_OFFER_MESSAGE se usa para recuperar el hash de segmento correspondiente de los datos, la lista de hashes de bloque y el secreto de segmento de la memoria caché de bloque del servidor de caché hospedada. Si el servidor de caché hospedada ya cuenta con toda la información de contenido de un segmento en particular, la respuesta a INITIAL_OFFER_MESSAGE será OK y no se produce la solicitud de descarga de bloques.

Si el servidor de caché hospedada no cuenta con todos los bloques de datos ofrecidos asociados con los hashes de bloque en el segmento, la respuesta a INITIAL_OFFER_MESSAGE es INTERESTED (interesado). Después, el cliente envía un SEGMENT_INFO_MESSAGE (mensaje de información del segmento) que describe el único segmento que se está ofreciendo. El servidor de caché hospedada responde con un mensaje OK e inicia la descarga de los bloques faltantes del equipo cliente que los ofrece.

El hash de datos del segmento, la lista de hashes de bloque y el secreto del segmento se usan para garantizar que el contenido que se descarga no esté modificado ni alterado de otra manera. Los bloques descargados se agregan después a la memoria caché del bloque del servidor de caché hospedada.

## <a name="bkmk_cache"></a>Seguridad de caché  
En esta sección se proporciona información sobre cómo BranchCache protege los datos almacenados en caché en equipos cliente y en servidores de caché hospedada.

### <a name="client-computer-cache-security"></a>Seguridad de la memoria caché en el equipo cliente
La mayor amenaza a los datos almacenados en BranchCache es la alteración. Si un atacante puede alterar el contenido y la información de contenido que se almacena en caché, se la podría usar para intentar y iniciar un ataque contra los equipos que están usando BranchCache. Los atacantes pueden iniciar un ataque al insertar software malintencionado en lugar de otros datos. BranchCache mitiga esta amenaza al validar todo el contenido que usa hashes de bloque encontrados en la información de contenido. Si un atacante intenta alterar estos datos, se descartan y se reemplazan con datos válidos de la fuente original.

Una segunda amenaza a los datos almacenados en BranchCache es la divulgación de información. En modo Caché distribuida, el cliente solo almacena en memoria caché el contenido que solicitó; sin embargo, esos datos se almacenan en texto no cifrado y podrían estar en riesgo. Para ayudar a restringir el acceso de la memoria caché al Servicio BranchCache solamente, la memoria caché local está protegida por permisos del sistema de archivos que se especifican en una ACL. 

Aunque la ACL es eficaz a la hora de impedir que usuarios no autorizados accedan a la memoria caché, es posible que un usuario con privilegios administrativos obtenga acceso a la memoria caché al modificar manualmente los permisos que se especifican en la ACL. BranchCache no ofrece protección contra el uso malintencionado de una cuenta administrativa.

Los datos que se almacenan en la caché de contenido no están cifrados, por lo tanto si la fuga de datos representa una inquietud, puede usar tecnologías de cifrado como BitLocker o el Sistema de cifrado de archivos (EFS) La memoria caché local que usa BranchCache no aumenta la amenaza de la divulgación de información que soporta el equipo de una sucursal; la memoria caché solo contiene copias de archivos que residen sin cifrar en otra parte del disco. 

El cifrado de todo el disco es particularmente importante en entornos en los que la seguridad física del cliente es difícil de garantizar. Por ejemplo, cifrar todo el disco ayuda a proteger los datos confidenciales de equipos móviles que podrían eliminarse del entorno de la sucursal.

### <a name="hosted-cache-server-cache-security"></a>Seguridad de la memoria caché en el servidor de caché hospedada

En modo Caché hospedada, la amenaza más importante a la seguridad del servidor de caché hospedada es la divulgación de información. BranchCache en un entorno de caché hospedada se comporta de una manera similar al modo Caché distribuida, con un permiso de sistema de archivos que protege los datos almacenados en caché. La diferencia es que el servidor de caché hospedada almacena todo el contenido que solicita cualquier equipo habilitado para BranchCache de una sucursal, y no simplemente los datos que solicita un solo cliente. Las consecuencias de la intrusión no autorizada a esta memoria caché podría ser mucho más grave ya que hay muchos más datos en riesgo.  
  
En un entorno de caché hospedada en el que el servidor de caché hospedada ejecuta Windows Server 2008 R2, se recomienda el uso de tecnologías de cifrado como BitLocker o EFS si alguno de los clientes de la sucursal puede acceder a datos confidenciales a través del vínculo WAN. También es necesario impedir el acceso físico a la memoria caché hospedada, ya que el cifrado del disco solo funciona cuando el equipo está apagado cuando el atacante obtiene acceso físico.  Si el equipo está activado o en modo de suspensión, el cifrado del disco ofrece poca protección.

> [!NOTE]
> Los servidores de caché hospedada que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 cifran todos los datos de la caché de forma predeterminada, por lo que no es necesario el uso de tecnologías de cifrado adicionales.

Aun si el cliente está configurado en modo Caché hospedada, de todas maneras guardará los datos localmente en la memoria caché y es posible que desee realizar los pasos necesarios para proteger la memoria caché local además de la memoria caché del servidor de caché hospedada.
