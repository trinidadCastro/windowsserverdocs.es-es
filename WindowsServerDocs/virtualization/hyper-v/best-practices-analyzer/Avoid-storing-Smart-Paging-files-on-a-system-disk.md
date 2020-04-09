---
title: Evitar el almacenamiento de archivos de paginación inteligente en un disco del sistema
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 111cf3b3b95f50d6d36a6b30b5a0bb46e255ee28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857738"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evitar el almacenamiento de archivos de paginación inteligente en un disco del sistema

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|advertencia|  
|**Categoría**|Operaciones|  
  
En las secciones siguientes, cursiva indica el texto que aparece en la herramienta Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>Problema  
*La configuración de memoria de una o varias máquinas virtuales podría requerir el uso de la paginación inteligente si se reinicia la máquina virtual y la ubicación especificada para el archivo de paginación inteligente es el disco del sistema del servidor que ejecuta Hyper-V.*  
  
## <a name="impact"></a>Impacto  
*El uso del disco del sistema para la paginación inteligente podría provocar problemas en el servidor que ejecuta Hyper-V. Esto afecta a las siguientes máquinas virtuales:*  
  
\<lista de máquinas virtuales >  
  
## <a name="resolution"></a>Resolución  
*Vuelva a configurar las máquinas virtuales para almacenar los archivos de paginación inteligente en un disco que no sea del sistema.*  
  


