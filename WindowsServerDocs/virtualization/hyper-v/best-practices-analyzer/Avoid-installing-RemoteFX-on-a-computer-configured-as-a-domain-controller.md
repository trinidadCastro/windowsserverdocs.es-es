---
title: Evitar la instalación de RemoteFX en un equipo configurado como controlador de dominio de Active Directory
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 597dd26d0a317ca026cd94ab29138ce679d7c3b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857768"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Evitar la instalación de RemoteFX en un equipo configurado como controlador de dominio de Active Directory

>Se aplica a: Windows Server 2016

Para obtener más información sobre los análisis y los procedimientos recomendados, vea [ejecución de exámenes de analizador de procedimientos recomendados y administración de los resultados de los exámenes](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propiedad|Detalles|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Producto o característica**|Hyper-V|  
|**Gravedad**|Error|  
|**Categoría**|Configuración|  
  
En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.  
  
## <a name="issue"></a>**Problema**  
*RemoteFX está instalado en un controlador de dominio.*  
  
## <a name="impact"></a>**Impacto**  
*Los equipos virtuales configurados para RemoteFX no se pueden usar en estos equipos.*  
  
## <a name="resolution"></a>**Resolución**  
*Decida si desea configurar este servidor con RemoteFX para Hyper-V o como controlador de Dominio de Active Directory y, a continuación, vuelva a configurar el servidor según sea necesario.*  
  


