---
title: Habilitación de la pancarta de detección de extensión
description: Habilitación de la pancarta de detección de extensión
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fafc16d6acae3c5a7a58a379d2f88998b8e98c3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811881"
---
# <a name="enabling-the-extension-discovery-banner"></a>Habilitación de la pancarta de detección de extensión

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Una nueva característica disponible en versión preliminar 1903 de Windows Admin Center es el titular de la detección de extensión. Esta característica permite que una extensión declarar el fabricante de hardware de servidor y es compatible con los modelos, y cuando un usuario se conecta a un servidor o clúster para el que está disponible una extensión, se mostrará un banner de notificación para instalar fácilmente la extensión. Los desarrolladores de extensiones podrán obtener visibilidad de sus extensiones y los usuarios podrán fácilmente detectar más capacidades de administración para sus servidores.

![Banner de detección de extensión](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Cómo funciona el banner de detección de extensión

Cuando se inicia Windows Admin Center, se conectará a las fuentes de extensión registrada y capturar los metadatos para los paquetes de extensión disponibles. A continuación, cuando un usuario se conecta a un servidor o clúster en Windows Admin Center, leemos el fabricante de hardware de servidor y el modelo que se muestra en la herramienta de información general. Si encontramos una extensión que declara que es compatible con el servidor actual fabricante o modelo, se mostrará un mensaje emergente para informar al usuario. Al hacer clic en el vínculo "Instalar ahora" llevará al usuario a Administrador de extensiones, donde puede instalar la extensión.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Cómo implementar el banner de detección de extensión

Los metadatos de "tags" en el archivo .nuspec se usan para declarar qué fabricante de hardware o los modelos de la compatibilidad de la extensión. Las etiquetas están delimitadas por espacios y puede agregar en un fabricante de etiquetas de modelo o ambos para declarar el fabricante compatible y/o los modelos. El formato de etiqueta es ``"[value type]_[value condition]"`` donde [tipo de valor] es "Manufacturer" o "Model" (distingue mayúsculas de minúsculas) y [valor condición] es un [expresión regular de Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) define el fabricante o la cadena de modelo y el [tipo de valor] y [ valor de la condición] están separados por un carácter de subrayado. Esta cadena, a continuación, se codifica mediante URI de codificación y agregan a la cadena de metadatos del archivo .nuspec "tags".

### <a name="example"></a>Ejemplo

Supongamos que he desarrollado una extensión que es compatible con servidores de una empresa llamada Contoso Inc., con el modelo de nombre R3xx y R4xx.

1. La etiqueta para el fabricante sería ``"Manufacturer_/Contoso Inc./"``. La etiqueta para los modelos podría ser ``"Model_/^R[34][0-9]{2}$/"``. Según cómo estrictamente que desee definir la condición de coincidencia, habrá diferentes formas para definir la expresión regular. También puede separar las etiquetas del fabricante o modelo a varias etiquetas, por ejemplo, la etiqueta del modelo también podría ser ``"Model_/R3../ Model_/R4../"``.
2. Puede probar la expresión regular con la consola de DevTools su explorador web. En Edge o Chrome, presione F12 para abrir la ventana DevTools y, en la pestaña de la consola, escriba el siguiente y presione ENTRAR:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Si escribe y ejecute lo siguiente, se devolverá 'true'.

   ```javascript
   regex.test('R300')
   ```

   Y si ejecuta lo siguiente, se devolverá 'false'.

   ```javascript
   regex.test('R500')
   ```

3. Una vez que haya comprobado la expresión regular, puede codificarlo en la consola de DevTools, utilizando el método de Javascript siguiente:

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   El formato de la cadena de etiqueta para agregar al archivo .nuspec final sería:

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Somos conscientes de que un fabricante de hardware puede tener una gran variedad de nombres del modelo de los cuales algunos pueden admitirse aunque algunos no lo son. Tenga en cuenta que esta característica está pensada para ayudar con la **detección** de su extensión, pero no tiene que ser un inventario actualizado perfectamente de todos los modelos. Puede definir la expresión regular para que sea una expresión más sencilla que coincide con un subconjunto de los modelos. Un usuario que no vea el banner de detección si primero se conectan a un modelo de servidor que no coincide con la condición, pero tarde o temprano, se conectarán a otro servidor que y detectar e instalar la extensión. También puede definir una expresión regular simple que solo coincide con el nombre del fabricante. En algunos casos, la extensión podría no admitir un modelo específico, pero puede usar el [característica de presentación de herramientas dinámica](./dynamic-tool-display.md) para definir un script de PowerShell para comprobar la compatibilidad con el modelo y mostrar solo la extensión cuando sea aplicable, o proporcionan una funcionalidad limitada en la extensión para los modelos que no son compatibles con todas las funcionalidades.