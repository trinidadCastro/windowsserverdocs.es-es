---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicaciones de servicios
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 91ef716f518676fe9cf42b2136afcbcd42c50946
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858528"
---
# <a name="service-communications-certificates"></a>Certificados de comunicaciones de servicios

Un servidor de Federación requiere el uso de certificados de comunicación de servicio para los escenarios en los que se usa la seguridad de mensajes de WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificados de comunicación de servicios  
Los certificados de comunicación de servicio deben cumplir los requisitos siguientes para trabajar con AD FS:  
  
-   El certificado de comunicación de servicio debe incluir la extensión de uso mejorado de clave \(EKU de autenticación de servidor\).  
  
-   Las listas de revocación de certificados \(CRL\) deben ser accesibles para todos los certificados de la cadena desde el certificado de comunicación de servicio al certificado de CA raíz. La CA raíz también debe ser de confianza para los servidores proxy de Federación y los servidores web que confían en este servidor de Federación.  
  
-   El nombre de sujeto que se usa en el certificado de comunicación de servicio debe coincidir con el nombre de Servicio de federación en las propiedades de la Servicio de federación.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Consideraciones de implementación para los certificados de comunicación de servicio  
Configure los certificados de comunicación de servicio para que todos los servidores de Federación usen el mismo certificado. Si va a implementar el inicio de sesión único de\-Web federado\-en \(diseño de\) SSO, se recomienda que el certificado de comunicación de servicio sea emitido por una CA pública. Puede solicitar e instalar estos certificados a través del complemento Administrador de IIS\-en.  
  
Puede usar los certificados de comunicación de servicio\-firmados correctamente en los servidores de Federación en un entorno de laboratorio de pruebas. Sin embargo, para un entorno de producción, se recomienda obtener certificados de comunicación de servicio de una CA pública. A continuación se indican los motivos por los que no se deben usar certificados de comunicación de servicio\-firmados para una implementación dinámica:  
  
-   Un certificado SSL\-firmado debe agregarse al almacén raíz de confianza de cada uno de los servidores de Federación de la organización del asociado de recurso. Aunque esto por sí solo no permite a un atacante poner en peligro un servidor de Federación de recursos, confiar en\-certificados firmados aumenta la superficie de ataque de un equipo y puede conducir a vulnerabilidades de seguridad si el firmante del certificado no es de confianza.  
  
-   Crea una experiencia de usuario incorrecta. Los clientes recibirán mensajes de alerta de seguridad cuando intenten obtener acceso a recursos federados que muestran el siguiente mensaje: "el certificado de seguridad fue emitido por una compañía en la que no ha elegido confiar". Este es el comportamiento esperado, ya que el certificado\-firmado no es de confianza.  
  
    > [!NOTE]  
    > Si es necesario, puede evitar esta condición mediante el uso de directiva de grupo para enviar manualmente el certificado auto\-firmado al almacén raíz de confianza de cada equipo cliente que intentará obtener acceso a un sitio AD FS.  
  
-   Las CA proporcionan características adicionales basadas en\-de certificados, como el archivo de claves privadas, la renovación y la revocación, que no proporcionan los certificados Auto\-firmados.  
  

