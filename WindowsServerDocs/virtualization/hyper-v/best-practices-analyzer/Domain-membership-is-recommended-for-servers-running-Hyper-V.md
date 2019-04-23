---
title: Pertenencia al dominio se recomienda para los servidores que ejecutan Hyper-V
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e9db1d28cfe1ae4afd6c5dc1a93253c83fc42113
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860906"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>Pertenencia al dominio se recomienda para los servidores que ejecutan Hyper-V

>Se aplica a: Windows Server 2016


  
*Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Advertencia|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>Problema  
  
*Este servidor es miembro de un grupo de trabajo.*  
  
## <a name="impact"></a>Impacto  
  
*No hay ninguna administración central para este servidor.*  
  
Unir este equipo al dominio, permite una administración centralizada a través de directivas de identidad, seguridad y auditoría.  
  
## <a name="resolution"></a>Resolución  
  
*Si tiene un entorno de dominio disponible, unir este servidor a ese dominio.*  
  
> [!IMPORTANT]  
> Se recomienda que revise las cargas de trabajo que se ejecutan en las máquinas virtuales en este equipo para determinar si hay implicaciones de seguridad de unir este equipo a un dominio. Si cualquiera de las máquinas virtuales son controladores de dominio virtualizados, consulte [consideraciones de planeación para controladores de dominio virtualizados](https://go.microsoft.com/fwlink/?LinkId=190192) (https://go.microsoft.com/fwlink/?LinkId=190192).  
  
Unir un equipo a un dominio requiere permisos en el equipo y el dominio:   
- En el equipo, necesitará una cuenta de usuario que sea miembro del grupo Administradores. Inicie sesión con este tipo de cuenta, o proporcione el nombre de usuario y la contraseña de la cuenta cuando se le pida.   
- En el dominio, necesitará una cuenta de usuario que ha autorizado para unir el equipo al dominio. Se le pedirá el nombre de usuario y contraseña.  
  
Para obtener instrucciones, consulte [unir el equipo al dominio](https://go.microsoft.com/fwlink/?LinkId=190193) (https://go.microsoft.com/fwlink/?LinkId=190193).  
  


