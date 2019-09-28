---
title: Pasos posteriores a la implementación para la Controladora de red
description: En este tema se proporcionan instrucciones de configuración de certificados para implementaciones que no son de Kerberos de la controladora de red en Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3f95d2884a808239c1d171eecbc983e26e799102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355619"
---
# <a name="post-deployment-steps-for-network-controller"></a>Pasos posteriores a la implementación para la Controladora de red

Al instalar el controlador de red, puede elegir implementaciones de Kerberos o no Kerberos.

En el caso de las implementaciones que no sean @ no__t-0Kerberos, debe configurar los certificados.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configuración de certificados para implementaciones que no son de Kerberos

Si los equipos o máquinas virtuales \(VMs @ no__t-1 para la controladora de red y el cliente de administración no son dominio @ no__t-2joined, debe configurar el certificado @ no__t-3based autenticación completando los pasos siguientes.

- Cree un certificado en la controladora de red para la autenticación del equipo. El nombre del firmante del certificado debe ser el mismo que el nombre DNS del equipo o la máquina virtual de la controladora de red.

- Cree un certificado en el cliente de administración. Este certificado debe ser de confianza para la controladora de red.
  
- Inscriba un certificado en el equipo o la máquina virtual de la controladora de red. El certificado debe cumplir los siguientes requisitos.
  
    -  Tanto el propósito de autenticación de servidor como el propósito de autenticación del cliente deben configurarse en uso mejorado de clave \(EKU @ no__t-1 o en extensiones de directivas de aplicación. El identificador de objeto para la autenticación del servidor es 1.3.6.1.5.5.7.3.1. El identificador de objeto para la autenticación del cliente es 1.3.6.1.5.5.7.3.2.
  
    - El nombre del firmante del certificado debe resolverse en:
  
        - La dirección IP del equipo o la máquina virtual de la controladora de red, si la controladora de red está implementada en un único equipo o máquina virtual.

        - La dirección IP de REST, si el controlador de red se implementa en varios equipos, varias máquinas virtuales o ambos.
  
    - Este certificado debe ser de confianza para todos los clientes REST. El certificado también debe ser de confianza para el multiplexor (MUX) de equilibrio de carga de software (SLB) y los equipos host de southbound administrados por la controladora de red.
  
    - El certificado puede ser inscrito por una entidad de certificación (CA) o puede ser un certificado autofirmado. Los certificados autofirmados no se recomiendan para las implementaciones de producción, pero son aceptables para entornos de laboratorio de pruebas.
  
    - Se debe aprovisionar el mismo certificado en todos los nodos de la controladora de red. Después de crear el certificado en un nodo, puede exportar el certificado (con clave privada) e importarlo en los otros nodos.

Para obtener más información, consulte [Controladora de red](Network-Controller.md).
