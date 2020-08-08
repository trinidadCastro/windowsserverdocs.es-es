---
ms.assetid: dc24adb7-385d-4a92-ab81-78ba73df0118
title: Requisitos de certificado para servidores proxy de federación
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 51eb0e72f52fafdb2f1b8bbb3f9a2850602c3f75
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954341"
---
# <a name="certificate-requirements-for-federation-server-proxies"></a>Requisitos de certificado para servidores proxy de federación

Los servidores que se ejecutan en el rol de servidor proxy de Federación en Servicios de federación de Active Directory (AD FS) \( AD FS \) son necesarios para usar capa de sockets seguros \( certificados de autenticación de \) servidor SSL. Los servidores proxy de federación usan certificados de autenticación de servidor SSL para proteger la comunicación del tráfico de servidor web con los clientes web.

Los servidores proxy de Federación suelen estar expuestos a equipos de Internet que no están incluidos en la PKI de la infraestructura de clave pública de la empresa \( \) . Por lo tanto, use un certificado de autenticación de servidor emitido por \( una \- \) CA pública \( de entidad \) de certificación de terceros, por ejemplo, VeriSign.

Si tiene una granja de servidores proxy de Federación, todos los equipos de servidor proxy de Federación deben usar el mismo certificado de autenticación de servidor. Para obtener más información, consulte [When to Create a Federation Server Proxy Farm](When-to-Create-a-Federation-Server-Proxy-Farm.md).

Es importante comprobar que el nombre de sujeto del certificado de autenticación de servidor coincide con el valor de nombre de Servicio de federación especificado en el complemento de administración \- de AD FS. Para buscar este valor, abra el complemento \- , haga clic con el botón secundario \- en **servicio**, haga clic en **Editar servicio de Federación propiedades**y, a continuación, busque el valor en servicio de Federación cuadro de texto **nombre** .

Para obtener información general sobre el uso de certificados SSL, consulte Configuración de Capa de sockets seguros en IIS 7,0 \( [http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkID \= 108544](https://go.microsoft.com/fwlink/?LinkID=108544) \) y configuración de certificados de servidor en IIS 7,0 \( [http: \/ \/ Go.Microsoft.com \/ fwlink \/ ? LinkID \= 108545](https://go.microsoft.com/fwlink/?LinkID=108545) \) .

> [!NOTE]
> Los certificados de autenticación de cliente no son necesarios para AD FS servidores proxy de Federación.

Si alguno de los certificados que utiliza tiene listas de revocación de certificados \( \) , el servidor con el certificado configurado debe poder ponerse en contacto con el servidor que distribuye las CRL. El tipo de CRL determina los puertos que se utilizan.

## <a name="see-also"></a>Consulte también
[Guía de diseño de AD FS en Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
