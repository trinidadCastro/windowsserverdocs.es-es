---
title: Configurar máquinas virtuales para usar SR-IOV solo cuando sea compatible con el sistema operativo invitado
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
ms.date: 8/16/2016
ms.openlocfilehash: 3a167b72f9c5c9a011980c07afe5a9a262aaa1be
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745690"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurar máquinas virtuales para usar SR-IOV solo cuando sea compatible con el sistema operativo invitado

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Advertencia|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Problema
*Una o varias máquinas virtuales están configuradas para usar la virtualización de e/s de raíz única (SR-IOV), pero el sistema operativo invitado no es compatible con SR-IOV.*

## <a name="impact"></a>Impacto
*Las funciones virtuales de SR-IOV no se asignarán a las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>Solución
*Deshabilite SR-IOV en todas las máquinas virtuales que ejecutan sistemas operativos invitados que no son compatibles con SR-IOV.*

SR-IOV solo se admite en algunos invitados de Windows de 64 bits. Para obtener más información, consulte compatibilidad de las [características de Hyper-V por generación e invitado](../Hyper-V-feature-compatibility-by-generation-and-guest.md).



