---
title: Crear una propiedad de clasificación
description: En este artículo se describen las propiedades de clasificación que se usan para asignar valores a los archivos de un volumen o carpeta especificados.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d330f896c71cced8e97701af2c1008b3531e065d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394215"
---
# <a name="create-a-classification-property"></a>Crear una propiedad de clasificación

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las propiedades de clasificación se usan para asignar valores a los archivos de un volumen o carpeta especificados. Existen muchos tipos de propiedad entre los que puedes elegir, según tus necesidades. En la tabla siguiente se definen los tipos de propiedad disponibles.

|Property | Descripción |
| --- | --- |
| Sí/No | Una propiedad booleana que puede ser **Sí** o **No**. Al combinar varios valores durante la clasificación o desde el contenido del archivo, un valor **No** reemplazará un valor **Sí**. |
| Fecha-hora | Una propiedad sencilla de fecha y hora. Al combinar varios valores durante la clasificación o desde el contenido del archivo, los valores en conflicto impedirán una nueva clasificación. |
| Número | Una propiedad sencilla numérica. Al combinar varios valores durante la clasificación o desde el contenido del archivo, los valores en conflicto impedirán una nueva clasificación. |
| Lista ordenada | Una lista de valores fijos. Solo se puede asignar un único valor a una propiedad a la vez. Al combinar varios valores durante la clasificación o desde el contenido del archivo, se usará el valor más alto de la lista. |
| Cadena | Una propiedad sencilla de cadena. Al combinar varios valores durante la clasificación o desde el contenido del archivo, los valores en conflicto impedirán una nueva clasificación. |
| Selección múltiple | Una lista de valores que se pueden asignar a una propiedad. Se puede asignar más de un valor a una propiedad a la vez. Al combinar varios valores durante la clasificación o desde el contenido del archivo, se usará cada valor de la lista. |
| Cadena múltiple | Una lista de cadenas que se pueden asignar a una propiedad. Se puede asignar más de un valor a una propiedad a la vez. Al combinar varios valores durante la clasificación o desde el contenido del archivo, se usará cada valor de la lista. |

<br />

El siguiente procedimiento te guiará por el proceso de crear una propiedad de clasificación.

## <a name="to-create-a-classification-property"></a>Para crear una propiedad de clasificación

1.  En **Administración de clasificaciones**, haz clic en el nodo **Propiedades de clasificación**.

2.  Haz clic con el botón derecho en **Propiedades de clasificación**y luego haz clic en **Crear propiedad** (o haz clic en **Crear propiedad** en el panel **Acciones**). Se abrirá el cuadro de diálogo **Definiciones de propiedad de clasificación**.

3.  En el cuadro de texto **Nombre de propiedad**, escribe un nombre para el informe.

4.  En el cuadro de texto **Descripción**, agrega una descripción opcional para la propiedad.

5.  En el menú desplegable **Tipo de propiedad**, selecciona un tipo de propiedad de la lista.

6.  Haga clic en **Aceptar**.

## <a name="see-also"></a>Vea también

-   [Crear una regla de clasificación automática](create-automatic-classification-rule.md)
-   [Administración de clasificaciones](classification-management.md)