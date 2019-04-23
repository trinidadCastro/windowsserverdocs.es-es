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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879566"
---
# <a name="metadata-and-markdown-template"></a>Metadatos y plantilla Markdown

Esta plantilla OPS contiene ejemplos de sintaxis de Markdown, así como instrucciones sobre cómo establecer los metadatos. Para obtener el máximo provecho de él, debe ver el [Markdown sin formato](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D) y [vista representada](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md). (El Markdown sin formato muestra el bloque de metadatos, mientras que la vista representada no).

Al crear un archivo de Markdown, debe copiar la plantilla en un archivo nuevo, rellenar los metadatos como se indica a continuación, Establece el encabezado H1 anterior para el título del artículo y eliminar el contenido. Cualquier cosa en mayúsculas en los corchetes que requiere su atención.


## <a name="metadata"></a>Metadatos 

El bloque de metadatos completo está por encima. Algunas notas importantes:

- Le **debe** tiene un espacio entre los dos puntos (:) y el valor de un elemento de metadatos.
- Dos puntos en un valor (por ejemplo, un título) interrumpen el analizador de metadatos. En su lugar, utilice la codificación HTML de dos puntos de `&#58;` (por ejemplo, `"title: Azure Rights Management&#58; the basics | Azure RMS"`).
- **title**: Este título aparecerá en los resultados del motor de búsqueda. 
- **Autor**: El campo autor debe contener el **nombre de usuario de GitHub** del autor, no su alias.
- **ms.prod**, **ms.technology**: Use "windows-server-threshold" ms.prod (o con 10 si usa esta plantilla para crear contenido para Windows 10). Hable con su contacto CX para obtener el valor de ms.technology.

## <a name="basic-markdown-gfm-and-special-characters"></a>Markdown básico, GFM y caracteres especiales

Es compatible con todos los Markdown básicos y característico de GitHub. Para obtener más información, consulte:

- [Sintaxis de Markdown de línea de base](https://daringfireball.net/projects/markdown/syntax)
- [Documentación de Markdown (GFM) característico de GitHub](https://guides.github.com/features/mastering-markdown)

Markdown utiliza caracteres especiales como \*, \`, y \# para dar formato. Si desea incluir uno de estos caracteres en el contenido, debe hacer dos cosas:

- Escriba una barra diagonal inversa delante del carácter especial "escapar" (por ejemplo, \\ \* para un \*)
- Use la [código de entidad HTML](http://www.ascii.cl/htmlcodes.htm) para el carácter (por ejemplo, \& \#42\; para un &#42;).

## <a name="headings"></a>Encabezados

Los encabezados deben realizarse con estilo de atx, es decir, usar caracteres de 1 a 6 hash (#) al principio de la línea para indicar un título, correspondientes a los niveles de encabezados HTML H1 a H6. Ejemplos de encabezados de primer y segundo nivel se utilizan anteriormente. 

Hay **debe** ser solo un encabezado de primer nivel (H1) en el tema, que se mostrará como título en la página.  

Los encabezados de segundo nivel generarán la tabla de contenido en la página que aparece en la sección "en este artículo" debajo del título en la página.

### <a name="third-level-heading"></a>Encabezado de tercer nivel
#### <a name="fourth-level-heading"></a>Encabezado de cuarto nivel
##### <a name="fifth-level-heading"></a>Encabezado de quinto nivel
###### <a name="sixth-level-heading"></a>Encabezado de sexto nivel

## <a name="text-styling"></a>Estilo del texto

*Cursiva* 

**Negrita** 

~~Tachado~~

## <a name="links"></a>Vínculos

### <a name="internal-links"></a>Vínculos internos

Para vincular a un encabezado en el mismo archivo de Markdown, vea el origen del artículo publicado, busque el identificador del encabezado (por ejemplo, `id="blockquote"`) y vincúlelo con # + identificador (por ejemplo, `#blockquote`).

- Por ejemplo: [Blockquotes](#blockquote)

Para vincular a un archivo de Markdown en el mismo repositorio, utilice [vínculos relativos](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2), incluyendo el ".md" al final del nombre de archivo.

- Por ejemplo: [Sugerencias y trampas](tips-gotchas.md)
- Por ejemplo: [Herramientas e instalación para los colaboradores](../readme.md)

Para vincular a un encabezado en un archivo de Markdown en el mismo repositorio, utilice la vinculación relativa + vinculación hashtag.

- Por ejemplo: [Eliminación de archivos](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>Vínculos externos

Para vincular a un archivo externo, use la dirección URL completa como vínculo.

- Por ejemplo: [GitHub](http://www.github.com)

Si aparece una dirección URL en un archivo Markdown, se transformará en un vínculo.

- Ejemplo: http://www.github.com

## <a name="lists"></a>Listas

### <a name="ordered-lists"></a>Listas ordenadas

1. Este 
1. Is
1. Una
1. Ordenada
1. Lista  


#### <a name="ordered-list-with-an-embedded-list"></a>Lista ordenada con una lista insertada

1. Aquí
1. incluye
1. Una
1. embedded
    1. Error de Scarlett
    1. Profesor Plum
1. Ordenada
1. list


### <a name="unordered-lists"></a>Listas sin ordenar

- Este
- estará
- a
- con viñetas
- list


##### <a name="unordered-list-with-an-embedded-list"></a>Lista desordenada con una lista insertada

- Este 
- con viñetas 
- list
    - Pavo real Sra.
    - El Sr. verde
- Contiene  
- otro
    1. Tanto mostaza
    1. Notas de MRS.
- Listas


## <a name="horizontal-rule"></a>Regla horizontal

---

## <a name="tables"></a>Tablas

En casi todas las instancias, usar MD formato de tablas. Mientras que las tablas HTML proporcionan más flexibilidad no lo usamos en nuestro contenido. Si tiene una tabla HTML en el artículo, no se combinará ese artículo.

| Tablas        | Son           | Niveles de acceso esporádico  |
| ------------- |:-------------:| -----:|
| la col. 3 está      | alineado a la derecha | $1600 |
| la col. 2 está      | centrado      |   12 $ |
| la col. 1 es el valor predeterminado | alineado a la izquierda     |    $1 |


## <a name="code"></a>Código

### <a name="generic-codeblock"></a>Codeblock genérico

Aplicar sangría a los espacios de cuatro del código para la codificación de codeblock genérico.

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks con el identificador de idioma

Utilice tres acentos graves (&#96;&#96;&#96;) + un identificador de idioma para aplicar colores específica del lenguaje de codificación para un bloque de código.  Esta es la lista completa de [identificadores de idioma de GitHub Flavored Markdown (GFM)](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs).

##### <a name="c9839"></a>C&AMP;#9839;

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

Use acentos graves (&#96;) para `inline code`.

## <a name="blockquotes"></a>Blockquotes

> La sequía había durado ya diez millones de años, y el reinado de los terribles saurios tiempo ha que había terminado. Aquí en el Ecuador, en el continente que podría ser conocido un día como África, la batalla por la existencia había alcanzado un nuevo clímax de ferocidad, y el victor aún no estaba a la vista. En este terreno estéril y desecado, sólo el pequeño raudo o lo feroz podría medrar, o aún esperar sobrevivir.

## <a name="images"></a>Imágenes

### <a name="static-image"></a>Imagen estática

![Este es el texto alt](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>Imagen vinculada

[![texto alternativo para la imagen vinculada](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

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

