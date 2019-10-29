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
ms.date: 09/23/2016
manager: scottman
ms.openlocfilehash: c04909e9e0cfbf71b6632c154ac8b9b20b5bac10
ms.sourcegitcommit: b4b0e73ce35f8b594eb467a2bb2aa91bd6d86369
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72893077"
---
# <a name="remote-desktop-services---cater-to-different-kinds-of-users"></a>Servicios de Escritorio remoto: Destinado a distintos tipos de usuarios

>Se aplica a: Windows Server (Canal semianual), Windows Server 2019 y Windows Server 2016

Los Servicios de Escritorio remoto admiten distintos tipos de cargas de trabajo. Escala la implementación según la necesidad esperada de cada tipo de usuario.

## <a name="types-of-users"></a>Tipos de usuarios

### <a name="knowledge-user"></a>Usuario del conocimiento

Los usuarios del conocimiento usan aplicaciones de productividad ligeras como Microsoft Word, Excel, Outlook y el explorador Microsoft Edge.

### <a name="professional-user"></a>Usuario profesional

Los usuarios profesionales usan aplicaciones de productividad y exploradores de Internet, además de admitir cargas de trabajo más intensivas, como desarrollar software y crear contenido multimedia.

### <a name="power-user"></a>Usuario avanzado

Los usuarios avanzados usan aplicaciones gráficas y de ingeniería como el diseño asistido por PC (CAD) y Adobe Photoshop. Las GPU suelen ser una buena elección para los usuarios que usan programas con muchos gráficos para representar vídeo, diseño 3D y simulaciones.

Para obtener más información sobre la aceleración de gráficos, consulta [Elección de la tecnología de representación de gráficos](rds-graphics-virtualization.md).

Azure tiene disponibles otras opciones de implementación de aceleración de gráficos y varios tamaños de máquina virtual de GPU. Obtén información sobre ellas en [Tamaños de máquinas virtuales optimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

## <a name="test-workload"></a>Probar la carga de trabajo

Se recomienda cargar las implementaciones de prueba con las pruebas de esfuerzo y las simulaciones de uso real. Puedes usar herramientas de simulación, como LoginVSI, para la prueba de carga de la implementación. Asegúrate de que el sistema responde y de que es lo suficientemente resistente como para satisfacer las necesidades del usuario, y recuerda cambiar el tamaño de la carga para evitar sorpresas.
