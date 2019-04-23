# <a name="rename-or-delete-an-article"></a>Cambiar el nombre o eliminar un artículo

Antes de cambiar el nombre o eliminar un artículo, deberá seguir este proceso para evitar o reducir el número de vínculos rotos. Los clientes odian vínculos rotos y no son poco tímidos sobre sonido off cuando alcance uno. Tenga en cuenta que el cambio de nombre o eliminar un artículo es el último paso en este proceso, no el primero.


## <a name="step-1-manage-inbound-links"></a>Paso 1: Administrar vínculos de entrada

Determine si hay cualquier vínculos de entrada que no sean de Microsoft a su contenido, como blogs, foros y otros contenidos en la web. Póngase en contacto con los propietarios del blog para cambiar estos vínculos y quite o actualice los vínculos de mensajes del foro. Herramientas de análisis Web pueden ayudar a identificar los vínculos de entrada de tráfico elevado es posible que deba administrar de esta manera.

## <a name="step-2-remove-all-crosslinks-to-the-article-from-the-windowsserverdocs-pr-repository"></a>Paso 2: Quitar todos los vínculos cruzados al artículo del repositorio WindowsServerDocs pr

1. Actualización local rama – ejecutar `git pull upstream <branch>`, Internet Explorer para actualizar desde el maestro, ejecute `git pull upstream master`

2.  Examinará la carpeta WindowsServerDocs pr para encontrar artículos que tienen un vínculo al artículo que desea cambiar el nombre o retirada, a continuación, actualice los artículos para quitar los vínculos o reemplazarlos con vínculos a otro artículo. Puede usar una búsqueda y reemplace la utilidad para buscar los vínculos cruzados si tiene uno instalado. Si no lo hace, puede usar Windows PowerShell:

 a. Inicie Windows PowerShell.

 b. En el símbolo del sistema de PowerShell, cambie a la carpeta WindowsServerDocs pr:

 `cd WindowsServerDocs-pr\WindowsServerDocs-pr`

 c. Ejecute un comando para enumerar todos los archivos que contienen un vínculo al artículo para cambiar el nombre o eliminar:

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  Para enviar la lista de nombres de archivo a un archivo de texto (en este caso, denominado psoutput.txt), ejecute:

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. Agregar y confirme todos los cambios, insértelos en la bifurcación y abrir una solicitud de incorporación de cambios. Para obtener instrucciones, consulte [comandos Git para crear o actualizar un artículo](git-steps-create-update-content.md).

## <a name="step-3-update-fwlinks"></a>Paso 3: Actualizar vínculos

Busque los vínculos que apuntan al artículo de la herramienta FWLink. Seleccione los vínculos al contenido de reemplazo; Si no está en el alias que posee el vínculo, únase a él. Si los propietarios no actualizarán el vínculo, presente una incidencia con MSCOM para que el vínculo cambiarlo. Más información: [wiki interno](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx).

## <a name="step-4-remove-crosslinks-to-the-article-from-table-of-contents"></a>Paso 4: Quitar vínculos cruzados al artículo de tabla de contenido

Trabajar con la persona que mantiene el archivo ToC.md. Este archivo rellena la tabla de contenido de la biblioteca técnica de la izquierda. Si no sabe quién ponerse en contacto, envíe un correo electrónico a wssc-pra@microsoft.com.

## <a name="step-5-add-redirects"></a>Paso 5: Agregar redirecciones
Si le está cambiando el nombre o eliminar un archivo, agregue una redirección de modo que no se rompan los vínculos existentes:

1. Deje el antiguo archivo en la ubicación existente con el nombre de archivo existente.
2. Reemplace el contenido del archivo con este fragmento de metadatos:
   ```
   ---
   redirect_url: <redirection-URL-or-file>
   ---
   ```
   \<redirección de URL o archivo > es la dirección URL completa a una ubicación diferente o es la ruta de acceso + nombre de archivo a un tema diferente en el mismo repositorio OPS.

   Por ejemplo, esto sería todo el archivo:

   ```
   ---
   redirect_url: ../../failover-clustering/whats-new-in-failover-clustering
   ---
   ```

3. Después de crear una solicitud para la redirección, haga clic en los vínculos en los comentarios para el RP - si el redireccionamiento funciona, deben obtener el tema de destino.

## <a name="step-6-rename-or-delete-the-article"></a>Paso 6: Cambiar el nombre o eliminar el artículo

Si no usa una redirección, realice este paso después de haber completado los pasos anteriores y se publican todos los artículos afectados. Si usas una redirección, el cambio de nombre o la eliminación del artículo podría deshacer esta operación y hacer que un vínculo roto. Para cambiar el nombre de un archivo, simplemente cambie el nombre del sistema de archivos, a continuación, agregar, confirmar e inserte el cambio y, a continuación, abra una solicitud de incorporación de cambios.
Para eliminar un archivo, primero debe saber que no funciona para simplemente eliminar un archivo desde el sistema de archivos porque se trata de un sistema de control de código fuente y debe saber que pretende hacer esto. En caso contrario, los archivos eliminados probablemente volverá a aparecer.
Hay dos maneras de hacerlo:

- Sistema de archivos y git: Elimine el archivo del sistema de archivos. A continuación, desde su herramienta git, ejecute uno de los siguientes  ```Git add -A``` | ```Git add --all``` | ```Git add -u```
- Git simplemente:   Ejecute ```git rm foo.md```:

    Obtener más información [ http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo ](http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo) y [https://git-scm.com/docs/git-rm](https://git-scm.com/docs/git-rm) 

## <a name="step-7-find-and-fix-straggler-broken-links"></a>Paso 7: Buscar y corregir los vínculos rotos straggler

Utilice la herramienta de control de calidad de contenido para buscar vínculos rotos que los pasos anteriores no catch y, a continuación, quitar o reparar los vínculos.

## <a name="step-8-remove-cached-pages-from-search-engines"></a>Paso 8: Quitar páginas en caché de los motores de búsqueda

Vaya a estas páginas web para quitar páginas web en caché de los motores de búsqueda: [Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>Vínculos de la Guía de colaboradores

- [Índice de artículos con instrucciones](./contributor-guide-index.md)

