---
title: Crear una propiedad de clasificación
description: En este artículo se describen las propiedades de clasificación, que se usan para asignar valores a archivos dentro de una carpeta o un volumen especificado.
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8a5d044ae78ad45b59fa4cb97694c15aad3c4bbe
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957502"
---
# <a name="create-a-classification-property"></a>Crear una propiedad de clasificación

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Las propiedades de clasificación se utilizan para asignar valores a archivos dentro de una carpeta o un volumen especificado. Hay muchos tipos de propiedades entre las que puede elegir, dependiendo de sus necesidades. En la tabla siguiente se definen los tipos de propiedad disponibles.

|Propiedad | Descripción |
| --- | --- |
| Sí/No | Una propiedad booleana que puede ser **sí** o **no**. Al combinar varios valores durante la clasificación o desde el contenido del archivo, un valor **no** se invalidará con un valor **sí** . |
| Fecha y hora | Propiedad de fecha y hora simple. Cuando se combinan varios valores durante la clasificación o desde el contenido del archivo, los valores en conflicto impedirán la reclasificación. |
| Number | Propiedad de número simple. Cuando se combinan varios valores durante la clasificación o desde el contenido del archivo, los valores en conflicto impedirán la reclasificación. |
| Lista ordenada | Una lista de valores fijos. Solo se puede asignar un valor a una propiedad cada vez. Al combinar varios valores durante la clasificación o desde el contenido del archivo, se usará el valor más alto de la lista. |
| String | Propiedad de cadena simple. Al combinar varios valores durante la clasificación o a partir de los valores conflictivos del contenido del archivo, impedirá la reclasificación. |
| Múltiples opciones | Una lista de valores que se pueden asignar a una propiedad. Se puede asignar más de un valor a una propiedad a la vez. Al combinar varios valores durante la clasificación o desde el contenido del archivo, se utilizará cada valor de la lista. |
| Cadena múltiple | Una lista de cadenas que se pueden asignar a una propiedad. Se puede asignar más de un valor a una propiedad a la vez. Al combinar varios valores durante la clasificación o desde el contenido del archivo, se utilizará cada valor de la lista. |

<br />

El siguiente procedimiento le guía por el proceso de creación de una propiedad de clasificación.

## <a name="to-create-a-classification-property"></a>Para crear una propiedad de clasificación

1.  En **Administración de clasificaciones**, haga clic en el nodo **propiedades de clasificación** .

2.  Haga clic con el botón secundario en **propiedades de clasificación**y, a continuación, haga clic en **crear propiedad** (o haga clic en **crear propiedad** en el panel **acciones** ). Se abrirá el cuadro de diálogo **definiciones de propiedad de clasificación** .

3.  En el cuadro de texto **nombre de propiedad** , escriba un nombre para la propiedad.

4.  En el cuadro de texto **Descripción** , agregue una descripción opcional para la propiedad.

5.  En el menú desplegable **tipo de propiedad** , seleccione un tipo de propiedad de la lista.

6.  Haga clic en **Aceptar**.

## <a name="additional-references"></a>Referencias adicionales

-   [Crear una regla de clasificación automática](create-automatic-classification-rule.md)
-   [Administración de clasificaciones](classification-management.md)