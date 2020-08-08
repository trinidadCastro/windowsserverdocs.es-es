---
title: Evitar la instalación de RemoteFX en un equipo configurado como controlador de dominio de Active Directory
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 5a5c2d23794ba2328da9a2a8c668a8525f6c6322
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960678"
---
# <a name="avoid-installing-remotefx-on-a-computer-that-is-configured-as-an-active-directory-domain-controller"></a>Evitar la instalación de RemoteFX en un equipo configurado como controlador de dominio de Active Directory

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*RemoteFX está instalado en un controlador de dominio.*

## <a name="impact"></a>**Impacto**
*Los equipos virtuales configurados para RemoteFX no se pueden usar en estos equipos.*

## <a name="resolution"></a>**Resolución**
*Decida si desea configurar este servidor con RemoteFX para Hyper-V o como controlador de Dominio de Active Directory y, a continuación, vuelva a configurar el servidor según sea necesario.*



