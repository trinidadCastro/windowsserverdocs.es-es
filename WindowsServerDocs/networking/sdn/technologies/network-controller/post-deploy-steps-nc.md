---
title: Pasos posteriores a la implementación para la Controladora de red
description: Este tema proporciona instrucciones de configuración del certificado para las implementaciones de Kerberos que no son de controladora de red en Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871086"
---
# <a name="post-deployment-steps-for-network-controller"></a>Pasos posteriores a la implementación para la Controladora de red

Cuando se instala el controlador de red, puede elegir las implementaciones de Kerberos o que no sea de Kerberos.

Para que no sean\-las implementaciones de Kerberos, debe configurar los certificados.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurar certificados para las implementaciones que no sea de Kerberos

Si los equipos o máquinas virtuales \(máquinas virtuales\) de controladora de red y el cliente de administración no son dominio\-Unido, debe configurar certificado\-autenticación basada en siguiendo pasos.

- Crear un certificado en la controladora de red para la autenticación de equipo. El nombre de sujeto del certificado debe ser igual que el nombre DNS del equipo de la controladora de red o la máquina virtual.

- Crear un certificado en el cliente de administración. Este certificado debe ser de confianza por la controladora de red.
  
- Inscribir un certificado en el equipo de la controladora de red o la máquina virtual. El certificado debe cumplir los requisitos siguientes.
  
    -  El propósito de autenticación del servidor y el propósito de autenticación del cliente deben configurarse en el uso mejorado de clave \(EKU\) o extensiones de directivas de aplicación. El identificador de objeto para la autenticación de servidor es 1.3.6.1.5.5.7.3.1. El identificador de objeto para la autenticación de cliente es 1.3.6.1.5.5.7.3.2.
  
    - Debe resolver el nombre de sujeto del certificado para:
  
        - La dirección IP del equipo de la controladora de red o la máquina virtual, si la controladora de red se implementa en un único equipo o máquina virtual.

        - La dirección IP de REST, si la controladora de red se implementa en varios equipos, varias máquinas virtuales o ambos.
  
    - Este certificado debe ser de confianza por todos los clientes REST. El certificado también debe ser de confianza, el equilibrio de carga de Software (SLB) multiplexor (MUX) y los equipos host southbound administrados por controladora de red.
  
    - El certificado se puede inscribir por una entidad de certificación (CA) o puede ser un certificado autofirmado. Los certificados autofirmados no se recomiendan para las implementaciones de producción, pero son aceptables para los entornos de laboratorio de prueba.
  
    - El mismo certificado se debe aprovisionar en todos los nodos de controladora de red. Después de crear el certificado en un nodo, puede exportar el certificado (con clave privada) e importarlo en los demás nodos.

Para obtener más información, consulte [Controladora de red](Network-Controller.md).
