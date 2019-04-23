# <a name="tips-and-gotchas"></a>Sugerencias y del problema

Si está familiarizado con Git, aprender a trabajar de manera eficaz puede resultar frustrante. Información se recopila aquí, por lo que es más fácil de encontrar que si ha incluido dentro de la otra información en esta guía.

## <a name="basics"></a>Conceptos básicos:

Comandos en la Guía del este colaborador dan por hecho ha configurado Github para especificar el repositorio predeterminado donde se extraen los archivos de. En Github es terminología, donde obtener archivos desde su *ascendente*. Donde insertar los archivos es su *origen*. 

Debido en cómo se diseñan nuestro flujo de trabajo y repositorio, el nivel superior debe establecerse en nuestro repositorio, que se encuentra en la organización de Microsoft - https://github.com/Microsoft/WindowsServerDocs-pr y su origen debe ser la bifurcación de este repositorio, que se encuentra en su propia cuenta de Github. Por ejemplo, https://github.com/MY-GITHUB-ALIAS/WindowsServerDocs-pr 

>Para comprobar la configuración, escriba ```git config -l```. Examine las direcciones URL para asegurarse de que hacen referencia a la ubicación deseada.

Algunas sugerencias para la rama básica:

-  Trabajar en sucursales, no en la copia local del patrón. Esto facilita para recuperarse de problemas en las ramas y vuelva a un buen punto conocido.

-  Basar sus ramas locales en una rama del repositorio compartido, no en la bifurcación remota. A continuación, insertará las ramas locales en la bifurcación remota. Desde la bifurcación remota, deberá solicitar que las actualizaciones en la bifurcación que se combinan en el repositorio compartido. Los comandos en este artículo muestran cómo hacerlo.

-  Cuando se crea una bifurcación local, el comando indica a Git qué rama que desea basar la nueva rama. Esto es cuándo y cómo especificar patrón o una rama de hito o característica. Por ejemplo, este comando crea una nueva bifurcación de una rama ascendente y, a continuación, retira la nueva rama, por lo que está listo para trabajar en ella:

        git checkout upstream/upstream-branch-name -b your-local-branch-name

Para obtener ayuda de markdown, comenzar con este [artículo](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) en Github. Para las tablas, consulte este [uno](https://help.github.com/articles/organizing-information-with-tables/).

## <a name="gotchas"></a>Del problema

### <a name="deleting-files"></a>Eliminación de archivos
No funciona para eliminar un archivo de su sistema de archivos. Utilice los comandos de Git para que el sistema sepa pretenden hacer esto, en caso contrario, los archivos eliminados probablemente se convertirá en archivos inerte que vuelven a aparecer. Y nunca en un buen momento. Después de todo, se trata de un sistema de control de código fuente y si no indica lo que realmente desea deshacerse de estos archivos, ofrece ayuda agregará nuevo para usted. Gracias, Git. Para obtener instrucciones, consulte [cambiar el nombre o retirada de artículos](rename-r-retire.md).

### <a name="merge-conflicts"></a>Conflictos de combinación

Si se produce un conflicto de combinación al trabajar de forma local, o bien deba corregirlo o realizar una copia fuera de ella. La mayoría de las veces es mejor simplemente corregirlos, a menos que no sean los archivos. Tenga en cuenta si no hace nada, podría estar bloqueado de insertar los cambios en su origen, por lo que tendrá que hacer algo.

**Cómo corregir** : Guía de Azure tiene más información sobre este y las instrucciones para resolver el conflicto. **Nunca se debe aceptar un PR con conflictos de combinación**. No olvide sustituir el nombre de nuestro repositorio y sucursales en todos los ejemplos para revisar el [instrucciones](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Resolve%20a%20local%20merge%20conflict%20yourself.aspx). 

**Cómo revertir** : si los conflictos se encuentran en los archivos que no es propietario o hay una razón no puede corregirlos en el momento, aquí a revertir. Esto restablece el conjunto de archivos local a cómo estaban antes de que se produjo el conflicto de combinación. Ejecuta este comando:

```git merge --abort```

Si aún no ha almacenado provisionalmente y confirma su trabajo local, puede hacerlo ahora. Sin embargo, si aplicar esos cambios y abrir una solicitud, el conflicto probablemente volverá a aparecer si estaba en los archivos con cambios. Debe corregirla u obtener ayuda de otra persona para corregirlo, antes de que se puede aceptar la solicitud. Por este motivo, es mejor usar sólo el método para desbloquearse a sí mismo si tiene trabajo urgente, como una actualización crítica.

