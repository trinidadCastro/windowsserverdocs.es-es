---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Permitir que los clientes busquen el controlador de dominio más próximo siguiente
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7550bdcea4e7b06d31463744bfdc3319c012c62c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880366"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permitir que los clientes busquen el controlador de dominio más próximo siguiente

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si tiene un controlador de dominio que ejecuta Windows Server 2008 o posterior, puede hacer lo posible para los equipos cliente que ejecutan Windows Vista o versiones más recientes o Windows Server 2008 o posterior para buscar controladores de dominio de forma más eficaz habilitando la **intente siguiente Sitio más cercano** configuración de directiva de grupo. Esta configuración mejora el ubicador del controlador de dominio (Ubicador de DC) por lo que ayuda a optimizar el tráfico de red, especialmente en grandes empresas que tienen muchas de las sucursales y sitios.

Esta nueva configuración puede afectar a cómo configurar los costos del vínculo de sitio, ya que afecta el orden en que se encuentren los controladores de dominio. Para las empresas que tienen muchos sitios de concentrador y las sucursales, puede reducir significativamente el tráfico de Active Directory en la red al garantizar que los clientes conmutarán por error en el siguiente sitio más cercano de concentrador que no pueden encontrar un controlador de dominio en el sitio más cercano del concentrador.

Como procedimiento recomendado general, debe simplificar la topología de sitio y vínculo a sitios cuesta tanto como sea posible si habilita la **intentar siguiente sitio más cercano** configuración. En las empresas con muchos sitios concentradores, esto puede simplificar los planes que realice para controlar situaciones en las que los clientes en un sitio necesitan conmutar por error a un controlador de dominio en otro sitio.

De forma predeterminada, el **intentar siguiente sitio más cercano** no está habilitada. Cuando la opción no está habilitada, ubicador de DC usa el siguiente algoritmo para encontrar un controlador de dominio:

- Intenta encontrar un controlador de dominio en el mismo sitio.
- Si no hay ningún controlador de dominio está disponible en el mismo sitio, intente encontrar cualquier controlador de dominio en el dominio.

> [!NOTE]
> Este es el mismo algoritmo que utiliza el ubicador de DC en versiones anteriores de Active Directory. Para obtener más información, vea el artículo [cómo compatibilidad de DNS con funciona Active Directory](https://go.microsoft.com/fwlink/?LinkId=108587).

Si habilita la **intentar siguiente sitio más cercano** , ubicador de DC utilizará el siguiente algoritmo para encontrar un controlador de dominio:

- Intenta encontrar un controlador de dominio en el mismo sitio.
- Si ningún controlador de dominio está disponible en el mismo sitio, pruebe a buscar un controlador de dominio en el siguiente sitio más cercano. Un sitio está más próximo si tiene un vínculo de sitio menor costo que otro sitio con un vínculo de sitio mayor costo.
- Si ningún controlador de dominio está disponible en el siguiente sitio más cercano, intente encontrar cualquier controlador de dominio en el dominio.

El **intentar siguiente sitio más cercano** configuración funciona en coordinación con la cobertura de sitios automática. Por ejemplo, si el siguiente sitio más cercano no tiene ningún controlador de dominio, ubicador de DC intenta encontrar el controlador de dominio que realiza la cobertura de sitios automática para ese sitio.

De forma predeterminada, ubicador de DC no tiene en cuenta cualquier sitio que contiene un controlador de dominio de solo lectura (RODC) cuando determina el siguiente sitio más cercano. Además, cuando el cliente recibe una respuesta desde un controlador de dominio que ejecuta una versión anterior a Windows Server 2008, el comportamiento de ubicador de DC es el mismo que cuando no está habilitada la configuración.

Por ejemplo, suponga que una topología de sitio tiene cuatro sitios con los valores de vínculo de sitio en la siguiente ilustración. En este ejemplo, todos los controladores de dominio son controladores de dominio de escritura que ejecutan Windows Server 2008 o posterior.

![permitir que los clientes buscar el controlador de dominio](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Cuando el **intentar siguiente sitio más cercano** configuración de directiva de grupo está habilitado en este ejemplo, si un equipo cliente en sitio_B intenta localizar un controlador de dominio, primero intenta encontrar un controlador de dominio en su propio sitio_B. Si ninguno está disponible en sitio_B, intenta encontrar un controlador de dominio en sitio_A.

Si la opción no está habilitada, el cliente intenta encontrar un controlador de dominio en sitio_A, Site_C o Site_D si ningún controlador de dominio está disponible en sitio_B.

> [!NOTE]
> El **intentar siguiente sitio más cercano** configuración funciona en coordinación con la cobertura de sitios automática. Por ejemplo, si el siguiente sitio más cercano no tiene ningún controlador de dominio, ubicador de DC intenta encontrar el controlador de dominio que realiza la cobertura de sitios automática para ese sitio.

Para aplicar el **intentar siguiente sitio más cercano** establecer, puede crear un objeto de directiva de grupo (GPO) y vincúlelo al objeto adecuado para su organización o, puede modificar la directiva predeterminada de dominio para que afectan a todos los clientes que ejecutan Windows Vista o posterior y Windows Server 2008 o posterior en el dominio. Para obtener más información sobre cómo establecer el **intentar siguiente sitio más cercano** , vea [habilitar los clientes para buscar un controlador de dominio en el siguiente sitio más cercano](https://technet.microsoft.com/library/cc772592.aspx).
