---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisitos de certificado para servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca0b25480eedfc6471837ab8ae83b0d1d522e61e
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191658"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de certificado para servidores proxy de federación

Los servidores que se ejecutan en el rol de proxy del servidor de federación de Active Directory Federation Services \(AD FS\) son necesarios para usar capa de Sockets seguros \(SSL\) certificados de autenticación de servidor. Los servidores proxy de federación usan certificados de autenticación de servidor SSL para proteger la comunicación del tráfico de servidor web con los clientes web.  
  
Servidores proxy de federación suelen estar expuestos a equipos en Internet que no están incluidos en la infraestructura de clave pública de su empresa \(PKI\). Por lo tanto, use un certificado de autenticación de servidor emitido por una pública \(tercer\-entidad\) entidad de certificación \(CA\), por ejemplo, VeriSign.  
  
Cuando haya una granja de proxy de servidor de federación, todos los equipos de servidores proxy de federación deben usar el mismo certificado de autenticación de servidor. Para obtener más información, consulte [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Es importante comprobar que el nombre del sujeto en las coincidencias de certificado de autenticación de servidor de valor que es el nombre del servicio de federación especificado en el complemento Administración de AD FS\-en. Para ubicar este valor, abra el complemento\-, derecha\-haga clic en **servicio**, haga clic en **editar propiedades del servicio de federación**y, a continuación, busque el valor en **federación Nombre del servicio** cuadro de texto.  
  
¿Para obtener información general sobre el uso de certificados SSL, vea Configurar la capa de Sockets seguros en IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108544](https://go.microsoft.com/fwlink/?LinkID=108544) \) y configurar certificados de servidor en IIS 7.0 \( [http:\/\/go.microsoft.com\/fwlink\/? LinkID\=108545](https://go.microsoft.com/fwlink/?LinkID=108545)\).  
  
> [!NOTE]  
> Certificados de autenticación de cliente no son necesarios para los servidores proxy de federación de AD FS.  
  
Si cualquier certificado de uso tiene listas de revocación de certificados \(CRL\), el servidor con el certificado configurado debe poder ponerse en contacto con el servidor que distribuye las CRL. El tipo de CRL determina los puertos que se utilizan.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
