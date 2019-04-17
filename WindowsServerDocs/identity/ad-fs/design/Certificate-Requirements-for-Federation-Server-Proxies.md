---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: "Requisitos de certificado para Proxies de servidor de federación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7fb8e71afed1c0eb6b55857835d95f2dd0ec9d5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de certificado para Proxies de servidor de federación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Servidores que se ejecutan en el rol de proxy del servidor de federación de los servicios de federación de Active Directory \(AD FS\) son necesarias para usar certificados de autenticación de servidor \(SSL\) capa de Sockets seguros. Proxies de servidor de federación usan certificados de autenticación de servidor SSL para proteger la comunicación con los clientes Web tráfico del servidor Web.  
  
Proxies de servidor de federación normalmente se exponen a los equipos en Internet que no están incluidos en la infraestructura de clave pública de la empresa \(PKI\). Por lo tanto, usar un certificado de autenticación de servidor emitido por una entidad de certificación públicas \(third\-party\) \(CA\), por ejemplo, VeriSign.  
  
Cuando tengas un conjunto de federación servidores proxy, todos los equipos de proxy de servidor de federación deben usar el mismo certificado de autenticación de servidor. Para obtener más información, consulta [cuándo se debe crear una granja de Proxy del servidor de federación](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Es importante comprobar que el nombre del sujeto del certificado de servidor autenticación coincide con el valor de nombre de servicios de federación que se especifica en la administración de AD FS en snap\. Para buscar este valor, abra el snap\ en right\ clic **servicio**, haz clic en **editar propiedades del servicio de federación**y, a continuación, busca el valor en **nombre de servicios de federación** cuadro de texto.  
  
¿Para obtener información general sobre el uso de certificados SSL, consulta la configuración de capa de Sockets seguros en IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? ¿LinkID\ = 108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) y la configuración de certificados de servidor en IIS 7.0 \ ([http:///\/go.microsoft.com\/fwlink\/? LinkID\ = 108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificados de autenticación de cliente no son obligatorios para proxies de servidor de federación de AD FS.  
  
Si ningún certificado que usas con \(CRLs\) listas de revocación de certificados, el servidor con el certificado configurado debe poder ponerte en contacto con el servidor que distribuye las CRL. El tipo de CRL determina qué puertos se usan.  
  
## <a name="see-also"></a>Consulta también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
