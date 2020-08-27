---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planear la ubicación del controlador de dominio
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9c96537650f5c49aa25a75211e28430b6bcf5f34
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941045"
---
# <a name="planning-domain-controller-placement"></a>Planear la ubicación del controlador de dominio

> Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después de recopilar toda la información de red que se usará para diseñar la topología de sitio, Planee dónde desea colocar los controladores de dominio, incluidos los controladores de dominio raíz del bosque, los controladores de dominio regionales, los titulares de la función de maestro de operaciones y los servidores de catálogo global.

En Windows Server 2008, también puede aprovechar los controladores de dominio de solo lectura (RODC). Un RODC es un tipo de controlador de dominio nuevo que hospeda particiones de sólo lectura de la base de datos de Active Directory. A excepción de las contraseñas de cuenta, un RODC contiene todos los objetos Active Directory y atributos que contiene un controlador de dominio de escritura. Sin embargo, los cambios no se pueden realizar en la base de datos almacenada en el RODC. Los cambios se deben realizar en un controlador de dominio grabable y, a continuación, volver a replicarse en el RODC.

Un RODC está diseñado principalmente para implementarse en entornos remotos o de sucursales, que normalmente tienen relativamente pocos usuarios, una seguridad física deficiente, un ancho de banda de red relativamente pobre en un sitio concentrador y personal con conocimiento limitado de la tecnología de la información (TI). La implementación de RODC da como resultado una mejor seguridad y un acceso más eficaz a los recursos de red. Para obtener más información acerca de las características de RODC, consulte [AD DS: Controladores de dominio de solo lectura](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732801(v=ws.10)). Para obtener información sobre cómo implementar un RODC, consulte la [Guía paso a paso de controladores de dominio de solo lectura](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc772234(v=ws.10)) .

> [!NOTE]
> En esta guía no se explica cómo determinar el número adecuado de controladores de dominio y los requisitos de hardware del controlador de dominio para cada dominio que se representa en cada sitio.

## <a name="in-this-section"></a>En esta sección

- [Planear la ubicación del controlador de dominio de raíz del bosque](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)

- [Planeación de la ubicación del controlador de dominio regional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)

- [Planear la ubicación del servidor de catálogo global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)

- [Planear la ubicación del rol de maestro de operaciones](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)
