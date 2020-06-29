---
title: Crear una regla de clasificación automática
description: En este artículo se describe cómo crear una regla de clasificación para una propiedad.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9fc2034905408975f82f9348f151d99df17f9d3a
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475402"
---
# <a name="create-an-automatic-classification-rule"></a>Crear una regla de clasificación automática

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El siguiente procedimiento le guía por el proceso de creación de una regla de clasificación. Cada regla define el valor de una sola propiedad. De forma predeterminada, una regla se ejecuta solo una vez y omite los archivos que ya tienen asignado un valor de propiedad. Sin embargo, puede configurar una regla para evaluar archivos independientemente de si un valor ya está asignado a la propiedad.

## <a name="to-create-a-classification-rule"></a>Para crear una regla de clasificación

1.  En **Administración de clasificaciones** haga clic en el nodo **Reglas de clasificación**.

2.  Haga clic con el botón secundario en **reglas de clasificación**y, a continuación, haga clic en **crear una nueva regla** (o haga clic en **crear una nueva regla** en el panel **acciones** ). Se abrirá el cuadro de diálogo **definiciones de regla de clasificación** .

3.  En la pestaña **configuración de regla** , escriba la siguiente información:

    -   **Nombre de la regla**. Escriba un nombre para la regla.
    -   **Habilitado**. Esta regla solo se aplicará si está activada la casilla habilitado. Para deshabilitar la regla, desactive esta casilla.
    -   **Descripción**. Escriba una descripción opcional de la regla.
    -   **Ámbito**. Haga clic en **Agregar** para seleccionar una ubicación a la que se aplicará esta regla. Puede agregar varias ubicaciones o quitar una ubicación haciendo clic en **quitar**. La regla de clasificación se aplicará a todas las carpetas y sus subcarpetas en esta lista.

4.  Escriba la información siguiente en la pestaña **Clasificación**:

    -   **Mecanismo de clasificación**. Elija un método para asignar el valor de propiedad.
    -   **Nombre**de la propiedad. Seleccione la propiedad que asignará esta regla.
    -   **Valor de propiedad**. Seleccione el valor de propiedad que asignará esta regla.

5.  Opcionalmente, haga clic en el botón **avanzadas** para seleccionar otras opciones. En la pestaña **tipo de evaluación** , la casilla para **volver a evaluar los archivos** está desactivada de forma predeterminada. Las opciones que se pueden seleccionar aquí son las siguientes:

    -   **Volver a evaluar archivos** sin activar: se aplica una regla a un archivo si, y solo si, la propiedad especificada por la regla no se ha establecido en ningún valor del archivo.
    -   **Vuelva a evaluar los archivos** activados y la opción **sobrescribir el valor existente** seleccionada: la regla se aplicará a los archivos cada vez que se ejecute el proceso de clasificación automática. Por ejemplo, si un archivo tiene una propiedad booleana que se establece en **sí**, una regla que use el clasificador de carpetas para establecer todos los archivos en **no** con este conjunto de opciones dejará la propiedad establecida en **no**.
    -   **Vuelva a evaluar los archivos** activados y la opción **Agregar valores** seleccionados: la regla se aplicará a los archivos cada vez que se ejecute el proceso de clasificación automática. Sin embargo, cuando la regla ha decidido qué valor se va a establecer en el archivo de propiedades, agrega ese valor con el que ya está en el archivo. Por ejemplo, si un archivo tiene una propiedad booleana que se establece en **sí**, una regla que use el clasificador de carpetas para establecer todos los archivos en **no** con este conjunto de opciones dejará la propiedad establecida en **sí**.

    En la pestaña **parámetros de clasificación adicionales** , puede especificar parámetros adicionales reconocidos por el método de clasificación seleccionado; para ello, escriba el nombre y el valor y haga clic en el botón **Insertar** .

6.  Haga clic en **Aceptar** o en **Cancelar** para cerrar el cuadro de diálogo **Opciones avanzadas** .

7.  Haga clic en **OK**.

## <a name="additional-references"></a>Referencias adicionales

-   [Crear una propiedad de clasificación](create-classification-property.md)
-   [Administración de clasificaciones](classification-management.md)