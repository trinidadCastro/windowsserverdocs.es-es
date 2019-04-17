---
ms.assetid: 95e82190-68c5-4e40-87b1-f1bd816ef4e9
title: Certificados de comunicaciones de servicio
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 32e60f2c2d9e4fced04061ace44882b792e1a3bc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="service-communications-certificates"></a>Certificados de comunicaciones de servicio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un servidor de federación requiere el uso de certificados de comunicación de servicio para escenarios en que se utiliza la seguridad de mensajes WCF.  
  
## <a name="service-communication-certificate-requirements"></a>Requisitos de certificado de comunicación de servicio  
Certificados de comunicación de servicio deben cumplir los siguientes requisitos para que funcione con AD FS:  
  
-   El certificado de comunicación de servicio debe incluir la extensión \(EKU\) de uso de la clave de autenticación mejorada de servidor.  
  
-   El certificado revocación listas \(CRLs\) debe ser accesible para todos los certificados de la cadena desde el certificado de comunicación de servicio para el certificado de CA de raíz. La CA de raíz también debe ser de confianza por cualquier proxies de servidor de federación y los servidores Web este servidor de federación de confianza.  
  
-   El nombre del sujeto que se usa en el certificado de comunicación de servicio debe coincidir con el nombre de servicios de federación de las propiedades de los servicios de federación.  
  
## <a name="deployment-considerations-for-service-communication-certificates"></a>Consideraciones de implementación de certificados del servicio de comunicación  
Configura los certificados de comunicación de servicio para que todos los servidores de federación usan el mismo certificado. Si vas a implementar el diseño federados Web Single\-Sign\-On \(SSO\), te recomendamos que el certificado de comunicación de servicio se emitido por una CA pública. Puedes solicitar e instalar estos certificados a través del Administrador de IIS snap\.  
  
Puedes usar la firma self\, comunicación del servicio de certificados correctamente en los servidores de federación en un entorno de laboratorio de prueba. Sin embargo, para un entorno de producción, te recomendamos que obtengas certificados del servicio de comunicación de una CA pública. Estos son los motivos por qué debería no usa self\, servicio comunicación certificados firmados para una implementación dinámico:  
  
-   Una firma self\, certificado SSL debe agregarse a la tienda de la raíz de confianza en cada uno de los servidores de federación de la organización de partner de recurso. Aunque esto solo no permite que un atacante poner en peligro un servidor de federación de recursos, confiar en certificados firmados self\ aumentar la superficie de ataque de un equipo, y puede provocar vulnerabilidades de seguridad si el firmante del certificado no es de confianza.  
  
-   Crea una experiencia del usuario deficiente. Los clientes recibirán alerta de seguridad de mensajes que aparecen cuando intentan acceder a los recursos federados que muestran el siguiente mensaje: "el certificado de seguridad fue emitido por una compañía que decidió no confiar". Este es el comportamiento esperado, porque el certificado de firma de self\ no es de confianza.  
  
    > [!NOTE]  
    > Si es necesario, puede evitar esta condición mediante la directiva de grupo para insertar manualmente el certificado de firma de self\ en la tienda de la raíz de confianza en cada equipo cliente que intenta acceder a un sitio de AD FS.  
  
-   Entidades emisoras de certificados proporcionan basado en certificate\ características adicionales, como archivo de clave privada, renovación y revocación, que no se encuentren en certificados firmados self\.  
  

