---
title: Guía de implementación de BranchCache
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 14eb9e5b4d5a28a64d3cfa0d27b5294ba7168da9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356732"
---
# <a name="branchcache-deployment-guide"></a>Guía de implementación de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar esta guía para obtener información sobre cómo implementar BranchCache en Windows Server 2016.  
  
Además de este tema, esta guía contiene las siguientes secciones.  
  
-   [Elegir un diseño de BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Implementar BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>Información general sobre la implementación de BranchCache

BranchCache es una tecnología de optimización del ancho de banda de la red de área extensa (WAN) que se incluye en algunas ediciones de Windows Server 2016, Windows Server @ no__t-0 2012 R2, Windows Server @ no__t-1 2012, Windows Server @ no__t-2 2008 R2 y el cliente de Windows relacionado sistemas operativos.  
  
Para mejorar el ancho de banda de la WAN, BranchCache copia el contenido de los servidores de contenido de la oficina central y lo almacena en la memoria caché de las sucursales, lo que permite que los equipos cliente de dichas sucursales tengan acceso al contenido de forma local en lugar de hacerlo a través de la WAN.  
  
En las sucursales, el contenido se almacena en caché en los servidores que ejecutan la característica BranchCache de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2-o, si no hay servidores disponibles en la sucursal, el contenido es CAC Hed en los equipos cliente que ejecutan Windows 10 @ no__t-0, Windows @ no__t-1 8,1, Windows 8 o Windows 7 @ no__t-2.  
  
Una vez que un equipo cliente solicita y recibe contenido del centro de recursos de la oficina central o de la nube y el contenido se almacena en caché en la sucursal, otros equipos de la misma sucursal pueden obtener el contenido localmente en lugar de ponerse en contacto con el servidor de contenido a través de la Vínculo WAN.  
  
**Ventajas de la implementación de BranchCache**  
  
BranchCache almacena en caché el contenido del archivo, Web y de la aplicación en las sucursales, lo que permite que los equipos cliente tengan acceso a los datos mediante la red de área local (LAN) en lugar de tener acceso al contenido a través de conexiones WAN lentas.  
  
BranchCache reduce tanto el tráfico WAN como el tiempo necesario para que los usuarios de la sucursal abran archivos en la red.  BranchCache siempre proporciona a los usuarios los datos más recientes y protege la seguridad del contenido mediante el cifrado de las memorias caché en el servidor de caché hospedada y en los equipos cliente.  
  
### <a name="what-this-guide-provides"></a>Qué incluye esta guía  
Esta guía de implementación permite implementar BranchCache en los siguientes modos:  
  
-   Modo Caché distribuida. En este modo, los equipos cliente de la sucursal descargan contenido de los servidores de contenido de la oficina central o la nube y, a continuación, almacenan en caché el contenido de otros equipos de la misma sucursal. El modo Caché distribuida no requiere la existencia de un equipo servidor en la sucursal.  
  
-   Modo Caché hospedada. En este modo, los equipos cliente de la sucursal descargan contenido de los servidores de contenido de la oficina central o la nube, y un servidor de caché hospedada recupera el contenido de los clientes. A continuación, el servidor de caché hospedada almacena en memoria caché el contenido de otros equipos cliente.  
  
Esta guía también proporciona instrucciones sobre cómo implementar tres tipos de servidores de contenido. Los servidores de contenido poseen el contenido de origen que los equipos cliente de la sucursal descargan y se necesitan uno o varios servidores de contenido para implementar BranchCache en cualquier modo. Los tipos de servidor de contenido son:  
  
-   **Servidores de contenido basados en servidor Web**. Estos servidores de contenido envían contenido a los equipos cliente de BranchCache utilizando los protocolos HTTP y HTTPS. Estos servidores de contenido deben ejecutar Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o las versiones de Windows Server 2008 R2 que admiten BranchCache y en los que está instalada la característica BranchCache.  
  
-   **Servidores de aplicaciones basados en bits**. Estos servidores de contenido envían contenido a los equipos cliente de BranchCache utilizando el Servicio de transferencia inteligente en segundo plano (BITS). Estos servidores de contenido deben ejecutar Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o las versiones de Windows Server 2008 R2 que admiten BranchCache y en los que está instalada la característica BranchCache.  
  
-   **Servidores de contenido basados en servidor de archivos**. Estos servidores de contenido deben ejecutar Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o las versiones de Windows Server 2008 R2 que admiten BranchCache y en los que está instalado el rol de servidor servicios de archivo. Además, debe estar instalado y configurado el servicio de rol **BranchCache para archivos de red** del rol de servidor Servicios de archivo. Estos servidores de contenido envían contenido a los equipos cliente de BranchCache utilizando el protocolo SMB (Bloque de mensajes del servidor).  
  
Para obtener más información, consulte [versiones de sistema operativo para BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Requisitos de implementación de BranchCache

A continuación se indican los requisitos para implementar BranchCache mediante esta guía.  
  
-   **Los servidores de contenido web y de archivos** deben ejecutar uno de los siguientes sistemas operativos para proporcionar la funcionalidad de BranchCache: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2. Los clientes de Windows 8 y versiones posteriores continúan viendo las ventajas de BranchCache al acceder a los servidores de contenido que ejecutan Windows Server 2008 R2; sin embargo, no pueden usar las nuevas tecnologías de fragmentación y hash en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012.  
  
-   **Los equipos cliente** deben ejecutar Windows 10, Windows 8.1 o Windows 8 para usar el modelo de implementación más reciente y las mejoras de fragmentación y hash que se introdujeron en Windows Server 2012.  
  
-   **Los servidores de caché hospedada** deben ejecutar windows Server 2016, windows Server 2012 R2 o windows Server 2012 para usar las mejoras de implementación y las características de escalado que se describen en este documento.  Un equipo que ejecuta uno de estos sistemas operativos que está configurado como un servidor de caché hospedada puede seguir atendiendo a equipos cliente que ejecutan Windows 7, pero para ello debe estar equipado con un certificado que sea adecuado para la seguridad de la capa de transporte (TLS). ), como se describe en la [Guía de implementación](https://technet.microsoft.com/library/ee649232.aspx)de windows Server 2008 R2 y Windows 7 BranchCache.  
  
-   Se requiere **un dominio de Active Directory** para aprovechar las ventajas de la detección automática de directiva de grupo y de caché hospedada, pero no es necesario que un dominio use BranchCache.  Puede configurar equipos individuales mediante Windows PowerShell. Además, no es necesario que los controladores de dominio ejecuten Windows Server 2012 o posterior para utilizar la nueva configuración de directiva de grupo de BranchCache. puede importar las plantillas administrativas de BranchCache en los controladores de dominio que ejecutan sistemas operativos anteriores, o puede crear los objetos de directiva de grupo de forma remota en otros equipos que ejecutan Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 o Windows Server 2012.

-   Los **sitios de Active Directory** se usan para limitar el ámbito de los servidores de caché hospedada que se detectan automáticamente.  Para detectar automáticamente un servidor de caché hospedada, los equipos cliente y servidor deben pertenecer al mismo sitio. BranchCache está diseñado para tener un impacto mínimo en los clientes y servidores, y no impone requisitos de hardware adicionales más allá de los necesarios para ejecutar sus respectivos sistemas operativos.  

**Documentación y historial de BranchCache**

BranchCache se presentó por primera vez en Windows 7 @ no__t-0 y Windows Server @ no__t-1 2008 R2, y se mejoró en Windows Server 2012, Windows 8 y sistemas operativos posteriores.

> [!NOTE]
> Si está implementando BranchCache en sistemas operativos distintos de Windows Server 2016, están disponibles los siguientes recursos de la documentación.
> 
> - Para obtener información sobre BranchCache en Windows 8, Windows 8.1, Windows Server 2012 y Windows Server 2012 R2, consulte [información general sobre BranchCache](https://technet.microsoft.com/library/hh831696.aspx).  
> - Para obtener información sobre BranchCache en Windows 7 y Windows Server 2008 R2, vea [BranchCache para Windows server 2008 R2](https://technet.microsoft.com/library/dd996634.aspx).  
  


