---
title: Guía de implementación de BranchCache
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e7aa35260213a8a236b7c27cf74e36692438cd2a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="branchcache-deployment-guide"></a>Guía de implementación de BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar a esta guía para aprender a implementar BranchCache en Windows Server 2016.  
  
Además de este tema, esta guía contiene las siguientes secciones.  
  
-   [Elegir un diseño BranchCache](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [Implementar BranchCache](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache de implementación

BranchCache es una tecnología de optimización de ancho de banda de red (WAN) de área extensa que se incluye en algunas ediciones de Windows Server 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012, Windows Server&reg; 2008 R2 y relacionados con los sistemas operativos Windows.  
  
Para optimizar el ancho de banda WAN, BranchCache copia el contenido de los servidores de contenido principal de office y se almacena en caché el contenido en sucursales, permitir que los equipos en sucursales para acceder al contenido localmente en lugar de en la red WAN de cliente.  
  
En las sucursales, contenido se almacena en caché en servidores que ejecutan la característica BranchCache de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 - o bien, si no hay ningún servidor disponibles en las sucursales, se almacena en caché contenido en los equipos cliente que ejecutan Windows 10&reg;, Windows&reg; 8.1, Windows 8 o Windows 7&reg; .  
  
Después de un equipo cliente solicita y recibe contenido desde el centro de datos principal de office o en la nube y se almacena en caché el contenido en la sucursal, otros equipos en la misma sucursal pueden obtener el contenido de forma local en lugar de ponerte en contacto con el servidor de contenido a través del vínculo WAN.  
  
**Ventajas de implementar BranchCache**  
  
BranchCache cachés de archivo, web y contenido de la aplicación en sucursales, lo que permite a cliente equipos tener acceso a datos con la red de área local (LAN) en lugar de acceso al contenido en conexiones WAN lentas.  
  
BranchCache reduce el tráfico WAN y el tiempo que se necesita para los usuarios de sucursales abrir archivos en la red.  BranchCache siempre proporciona a los usuarios con los datos más recientes y protege la seguridad del contenido mediante el cifrado de las memorias caché en el servidor de la memoria caché hospedada y en los equipos cliente.  
  
### <a name="what-this-guide-provides"></a>Esta guía proporciona  
Esta guía de implementación le permite implementar BranchCache en los siguientes modos:  
  
-   Modo de caché distribuida. En este modo, los equipos de cliente de office rama descargar contenido de los servidores de contenido en la oficina principal o la nube y, a continuación, almacena en caché el contenido de otros equipos de la misma sucursal. Modo de caché distribuida no requiere un equipo de servidor en las sucursales.  
  
-   Modo de caché hospedada. En este modo, el contenido de los servidores de contenido en la oficina principal o la nube y un servidor de la memoria caché hospedada de descarga de equipos del cliente de office rama recupera el contenido de los clientes. El servidor de la memoria caché hospedada, a continuación, almacena en caché el contenido de otros equipos cliente.  
  
Esta guía también proporciona instrucciones sobre cómo implementar los tres tipos de servidores de contenido. Servidores de contenido contienen el contenido de origen que se descarga los equipos de cliente de office rama y servidor de contenido de uno o más es necesaria para implementar BranchCache en cualquiera de los modos. Los tipos de servidor de contenido son:  
  
-   **Servidores de contenido basada en el servidor Web**. Estos servidores contenidos envían contenido a los equipos de cliente BranchCache mediante los protocolos HTTP y HTTPS. Estos servidores de contenido deben estar ejecutando versiones de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que admiten BranchCache y al que se instala la característica BranchCache.  
  
-   **Servidores de aplicaciones basados en BITS**. Estos servidores contenidos envían contenido a los equipos de cliente de BranchCache mediante el servicio de transferencia inteligente en segundo plano (BITS). Estos servidores de contenido deben estar ejecutando versiones de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que admiten BranchCache y al que se instala la característica BranchCache.  
  
-   **Servidores de contenido basada en el servidor de archivos**. Estos servidores de contenido deben estar ejecutando versiones de Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 que admiten BranchCache y en qué servicios el archivo está instalado el rol de servidor. Además, la **BranchCache para archivos de red** servicio de rol del rol de servidor de servicios de archivo debe estar instalada y configurada. Estos servidores contenidos envían contenido a los equipos de cliente BranchCache mediante el protocolo bloque de mensajes de servidor (SMB).  
  
Para obtener más información, consulta [versiones del sistema operativo para BranchCache](https://technet.microsoft.com/en-us/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache).  
  
### <a name="branchcache-deployment-requirements"></a>Requisitos de implementación de BranchCache

Siguiente es los requisitos para implementar BranchCache mediante el uso de esta guía.  
  
-   **Los servidores de contenido Web y el archivo** debe ejecutarse en uno de los siguientes sistemas operativos para proporcionar la funcionalidad de BranchCache: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2. Windows 8 y versiones posteriores clientes siguen viendo los beneficios de BranchCache al acceder a contenido servidores que ejecutan Windows Server 2008 R2, pero no pueden hacer uso de las nuevas fragmentación y operaciones hash tecnologías en Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016.  
  
-   **Los equipos cliente** debe ejecutar Windows 10, Windows 8.1 o Windows 8 para hacer uso del modelo de implementación más reciente y la fragmentación y hash mejoras que se introdujeron con Windows Server 2012.  
  
-   **Hospedado servidores de caché** debe ejecutar Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 para hacer uso de las mejoras de implementación y escalar características descritas en este documento.  Un equipo que ejecuta uno de estos sistemas operativos que está configurado como un servidor de la memoria caché hospedada puede seguir para atender a los equipos cliente que ejecutan Windows 7, pero para ello, se debe estar equipado con un certificado que es apropiado para seguridad de capa de transporte (TLS), como se describe en el Windows Server 2008 R2 y Windows 7 [Guía de implementación de BranchCache](https://technet.microsoft.com/en-us/library/ee649232.aspx).  
  
-   **Un dominio de Active Directory** es necesaria para sacar provecho de la directiva de grupo y la detección automática de la memoria caché hospedada, pero no es necesario usar BranchCache un dominio.  Puedes configurar los equipos individuales mediante Windows PowerShell. Además, no es necesario que los controladores de dominio ejecutan Windows Server 2012 o posterior para usar la nueva configuración de directiva de grupo BranchCache; puede importar las plantillas administrativas BranchCache en controladores de dominio que ejecutan sistemas operativos anteriores, o puedes crear los objetos de directiva de grupo remotamente en otros equipos que ejecutan Windows 10, Windows Server 2016, Windows 8.1, Windows Server 2012 R2, Windows 8 o Windows Server 2012.

-   **Sitios de Active Directory** se usan para limitar el ámbito de los servidores de la memoria caché hospedada que se detecta automáticamente.  Para detectar automáticamente un servidor de la memoria caché hospedada, los equipos cliente y servidor deben pertenecer al mismo sitio. BranchCache está diseñado para tener un impacto mínimo en los clientes y servidores e imponen requisitos de hardware adicionales más allá de los necesarios para ejecutar sus respectivos sistemas operativos.  

**Documentación y el historial de BranchCache**

BranchCache se presentó por primera vez en Windows 7&reg; y Windows Server&reg; 2008 R2 y se ha mejorado en Windows Server 2012, Windows 8 y sistemas operativos posteriores.

> [!NOTE]
> Si vas a implementar BranchCache en sistemas operativos que no sean de Windows Server 2016, están disponibles los siguientes recursos de documentación.
> 
> - Para obtener información acerca de BranchCache en Windows 8, Windows 8.1, Windows Server 2012 y Windows Server 2012 R2, consulta [BranchCache de](https://technet.microsoft.com/en-us/library/hh831696.aspx).  
> - Para obtener información acerca de BranchCache en Windows 7 y Windows Server 2008 R2, consulta [BranchCache para Windows Server 2008 R2](https://technet.microsoft.com/en-us/library/dd996634.aspx).  
  


