---
title: Agregar categorías de nivel superior a LaunchPad (sistema operativo Macintosh)
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c8a15a831a89afc55d20db4e1c1195173d466b3c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817518"
---
# <a name="add-top-level-categories-to-the-launchpad-macintosh-operating-system"></a>Agregar categorías de nivel superior a LaunchPad (sistema operativo Macintosh)

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Se pueden agregar categorías de nivel superior al Launchpad en equipos que ejecuten el sistema operativo Macintosh. Para crear un complemento de Launchpad que agregue categorías de nivel superior, puede usar una combinación de información de esta página y del tema titulado Cómo: agregar tareas y categorías al Launchpad. en el [SDK de soluciones de Windows Server](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
 En el siguiente ejemplo se muestra cómo especificar que una entrada de Launchpad sea una categoría de nivel superior en el archivo .launchpad:  
  
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
  
 Para que la entrada sea una categoría de nivel superior, el atributo Id del elemento Categoría debe ser "Microsoft.Launchpad.HomeCategory".  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)