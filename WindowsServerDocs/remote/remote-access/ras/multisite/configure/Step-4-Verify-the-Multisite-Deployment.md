---
title: Paso 4 comprobar la implementación multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17b944bd0e2c13f9a3d324eeda09c67b110ce49d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821856"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Paso 4 comprobar la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Este tema describe cómo comprobar que ha configurado correctamente la implementación multisitio de acceso remoto.  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Para comprobar el acceso a los recursos internos a través de la implementación multisitio  
  
1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.  
  
2.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  
3.  Probar la conectividad a través de cada servidor en la implementación multisitio por apagar o desconectar de la red externa, todos menos uno de los servidores de acceso remoto. En el equipo cliente, intente acceder a recursos corporativos. Repita la prueba en un servidor distinto de multisitio. Pueden tardar hasta 10 minutos para el equipo cliente para conectarse al nuevo punto de entrada. Esto es porque el sondeo se ha desactivado durante 10 minutos para un punto de entrada después de que se considera están inaccesibles, con el fin de optimizar el ancho de banda y duración de batería. Como alternativa, puede cambiar entre los diversos puntos de entrada manualmente al elegir el punto de entrada deseado en el cuadro combinado se muestra al ejecutar **daprop.exe**.  
  
    Debe poder tener acceso a todos los recursos corporativos a través de cada servidor multisitio.  
  
4.  Conectarse a Windows 7&reg; equipo cliente a la empresa de red y obtener la directiva de grupo.  
  
5.  Conectar el equipo cliente Windows 7 a la red externa e intente acceder a recursos internos.  
  
    Debe poder tener acceso a todos los recursos corporativos.  
  
6.  Probar la conectividad para los clientes de Windows 7 a través de cada servidor de la implementación multisitio, acceso a la consola de equipos y usuarios de Active Directory y mover el equipo cliente al grupo de seguridad que corresponde a cada servidor. Después de que los cambios se han replicado en todo el dominio, reinicie el equipo cliente mientras está conectado a la red corporativa para obtener la nueva directiva de grupo. Intento de acceder a recursos corporativos. Repita la prueba en un servidor distinto de multisitio.  
  
    Debe poder tener acceso a todos los recursos corporativos a través de cada servidor multisitio.  
  
    Este método puede no ser factible debido a la cantidad de tiempo necesario para que los cambios se repliquen en todo el dominio en un entorno de producción. Es posible que desee forzar la replicación siempre que sea posible. Las pruebas también pueden realizarse desde varios equipos cliente Windows 7 diferentes que ya son miembros de los diferentes grupos de seguridad de Windows 7 en la implementación multisitio.  
  


