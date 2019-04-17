---
title: POST-Post-Deployment Pasos para el controlador de red
description: Este tema proporciona instrucciones de configuración de certificado de implementaciones de Kerberos que no son de controlador de red en Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="post-deployment-steps-for-network-controller"></a>POST-Post-Deployment Pasos para el controlador de red

Cuando se instala el controlador de red, puedes elegir las implementaciones de Kerberos o no Kerberos.

Para implementaciones non\ Kerberos, debes configurar certificados.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurar certificados para las implementaciones de Kerberos no

Si la \(VMs\) equipos o máquinas virtuales de controlador de red y el cliente de administración no están unidos a un domain\, debes configurar autenticación basada en certificate\ al completar los pasos siguientes.

- Crear un certificado en el controlador de red para la autenticación de equipo. El nombre de firmante del certificado debe ser igual que el nombre DNS del controlador de red equipo o máquina virtual.

- Crear un certificado en el cliente de administración. Este certificado debe ser de confianza para el controlador de red.
  
- Inscribir un certificado en el equipo de controlador de red o máquina virtual. El certificado debe cumplir los requisitos siguientes.
  
    -  El propósito de autenticación del servidor y el propósito de autenticación de cliente deben estar configurados en uso mejorado de claves \(EKU\) o extensiones de directivas de aplicación. El identificador de objeto para la autenticación de servidor es 1.3.6.1.5.5.7.3.1. El identificador de objeto para la autenticación de cliente es 1.3.6.1.5.5.7.3.2.
  
    - Debería resolver el nombre de firmante del certificado para:
  
        - La dirección IP del equipo de controlador de red o máquina virtual, si el controlador de red se implementa en un solo equipo o máquina virtual.

        - La dirección IP de REST, si el controlador de red se implementa en varios equipos, varias de las máquinas virtuales o ambos.
  
    - Este certificado debe ser de confianza para todos los clientes REST. El certificado también debe ser de confianza por el equilibrio de carga de Software (SLB) multiplexor (MUX) y los equipos host southbound que administran por el controlador de red.
  
    - El certificado se puede inscribir por una entidad de certificación (CA) o puede ser un certificado autofirmado. Certificados autofirmados no se recomiendan para entornos de producción, pero son aceptables para entornos de laboratorio de prueba.
  
    - El mismo certificado debe estar aprovisionado en todos los nodos de controlador de red. Después de crear el certificado en un nodo, puedes exportar el certificado (con la clave privada) e importarla en otros nodos.

Para obtener más información, consulta [controlador de red](Network-Controller.md).
