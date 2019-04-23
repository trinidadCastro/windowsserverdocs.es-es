<properties pageTitle="Comandos de GIT para crear un nuevo artículo o actualizar un artículo existente" description="Pasos para crear y actualizar un artículo en WindowsServerDocs-pr." metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy Davies" videoId="" scriptId="" manager="dongill" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="08/24/16" ms.author="kathydav" />

# <a name="git-commands-to-create-or-update-an-article"></a>Comandos de GIT para crear o actualizar un artículo

>! NOTA: Estos comandos dan por hecho ha configurado Github para especificar el repositorio predeterminado donde se extraen los archivos de. En Github es terminología, donde obtener archivos desde su *ascendente*. Donde insertar los archivos es su *origen*. Según cómo se diseñan nuestro flujo de trabajo y repositorio, el nivel superior debe establecerse en nuestro repositorio, que se encuentra en la organización de Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr y su origen debe ser la bifurcación de este repositorio, bajo su propia cuenta de Github. Por ejemplo, es de Kathy https://github.com/KBDAzure/WindowsServerDocs-pr 

>Para comprobar la configuración, escriba ```git config -l```. Examine las direcciones URL para asegurarse de que hacen referencia a la ubicación deseada.

## <a name="add-or-update-an-article"></a>Agregar o actualizar un artículo

Aquí es cómo crear una bifurcación local, guarde los cambios y, a continuación, inserta en la bifurcación remota.

1. Inicie Git Bash (o la herramienta de línea de comandos que se usa para Git).

2. Cambie a WindowsServerDocs pr:

        cd WindowsServerDocs-pr

3. Es mejor mantener el servidor maestro local rama y el maestro remoto rama en la bifurcación en sincronización con el patrón del repositorio rama. Esto puede ayudar a ahorrar una gran cantidad de frustración y pierde tiempo--es más probable que la detección anticipada de problemas y mantener las cosas en un estado de funcionamiento conocidos, buena. Ejecute:

        git checkout master
        git pull upstream master
        git push origin master

4. Ahora está listo para crear una rama para realizar su diaria o entrega basada funciona. Es la mejor forma de crear una rama de trabajo local de una rama de versión se utiliza para ejecutar:

        git checkout upstream/upstream-branch-name -b your-local-branch-name

   Esto crea la rama local directamente desde la rama ascendente y le ayuda a evitar mezclar archivos incorrectos en la nueva rama local. Por ejemplo, para crear una rama de trabajo en función de la rama de umbral de ga, podría ejecutar un comando similar al siguiente:
      
        git checkout upstream/master -b working-11-18

   Si está trabajando en el contenido que debe combinarse en la rama que no es         

5. Agregue la rama de trabajo local en la bifurcación:

        git push origin <working branch>

6. Cree el nuevo artículo o realizar cambios en un artículo existente. Utilice el Explorador de Windows para abrir los archivos de markdown y markdown editor para crear y editar archivos. Para obtener Ayuda básica del formato, vea este [artículo](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) en Github.

7. Agregar metadatos necesarios y la información de versión, según [control de versiones de producto y los metadatos](metadata-OSversioning-and-trademarks.md).

8. Comprobar el estado, agregue y confirme los cambios:

        git status

   Revise los resultados para asegurarse de que los archivos enumerados son los que se ha cambiado. Si todos los archivos parecen correctos, ejecute este comando para agregar todos los archivos:

        git add .
        git commit –m "<comment>"

   Para agregar solo los archivos específicos (por ejemplo, si ```git status``` se incluyen archivos que no desea enviar), en su lugar, debe ejecutar:

        git add <file path>
        git commit –m "<comment>"

   >[!IMPORTANT]
   >El comando ```git add .``` agrega todos los cambios pendientes notificados por ```git status```. Esto significa que si ```git status``` muestra sin seguimiento de las actualizaciones que no desea agregar, use ```git add <file path>``` en su lugar.  

9. Actualizar la rama de trabajo local con los cambios del nivel superior:

        git pull upstream master

10. Inserte los cambios en la bifurcación en GitHub:

        git push origin <working branch>

## <a name="submit-your-changes"></a>Enviar los cambios

Cuando esté listo para enviar el contenido de almacenamiento provisional, validación o publicación, use la UI GitHub para crear una solicitud de incorporación de cambios. 

Al abrir una solicitud de incorporación de cambios (PR), esto desencadena un paso de prueba, compila el proyecto y publica en nuestro sitio de ensayo interna. No pasa nada abrir una solicitud de incorporación de cambios que no está listo para que se han combinado porque se trata de cómo obtener un paso de prueba y comprobar las actualizaciones en el sitio de ensayo. Detalles de la compilación y los vínculos preconfigurados se registran como comentarios en el PR. 

Es su responsabilidad hacer lo siguiente **antes de que pregunte para que los cambios que se combinan**:
  - Revisar detalles para asegurarse de que no contiene errores de la compilación. 
  - Revise las actualizaciones en el sitio de ensayo.

Una vez hecho esto, puede indicar por alguno de ellos:
- Agregar la etiqueta "ready-to-merge" para los PR. \(Haga clic en **etiquetas** o el icono de engranaje a la derecha de la secuencia de comentarios en el PR.)
- Agregar ready-to-merge como un comentario y enviar correo electrónico al alias WSSC extraer revisores: wssc pra

El revisor comprueba los cambios y acepta la solicitud a menos que haya problemas o preguntas. Solicitudes para solucionar problemas o comentarios se agregan como comentarios. Revise [criterios de calidad de incorporación de cambios solicitar revisión](contributor-guide-pr-criteria.md) para obtener información sobre lo que se espera.

## <a name="publishing"></a>Publishing

- Se publican artículos en aproximadamente 10:00 a. M. y las 3:00 P.M. hora del Pacífico, de lunes a viernes. Tenga en cuenta que un revisor de la solicitud de incorporación de cambios necesita tiempo para revisar y aceptar los cambios antes de que pueden combinan. Los cambios se deben combinar para se recogerán en el siguiente ciclo de publicación programado. Si necesita un artículo publicado en un ciclo de publicación específica, permiten que un revisor saber de antemano. Cuando se acepta su solicitud, los cambios se combinan en el repositorio y se incluirá en la siguiente publicación de tiempo que se ejecute. Puede tardar hasta 30 minutos para que los artículos aparecen en línea después de la publicación. 
