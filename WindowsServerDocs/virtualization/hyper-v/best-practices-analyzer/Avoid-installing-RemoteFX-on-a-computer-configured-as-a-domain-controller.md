---
title: Evitar la instalación de RemoteFX en un equipo configurado como controlador de dominio de Active Directory
description: Obtenga información acerca de qué hacer cuando RemoteFX está instalado en un controlador de dominio.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: da58694e-91f6-45d8-a599-18966db165f4
ms.date: 8/16/2016
ms.openlocfilehash: d22f21af2fcc54c44d142ef790b9da4ae66483c9
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834670"
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



