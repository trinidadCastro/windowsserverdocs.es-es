---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planear la ubicación del controlador de dominio
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 68406d4973dd585bf0a98562c987c1b1512c095c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883366"
---
# <a name="planning-domain-controller-placement"></a>Planear la ubicación del controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Después que reunir toda la información de red que se usará para diseñar la topología de sitio, planee donde desea colocar los controladores de dominio, incluidos los controladores de dominio raíz del bosque, los controladores de dominio regional, los titulares de función de maestro de operaciones, y servidores de catálogo global.  
  
En Windows Server 2008, también se puede sacar partido de los controladores de dominio de solo lectura (RODC). Un RODC es un nuevo tipo de controlador de dominio que hospeda particiones de sólo lectura de la base de datos de Active Directory. Excepto por las contraseñas de cuentas, un RODC dispone de todos los objetos de Active Directory y los atributos que contiene un controlador de dominio de escritura. Sin embargo, no se pueden realizar cambios a la base de datos que se almacena en el RODC. Los cambios deben ser realizados en un controlador de dominio de escritura y, a continuación, vuelven a replicar en el RODC.  
  
Un RODC está diseñado principalmente para implementarse de oficinas remotas o entornos de sucursales, que normalmente tienen relativamente pocos usuarios, seguridad física deficiente, ancho de banda relativamente bajo para un sitio concentrador y el personal con conocimiento limitado de información tecnología (TI). Implementación de los resultados de los RODC en mejorar la seguridad y acceso más eficaz a los recursos de red. Para obtener más información acerca de las características RODC, consulte AD DS: Los controladores de dominio de solo lectura ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Para obtener información sobre cómo implementar un RODC, vea la Guía paso a paso para controladores de dominio de sólo lectura ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Esta guía no explica cómo determinar el número apropiado de controladores de dominio y los requisitos de hardware del controlador de dominio para cada dominio que se representa en cada sitio.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Planear la ubicación del controlador de dominio de raíz de bosque](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planear la ubicación del controlador de dominio Regional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planear la ubicación del servidor de catálogo Global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planear la ubicación de rol de maestro de operaciones](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


