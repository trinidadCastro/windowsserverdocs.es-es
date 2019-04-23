---
title: Guía de implementación de BranchCache
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9bccf69f0a913159a395fabc670a63e2c159bd91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888186"
---
# <a name="branchcache-deployment-guide"></a>Guía de implementación de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar a esta guía para obtener información sobre cómo implementar BranchCache en Windows Server 2016.  
  
Además de este tema, esta guía contiene las siguientes secciones.  
  
-   [Elegir un diseño de BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Implementar BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Información general sobre la implementación de BranchCache

BranchCache es una tecnología de optimización de área extensa (WAN) de red ancho de banda que se incluye en algunas ediciones de Windows Server 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012, Windows Server&reg; 2008 R2 y relacionados Sistemas operativos de cliente de Windows.  
  
Para mejorar el ancho de banda de la WAN, BranchCache copia el contenido de los servidores de contenido de la oficina central y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales tengan acceso al contenido de forma local en lugar de hacerlo a través de la WAN.  
  
En las sucursales, se almacena en caché contenido en servidores que ejecutan la característica BranchCache de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 - o, si no hay ningún servidor disponible en la sucursal, content es cac hé en equipos cliente que ejecutan Windows 10&reg;, Windows&reg; 8.1, Windows 8 o Windows 7&reg; .  
  
Después de un equipo cliente solicita y recibe el contenido desde el centro de datos de office o en la nube principal y el contenido se almacena en caché en la sucursal, otros equipos en la misma sucursal pueden obtener el contenido localmente, en lugar de ponerse en contacto con el servidor de contenido a través de la Vínculo WAN.  
  
**Ventajas de la implementación de BranchCache**  
  
Archivo de las memorias caché de BranchCache, web y contenido de la aplicación en las ubicaciones de sucursales, lo que permite a cliente equipos tengan acceso a datos mediante la red de área local (LAN) en lugar de obtener acceso al contenido a través de conexiones WAN lentas.  
  
BranchCache reduce el tráfico WAN y el tiempo necesario para los usuarios de la sucursal abrir archivos en la red.  BranchCache siempre proporciona a los usuarios con los datos más recientes, y protege la seguridad de su contenido mediante el cifrado de las memorias caché en el servidor de caché hospedada y en los equipos cliente.  
  
### <a name="what-this-guide-provides"></a>Qué incluye esta guía  
Esta guía de implementación permite implementar BranchCache en los siguientes modos:  
  
-   Modo Caché distribuida. En este modo, los equipos cliente la sucursal descargar contenido desde los servidores de contenido en la oficina central o en la nube y, a continuación, almacenar en caché el contenido de otros equipos en la misma sucursal. El modo Caché distribuida no requiere la existencia de un equipo servidor en la sucursal.  
  
-   Modo Caché hospedada. En este modo, branch office cliente equipos descargan el contenido de los servidores de contenido en la oficina central o en la nube y servidor de caché hospedada recupera el contenido de los clientes. A continuación, el servidor de caché hospedada almacena en memoria caché el contenido de otros equipos cliente.  
  
Esta guía también proporciona instrucciones sobre cómo implementar tres tipos de servidores de contenido. Los servidores de contenido poseen el contenido de origen que los equipos cliente de la sucursal descargan y se necesitan uno o varios servidores de contenido para implementar BranchCache en cualquier modo. Los tipos de servidor de contenido son:  
  
-   **Servidores de contenido basado en servidor Web**. Estos servidores de contenido envían contenido a los equipos cliente de BranchCache utilizando los protocolos HTTP y HTTPS. Estos servidores de contenido deben ejecutar versiones de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que admiten BranchCache y una vez que se instala la característica BranchCache.  
  
-   **Los servidores de aplicaciones basados en BITS**. Estos servidores de contenido envían contenido a los equipos cliente de BranchCache utilizando el Servicio de transferencia inteligente en segundo plano (BITS). Estos servidores de contenido deben ejecutar versiones de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que admiten BranchCache y una vez que se instala la característica BranchCache.  
  
-   **Servidores de contenido basado en el servidor de archivos**. Estos servidores de contenido deben ejecutar las versiones de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que admiten BranchCache y en el que el archivo de servicios está instalado el rol de servidor. Además, debe estar instalado y configurado el servicio de rol **BranchCache para archivos de red** del rol de servidor Servicios de archivo. Estos servidores de contenido envían contenido a los equipos cliente de BranchCache utilizando el protocolo SMB (Bloque de mensajes del servidor).  
  
Para obtener más información, consulte [versiones del sistema operativo para BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Requisitos de implementación de BranchCache

Siguientes son los requisitos para la implementación de BranchCache mediante el uso de esta guía.  
  
-   **Servidores de contenido Web y el archivo** debe estar ejecutando uno de los siguientes sistemas operativos para proporcionar funcionalidad de BranchCache: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2. Windows 8 y versiones posteriores clientes siguen viendo las ventajas de BranchCache al tener acceso a servidores de contenido que se ejecutan Windows Server 2008 R2, pero no pueden hacer uso de la fragmentación de nuevo y el hash de las tecnologías en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012.  
  
-   **Los equipos cliente** deben ejecutar Windows 10, Windows 8.1 o Windows 8 para hacer uso del modelo de implementación más reciente y la fragmentación y hash de las mejoras que se introdujeron con Windows Server 2012.  
  
-   **Servidores de caché hospedada** debe ejecutar Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 para que el uso de las mejoras de implementación y escalado de las características descritas en este documento.  Un equipo que ejecute uno de estos sistemas operativos que se configura como un servidor de caché hospedada puede seguir para servir a equipos cliente que ejecutan Windows 7, pero para ello, se debe estar provisto de un certificado que es adecuado para la capa de transporte (TLS ), como se describe en el Windows Server 2008 R2 y Windows 7 [BranchCache Deployment Guide](https://technet.microsoft.com/library/ee649232.aspx).  
  
-   **Un dominio de Active Directory** es necesario para aprovechar las ventajas de la directiva de grupo y la detección automática de caché hospedada, pero no se requiere un dominio para que usen BranchCache.  Puede configurar equipos individuales mediante el uso de Windows PowerShell. Además, no es necesario que los controladores de dominio ejecutan Windows Server 2012 o posterior para usar la nueva configuración de directiva de grupo; puede importar las plantillas administrativas de BranchCache en los controladores de dominio que ejecutan sistemas operativos anteriores, o puede crear los objetos de directiva de grupo remota en otros equipos que ejecutan Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 o Windows Server 2012.

-   **Sitios de Active Directory** se usan para limitar el ámbito de los servidores de caché hospedada que se detectan automáticamente.  Para detectar automáticamente un servidor de caché hospedada, el cliente y el servidor de los equipos deben pertenecer al mismo sitio. BranchCache está diseñado para tener un impacto mínimo en los clientes y servidores y no impone requisitos de hardware adicionales más allá de los necesarios para ejecutar sus respectivos sistemas operativos.  

**Documentación y el historial de BranchCache**

BranchCache se presentó en Windows 7&reg; y Windows Server&reg; 2008 R2 y se ha mejorado en Windows Server 2012, Windows 8 y los sistemas operativos posteriores.

> [!NOTE]
> Si va a implementar BranchCache en los sistemas operativos distintos de Windows Server 2016, están disponibles los siguientes recursos de documentación.
> 
> - Para obtener información acerca de BranchCache en Windows 8, Windows 8.1, Windows Server 2012 y Windows Server 2012 R2, consulte [información general sobre BranchCache](https://technet.microsoft.com/library/hh831696.aspx).  
> - Para obtener información acerca de BranchCache en Windows 7 y Windows Server 2008 R2, consulte [BranchCache para Windows Server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx).  
  


