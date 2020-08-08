---
title: Habilitación del banner de detección de extensión
description: Habilitación del banner de detección de extensión
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: ef08eec08b43f83121bc94abc46a5f657556db65
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945027"
---
# <a name="enabling-the-extension-discovery-banner"></a>Habilitación del banner de detección de extensión

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

La característica de banner de detección de extensión se presentó en la versión Preview 1903 del centro de administración de Windows. Esta característica permite a una extensión declarar el fabricante del hardware de servidor y los modelos que admite, y cuando un usuario se conecta a un servidor o clúster para el que hay una extensión disponible, se muestra un banner de notificación para instalar la extensión fácilmente. Los desarrolladores de extensiones podrán obtener más visibilidad de sus extensiones y los usuarios podrán detectar fácilmente más capacidades de administración para sus servidores.

![Banner de detección de extensión](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Cómo funciona el banner de detección de extensiones

Cuando se inicia el centro de administración de Windows, se conectará a las fuentes de extensión registradas y capturará los metadatos de los paquetes de extensión disponibles. Después, cuando un usuario se conecta a un servidor o clúster en el centro de administración de Windows, se lee el modelo y el fabricante de hardware del servidor para mostrarlo en la herramienta de información general. Si encontramos una extensión que declara que es compatible con el fabricante o el modelo del servidor actual, mostraremos un banner para que el usuario lo sepa. Al hacer clic en el vínculo "configurar ahora", el usuario pasará al administrador de extensiones, donde podrá instalar la extensión.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Cómo implementar el banner de detección de extensión

Los metadatos de "Tags" del archivo. nuspec se usan para declarar qué fabricante de hardware o modelos admite su extensión. Las etiquetas están delimitadas por espacios y puede Agregar un fabricante o una etiqueta de modelo, o ambos, para declarar el fabricante o los modelos admitidos. El formato de etiqueta es ``"[value type]_[value condition]"`` donde [tipo de valor] es "manufacturer" o "Model" (distingue mayúsculas de minúsculas) y [valor Condition] es una [expresión regular de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Regular_Expressions) que define el fabricante o la cadena de modelo, y [tipo de valor] y [condición de valor] están separados por un carácter de subrayado. A continuación, esta cadena se codifica mediante la codificación de URI y se agrega a la cadena de metadatos "Tags" de. nuspec.

### <a name="example"></a>Ejemplo

Supongamos que he desarrollado una extensión que admite servidores de una empresa denominada contoso Inc., con el nombre de modelo R3xx y R4xx.

1. La etiqueta del fabricante sería ``"Manufacturer_/Contoso Inc./"`` . La etiqueta de los modelos podría ser ``"Model_/^R[34][0-9]{2}$/"`` . Dependiendo de cómo desee definir la condición de coincidencia, habrá diferentes maneras de definir la expresión regular. También puede separar las etiquetas de fabricante o modelo en varias etiquetas; por ejemplo, la etiqueta del modelo también podría ser ``"Model_/R3../ Model_/R4../"`` .
2. Puede probar la expresión regular con la consola de DevTools del explorador Web. En Edge o Chrome, presione F12 para abrir la ventana de DevTools y, en la pestaña consola, escriba lo siguiente y presione ENTRAR:

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Después, si escribe y ejecuta lo siguiente, devolverá ' true '.

   ```javascript
   regex.test('R300')
   ```

   Y si ejecuta lo siguiente, devolverá ' false '.

   ```javascript
   regex.test('R500')
   ```

3. Una vez que haya comprobado la expresión regular, puede codificarla también en la consola de DevTools con el siguiente método de JavaScript:

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   El formato final de la cadena de etiqueta que se va a agregar al archivo. nuspec sería:

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Sabemos que un fabricante de hardware puede tener una amplia gama de nombres de modelo de los que algunos se pueden admitir, mientras que otros no. Tenga en cuenta que esta característica está pensada para ayudar con la **detección** de la extensión, pero no tiene que ser un inventario absolutamente actualizado de todos los modelos. Puede definir la expresión regular para que sea una expresión más sencilla que coincida con un subconjunto de los modelos. Es posible que un usuario no vea el banner de detección si se conecta por primera vez a un modelo de servidor que no coincide con la condición, pero antes o después se conectará a otro servidor que lo haga y detectará e instalará la extensión. También puede considerar la posibilidad de definir una expresión regular simple que solo coincida con el nombre del fabricante. En algunos casos, es posible que la extensión no admita realmente un modelo específico, pero puede usar la característica de visualización de la [herramienta dinámica](./dynamic-tool-display.md) para definir un script de PowerShell personalizado para comprobar la compatibilidad con modelos y mostrar solo la extensión cuando sea aplicable, o bien proporcionar una funcionalidad limitada en la extensión para los modelos que no admiten todas las funcionalidades.