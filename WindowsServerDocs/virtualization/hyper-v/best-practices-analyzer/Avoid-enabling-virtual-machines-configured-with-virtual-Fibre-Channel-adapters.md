---
title: Evite habilitar máquinas virtuales configuradas con los adaptadores de canal de fibra virtuales para permitir que las migraciones en vivo cuando hay menos rutas de acceso a unidades lógicas (LUN de) canal de fibra en el destino que no sea en el origen
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6ff69d5cb09133a806c2a2df3446713264a4e892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849556"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>Evite habilitar máquinas virtuales configuradas con los adaptadores de canal de fibra virtuales para permitir que las migraciones en vivo cuando hay menos rutas de acceso a unidades lógicas (LUN de) canal de fibra en el destino que no sea en el origen

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|

En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.
  
## <a name="issue"></a>**Problema**  
*Una o más máquinas virtuales tienen la propiedad de AllowReducedFcRedunancy establecida en el proveedor WMI de virtualización.*  
  
## <a name="impact"></a>**Impact**  
*Migración en vivo de las siguientes máquinas virtuales podría provocar la pérdida de datos o interrupción de E/S en el almacenamiento:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>**Resolución**  
*Considere la posibilidad de borrar la propiedad AllowReducedFcRedundancy WMI en las máquinas virtuales afectadas. Cuando esta propiedad está desactivada, puede realizar una migración en vivo en máquinas virtuales configuradas con los adaptadores de canal de fibra virtuales solo cuando el número de rutas de acceso al canal de fibra en el destino sea igual o mayor que el número de rutas de acceso en el origen. Estas comprobaciones ayudan a evitar la pérdida de datos o la interrupción de E/S en el almacenamiento.* 