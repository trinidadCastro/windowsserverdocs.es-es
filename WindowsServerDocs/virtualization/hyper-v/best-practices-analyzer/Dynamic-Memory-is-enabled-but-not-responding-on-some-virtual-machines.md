---
title: Memoria dinámica está habilitado pero no responde en algunas máquinas virtuales
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales experimentan problemas con el controlador necesario para Memoria dinámica en el sistema operativo invitado.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 91b7f50f-a071-4ab6-beb1-1b29f92f52b6
ms.date: 8/16/2016
ms.openlocfilehash: 2fbf600e2fa06f01ddbf3ad3dafc02ded25dc6fc
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846107"
---
# <a name="dynamic-memory-is-enabled-but-not-responding-on-some-virtual-machines"></a>Memoria dinámica está habilitado pero no responde en algunas máquinas virtuales

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Una o varias máquinas virtuales están experimentando problemas con el controlador necesario para Memoria dinámica en el sistema operativo invitado.*

## <a name="impact"></a>Impacto
*Es posible que el sistema operativo invitado de las siguientes máquinas virtuales no se ejecute o se ejecute de manera no confiable porque Hyper-V no puede ajustar la memoria de forma dinámica para responder a los cambios en la demanda de memoria. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Este es el comportamiento esperado si se está iniciando la máquina virtual. Si la máquina virtual no arranca, asegúrese de que Integration Services se actualiza a la versión más reciente y que el sistema operativo invitado admite Memoria dinámica.*

A partir de Windows Server 2016, los servicios de integración se entregan a través de Windows Update. Asegúrese de que las máquinas virtuales están configuradas para recibir actualizaciones para obtener la versión más reciente de Integration Services.

Memoria dinámica funciona con versiones específicas de invitados admitidos. Vea [información general de memoria dinámica de Hyper-V](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831766(v=ws.11)) para versiones anteriores a windows Server 2016 y Windows 10.