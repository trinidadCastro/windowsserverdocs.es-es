---
title: Configurar el servidor con una cantidad suficiente de direcciones MAC dinámicas
description: Obtenga información acerca de qué hacer cuando el número de direcciones MAC dinámicas disponibles es bajo.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
ms.date: 8/16/2016
ms.openlocfilehash: d28035ab63f89986e78e5f17501f7df170742ad7
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846426"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurar el servidor con una cantidad suficiente de direcciones MAC dinámicas

>Se aplica a: Windows Server 2016

*El objetivo de este tema es resolver un problema específico identificado por un análisis de analizador de procedimientos recomendados. Debe aplicar la información de este tema únicamente a los equipos en los que se ha realizado Analizador de procedimientos recomendados de Hyper-V y que experimentan el problema que se trata en este tema. Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema

*El número de direcciones MAC dinámicas disponibles es bajo.*

## <a name="impact"></a>Impacto

*Cuando no hay ninguna dirección MAC dinámica disponible, no se pueden iniciar las máquinas virtuales configuradas para usar una dirección MAC dinámica.*

## <a name="resolution"></a>Solución

*Use el administrador de conmutadores virtuales para ver y ampliar el intervalo de direcciones dinámicas.*



