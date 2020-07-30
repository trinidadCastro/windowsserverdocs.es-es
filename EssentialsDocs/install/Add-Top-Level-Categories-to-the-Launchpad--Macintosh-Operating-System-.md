---
title: Agregar categorías de nivel superior a LaunchPad (sistema operativo Macintosh)
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: ee2173c3-e464-4001-9f43-6d926a575092
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 5ac16ca5c1b4e01364dc0fa0bfcb5e8dd0c5e54e
ms.sourcegitcommit: d99bc78524f1ca287b3e8fc06dba3c915a6e7a24
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2020
ms.locfileid: "87181561"
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