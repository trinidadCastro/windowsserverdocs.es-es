---
title: "Agregar categorías de nivel superior al LaunchPad (sistema de operativo Macintosh)"
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ae4eb5943d37b4a9d3b554af28cb425420782cf8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Agregar categorías de nivel superior al LaunchPad (sistema de operativo Macintosh)

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Puedes agregar categorías de nivel superior al LaunchPad en un equipo que ejecute el sistema operativo Macintosh. ¿Para crear un complemento de Launchpad que agregue categorías de nivel superior, puedes usar una combinación de información de esta página y desde el tema de procedimientos: agregar tareas y categorías al LaunchPad? en la [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 El siguiente ejemplo muestra cómo se puede especificar la entrada de Launchpad para que sea una categoría de nivel superior en el archivo .launchpad:  
  
```  
  
<?xml version="1.0" encoding="utf-8" ?>  
<LaunchPad resFile="Localizable">  
   <Category id="Microsoft.Launchpad.HomeCategory" name="SampleAddin"  image="SampleImage01.png">  
      <Task id="Microsoft.Launchpad.SampleAddin.Pictures"   
          name="Pictures"       
           src="smb://%ServerAddress%/Pictures"   
         image="SampleImage02.png"/>  
   </Category>  
</LaunchPad>  
```  
  
 Para que la entrada como una categoría de nivel superior, el atributo Id del elemento categoría debe ser "Microsoft.Launchpad.HomeCategory".  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)