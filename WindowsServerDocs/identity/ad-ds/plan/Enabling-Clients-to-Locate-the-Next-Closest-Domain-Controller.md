---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: Permitir que los clientes busquen el controlador de dominio más próximo siguiente
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: ce946fb0fe581a66f4a4e29cf8491f00294b5bcb
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86953587"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>Permitir que los clientes busquen el controlador de dominio más próximo siguiente

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si tiene un controlador de dominio que ejecuta Windows Server 2008 o una versión más reciente, puede permitir que los equipos cliente que ejecutan Windows Vista o posterior o Windows Server 2008 o posterior busquen controladores de dominio de forma más eficaz si habilita la opción **intentar el sitio más cercano más próximo** Directiva de grupo. Esta configuración mejora el localizador de controladores de dominio (Ubicador de DC) ayudando a simplificar el tráfico de red, especialmente en grandes empresas que tienen muchas sucursales y sitios.

Esta nueva configuración puede afectar al modo en que se configuran los costos de vínculo a sitios porque afecta al orden en el que se encuentran los controladores de dominio. En el caso de las empresas que tienen muchos sitios de concentrador y sucursales, puede reducir significativamente el tráfico de Active Directory en la red asegurándose de que los clientes conmuten por error al siguiente sitio concentrador más cercano cuando no encuentren un controlador de dominio en el sitio concentrador más cercano.

Como práctica recomendada general, debe simplificar la topología de sitio y los costos de vínculo de sitio tanto como sea posible si habilita la opción **intentar el siguiente sitio más cercano** . En empresas con muchos sitios de concentrador, esto puede simplificar los planes que realice para controlar las situaciones en las que los clientes de un sitio deben conmutar por error a un controlador de dominio de otro sitio.

De forma predeterminada, la opción **intentar el siguiente sitio más cercano** no está habilitada. Cuando la opción no está habilitada, el ubicador de DC usa el siguiente algoritmo para buscar un controlador de dominio:

- Intente buscar un controlador de dominio en el mismo sitio.
- Si no hay ningún controlador de dominio disponible en el mismo sitio, intente encontrar cualquier controlador de dominio en el dominio.

> [!NOTE]
> Este es el mismo algoritmo que el ubicador de DC usado en las versiones anteriores de Active Directory. Para obtener más información, consulte el artículo [Cómo funciona la compatibilidad con DNS para Active Directory](/previous-versions/windows/it-pro/windows-server-2003/cc759550(v=ws.10)).

Si habilita la opción **probar el siguiente sitio más cercano** , el ubicador de DC usa el siguiente algoritmo para buscar un controlador de dominio:

- Intente buscar un controlador de dominio en el mismo sitio.
- Si no hay ningún controlador de dominio disponible en el mismo sitio, intente encontrar un controlador de dominio en el siguiente sitio más cercano. Un sitio está más cerca si tiene un menor costo de vínculo de sitio que otro sitio con un costo de vínculo de sitio superior.
- Si no hay ningún controlador de dominio disponible en el siguiente sitio más cercano, intente encontrar cualquier controlador de dominio en el dominio.

La configuración **intentar siguiente sitio más cercano** funciona en coordinación con la cobertura automática de sitios. Por ejemplo, si el siguiente sitio más cercano no tiene ningún controlador de dominio, el ubicador de DC intenta encontrar el controlador de dominio que realiza la cobertura automática de sitios para ese sitio.

De forma predeterminada, el ubicador de DC no tiene en cuenta ningún sitio que contenga un controlador de dominio de solo lectura (RODC) cuando determina el siguiente sitio más cercano. Además, cuando el cliente recibe una respuesta de un controlador de dominio que ejecuta una versión anterior a Windows Server 2008, el comportamiento del Ubicador de DC es el mismo que cuando la opción no está habilitada.

Por ejemplo, supongamos que una topología de sitio tiene cuatro sitios con los valores de vínculo de sitio en la siguiente ilustración. En este ejemplo, todos los controladores de dominio son controladores de dominio de escritura que ejecutan Windows Server 2008 o posterior.

![habilitación de los clientes para buscar DC](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

Cuando la opción **intentar el siguiente sitio más cercano** Directiva de grupo está habilitada en este ejemplo, si un equipo cliente de Site_B intenta encontrar un controlador de dominio, primero intenta encontrar un controlador de dominio en su propio Site_B. Si no hay ninguna disponible en Site_B, intenta encontrar un controlador de dominio en Site_A.

Si la opción no está habilitada, el cliente intenta encontrar un controlador de dominio en Site_A, Site_C o Site_D si no hay ningún controlador de dominio disponible en Site_B.

> [!NOTE]
> La configuración **intentar siguiente sitio más cercano** funciona en coordinación con la cobertura automática de sitios. Por ejemplo, si el siguiente sitio más cercano no tiene ningún controlador de dominio, el ubicador de DC intenta encontrar el controlador de dominio que realiza la cobertura automática de sitios para ese sitio.

Para aplicar la configuración **intentar el siguiente sitio más cercano** , puede crear un objeto de directiva de grupo (GPO) y vincularlo al objeto adecuado para su organización, o bien puede modificar la Directiva de dominio predeterminada para que afecte a todos los clientes que ejecutan Windows Vista o posterior y windows Server 2008 o una versión más reciente en el dominio. Para obtener más información acerca de cómo establecer la configuración **probar el siguiente sitio más cercano** , consulte [habilitación de clientes para buscar un controlador de dominio en el siguiente sitio más cercano](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc772592(v=ws.10)).
