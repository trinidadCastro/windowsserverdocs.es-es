<properties title="" pageTitle="Los nombres de archivo y ubicaciones para los artículos técnicos de Windows Server 2016" description="Explica la estructura de archivos en los artículos y las convenciones de nomenclatura que debe seguir al crear un nuevo artículo." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy-Davies" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="jimpark; tysonn" />

# <a name="file-names-and-locations-for-windows-server-technical-articles"></a>Los nombres de archivo y ubicaciones para los artículos técnicos de Windows Server

Conocer y siguiendo las reglas le ayuda a obtener la solicitud de incorporación de cambios aceptada con mayor rapidez.

+ [reglas]
+ [Pattern]
+ [Aprobación de nombres de archivo]

## <a name="rules"></a>Reglas

- Todos los archivos deben estar en markdown y utilizar la extensión .md.
- Conservar los nombres de archivo a las 3 a 5 palabras si es posible: 10 palabras realmente es demasiado largo para mejorar la legibilidad y SEO.
- Utilice minúsculas y solo letras, números y guiones.
- Sin espacios ni caracteres de puntuación. Use un guión para separar las palabras y números en el nombre de archivo.
- Usar la forma abreviada de verbos de acción, no el '-ing "formulario: `deploy-nano-server` no `Deploying-Nano-Server`
- Omita palabras cortas, como un y, el, en, o.
- Spell palabras evitar los acrónimos innecesarios o no aprobados en los nombres de archivo
- Los nombres de archivo deben ser únicos: en lugar de `overview.md` usar `storage-spaces-overview.md`

Acrónimos y siglas en nombres de archivo - directrices específicas:

- Siga las instrucciones de Microsoft existente de abreviaturas de nombres aceptable
- Estándar del sector abreviaturas son aceptables según sea necesario en los nombres de archivo.

## <a name="pattern"></a>Patrón

Revise la lista de artículos en el repositorio para hacerse una idea de los nombres existentes. Este es el patrón general:

 **component-topic-title.md**
 
Por ejemplo, `storage-spaces-direct-overview.md`

## <a name="file-name-approval"></a>Aprobación de nombres de archivo

Cuando se envía un nuevo archivo, los revisores revisión el nombre y proporcionan comentarios a través de la secuencia de comentarios de solicitud de incorporación de cambios si es necesario realizar cambios. El nombre de archivo debe corregirse antes de que se acepta la solicitud de incorporación de cambios. Los colaboradores pueden enviar fácilmente la actualización a la solicitud de incorporación de cambios pendiente.

## <a name="folders-in-the-repo"></a>Carpetas en el repositorio

Use la estructura de carpetas existente. No se crean las carpetas sin obtener la aprobación del Administrador de repositorio. Si cree que necesita una nueva carpeta, hablas con ellos.

El repositorio de GitHub debe haber un sencillo y relativamente planos estructura de carpetas: \<área >\\\<tecnología >: para la desduplicación storage\data de ejemplo. Al hacerlo, tiene las siguientes ventajas:
 - Es muy sencillo.
 - Es más de cerca a cómo funciona Azure.com
 - Facilita a los escritores/PMs no encontrar rápidamente sus temas – necesidad de que bucear en torno a otras carpetas buscando donde se anida una tecnología específica.
 - Mantiene la URL corta, que es adecuada para SEO y experiencia del usuario, en ocasiones, los clientes buscar en la dirección URL para las pilas.

## <a name="changing-case-in-file-names"></a>Cambiar mayúsculas y minúsculas en nombres de archivo

Los sistemas operativos Windows distinguen mayúsculas de minúsculas. Si necesita cambiar el nombre de archivo para corregir las mayúsculas y minúsculas, es mejor realizar un cambio en el contenido del archivo, a menos que esté en un Linux o Mac. Por ejemplo:

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services--> biztalk-services-administration-and-development-task-list

Use el `git mv` comando para cambiar el nombre de un archivo:
```
  git mv <WindowsServerDocs/tech-area/subarea/current-file-name.md> <WindowsServerDocs/tech-area/subarea/new-file-name.md>
```

### <a name="contributors-guide-links"></a>Vínculos de la Guía de colaboradores

- [Índice de artículos con instrucciones](./contributor-guide-index.md)


<!--Anchors-->
[reglas]: #rules
[Pattern]: #pattern
[Aprobación de nombres de archivo]: #file-name-approval
