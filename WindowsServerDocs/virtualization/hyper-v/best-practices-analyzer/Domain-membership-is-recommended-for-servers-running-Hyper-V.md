---
title: Se recomienda la pertenencia al dominio para servidores que ejecutan Hyper-V
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d83ccf628e27595d5eb367791937b196fc708a6e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935487"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>Se recomienda la pertenencia al dominio para servidores que ejecutan Hyper-V

>Se aplica a: Windows Server 2016



*Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia

*Este servidor es miembro de un grupo de trabajo.*

## <a name="impact"></a>Impacto

*No hay ninguna administración central para este servidor.*

Unir este equipo al dominio permite la administración centralizada a través de directivas de identidad, seguridad y auditoría.

## <a name="resolution"></a>Resolución

*Si tiene un entorno de dominio disponible, únase a este servidor a ese dominio.*

> [!IMPORTANT]
> Se recomienda que revise las cargas de trabajo que se ejecutan en las máquinas virtuales de este equipo para determinar si hay implicaciones de seguridad de unir este equipo a un dominio. Si alguna de las máquinas virtuales son controladores de dominio virtualizados, consulte [consideraciones de planeación para controladores de dominio virtualizados](https://go.microsoft.com/fwlink/?LinkId=190192) ( https://go.microsoft.com/fwlink/?LinkId=190192) .

La Unión de un equipo a un dominio requiere permisos en el equipo y en el dominio:
- En el equipo, necesitará una cuenta de usuario que sea miembro del grupo administradores. Inicie sesión con este tipo de cuenta o proporcione el nombre de usuario y la contraseña de la cuenta cuando se le solicite.
- En el dominio, necesitará una cuenta de usuario que tenga autorización para unir el equipo al dominio. Se le pedirá el nombre de usuario y la contraseña.

Para obtener instrucciones, consulte [unir el equipo al dominio](https://go.microsoft.com/fwlink/?LinkId=190193) ( https://go.microsoft.com/fwlink/?LinkId=190193) .



