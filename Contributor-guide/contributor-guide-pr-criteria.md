# <a name="quality-criteria-for-pull-request-review"></a>Revisión los criterios de calidad para la solicitud de incorporación de cambios

Las solicitudes de incorporación de cambios deben cumplir los criterios mínimos sean aceptados para la integración en el repositorio. Los revisores revisión generalmente sólo lo que es nuevo o modificado, mediante el bloqueo y sin bloqueo elementos de revisión de calidad indicados en este artículo para evaluar los cambios. Para obtener más información o para un solicitante corregir problemas antes de aceptar una solicitud de incorporación de cambios, pueden solicitar los revisores. El solicitante es responsable de realizar los cambios.

>[!IMPORTANT] 
>Abrir una solicitud de incorporación de cambios desencadena las comprobaciones de calidad y publicación en nuestro sitio de ensayo interna. Es su responsabilidad revisar ambos, de vínculos en comentarios \(en la pestaña de la conversación de la incorporación de cambios, haga clic en **mostrar todas las comprobaciones de** > **detalles**\). Una vez hecho esto, se indican mediante la adición de la **"ready-to-merge"** etiqueta para los PR. \(Haga clic en **etiquetas** o el icono de engranaje a la derecha de la secuencia de comentarios en el PR.) Si no se puede agregar la etiqueta \(vienen colaboradores informaron esto), agregue "ready-to-merge" un comentario a la solicitud y enviar correo electrónico al alias del revisor, wssc-pra@microsoft.com.

## <a name="blocking-content-quality-items"></a>Bloqueo de los elementos de la calidad del contenido

Las actualizaciones deben cumplir los siguientes criterios para que se va a combinar.

| Category | Elemento de la revisión de calidad |
|----------|---------------------|
|Requisitos previos| La solicitud de extracción no tiene ningún:<br>-errores de validación o advertencias<br>-los conflictos de combinación<br>-obvias regresiones de contenido<br>-incrustado repositorio ni archivos extraños no habituales|
|Integridad del repositorio |Solicitud de incorporación de cambios contiene menos de 100 archivos modificados a menos que se está actualizando una rama de lanzamiento de patrón o realizar una actualización masiva similares como una corrección de los metadatos. (En realidad, una solicitud debe contener muchos menos, pero después de 100 archivos modificados, GitHub no muestra las diferencias).|
|Integridad del repositorio| Solicitud de extracción no combinar la persona que creó la solicitud de incorporación de cambios.|
|nomenclatura |Nombres de archivo para los nuevos archivos siguen la [directrices de nomenclatura de archivos](file-names-and-locations.md).|
|nomenclatura |Siguen las nuevas carpetas introducidas en el repositorio el [carpeta directrices de nomenclatura](file-names-and-locations.md#folder-names-in-the-repo).|
|Nomenclatura/SEO|El título H1 tiene información suficiente para describir el contenido del artículo, para diferenciarlo de otros artículos y para asignar a las palabras clave de cliente probable. Por ejemplo, un título H1 de "Introducción" no cumple este criterio.|
|Contenido|Es un documento técnico. Consulte la [destino de las instrucciones](content-channel-guidance.md).|
|Contenido|Tiene un párrafo de introducción y un cuerpo procedimental y conceptual del contenido. Tiene suficiente contenido para depender de sí mismas como un artículo y no es un pequeño fragmento de información.|
|Contenido| Sigue un formato estándar y el diseño del contenido: los elementos que deben constituir listas numeradas están numerados, los elementos que deben ser sin ordenar listas con viñetas. Esto es una utilidad básica.|
|Contenido| No tiene ninguna gráficos inusuales, arquitectura de la información o las estructuras o diseños obviamente no estándar.|
|Sitio/funcionalidad de diseño| El artículo no tiene una tabla de contenido creado manualmente dentro del contenido. El artículo debe basarse en H2s para su tabla de contenido en la página.|
|Sitio/funcionalidad de diseño| Si se usan encabezados H2, hay al menos dos. Los encabezados H3 van precedidos de un encabezado H2. Uso de un encabezado H2, crea un artículo de elemento único TDC. Use un encabezados H2 antes un título H3 para garantizar la que creación de una tabla de contenido.|
|Markdown| Artículo no contiene ningún bloques de HTML; se permite la menor insertado HTML:, como superíndice, subíndice, caracteres especiales y otros formatos menores que no es compatible con Markdown. Las tablas HTML se admiten únicamente si la tabla contiene con viñetas o listas numeradas, pero normalmente indica que el contenido podría simplificarse para que lo pueda codificarse en Markdown.|
|Markdown   |Los elementos markdown personalizados se usan en su caso. P. ej.: Las notas se codifican mediante el. Tenga en cuenta la extensión, no como texto sin formato.|
|Imágenes|Las imágenes incluyen texto alternativo. **Esto es un requisito de accesibilidad que se debe cumplir como parte de los criterios de lanzamiento.** [Instrucciones detalladas aquí](https://worldready.cloudapp.net/Styleguide/Read?id=2665&topicid=28349). |
|Imágenes |Archivos GIF animados no se aceptan en el repositorio.|
|Imágenes | Las imágenes tienen una resolución clara, son palabras mal escritas y no contienen ninguna información privada [consulte las instrucciones de la imagen](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md). |
|Preconfiguración| El artículo ha sido una vista previa en almacenamiento provisional y no contiene cualquier error obvio formato:<br>-Una lista numerada o con viñetas que aparece como un párrafo. <br> -Código de un bloque de código que aparece parcialmente en el bloque de código y parcialmente fuera de él <br>– Pasos numerados incorrectamente debido a una sangría errónea.|

## <a name="non-blocking-content-quality-items"></a>Sin bloqueo de contenido de elementos de calidad

Para estos elementos, los revisores de solicitudes de incorporación de cambios proporcionan comentarios e instrucciones para el autor realice un seguimiento con correcciones en una solicitud de incorporación de cambios posterior. Sin embargo, estos comentarios no bloquean la decisión de combinar. Los autores deben realizar un seguimiento durante 3 días laborables correcciones.

| Category | Elemento de la revisión de calidad |
|----------|---------------------|
|Contenido|Los artículos deben incorporar "pasos siguientes" al final con los pasos siguientes pertinentes e interesantes de 1 a 3. Se debe incluir un texto breve que permite a los clientes a comprender por qué los pasos siguientes son relevantes. (Solo para nuevos artículos).
|Imágenes|Las imágenes que usan el estilo de llamada correcta y el color y capturas de pantalla usan el estilo de borde y el marcador de posición correcto. [Consulte las instrucciones de la imagen](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Sitio/funcionalidad de diseño|Los encabezados H2, cuando se representan en la tabla de contenido en la página, lo ideal es que se deben ajustar a líneas de no más de 2. Los encabezados más dificultar el artículo de tabla de contenido Examinar.|
|Convenciones de estilo|Todos los títulos y encabezados son oración.|
|Proceso|Al eliminar o cambiar el nombre de un artículo, asegúrese de que siga el proceso. Los revisores deben agregar el comentario y el vínculo siguiente en un comentario de solicitud de incorporación de cambios:<br><br>*Compruebe que ha seguido el proceso en la Guía del colaborador: [Cambiar el nombre de un artículo o eliminarlo](rename-or-retire.md).*|
