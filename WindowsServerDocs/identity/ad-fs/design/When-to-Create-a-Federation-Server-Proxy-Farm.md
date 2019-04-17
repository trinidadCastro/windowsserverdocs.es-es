---
ms.assetid: ad0bf21d-2ace-4565-b1f5-ce57c8eb2689
title: "Cuándo se debe crear una granja de Proxy del servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8935760cad272d5b82edb675cda85caf0456565f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-proxy-farm"></a>Cuándo se debe crear una granja de Proxy del servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considera la posibilidad de instalar proxies de servidor de federación adicional cuando tienes una implementación de \(AD FS\) grande de los servicios de federación de Active Directory y desea proporcionar tolerancia a errores, equilibrio load\ y escalabilidad para la implementación de proxy. El act de crear proxies de servidor de federación de dos o más en la misma red perímetro y configurar cada uno de ellos para proteger el servicio de federación de AD FS mismo crea un conjunto de federación servidores proxy.  
  
Puedes crear una granja de proxy del servidor de federación o instalar a proxies de servidor de federación adicional a un conjunto existente mediante el Asistente para configuración de Proxy de federación de servidor de AD FS. Para obtener más información, consulta [cuándo se debe crear un Proxy de servidor de federación](When-to-Create-a-Federation-Server-Proxy.md).  
  
Antes de que todos los proxy de servidor de federación pueden funcionar juntos como una granja de servidores, primero debe clúster ellos en una dirección IP y dominio completo de un sistema de nombres de dominio \(DNS\) nombre \(FQDN\). Puede agregar al clúster los servidores mediante la implementación de equilibrio de carga de red de Microsoft \(NLB\) dentro de la red de perímetro. Las tareas en la siguiente tabla requieren NLB para configurarse correctamente para los proxy de servidor de federación de la batería del clúster.  
  
Para obtener más información sobre cómo configurar un FQDN para un clúster mediante la tecnología Microsoft NLB, consulta [especificar los parámetros de clúster](https://go.microsoft.com/fwlink/?linkid=74651).  
  
## <a name="configuring-federation-server-proxies-for-a-farm"></a>Configuración de proxy de servidor de federación para una granja de servidores  
La siguiente tabla describe las tareas que deben realizarse para que cada proxy del servidor de federación pueda participar en una granja de servidores.  
  
|Tarea|Descripción|  
|--------|---------------|  
|Seleccione el mismo nombre de servicio de federación de AD FS todos los servidores proxy de la batería|Cuando crees la federación proxies de servidor, debe escribir el mismo nombre de servicio de federación en el Asistente para configuración de Proxy de federación de servidor de AD FS para todos los proxy de servidor de federación que participarán en el conjunto. El proxy de servidor de federación usa la dirección URL que facilita este nombre de host DNS para determinar qué instancia de servicios de federación de AD FS contactos.<br /><br />Para obtener más información, consulta [configurar un equipo para el rol de Proxy del servidor de federación](../../ad-fs/deployment/Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md).|  
|Obtener y compartir certificados|Puedes obtener un servidor de certificados de autenticación de una entidad de certificación públicas \ (CA\), por ejemplo, VeriSign y, a continuación, configura el certificado para que todos los servidores proxy de servidor de federación compartan el mismo parte de clave privada el mismo certificado en el sitio Web predeterminado para cada proxy de servidor de federación. Para compartir el certificado, debes instalar el mismo certificado de autenticación de servidor en el sitio Web predeterminado para cada proxy de servidor de federación. Para obtener más información, consulta [importar un certificado de autenticación de servidor en el sitio Web predeterminado](../../ad-fs/deployment/Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md).<br /><br />Para obtener más información, consulta [requisitos de certificado para Proxies de servidor de federación](Certificate-Requirements-for-Federation-Server-Proxies.md).|  
  
Para obtener más información sobre cómo agregar nuevos proxies de servidor de federación para crear un conjunto de federación servidores proxy, consulta [lista de comprobación: configuración de un Proxy de servidor de federación](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
