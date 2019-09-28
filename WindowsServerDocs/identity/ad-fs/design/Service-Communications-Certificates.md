---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicaciones de servicios
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 624d2e26bc0277129e44eee3fdce7c7396b735a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407900"
---
# <a name="service-communications-certificates"></a>Certificados de comunicaciones de servicios

Un servidor de Federación requiere el uso de certificados de comunicación de servicio para los escenarios en los que se usa la seguridad de mensajes de WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificados de comunicación de servicios  
Los certificados de comunicación de servicio deben cumplir los requisitos siguientes para trabajar con AD FS:  
  
-   El certificado de comunicación de servicio debe incluir la extensión uso mejorado de clave de autenticación de servidor \(EKU @ no__t-1.  
  
-   Las listas de revocación de certificados \(CRLs @ no__t-1 deben ser accesibles para todos los certificados de la cadena desde el certificado de comunicación de servicio al certificado de CA raíz. La CA raíz también debe ser de confianza para los servidores proxy de Federación y los servidores web que confían en este servidor de Federación.  
  
-   El nombre de sujeto que se usa en el certificado de comunicación de servicio debe coincidir con el nombre de Servicio de federación en las propiedades de la Servicio de federación.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Consideraciones de implementación para los certificados de comunicación de servicio  
Configure los certificados de comunicación de servicio para que todos los servidores de Federación usen el mismo certificado. Si va a implementar el diseño único Web federado @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3, se recomienda que el certificado de comunicación de servicio sea emitido por una CA pública. Puede solicitar e instalar estos certificados a través del administrador de IIS Snap @ no__t-0in.  
  
Puede usar los certificados de comunicación de servicio de no__t-0signed correctamente en los servidores de Federación en un entorno de laboratorio de pruebas. Sin embargo, para un entorno de producción, se recomienda obtener certificados de comunicación de servicio de una CA pública. A continuación se indican los motivos por los que no se deben usar los certificados de comunicación de servicio de @ no__t-0signed para una implementación dinámica:  
  
-   Se debe agregar un certificado SSL Self @ no__t-0signed al almacén raíz de confianza en cada uno de los servidores de Federación de la organización del asociado de recurso. Aunque esto por sí solo no permite a un atacante poner en peligro un servidor de Federación de recursos, confiar en los certificados Self no__t-0signed aumenta la superficie de ataque de un equipo y puede conducir a vulnerabilidades de seguridad si el firmante del certificado no es digna.  
  
-   Crea una experiencia de usuario incorrecta. Los clientes recibirán mensajes de alerta de seguridad cuando intenten obtener acceso a recursos federados que muestran el siguiente mensaje: "El certificado de seguridad fue emitido por una compañía en la que no ha elegido confiar." Este es el comportamiento esperado, ya que el certificado Self @ no__t-0signed no es de confianza.  
  
    > [!NOTE]  
    > Si es necesario, puede solucionar esta situación mediante el uso de directiva de grupo para enviar manualmente el certificado Self @ no__t-0signed al almacén raíz de confianza de cada equipo cliente que intentará obtener acceso a un sitio AD FS.  
  
-   Las CA proporcionan características adicionales de certificado @ no__t-0based, como el archivo de clave privada, la renovación y la revocación, que no se proporcionan mediante certificados de autono__t-1signed.  
  

