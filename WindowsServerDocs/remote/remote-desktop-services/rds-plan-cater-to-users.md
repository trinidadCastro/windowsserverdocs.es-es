---
title: 'Servicios de Escritorio remoto: Destinado a distintos tipos de usuarios'
description: Describe los distintos tipos de usuarios para RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da522a18-c33f-468e-b9d6-3ad7d3cfba26
author: spatnaik
ms.author: spatnaik
ms.date: 11/06/2019
manager: scottman
ms.openlocfilehash: dc83d76dbfaded9dadae7c16ae9e1c8df7958a7d
ms.sourcegitcommit: d25e47a54a120b9bc26ff43597518fca58c1cc5b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73753821"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>Servicios de Escritorio remoto: Destinado a distintos tipos de usuarios

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Los Servicios de Escritorio remoto admiten distintos tipos de cargas de trabajo. Escala la implementación según la necesidad esperada de cada tipo de usuario.

## <a name="types-of-users"></a>Tipos de usuarios

### <a name="knowledge-user"></a>Usuario del conocimiento

Los usuarios del conocimiento usan aplicaciones de productividad ligeras como Microsoft Word, Excel, Outlook y el explorador Microsoft Edge.

En el caso de los usuarios de conocimientos, no recomendamos tener más de cuatro usuarios por CPU virtual (vCPU).

### <a name="professional-user"></a>Usuario profesional

Los usuarios profesionales usan aplicaciones de productividad y exploradores de Internet, además de admitir cargas de trabajo más intensivas, como desarrollar software y crear contenido multimedia.

Para los usuarios profesionales, no recomendamos tener más de dos usuarios por vCPU.

### <a name="power-user"></a>Usuario avanzado

Los usuarios avanzados usan aplicaciones gráficas y de ingeniería como el diseño asistido por PC (CAD) y Adobe Photoshop. Las GPU suelen ser una buena elección para los usuarios que usan programas con muchos gráficos para representar vídeo, diseño 3D y simulaciones.

Para los usuarios profesionales, no recomendamos tener más de un usuario por vCPU.

Para obtener más información sobre la aceleración de gráficos, consulta [Elección de la tecnología de representación de gráficos](rds-graphics-virtualization.md).

Azure tiene disponibles otras opciones de implementación de aceleración de gráficos y varios tamaños de máquina virtual de GPU. Obtén información sobre ellas en [Tamaños de máquinas virtuales optimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="test-workload"></a>Probar la carga de trabajo

Se recomienda cargar las implementaciones de prueba con las pruebas de esfuerzo y las simulaciones de uso real. Puedes usar herramientas de simulación para la prueba de carga de la implementación. Asegúrate de que el sistema responde y de que es lo suficientemente resistente como para satisfacer las necesidades del usuario, y recuerda cambiar el tamaño de la carga para evitar sorpresas.
