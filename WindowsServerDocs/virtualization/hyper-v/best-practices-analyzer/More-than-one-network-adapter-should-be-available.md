---
title: Hay más de un adaptador de red disponible
description: Obtenga información acerca de qué hacer cuando el servidor se configura con un adaptador de red, que el sistema operativo de administración debe compartir y todas las máquinas virtuales que requieren acceso a una red física.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
ms.date: 8/16/2016
ms.openlocfilehash: 4cf106b15d69079e860b5a76febbc0ab549b0f34
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846296"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Hay más de un adaptador de red disponible

>Se aplica a: Windows Server 2016

Para más información acerca de los análisis y los procedimientos recomendados, vea [Analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*Este servidor se configura con un adaptador de red, que debe ser compartido por el sistema operativo de administración y todas las máquinas virtuales que requieran acceso a una red física.*

## <a name="impact"></a>Impacto

*El rendimiento de red puede verse afectado en el sistema operativo de administración.*

## <a name="resolution"></a>Solución

*Agregue más adaptadores de red a este equipo. Para reservar un adaptador de red para uso exclusivo del sistema operativo de administración, no lo configure para su uso con una red virtual externa.*

Para obtener información acerca de cómo agregar un adaptador de red al equipo, consulte la documentación del equipo o del adaptador de red. A continuación, para reservarlo exclusivamente para el sistema operativo de administración, no lo conecte a un conmutador virtual.



