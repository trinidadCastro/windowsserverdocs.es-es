---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicaciones de servicios
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b781d6fe99864b13d6e7f8ab65f3a14d205c2aa6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190798"
---
# <a name="service-communications-certificates"></a>Certificados de comunicaciones de servicios

Un servidor de federación, requiere el uso de certificados de comunicación de servicio para escenarios en los que se utiliza la seguridad de mensaje WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificado de comunicación de servicio  
Certificados de comunicación de servicio deben cumplir los siguientes requisitos para que funcione con AD FS:  
  
-   El certificado de comunicación de servicio debe incluir el uso de claves de autenticación mejorado servidor \(EKU\) extensión.  
  
-   Las listas de revocación de certificados \(CRL\) debe ser accesible para todos los certificados de la cadena desde el certificado de comunicación de servicio para el certificado de CA raíz. La entidad emisora raíz también debe ser de confianza por los servidores proxy de federación y los servidores Web que confiar en este servidor de federación.  
  
-   El nombre de sujeto que se usa en el certificado de comunicación de servicio debe coincidir con el nombre del servicio de federación en las propiedades del servicio de federación.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Consideraciones de implementación de certificados de comunicación de servicio  
Configurar certificados de comunicación de servicio para que todos los servidores de federación usen el mismo certificado. Si está implementando Federated Web único\-sesión\-en \(SSO\) diseño, se recomienda que el certificado de comunicación de servicio emitirlo una CA pública. Puede solicitar e instalar estos certificados mediante el complemento Administrador de IIS\-en.  
  
Puede usar self\-firmado, certificados de comunicación correctamente en los servidores de federación en un entorno de laboratorio de pruebas de servicio. Sin embargo, para un entorno de producción, recomendamos que obtenga certificados de comunicación de servicio de una entidad de certificación pública. Los siguientes son los motivos de por qué no debe usar self\-firmado, certificados de comunicación para una implementación en vivo de servicio:  
  
-   Un autoservicio\-firmado, certificado SSL debe agregarse al almacén raíz de confianza en cada uno de los servidores de federación en la organización del asociado de recurso. Si bien esto por sí solo no permite que un atacante poner en peligro un servidor de federación de recursos, confiar en self\-certificados autofirmados aumenta la superficie de ataque de un equipo y que puede dar lugar a vulnerabilidades de seguridad si el firmante del certificado no es Trustworthy.  
  
-   Crea una mala experiencia del usuario. Los clientes recibirán mensajes de alerta de seguridad cuando intenten obtener acceso a los recursos federados que aparezca el mensaje siguiente: "El certificado de seguridad fue emitido por una compañía que no ha depositado su confianza." Este es el comportamiento esperado, porque el valor self\-certificado firmado no es de confianza.  
  
    > [!NOTE]  
    > Si es necesario, puede solucionar esta condición mediante Directiva de grupo para insertar manualmente el autoservicio\-firmó el certificado al almacén raíz de confianza en cada equipo cliente que intente obtener acceso a un sitio de AD FS.  
  
-   Entidades emisoras de certificados proporcione certificados adicionales\-características que no se proporcionan por sí mismo, como archivo de claves privadas, renovación y revocación, basada en\-certificados autofirmados.  
  

