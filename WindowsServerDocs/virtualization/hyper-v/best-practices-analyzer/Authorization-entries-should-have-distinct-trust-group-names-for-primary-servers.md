---
title: Las entradas de autorización deben tener nombres de grupos de confianza distintos para los servidores principales con máquinas virtuales que no forman parte del mismo grupo de confianza
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53cb81e0071dee09feffb6e37f2ab6dbece27d28
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970012"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>Las entradas de autorización deben tener nombres de grupos de confianza distintos para los servidores principales con máquinas virtuales que no forman parte del mismo grupo de confianza

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
*El servidor aceptará solicitudes de replicación para la máquina virtual de réplica de cualquiera de los servidores de la lista de autorización asociada a la misma etiqueta de replicación que la máquina virtual.*

## <a name="impact"></a>**Impacto**
*Puede haber problemas de privacidad y seguridad con una máquina virtual que acepte la replicación de los servidores principales que pertenecen a diferentes entradas de autorización. Esto afecta a las siguientes entradas de autorización:\<list of authorization entries>*

## <a name="resolution"></a>**Resolución**
*Use etiquetas diferentes en las entradas de autorización para los servidores principales con máquinas virtuales que no forman parte del mismo grupo de seguridad. Modifique la configuración de Hyper-V para configurar las etiquetas de replicación.*



