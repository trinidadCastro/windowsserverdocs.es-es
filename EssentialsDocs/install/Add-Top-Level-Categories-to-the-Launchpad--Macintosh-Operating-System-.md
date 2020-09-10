---
title: Agregar categorías de nivel superior a LaunchPad (sistema operativo Macintosh)
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 8becc3159dc1f9374b95bbc90b51a44ae0f224e4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624028"
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

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)