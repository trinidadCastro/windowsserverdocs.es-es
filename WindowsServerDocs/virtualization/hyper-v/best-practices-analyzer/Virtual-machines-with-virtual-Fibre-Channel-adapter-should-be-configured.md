---
title: Las máquinas virtuales configuradas con un adaptador de Canal de fibra virtual deben configurarse para alta disponibilidad en el almacenamiento basado en Canal de fibra
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales no tienen una conexión de alta disponibilidad con almacenamiento basado en Canal de fibra porque esas máquinas virtuales están configuradas con un adaptador de Canal de fibra virtual que está conectado a un solo adaptador de bus host (HBA).
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
ms.date: 8/16/2016
ms.openlocfilehash: 407db3e9e9f289deb438f67ad7e79c98cb2b914b
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845833"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Las máquinas virtuales configuradas con un adaptador de Canal de fibra virtual deben configurarse para alta disponibilidad en el almacenamiento basado en Canal de fibra

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Información|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Una o varias máquinas virtuales no tienen una conexión de alta disponibilidad con el almacenamiento basado en Canal de fibra porque esas máquinas virtuales están configuradas con un adaptador de Canal de fibra virtual que está conectado a un único adaptador de bus host (HBA).*

## <a name="impact"></a>**Impacto**
*Un error del adaptador de bus host podría bloquear la Canal de fibra conexión entre el almacenamiento y las máquinas virtuales. Esto afecta a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Agregue otra conexión desde la máquina virtual al adaptador de bus host y configure e/s de múltiples rutas (MPIO) en el sistema operativo invitado para establecer conexiones Canal de fibra redundantes.*



