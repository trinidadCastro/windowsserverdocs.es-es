---
title:
- TÍTULO DEL ARTÍCULO
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS
ms.date:
- DATE
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: ''
ms.localizationpriority:
- high/medium/low
ms.openlocfilehash: 4f885680426c0bfa55d5f73a7ef0c2143a8dd5a9
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082637"
---
# <a name="metadata-and-markdown-template"></a>Metadatos y la plantilla de descuento

Esta plantilla OPS contiene ejemplos de sintaxis de descuento, así como instrucciones sobre el establecimiento de los metadatos. Para obtener el máximo provecho de él, debe ver el [descuento sin procesar](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) y la [procesa la vista](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (El descuento sin procesar muestra el bloque de metadatos, mientras que la vista representada no).

Al crear un archivo de descuento, debe copiar la plantilla en un archivo nuevo, rellene los metadatos como conjunto especificado a continuación, el encabezado H1 anteriormente para el título del artículo y eliminar el contenido. Cualquier cosa en mayúsculas en corchetes requiere su atención.


## <a name="metadata"></a>Metadatos 

El bloque de metadatos completo está por encima. Algunas notas claves:

- **Debe** tener un espacio entre los dos puntos (:) y el valor de un elemento de metadatos.
- Dos puntos en un valor (por ejemplo, un título) interrumpen el analizador de metadatos. En su lugar, use la codificación HTML para un punto y coma de `&#58;` (por ejemplo, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **título**: este título aparecerá en los resultados del motor de búsqueda. 
- **autor**: el campo autor debe contener el **nombre de usuario de depósito** del autor, no sus alias.
- **ms.prod**, **ms.technology**: usar "umbral de servidor de windows" de ms.prod (o con 10 si usa esta plantilla para crear contenido para Windows 10). Hablar con su contacto CX para obtener el valor de ms.technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Descuento básico, GFM y caracteres especiales

Es compatible con todos los descuentos básica y con sabor depósito. Para obtener más información, vea:

- [Sintaxis de descuento de línea base](https://daringfireball.net/projects/markdown/syntax)
- [Documentación de descuento (GFM) con sabor depósito](https://guides.github.com/features/mastering-markdown)

Descuento usa caracteres especiales, como \ *, \', y \ # para aplicar el formato. Si desea incluir uno de los siguientes caracteres en el contenido, debe hacer dos cosas:

- Colocar una barra diagonal inversa antes de que "escape" el carácter especial (por ejemplo, \\\ * para un \ *)
- Use el [código de la entidad HTML](http://www.ascii.cl/htmlcodes.htm) para el carácter (por ejemplo, \ & \#42\; para un & #42;).

## <a name="headings"></a>Encabezados

Los encabezados deben realizarse con el estilo de atx, es decir, utilice 1-6 caracteres de hash (#) al principio de la línea para indicar un encabezado, correspondiente a los niveles de los encabezados HTML H1 a H6. Ejemplos de encabezados de primer y segundo nivel se usan por encima. 

Existe **debe** ser sólo un título de primer nivel (H1) en el tema, que se mostrará como el título en la página.  

Títulos de segundo nivel, generarán la tabla de contenido en la página que aparece en la sección "de este artículo" debajo del título en la página.

### <a name="third-level-heading"></a>Encabezado de tercer nivel
#### <a name="fourth-level-heading"></a>Título del cuarto nivel
##### <a name="fifth-level-heading"></a>Título de nivel quinto
###### <a name="sixth-level-heading"></a>Título del sexto nivel

## <a name="text-styling"></a>Estilo de texto

*Cursiva* 

**Negrita** 

~~Tachado~~

## <a name="links"></a>Vínculos

### <a name="internal-links"></a>Vínculos internos

Para vincular un encabezado en el mismo archivo de descuento, ver el origen del artículo publicado, busque el identificador de la cabeza (por ejemplo, `id="blockquote"`) y vincular con # + identificador (por ejemplo, `#blockquote`).

- Ejemplo: [Blockquotes](#blockquote)

Para vincular un archivo de descuento en el mismo repo, use [vínculos relativos](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), incluida la ".md" al final del nombre de archivo.

- Ejemplo: [sugerencias y problemas comunes](tips-gotchas.md)
- Ejemplo: [el programa de instalación para los colaboradores y herramientas](../readme.md)

Para vincular un encabezado en un archivo de descuento en el mismo repo, utilice vinculación relativa + hashtag vinculación.

- Ejemplo: [Eliminar archivos](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Vínculos externos

Para vincular a un archivo externo, utilice la dirección URL completa como el vínculo.

- Ejemplo: [depósito](http://www.github.com)

Si aparece una dirección URL en un archivo de descuento, se van a transformar en un vínculo.

- Ejemplo:http://www.github.com

## <a name="lists"></a>Listas

### <a name="ordered-lists"></a>Listas ordenadas

1. Esto 
1. Es
1. Un
1. Ordenados
1. Lista  


#### <a name="ordered-list-with-an-embedded-list"></a>Lista con una lista incrustada ordenada

1. Aquí
1. viene
1. un
1. incrustado
    1. Error de Scarlett
    1. Ciruela profesor
1. ordenados
1. list


### <a name="unordered-lists"></a>Listas no ordenadas

- Esto
-  esté 
- a
- con viñetas
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Lista no ordenada con una lista incrustada

- Esto 
- con viñetas 
- list
    - Pavo real Sra.
    - Verde de d.
- contiene  
- otros
    1. Tanto mostaza
    1. Notas del Sra.
- listas


## <a name="horizontal-rule"></a>Regla horizontal

---

## <a name="tables"></a>Tablas

En casi todas las instancias, usar MD en el formato de las tablas. Mientras que las tablas HTML proporcionan más flexibilidad no se use en nuestro contenido. Si tiene una tabla HTML en su artículo, no se va a combinar ese artículo.

| Tablas        | Son           | Luz fluorescente  |
| ------------- |:-------------:| -----:|
| col 3 es      | alineada a la derecha | $1600 |
| Col 2 es      | centrado      |   12$ |
| Col 1 es el valor predeterminado | alineado a la izquierda     |    $1 |


## <a name="code"></a>Código

### <a name="generic-codeblock"></a>Codeblock genérica

Aplicar sangría a espacios de código cuatro para codificación codeblock genérica.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks con el identificador de idioma

Usar tres backticks (& #96; & #96; & #96;) + codificación un bloque de código de un identificador de idioma que se debe aplicar el color específicos del idioma.  Aquí está toda la lista de [identificadores de idioma de depósito con sabor descuento (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C & #9839;

```c#
using System;
namespace HelloWorld
{
    class Hello 
    {
        static void Main() 
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```
#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```
#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="inline-code"></a>Código en línea

Utilice backticks (& #96;) para `inline code`.

## <a name="blockquotes"></a>Blockquotes

> La sequía tenía duró ahora para millones de diez años, y el Reino de la lagartos terribles ya tenía finalizó. Aquí en el Ecuador, en el continente que sería un día conocido como África, la batalla existencia había alcanzado un nuevo climax de ferocity y el victor aún no estaba en la vista. En este land barren y desecado, solo la pequeña o swift o la feroz podría florido, o incluso esperamos sobrevivir.

## <a name="images"></a>Imágenes

### <a name="static-image"></a>Imagen estática

![Este es el texto alternativo](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Imagen vinculada

[![alt texto para la imagen vinculada](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>Alertas

### <a name="note"></a>Nota

> [!NOTE]
> Se trata de nota

### <a name="warning"></a>Advertencia

> [!WARNING]
> Esto es de advertencia

### <a name="tip"></a>Sugerencia

> [!TIP]
> Se trata de sugerencia

### <a name="important"></a>Importante

> [!IMPORTANT]
> Esto es importante

