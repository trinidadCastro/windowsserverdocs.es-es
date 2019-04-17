---
title: Habilitar el banner de detección de extensión
description: Habilitar el banner de detección de extensión
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/08/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 389acba6d1fe6f65320bd780c9fde6469b387e0b
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "9263010"
---
# Habilitar el banner de detección de extensión #

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Una nueva característica disponible en Windows Admin Center Preview 1903 es el banner de detección de extensión. Esta característica permite que una extensión declarar el fabricante del hardware de servidor y es compatible con los modelos y, cuando un usuario se conecta a un servidor o clúster para el que está disponible una extensión, se mostrará un banner de notificación para instalar fácilmente la extensión. Los desarrolladores de extensión podrán obtener visibilidad más de sus extensiones y los usuarios podrán fácilmente detectar más funcionalidades de administración para sus servidores.

![Banner de detección de extensión](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## Cómo funciona el banner de detección de extensión ##

Cuando se inicia Windows Admin Center, tendrá conectarse a las fuentes de extensión registrada y recuperar los metadatos de los paquetes de extensión disponibles. A continuación, cuando un usuario se conecta a un servidor o clúster en Windows Admin Center, leemos el fabricante del hardware de servidor y el modelo para mostrar en la herramienta de información general. Si encontramos una extensión que declara que admite el servidor actual fabricante y modelo, se mostrará un banner para informar al usuario. Al hacer clic en el vínculo "Configurar ahora" llevará al usuario a Administrador de extensiones, donde pueden instalar la extensión.

## Cómo implementar el banner de detección de extensión ##

Los metadatos de "etiquetas" en el archivo .nuspec se usan para declarar el fabricante del hardware o modelos admite la extensión. Las etiquetas están delimitadas por espacios y se pueden agregar en un fabricante de etiquetas de modelo o ambos para declarar el fabricante compatible o modelos. El formato de etiqueta es ``"[value type]_[value condition]"`` donde [tipo de valor] es "Fabricante" o "Modelo" (distingue mayúsculas de minúsculas) y [condición de valor] es una [coincidencia de expresión regular Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) define el fabricante o la cadena de modelo y el [tipo de valor] y [valor condición] son separados por un carácter de subrayado. Esta cadena, a continuación, se codifica mediante URI codificación y se agregan a la cadena de metadatos .nuspec "etiquetas".

### Ejemplo ###

Supongamos que he desarrollado una extensión que es compatible con los servidores de una empresa llamada Contoso Inc., con el modelo de nombre R3xx y R4xx.

1. La etiqueta para que el fabricante sería ``"Manufacturer_/Contoso Inc./"``. Podría ser la etiqueta de los modelos de ``"Model_/^R[34][0-9]{2}$/"``. Dependiendo de cómo estrictamente que quieras definir la condición coincidente, habrá diferentes maneras de definir la expresión regular. También puede separar las etiquetas fabricante o el modelo en varias etiquetas, por ejemplo, también podría ser la etiqueta de modelo ``"Model_/R3../ Model_/R4../"``.
2. Puedes probar la expresión regular con la consola de DevTools del explorador web. En el borde o Chrome, F12 para abrir la ventana DevTools de visitas y en la pestaña de la consola, escriba la ENTRAR posicionamiento y los siguiente:

```javascript
var regex = /^R[34][0-9]{2}$/
```

A continuación, si escribes y ejecuta lo siguiente, se volverá a 'true'.

```javascript
regex.test('R300')
```

Y si ejecuta lo siguiente, se devolverá 'false'.

```javascript
regex.test('R500')
```

3. Una vez que hayas comprobado la expresión regular, se puede codificar en la consola de DevTools también, mediante el siguiente método de Javascript:

```javascript
encodeURI(/^R[34][0-9]{2}$/)
```

El formato final de la cadena de etiqueta para agregar a tu archivo .nuspec sería:

```
<tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
```

> [!Tip]
> Sabemos que un fabricante de hardware puede tener un rango muy amplio de nombres del modelo de los cuales algunos pueden admitirse mientras que otras no lo son. Ten en cuenta que esta característica está pensada para ayudar con la **detección** de la extensión, pero no tienen que ser un inventario actualizado perfectamente de todos los modelos. Puedes definir la expresión regular para que sea una expresión más sencilla que coincida con un subconjunto de los modelos. Un usuario puede que no veas el banner de detección si primero se conecta a un modelo de servidor que no coincide con la condición, pero antes o después que se conectan a otro servidor que y detectar e instala la extensión. También puede definir una expresión regular simple que solo coincida con el nombre del fabricante. En algunos casos, la extensión podría no admitir un modelo específico, pero puedes usar la [herramienta dinámica muestra la característica](./dynamic-tool-display.md) para definir un script de PowerShell personalizado para comprobar la compatibilidad con el modelo y mostrar solo la extensión cuando sea aplicable o proporcionar limitado funcionalidad de la extensión para los modelos que no admiten todas las funcionalidades.