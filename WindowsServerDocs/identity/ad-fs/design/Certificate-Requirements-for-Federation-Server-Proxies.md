---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisitos de certificado para servidores proxy de federación
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: dab77c3e3226e89eb3ac9b74e7db9b6df8f181bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408145"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de certificado para servidores proxy de federación

Los servidores que se ejecutan en el rol de servidor proxy de Federación en Servicios de federación de Active Directory (AD FS) \(AD FS @ no__t-1 son necesarios para usar Capa de sockets seguros \(SSL @ no__t-3 certificados de autenticación de servidor. Los servidores proxy de federación usan certificados de autenticación de servidor SSL para proteger la comunicación del tráfico de servidor web con los clientes web.  
  
Los servidores proxy de Federación suelen estar expuestos a equipos de Internet que no están incluidos en la infraestructura de clave pública de la empresa @no__t 0PKI @ no__t-1. Por lo tanto, use un certificado de autenticación de servidor emitido por una entidad de certificación pública \(third @ no__t-1party @ no__t-2 \(CA @ no__t-4, por ejemplo, VeriSign.  
  
Si tiene una granja de servidores proxy de Federación, todos los equipos de servidor proxy de Federación deben usar el mismo certificado de autenticación de servidor. Para obtener más información, consulte [Cuándo se debe crear una granja de servidores proxy de federación](When-to-Create-a-Federation-Server-Proxy-Farm.md).  
  
Es importante comprobar que el nombre de sujeto del certificado de autenticación del servidor coincide con el valor de nombre de Servicio de federación que se especifica en el AD FS de administración del complemento @ no__t-0in. Para buscar este valor, abra el **servicio**ajustar @ no__t-0in, derecho @ no__t-1Click, haga clic en **Editar servicio de Federación propiedades**y, a continuación, busque el valor en servicio de Federación cuadro de texto **nombre** .  
  
Para obtener información general sobre el uso de certificados SSL, vea Configurar Capa de sockets seguros en IIS 7,0 \([http: @no__t -2\/go.microsoft.com @ no__t-4fwlink @ no__t-5? LinkId @ no__t-6108544](https://go.microsoft.com/fwlink/?LinkID=108544)\) y configurar certificados de servidor en IIS 7,0 \([http: @no__t -101go.microsoft.com @ no__t-12fwlink @ no__t-13? LinkId @ no__t-14108545](https://go.microsoft.com/fwlink/?LinkID=108545)5.  
  
> [!NOTE]  
> Los certificados de autenticación de cliente no son necesarios para AD FS servidores proxy de Federación.  
  
Si un certificado que utiliza tiene listas de revocación de certificados \(CRLs @ no__t-1, el servidor con el certificado configurado debe poder ponerse en contacto con el servidor que distribuye las CRL. El tipo de CRL determina los puertos que se utilizan.  
  
## <a name="see-also"></a>Vea también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
