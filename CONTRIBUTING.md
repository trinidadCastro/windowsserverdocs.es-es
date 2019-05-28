# <a name="contributing-to-windows-server-technical-documentation"></a>Contribuir a la documentación técnica de Windows Server

Le agradecemos su interés en la documentación técnica de Windows Server. Agradecemos tus comentarios, modificaciones y adiciones a nuestros documentos. Hay dos ubicaciones independientes, que es donde guardamos el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs) mientras la otra es privada (windowsserverdocs-pr). Quién es usted determina qué ubicación que contribuye al:

- **No soy empleado de Microsoft.** Como un empleado que no sean de Microsoft, debe contribuir a la ubicación pública. Para obtener información acerca de cómo hacer esto, continúe leyendo este artículo.

- **Soy empleado de Microsoft.** Como empleado de Microsoft, dispone de opciones, según lo que está intentando hacer:

    - **Crear un nuevo artículo.** Para crear un nuevo artículo, debe crear y configurar su cuenta de GitHub y herramientas, bifurcar y clonar el repositorio windowsserverdocs-pr, configure la rama remota, crear el artículo y, por último, cree una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, consulte el [crear artículos nuevos de Windows Server mediante GitHub y Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md) artículo.

    - **Asegúrese de realizar grandes cambios en un artículo existente.** Para realizar cambios sustanciales en un artículo existente, puede seguir las instrucciones de la [editar un artículo existente de Windows Server mediante GitHub y Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md) artículo.

    - **Realizar cambios menores a un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones de la [actualizar artículos existentes de Windows Server mediante un explorador web y GitHub](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md) artículo.

## <a name="sign-a-cla"></a>Firmar un CLC

Todos los colaboradores que se encuentran ***no*** empleado de Microsoft debe [firmar un contrato de licencia de colaboración (CLA) de Microsoft](https://cla.microsoft.com/) antes de editar los repositorios de Microsoft. Si ya se ha editado en repositorios de Microsoft en el pasado, ¡enhorabuena!
Ya ha completado este paso.

## <a name="editing-topics"></a>Edición de temas

Hemos intentado facilitan la modificación de un archivo existente, público tan simple como sea posible.

### <a name="to-edit-a-topic"></a>Para editar un tema

1. Vaya a la página https://docs.microsoft.com/windows-server que desea actualizar y, a continuación, seleccione **editar**.

    ![Web de GitHub, que muestra el vínculo de edición](media/contribute-link.png)

2. Inicie sesión en (o suscribirse) una cuenta de GitHub.

    Debe tener una cuenta de GitHub para acceder a la página que le permite editar un tema.

3. Seleccione el **lápiz** icono (en el cuadro rojo) para editar el contenido.

    ![Web de GitHub, que muestra el icono de lápiz en el cuadro rojo](media/pencil-icon.png)

4. Mediante el lenguaje de marcado, realice los cambios en el tema. Para obtener información sobre cómo editar Markdown de contenido, consulte:

    - **Si está vinculado a la organización de Microsoft en GitHub:** [Guía del colaborador de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **Si es externo a Microsoft:** [Punto de dominar Markdown](https://guides.github.com/features/mastering-markdown/)

5. Realice el cambio sugerido y, a continuación, seleccione **vista previa de cambios** para asegurarse de que parece correcto.

    ![Web de GitHub, que muestra la pestaña de vista previa de cambios](media/preview-changes.png)

6. Cuando haya terminado el tema de edición, desplácese hasta el final de la página, escriba un nombre descriptivo para la bifurcación y, a continuación, haga clic en **proponer cambio de archivo** para crear la bifurcación en su cuenta personal de GitHub.

    ![Web de GitHub, que muestra el botón de cambio de archivo proponer](media/propose-file-change.png)

    El **comparar cambios** aparece en pantalla para ver cuáles son los cambios entre la bifurcación y el contenido original.

7. En el **comparar cambios** pantalla, podrá ver si hay algún problema con el archivo que está protegiendo.

    Si no hay ningún problema, verá el mensaje, **tienen la capacidad de combinar**.

    ![Web de GitHub, que muestra la comparación cambia la pantalla](media/compare-changes.png)

8. Seleccione **crear solicitud de incorporación de cambios**.

    Solicitudes de incorporación de cambios permiten indicar a otros usuarios sobre los cambios que ha insertado en una rama en un repositorio de GitHub. Después de abre una solicitud de incorporación de cambios, puede analizar y revisar los cambios potenciales con colaboradores y agregar seguimiento confirmaciones antes de que se combinan los cambios en la base de la sucursal. Para obtener más información, consulte [acerca de las solicitudes de incorporación de cambios](https://help.github.com/articles/about-pull-requests)

9. Escriba un título y una descripción para dar el aprobador el contexto adecuado sobre las novedades en la solicitud.

10. Desplácese hasta el final de la página, asegurándose de que solo los archivos modificados se encuentran en esta solicitud de incorporación de cambios. En caso contrario, podría sobrescribir los cambios de otras personas.

11. Seleccione **crear solicitud de incorporación de cambios** nuevo para enviar realmente la solicitud de incorporación de cambios.

    Se envía la solicitud de incorporación de cambios en el sistema de escritura del tema y se revisan las modificaciones. Si se aceptó la solicitud, se publican las actualizaciones.

## <a name="resources"></a>Recursos

- Puede usar el editor de texto para editar Markdown. Se recomienda [Visual Studio Code](https://code.visualstudio.com/), un editor ligero gratuito de código abierto de Microsoft.