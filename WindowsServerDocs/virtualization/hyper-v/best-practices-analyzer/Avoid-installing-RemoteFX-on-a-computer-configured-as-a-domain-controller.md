---
title: Evite la instalación de RemoteFX en un equipo que está configurado como un controlador de dominio de Active Directory
description: Versión en línea del texto para esta regla de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9e1c642e3f36b5fe25f34bb417a83b8510adcc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832566"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Evite la instalación de RemoteFX en un equipo que está configurado como un controlador de dominio de Active Directory

>Se aplica a: Windows Server 2016

Para obtener más información sobre análisis y los procedimientos recomendados, consulte [Run Best Practices Analyzer Scans y Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Property|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Característica del producto**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, la cursiva indica texto de la interfaz de usuario que aparece en la herramienta Best Practices Analyzer para resolver este problema.  
  
## <a name="issue"></a>**Problema**  
*RemoteFX está instalado en un controlador de dominio.*  
  
## <a name="impact"></a>**Impact**  
*No se puede usar equipos virtuales configurados para RemoteFX en estos equipos.*  
  
## <a name="resolution"></a>**Resolución**  
*Decida si desea que este servidor se configura con RemoteFX de Hyper-V o como un controlador de dominio de Active Directory y, después, volver a configurar el servidor según sea necesario.*  
  


