---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: "Planeación de la ubicación del controlador de dominio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c6d9ca064699f95390e5b95863a6871ea2dc9d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="planning-domain-controller-placement"></a>Planeación de la ubicación del controlador de dominio

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Una vez ha recopilado toda la información de red que se usará para diseñar la topología de sitios, planear donde quieras colocar los controladores de dominio, incluidos los controladores de dominio raíz del bosque, controladores de dominio regional, de maestro de operaciones titulares de las funciones y los servidores de catálogo global.  
  
En Windows Server 2008, también puede sacar provecho de los controladores de dominio de solo lectura (RODC). Un RODC es un nuevo tipo de controlador de dominio que hospeda las particiones de solo lectura de la base de datos de Active Directory. Excepto por las contraseñas de cuenta, un RODC dispone de todos los objetos de Active Directory y atributos que contiene un controlador de dominio grabable. Sin embargo, no se pueden realizar cambios a la base de datos que se almacena en el RODC. Deben ser realizados en un controlador de dominio de escritura y vuelven a replicar el RODC cambios.  
  
Un RODC está diseñado principalmente para implementarse en el control remoto o entornos sucursales, que normalmente tienen relativamente pocos usuarios, deficiente seguridad física, el ancho de banda de red relativamente mala a un sitio de concentrador y personal con un conocimiento limitado de tecnología de información (TI). Implementar RODC resultados en seguridad mejorada y más eficiente acceso a recursos de red. Para obtener más información acerca de las características RODC, consulta el tema de AD DS: Read-Only controladores de dominio ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616)). Para obtener información sobre cómo implementar un RODC, vea la Step-by-Step Guide for Read-Only controladores de dominio ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728)).  
  
> [!NOTE]  
> Esta guía no explica cómo determinar el número apropiado de los controladores de dominio y los requisitos de hardware del controlador de dominio para cada dominio que se representa en cada sitio.  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Planeación de la ubicación del controlador de dominio de raíz de bosque](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [Planeación de la ubicación del controlador de dominio Regional](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [Planeación de la ubicación de servidor de catálogo Global](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [Planeación de asignación de rol de maestro de operaciones](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


