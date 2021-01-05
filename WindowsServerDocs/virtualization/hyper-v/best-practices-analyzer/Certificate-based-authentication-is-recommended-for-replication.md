---
title: Se recomienda la autenticación basada en certificados para la replicación
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales seleccionadas para la replicación están configuradas para la autenticación Kerberos.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
ms.date: 8/16/2016
ms.openlocfilehash: 83d4b90670678660da0610d800eaf1fbe87a863e
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833560"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>Se recomienda la autenticación basada en certificados para la replicación

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
*Una o varias máquinas virtuales seleccionadas para la replicación están configuradas para la autenticación Kerberos.*

## <a name="impact"></a>**Impacto**
*El tráfico de red de replicación del servidor principal al servidor de replicación no está cifrado. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Si se usa otro método para realizar el cifrado, puede pasarlo por alto. De lo contrario, modifique la configuración de la máquina virtual para elegir autenticación basada en certificados.*



