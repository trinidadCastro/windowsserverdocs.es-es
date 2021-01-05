---
title: Uno o más adaptadores de red deben configurarse como destino para la creación de reflejo del puerto
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales tienen un adaptador de red configurado como origen para la creación de reflejo del puerto, pero no hay ningún destino correspondiente en el conmutador virtual.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b83c166d-f010-47c4-a4bb-02167f2e3361
ms.date: 8/16/2016
ms.openlocfilehash: f69bd6da7d2451deaee97c3f3d23b5c861223388
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846361"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-destination-for-port-mirroring"></a>Uno o más adaptadores de red deben configurarse como destino para la creación de reflejo del puerto

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Una o varias máquinas virtuales tienen un adaptador de red configurado como origen para la creación de reflejo del puerto, pero no hay ningún destino correspondiente en el conmutador virtual.*

## <a name="impact"></a>**Impacto**
*La creación de reflejo del puerto no funcionará correctamente para los siguientes conmutadores virtuales y máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Use Windows PowerShell o el administrador de Hyper-V para completar o corregir la configuración de creación de reflejo del puerto.*



