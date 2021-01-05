---
title: Evite habilitar las máquinas virtuales configuradas con adaptadores de Canal de fibra virtuales para permitir migraciones en vivo cuando haya menos rutas de acceso a Canal de fibra unidades lógicas (LUN) en el destino que en el origen.
description: Obtenga información acerca de qué hacer cuando una o varias máquinas virtuales tienen la propiedad AllowReducedFcRedunancy establecida en el proveedor de WMI de virtualización.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.date: 8/16/2016
ms.openlocfilehash: 9ba20bb14c908566419ad7a2e3bb9fda10fe9367
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834660"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evite habilitar las máquinas virtuales configuradas con adaptadores de Canal de fibra virtuales para permitir migraciones en vivo cuando haya menos rutas de acceso a Canal de fibra unidades lógicas (LUN) en el destino que en el origen.

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
*Una o varias máquinas virtuales tienen la propiedad AllowReducedFcRedunancy establecida en el proveedor de WMI de virtualización.*

## <a name="impact"></a>**Impacto**
*La migración en vivo de las siguientes máquinas virtuales podría provocar la pérdida de datos o la e/s de interrupción en el almacenamiento:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Considere la posibilidad de borrar la propiedad WMI AllowReducedFcRedundancy en las máquinas virtuales afectadas. Cuando esta propiedad está desactivada, puede realizar una migración en vivo en máquinas virtuales configuradas con adaptadores de Canal de fibra virtual solo cuando el número de rutas de acceso que se van a Canal de fibra en el destino sea igual o mayor que el número de rutas de acceso en el origen. Estas comprobaciones ayudan a evitar la pérdida de datos o la interrupción de e/s en el almacenamiento.*