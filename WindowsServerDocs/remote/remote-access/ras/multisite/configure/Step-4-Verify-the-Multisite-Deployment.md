---
title: Paso 4 comprobación de la implementación multisitio
description: Obtenga información acerca de cómo comprobar que ha configurado correctamente la implementación multisitio de acceso remoto.
manager: brianlic
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 29c4c65673399b017716b2bfc299742f1d0c1307
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949201"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Paso 4 comprobación de la implementación multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación multisitio de acceso remoto.

### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Para comprobar el acceso a los recursos internos a través de la implementación multisitio

1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga la directiva de grupo.

2.  Conecte el equipo cliente a la red externa e intente tener acceso a recursos internos.

    Debe poder tener acceso a todos los recursos corporativos.

3.  Pruebe la conectividad a través de cada servidor en la implementación multisitio desactivando o desconectando de la red externa, excepto uno de los servidores de acceso remoto. En el equipo cliente, intente tener acceso a los recursos corporativos. Repita la prueba en un servidor multisitio diferente. El equipo cliente puede tardar hasta 10 minutos en conectarse al nuevo punto de entrada. Esto se debe a que el sondeo está desactivado durante 10 minutos para un punto de entrada después de que no se pueda tener acceso a él, con el fin de optimizar el ancho de banda y la duración de la batería. Como alternativa, puede alternar entre los distintos puntos de entrada de forma manual. para ello, elija el punto de entrada deseado en el cuadro combinado que se muestra al ejecutar **daprop.exe**.

    Debe poder tener acceso a todos los recursos corporativos a través de cada servidor multisitio.

4.  Conecte un &reg;  equipo cliente de Windows 7 a la red corporativa y obtenga la Directiva de grupo.

5.  Conecte el equipo cliente de Windows 7 a la red externa e intente acceder a los recursos internos.

    Debe poder tener acceso a todos los recursos corporativos.

6.  Pruebe la conectividad de los clientes de Windows 7 a través de cada servidor en la implementación multisitio. para ello, acceda a la consola de Active Directory usuarios y equipos y mueva el equipo cliente al grupo de seguridad correspondiente a cada servidor. Una vez que los cambios se hayan replicado en todo el dominio, reinicie el equipo cliente mientras está conectado a la red corporativa para obtener la nueva Directiva de grupo. Intente tener acceso a los recursos corporativos. Repita la prueba en un servidor multisitio diferente.

    Debe poder tener acceso a todos los recursos corporativos a través de cada servidor multisitio.

    En un entorno de producción, es posible que este método no sea factible debido a la cantidad de tiempo necesario para replicar los cambios en todo el dominio. Puede que desee forzar la replicación siempre que sea posible. Las pruebas también se pueden realizar desde varios equipos cliente de Windows 7 diferentes que ya son miembros de los distintos grupos de seguridad de Windows 7 en la implementación multisitio.



