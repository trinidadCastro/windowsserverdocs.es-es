# <a name="contributing-to-windows-server-technical-documentation"></a>Contribución a la documentación técnica de Windows Server

Gracias por su interés en la documentación técnica de Windows Server. Agradecemos sus comentarios, ediciones y adiciones en nuestros documentos. Hay dos ubicaciones independientes donde se conserva el contenido técnico de Windows Server. Una de las ubicaciones es pública (windowsserverdocs), mientras que la otra es privada (windowsserverdocs-PR). A quién está determinando qué ubicación contribuye:

- **No soy un empleado de Microsoft.** Como empleado que no es de Microsoft, debe contribuir a la ubicación pública. Para obtener información sobre cómo hacerlo, siga leyendo este artículo.

- **Soy empleado de Microsoft.** Como empleado de Microsoft, tiene opciones, en función de lo que está intentando hacer:

    - **Cree un nuevo artículo de marca.** Para crear un nuevo artículo de marca, debe crear y configurar la cuenta y las herramientas de GitHub, bifurcar y clonar el repositorio windowsserverdocs-PR, configurar la rama remota, crear el artículo y, por último, crear una nueva solicitud de incorporación de cambios para su aprobación y publicación. Para estas instrucciones, consulte el artículo [creación de nuevos artículos de Windows Server con GitHub y Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md) .

    - **Realizar cambios importantes en un artículo existente.** Para realizar cambios importantes en un artículo existente, puede seguir las instrucciones del artículo [edición de un artículo existente de Windows Server con GitHub y Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md) .

    - **Realizar pequeños cambios en un artículo existente.** Para realizar cambios menores en un artículo existente, puede seguir las instrucciones del artículo [actualización de artículos existentes de Windows Server mediante un explorador Web y GitHub](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md) .

## <a name="sign-a-cla"></a>Firmar un CLC

Todos los colaboradores que *_no_* son de un empleado de Microsoft deben [firmar un contrato de licencia de colaboración (CLA) de Microsoft antes de](https://cla.microsoft.com/) editar los repositorios de Microsoft. Si ya ha editado en repositorios de Microsoft en el pasado, ¡ enhorabuena!
Ya ha completado este paso.

## <a name="editing-topics"></a>Temas de edición

Hemos intentado editar un archivo público existente lo más sencillo posible.

### <a name="to-edit-a-topic"></a>Para editar un tema

1. Vaya a la página en la https://docs.microsoft.com/windows-server que desea actualizar y seleccione _ * editar * *.

    ![Web de GitHub, donde se muestra el vínculo de edición](media/contribute-link.png)

2. Inicie sesión en (o regístrese en) una cuenta de GitHub.

    Debe tener una cuenta de GitHub para acceder a la página que le permite editar un tema.

3. Seleccione el icono de **lápiz** (en el cuadro rojo) para editar el contenido.

    ![Web de GitHub, donde se muestra el icono de lápiz en el cuadro rojo](media/pencil-icon.png)

4. Con el lenguaje Markdown, realice los cambios en el tema. Para obtener información sobre cómo editar contenido con Markdown, consulte:

    - **Si está vinculado a la organización de Microsoft en github:** [Guía para colaboradores de Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **Si es externo a Microsoft:** [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)

5. Realice el cambio sugerido y, a continuación, seleccione **vista previa de los cambios** para asegurarse de que es correcto.

    ![Web de GitHub, en el que se muestra la pestaña vista previa de cambios](media/preview-changes.png)

6. Cuando haya terminado de editar el tema, desplácese hasta la parte inferior de la página, escriba un nombre descriptivo para la bifurcación y, a continuación, haga clic en **proponer cambio de archivo** para crear la bifurcación en su cuenta personal de github.

    ![Web de GitHub, donde se muestra el botón proponer el cambio de archivo](media/propose-file-change.png)

    Aparece la pantalla **comparar cambios** para ver cuáles son los cambios entre la bifurcación y el contenido original.

7. En la pantalla **comparar cambios** , verá si hay algún problema con el archivo que está protegiendo.

    Si no hay ningún problema, verá el mensaje, **capaz de fusionar mediante combinación**.

    ![Web de GitHub, en el que se muestra la pantalla comparar cambios](media/compare-changes.png)

8. Seleccione **Crear solicitud de incorporación de cambios**.

    Las solicitudes de incorporación de cambios permiten indicar a otros usuarios sobre los cambios que se han insertado en una rama en un repositorio de GitHub. Después de abrir una solicitud de incorporación de cambios, puede discutir y revisar los posibles cambios con colaboradores y agregar confirmaciones de seguimiento antes de que los cambios se combinen en la rama base. Para obtener más información, consulte [acerca de las solicitudes de incorporación](https://help.github.com/articles/about-pull-requests) de cambios.

9. Escriba un título y una descripción para dar al aprobador el contexto adecuado sobre lo que hay en la solicitud.

10. Desplácese hasta la parte inferior de la página y asegúrese de que solo los archivos modificados se encuentran en esta solicitud de incorporación de cambios. De lo contrario, podría sobrescribir los cambios de otras personas.

11. Seleccione **crear solicitud de incorporación** de cambios para enviar realmente la solicitud de incorporación de cambios.

    La solicitud de incorporación de cambios se envía al escritor del tema y se revisan las ediciones. Si se acepta la solicitud, se publican las actualizaciones.

## <a name="resources"></a>Recursos

- Puede usar su editor de texto favorito para editar el Markdown; se recomienda [Visual Studio Code](https://code.visualstudio.com/), un editor ligero gratuito de código abierto de Microsoft.
