---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Advanced AD DS Management Using Active Directory Administrative Center (Level 200)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc2aaa9f7c7c42b6e94995ff473a580ce560ed93
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820006"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Advanced AD DS Management Using Active Directory Administrative Center (Level 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe con detalle el Centro de administración de Active Directory actualizado, con la nueva papelera de reciclaje de Active Directory, la directiva de contraseña específica y el Visor del historial de Windows PowerShell, e incluye la arquitectura, ejemplos de tareas comunes e información sobre solución de problemas. Para obtener una introducción, consulte [Introducción a Active Directory Administrative Center Enhancements &#40;nivel 100&#41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
- [Arquitectura de centro de administración de Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
- [Habilitación y administración de Active Directory Papelera de reciclaje con el centro de administración de Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
- [Configuración y administración de directivas de contraseña específica mediante el centro de administración de Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
- [Usar el Visor del historial de PowerShell de Active Directory Administrative Center Windows](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
- [Solución de problemas de administración de AD DS](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Arquitectura de centro de administración de Active Directory  
  
### <a name="active-directory-administrative-center-executables-dlls"></a>Active Directory Administrative Center ejecutables, DLL  

Los módulos y la arquitectura subyacente del Centro de administración de Active Directory no han cambiado con las nuevas funcionalidades de papelera de reciclaje, FGPP y visor del historial.  
  
- Microsoft.ActiveDirectory.Management.UI.dll  
- Microsoft.ActiveDirectory.Management.UI.resources.dll  
- Microsoft.ActiveDirectory.Management.dll  
- Microsoft.ActiveDirectory.Management.resources.dll  
- ActiveDirectoryPowerShellResources.dll  
  
A continuación se ilustran la capa de operaciones y de Windows PowerShell subyacentes de la nueva funcionalidad de papelera de reciclaje:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>Habilitación y administración de Active Directory Papelera de reciclaje con el centro de administración de Active Directory  
  
### <a name="capabilities"></a>Funcionalidades  
  
- El Windows Server 2012 o versiones más recientes centro de administración de Active Directory le permite configurar y administrar la Papelera de reciclaje de Active Directory para cualquier partición de dominio en un bosque. Ya no es necesario usar Windows PowerShell o Ldp.exe para habilitar la papelera de reciclaje de Active Directory o restaurar objetos en particiones de dominio.
- El Centro de administración de Active Directory tiene criterios de filtrado avanzados que facilitan la restauración específica en entornos grandes en los que muchos objetos se eliminan intencionadamente.
  
### <a name="limitations"></a>Limitaciones  
  
- Debido a que el Centro de administración de Active Directory solo puede administrar particiones de dominio, no puede restaurar objetos eliminados de las particiones Configuración, DNS de dominio o DNS de bosque (no puedes eliminar objetos de la partición Esquema). Para restaurar objetos de particiones que no son de dominio, use [Restore-ADObject](https://technet.microsoft.com/library/ee617262.aspx).  

- El Centro de administración de Active Directory no puede restaurar subárboles de objetos en una sola acción. Por ejemplo, si eliminas una unidad organizativa (OU) con OU anidadas, usuarios, grupos y equipos, al restaurar la OU de base no se restauran los objetos secundarios.  
  
    > [!NOTE]  
    > La operación de restauración de lote de centro de administración de Active Directory hace un "mejor esfuerzo" tipo de los objetos eliminados *dentro de la selección solo* para elementos primarios se ordenan antes que los elementos secundarios de la lista de restauración. En casos de pruebas sencillos, los subárboles de objetos se pueden restaurar en una sola acción. Pero casos más complejos, como una selección que contiene árboles parciales - árboles con algunos de los nodos primarios eliminados que faltan: o casos de error, como la omisión de los objetos secundarios cuando se produce un error de restauración primario, no funcionen según lo previsto. Por este motivo, debes restaurar siempre los subárboles de objetos en una acción diferente después de restaurar los objetos primarios.  
  
La Papelera de reciclaje de Active Directory requiere un Windows Server 2008 R2 nivel funcional del bosque y debe ser miembro del grupo Administradores de empresas. Una vez habilitada, la papelera de reciclaje de Active Directory no se puede deshabilitar. La papelera de reciclaje de Active Directory aumenta el tamaño de la base de datos de Active Directory (NTDS.DIT) en todos los controladores de dominio del bosque. El espacio en disco que usa la papelera de reciclaje continúa aumentando con el tiempo porque conserva los objetos y todos sus datos de atributos.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Habilitar la papelera de reciclaje de Active Directory mediante el Centro de administración de Active Directory

Para habilitar la papelera de reciclaje de Active Directory, abre el **Centro de administración de Active Directory** y haz clic en el nombre del bosque en el panel de navegación. En el panel **Tareas**, haz clic en **Habilitar papelera de reciclaje**.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
El Centro de administración de Active Directory muestra el cuadro de diálogo **Habilitar confirmación de papelera de reciclaje** . Este cuadro de diálogo avisa de que la operación para habilitar la papelera de reciclaje es irreversible. Haz clic en **Aceptar** para habilitar la papelera de reciclaje de Active Directory. El Centro de administración de Active Directory muestra otro cuadro de diálogo para recordarte que la papelera de reciclaje de Active Directory no será completamente funcional hasta que todos los controladores de dominio repliquen el cambio de configuración.  
  
> [!IMPORTANT]  
> La opción para habilitar la papelera de reciclaje de Active Directory no está disponible si:  
>
> - El nivel funcional del bosque es inferior a Windows Server 2008 R2.  
> - Ya está habilitada.  

El cmdlet de Active Directory Windows PowerShell equivalente es:  

```powershell
Enable-ADOptionalFeature  
```

Para obtener más información sobre cómo usar Windows PowerShell para habilitar la papelera de reciclaje de Active Directory, consulte la [Guía paso a paso de la papelera de reciclaje de Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#active-directory-recycle-bin-step-by-step).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Administrar la papelera de reciclaje de Active Directory mediante el Centro de administración de Active Directory

Esta sección usa el ejemplo de un dominio existente llamado **corp.contoso.com**. Este dominio organiza los usuarios en una OU primaria llamada **UserAccounts**. La OU **UserAccounts** contiene tres OU secundarias con los nombres de departamento, que a su vez contienen otras OU, usuarios y grupos.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Almacenamiento y filtrado

La papelera de reciclaje de Active Directory conserva todos los objetos eliminados en el bosque. Guarda estos objetos según el atributo **msDS-deletedObjectLifetime** que, de forma predeterminada, está establecido para que coincida con el atributo **tombstoneLifetime** del bosque. En cualquier bosque creado usando Windows Server 2003 SP1 o posterior, el valor de **tombstoneLifetime** está establecido en 180 días de forma predeterminada. En los bosques actualizados desde Windows 2000 o instalados con Windows Server 2003 (sin Service Pack), el atributo tombstoneLifetime predeterminado NO ESTÁ ESTABLECIDO y, por lo tanto, Windows usa el valor interno predeterminado de 60 días. Todo esto se puede configurar. Puedes usar el Centro de administración de Active Directory para restaurar los objetos eliminados de las particiones de dominio del bosque. Debes continuar usando el cmdlet **Restore-ADObject** para restaurar los objetos eliminados de otras particiones, como Configuración. Al habilitar la papelera de reciclaje de Active Directory el contenedor **Objetos eliminados** se hace visible en cada partición de dominio del Centro de administración de Active Directory.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
El contenedor **Objetos eliminados** muestra todos los objetos restaurables en la partición de dominio. Los objetos eliminados anteriores a **msDS-deletedObjectLifetime** se conocen como objetos reciclados. El Centro de administración de Active Directory no muestra los objetos reciclados y estos objetos no se pueden restaurar usando el Centro de administración de Active Directory.  
  
Para obtener una explicación detallada de la arquitectura de la Papelera de reciclaje y las reglas de procesamiento, vea [la Papelera de reciclaje de AD: Descripción, implementación, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx).  
  
El Centro de administración de Active Directory limita artificialmente a 20.000 el número predeterminado de objetos que se devuelven de un contenedor. Puedes aumentar este límite hasta 100.000 objetos haciendo clic en el menú **Administrar** y, después, **Opciones de la lista de administración**.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Restauración  
  
##### <a name="filtering"></a>Filtrado

El Centro de administración de Active Directory ofrece unos criterios y unas opciones de filtrado eficaces con las que debes familiarizarte antes de que necesites usarlas en una restauración real. Los dominios eliminan intencionadamente muchos objetos a lo largo de su vida. La vigencia probable de un objeto eliminado es de 180 días, por lo que no puedes simplemente restaurar todos los objetos en caso de accidente.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
En lugar de escribir complejos filtros LDAP y convertir valores de UTC en fechas y horas, usa el menú **Filtro** avanzado y básico para mostrar solo los objetos relevantes. Si sabes el día de la eliminación, el nombre de los objetos y otros datos clave, usa esa información para ayudarte a filtrar. Para activar o desactivar las opciones avanzadas de filtro, haz clic en el botón de contenido adicional, a la derecha del cuadro de búsqueda.  
  
La operación de restauración admite todas las opciones de criterios de filtrado, igual que cualquier otra búsqueda. De los filtros integrados, los importantes para restaurar objetos suelen ser:  
  
- *ANR (resolución de nombres ambiguos; no aparece en el menú pero se usa cuando se escribe en el *** filtro *** cuadro)*  
- Última modificación entre las fechas indicadas  
- El tipo de objeto es usuario, inetOrgPerson, equipo, grupo o unidad organizativa  
- Nombre  
- Al eliminar  
- Principal último conocido  
- Tipo  
- Descripción  
- Ciudad  
- País o región  
- Departmento  
- Id. de empleado  
- Nombre  
- Puesto  
- Apellidos  
- SAMaccountname  
- Estado o provincia  
- Número de teléfono  
- UPN  
- Código postal  

Puedes agregar varios criterios. Por ejemplo, puede encontrar todos los objetos de usuario eliminados en 24 de septiembre de 2012 de Chicago, Illinois con un puesto de director.
  
También puedes agregar, modificar o reordenar los encabezados de columna para proporcionar más información a la hora de evaluar qué objetos se van a recuperar.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Para obtener más información sobre la resolución de nombres ambiguos, consulte [Atributos de ANR](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Objeto único

La restauración de objetos eliminados siempre ha sido una operación única.  El Centro de administración de Active Directory facilita esa operación. Para restaurar un objeto eliminado, como un único usuario:  
  
1. Haz clic en el nombre de dominio en el panel de navegación del Centro de administración de Active Directory.  
2. Haz doble clic en **Objetos eliminados** en la lista de administración.  
3. Haz clic con el botón derecho en el objeto y haz clic en **Restaurar**, o haz clic en **Restaurar** en el panel **Tareas** .  
  
El objeto se restaura a su ubicación original.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Haga clic en **restaurar a...**  para cambiar la ubicación de restauración. Esto es útil si también se eliminó el contenedor primario del objeto eliminado, pero no desea restaurar al elemento primario.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Varios objetos del mismo nivel

Puedes restaurar varios objetos del mismo nivel, como todos los usuarios de una OU. Mantén presionada la tecla CTRL y haz clic en uno o varios de los objetos eliminados que quieres restaurar. Haz clic en **Restaurar** en el panel Tareas. Para seleccionar todos los objetos mostrados, presiona las teclas CTRL y A, o para seleccionar un intervalo de objetos, presiona MAYÚS y haz clic.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Varios objetos primarios y secundarios

Es fundamental comprender el proceso de restauración para una restauración de varios objetos primarios y secundarios porque el Centro de administración de Active Directory no puede restaurar un árbol anidado de objetos eliminados con una única acción.  
  
1. Restaura el objeto eliminado situado más arriba en un árbol.  
2. Restaura los secundarios inmediatos de ese objeto primario.  
3. Restaura los secundarios inmediatos de esos objetos primarios.  
4. Repite las veces que sea necesario hasta restaurar todos los objetos.  
  
No puedes restaurar un objeto secundario antes de restaurar su primario. Intentar esta restauración devuelve el siguiente error:  
  
**No se puede realizar la operación porque el principal del objeto no se ha instanciado o se ha eliminado.**  
  
El atributo **Principal último conocido** muestra la relación primaria de cada objeto. El atributo **Principal último conocido** cambia de la ubicación eliminada a la ubicación restaurada cuando se actualiza el Centro de administración de Active Directory después de restaurar un objeto primario. Por lo tanto, puede restaurar ese objeto secundario cuando la ubicación de un objeto primario ya no muestra el nombre distintivo del contenedor de objetos eliminados.  
  
Piensa en un escenario en el que un administrador elimina accidentalmente la OU Sales, que contiene OU secundarias y usuarios.  
  
En primer lugar, observe el valor de la **principal último conocido** atributo para todos los usuarios eliminados y cómo lee **OU = Sales\0ADEL:*< guid + contenedor de objetos eliminados distintivo nombre > ***:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtra por el nombre ambiguo Sales para devolver la OU eliminada, que puedes restaurar:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Actualice el centro de administración de Active Directory para ver el usuario eliminado atributo del objeto principal último conocido cambie el nombre distintivo de OU Sales restaurado:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtra todos los usuarios de Sales. Presiona las teclas CTRL y A para seleccionar todos los usuarios de Sales eliminados. Haz clic en **Restaurar** para mover los objetos del contenedor **Objetos eliminados** a la OU Sales con sus pertenencias a grupos y atributos intactos.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Si la OU **Sales** contenía a su vez OU secundarias, restaurarías primero las OU secundarias antes de restaurar sus secundarias, y así sucesivamente.  
  
Para restaurar objetos eliminados anidados especificando un contenedor principal eliminado, consulte [Apéndice B: Restaurar varios elimina objetos de Active Directory (Script de ejemplo)](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
El cmdlet de Active Directory Windows PowerShell para restaurar objetos eliminados es:  

```powershell
Restore-adobject  
```

La funcionalidad del cmdlet **Restore-ADObject** no cambió de Windows Server 2008 R2 a Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtrado del lado del servidor

Es posible que, con el tiempo, el contenedor de objetos eliminados acumule más de 20.000 (o incluso 100.000) objetos en organizaciones de tamaño medio y grande, y que tenga dificultades para mostrarlos todos. Como el mecanismo de filtrado en el Centro de administración de Active Directory usa el filtrado del lado del cliente, no puede mostrar estos objetos adicionales. Para evitar esta limitación, sigue estos pasos para realizar una búsqueda del lado del servidor:  
  
1. Haz clic con el botón derecho en el contenedor **Objetos eliminados** y haz clic en **Buscar en este nodo**.  
2. Haz clic en el botón de contenido adicional para exponer el menú **+Agregar criterios** y selecciona y agrega **Última modificación entre las fechas indicadas**. La hora de Última modificación (el atributo **whenChanged**) es una aproximación cercana a la hora de eliminación; en la mayoría de los entornos, es idéntica. Esta consulta realiza una búsqueda en el lado del servidor.  
3. Busca los objetos eliminados que quieres restaurar usando más filtrado y ordenación, entre otros, en los resultados mostrados, y restáuralos normalmente.  
  
## <a name="BKMK_FGPP"></a>Configuración y administración de directivas de contraseña específica mediante el centro de administración de Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configurar directivas de contraseña específica

El Centro de administración de Active Directory permite crear y administrar objetos de directiva de contraseña específica (FGPP). Windows Server 2008 incorporó la característica FGPP pero Windows Server 2012 tiene la primera interfaz gráfica de administración para ella. Las directivas de contraseña específica se aplican en el nivel del dominio y permiten invalidar la contraseña de dominio única que Windows Server 2003 necesita. Creando diferentes FGPP con diferentes configuraciones, cada usuario o grupo obtiene diferentes directivas de contraseña en un dominio.  
  
Para obtener más información sobre la directiva de contraseña específica, consulte la [Guía paso a paso para la configuración de directivas de bloqueo de cuenta y contraseña específica de AD DS (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
En el panel de navegación, haz clic en la Vista de árbol, en tu dominio, en **Sistema**, en **Contenedor de configuraciones de contraseña** y, en el panel tareas, haz clic en **Nuevo** y **Configuración de contraseña**.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Administrar directivas de contraseña específica

Al crear una nueva FGPP o editar una existente, se abre el editor **Configuración de contraseña**. Aquí puedes configurar todas las directivas de contraseña que quieras, como harías en Windows Server 2008 o Windows Server 2008 R2, solo que ahora con un editor creado específicamente para ello.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Rellena todos los campos necesarios (asterisco rojo) y los campos opcionales, y haz clic en **Agregar** para establecer los usuarios o grupos que reciben esta directiva. FGPP invalida la configuración de directiva de dominio predeterminada para las entidades de seguridad especificadas. En la figura anterior, una directiva extremadamente restrictiva se aplica solo a la cuenta Administrador integrada, para evitar riesgos. La directiva es demasiado compleja para que los usuarios estándar la cumplan, pero es perfecta para cuentas de alto riesgo que usan solo los profesionales de TI.  
  
También se establece la prioridad y a qué usuarios y grupos se aplica la directiva en un dominio determinado.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Los cmdlets de Active Directory Windows PowerShell para la directiva de contraseña específica son:  
  
```powershell
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
```

La funcionalidad del cmdlet para la directiva de contraseña específica no cambió de Windows Server 2008 R2 a Windows Server 2012. Por comodidad, el siguiente diagrama ilustra los argumentos asociados para los cmdlets:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
El Centro de administración de Active Directory también permite encontrar el conjunto resultante de FGPP aplicadas para un usuario específico. Haga clic en cualquier usuario y haga clic en **ver configuración de contraseña resultante …**  para abrir el *configuración de contraseña* página que se aplica a ese usuario mediante asignación implícita o explícita:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
Al examinar las **Propiedades** de un usuario o grupo, se muestra la **Configuración de contraseña asociada directamente**, que son las FGPP explícitamente asignadas:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
Asignación implícita de FGPP aquí no se muestra; Para ello, debe usar el **ver configuración de contraseña resultante …**  opción.  
  
## <a name="BKMK_HistoryViewer"></a>Usar el Visor del historial de PowerShell de Active Directory Administrative Center Windows

El futuro de la administración de Windows es Windows PowerShell. Las herramientas gráficas se distribuyen por capas en un entorno de automatización de tareas, lo que permite administrar los sistemas distribuidos más complejos de forma coherente y eficaz. Tienes que comprender cómo funciona Windows PowerShell para aprovechar todas sus posibilidades y maximizar la inversión en informática.  
  
Ahora, el Centro de administración de Active Directory ofrece un historial completo de todos los cmdlets de Windows PowerShell que ejecuta y sus argumentos y valores. Puedes copiar el historial de cmdlets en otro lugar para estudiarlo, o para modificarlo y reutilizarlo. Puedes crear unas notas de tareas para ayudarte a aislar el resultado de los comandos del Centro de administración de Active Directory en Windows PowerShell. También puedes filtrar el historial para buscar puntos de interés.  
  
El objetivo del visor del historial de Windows PowerShell en el Centro de administración de Active Directory es que aprendas mediante experiencia práctica.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Haz clic en el botón de contenido adicional (flecha) para mostrar el visor del historial de Windows PowerShell.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Después, crea un usuario o modifica la pertenencia de un grupo. El visor del historial se actualiza continuamente con una vista contraída de cada cmdlet que el Centro de administración de Active Directory ejecutó con los argumentos especificados.  
  
Expande cualquier elemento de línea para ver todos los valores proporcionados a los argumentos del cmdlet:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Haz clic en el menú **Iniciar tarea** para crear una anotación manual antes de usar el Centro de administración de Active Directory para crear, modificar o eliminar un objeto. Escribe lo que estabas haciendo.  Cuando hayas terminado con el cambio, selecciona **Finalizar tarea**. La nota de tarea agrupa todas las acciones realizadas en una nota contraíble que puedes usar para obtener más información.  
  
Por ejemplo, para ver los comandos de Windows PowerShell usados para cambiar la contraseña de un usuario y quitarlo de un grupo:  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
Al seleccionar la casilla Mostrar todo también se muestran los cmdlets de Windows PowerShell del verbo Get-* que solo recuperan datos.  
  
![Administración avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
El visor del historial muestra los comandos literales ejecutados por el Centro de administración de Active Directory y quizás observes que algunos cmdlets parecen ejecutarse innecesariamente. Por ejemplo, puedes crear un nuevo usuario con:  

```powershell
new-aduser
```

y no necesitas usar:  

```powershell
set-adaccountpassword  
enable-adaccount  
set-aduser  
```

El diseño del Centro de administración de Active Directory requirió una modularidad y un uso de código mínimos. Por lo tanto, en lugar de un conjunto de funciones que crean nuevos usuarios y otras que modifican los usuarios existentes, realiza cada función mínimamente y, después, las encadena con los cmdlets. Ten esto en cuenta cuando estudies Active Directory Windows PowerShell. También puedes usarlo como técnica de aprendizaje, para ver lo simple que resulta usar Windows PowerShell para completar una sola tarea.  
  
## <a name="BKMK_Tshoot"></a>Solución de problemas de administración de AD DS  
  
### <a name="introduction-to-troubleshooting"></a>Introducción a la solución de problemas

Debido a que es muy reciente y que no se ha usado en entornos de clientes existentes, el Centro de administración de Active Directory tiene opciones de solución de problemas limitadas.  
  
### <a name="troubleshooting-options"></a>Opciones de solución de problemas  
  
#### <a name="logging-options"></a>Opciones de registro

El centro de administración de Active Directory contiene ahora el registro integrado, como parte de un archivo de configuración de seguimiento. Crea o modifica el siguiente archivo en la misma carpeta que dsac.exe:  
  
**dsac.exe.config**
  
Crea el contenido siguiente:  
  
```xml
<appSettings>  
  <add key="DsacLogLevel" value="Verbose" />  
</appSettings>  
<system.diagnostics>
 <trace autoflush="false" indentsize="4">
  <listeners>
   <add name="myListener"
    type="System.Diagnostics.TextWriterTraceListener"
    initializeData="dsac.trace.log" />
   <remove name="Default" />
  </listeners>
 </trace>
</system.diagnostics>
```

Los niveles de detalle de **DsacLogLevel** son **None**, **Error**, **Warning**, **Info** y **Verbose**. El nombre del archivo de salida se puede configurar y se escribe en la misma carpeta que dsac.exe. El archivo de salida proporciona más información sobre cómo funciona ADAC, con qué controladores de dominio se puso en contacto, qué comandos de Windows PowerShell se ejecutaron, cuáles fueron las respuestas y más información.  

Por ejemplo, al usar el nivel INFO, que devuelve todos los resultados excepto el detalle del nivel de seguimiento:  
  
- Se inicia DSAC.exe.  
- Se inicia el registro.  
- El controlador de dominio solicitó devolver la información inicial del dominio.  

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
   ```

- El controlador de dominio DC1 volvió del dominio Corp.  
- Se cargó la unidad virtual PS AD.  

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
   ```
  
- Se obtiene la información Root DSE del dominio.  

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
   ```

- Se obtiene la información de la papelera de reciclaje de AD del dominio.  

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- Se obtiene el bosque de AD.  

   ```  
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
   ```

- Se obtiene la información del esquema para los tipos de cifrado admitidos, FGPP, determinada información del usuario.  

   ```
   [12:42:50][TID 3][Info] Get-ADObject  
   -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"  
   -Properties:lDAPDisplayName  
   -ResultPageSize:"100"  
   -ResultSetSize:$null  
   -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"  
   -SearchScope:"OneLevel"  
   -Server:"dc1.corp.contoso.com"  
   [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7  
   [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50  
   ```

- Se obtiene toda la información acerca del objeto de dominio para mostrarla al administrador que hizo clic en el encabezado del dominio.  

   ```
   [12:42:50][TID 3][Info] Get-ADObject  
   -IncludeDeletedObjects:$false  
   -LDAPFilter:"(objectClass=*)"  
   -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence  
   -ResultPageSize:"100"  
   -ResultSetSize:"20201"  
   -SearchBase:"DC=corp,DC=contoso,DC=com"  
   -SearchScope:"Base"  
   -Server:"dc1.corp.contoso.com"  
   ```

Cuando se establece el nivel Verbose, también se muestran las pilas de .NET de cada función, pero no incluyen datos suficientes para que resulten especialmente útiles, excepto para solucionar problemas de Dsac.exe cuando sufre infracciones de acceso o bloqueo. Las dos causas probables de este problema son:
  
- El servicio ADWS no se está ejecutando en un controlador de dominio accesible.
- Se bloquean las comunicaciones de red con el servicio ADWS desde el equipo que ejecuta el centro de administración de Active Directory.

> [!IMPORTANT]  
> También hay una versión fuera de banda del servicio que se llama [Servicio de administración de la puerta de enlace de Active Directory](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), que se ejecuta en Windows Server 2008 SP2 y Windows Server 2003 SP2.
>

Los errores que se muestran cuando no hay disponibles instancias de Servicios web de Active Directory son:  
  
|Error|Operación|
| --- | --- |  
|"No se pudo conectar a ningún dominio. Actualice o vuelva a intentarlo cuando la conexión esté disponible"|Se muestra al iniciar la aplicación Centro de administración de Active Directory.|
|"No se puede encontrar un servidor disponible en el *<NetBIOS domain name>* dominio que se está ejecutando el servicio de Web de Active Directory (ADWS)"|Se muestra al intentar seleccionar un nodo del dominio en la aplicación Centro de administración de Active Directory.|
  
Para solucionar este problema, sigue estos pasos:  
  
1. Comprueba que el servicio Servicios web de Active Directory se ha iniciado al menos en un controlador de dominio del dominio (y preferiblemente en todos los controladores de dominio del bosque). Asegúrate de que está establecido para iniciarse automáticamente también en todos los controladores de dominio.
2. En el equipo donde se ejecuta el Centro de administración de Active Directory, comprueba que puedes encontrar un servidor que ejecuta ADWS ejecutando estos comandos de NLTest.exe:  

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   Si se producen errores en estas pruebas aunque el servicio ADWS se esté ejecutando, el problema es de la resolución de nombres o de LDAP, y no de ADWS o del Centro de administración de Active Directory. Sin embargo, esta prueba produce el error "1355 0x54B ERROR_NO_SUCH_DOMAIN" si ADWS no se está ejecutando en ningún controlador de dominio, por lo que debes realizar una doble comprobación antes de sacar conclusiones.  
  
3. En el controlador de dominio que devuelve NLTest, vuelca la lista de puertos de escucha con el comando:  

   ```
   Netstat -anob > ports.txt  
   ```

   Examina el archivo ports.txt y comprueba que el servicio ADWS está escuchando en el puerto 9389. Por ejemplo:  

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  

   TCP    [::]:9389       [::]:0       LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  
   ```

   Si está escuchando, comprueba las reglas de Firewall de Windows y asegúrate de que permiten el tráfico de entrada en el puerto TCP 9389. De forma predeterminada, los controladores de dominio habilitan la regla de firewall "Servicios web de Active Directory (TCP de entrada)". Si no está escuchando, comprueba de nuevo que el servicio se está ejecutando en este servidor y reinícialo. Comprueba que no haya otros procesos escuchando en el puerto 9389.  
  
4. Instala NetMon u otra utilidad de captura de red en el equipo donde se ejecuta el Centro de administración de Active Directory y en el controlador de dominio que NLTEST devuelve. Recopila capturas de red simultáneas en ambos equipos mientras inicias el Centro de administración de Active Directory y mira el error antes de detener las capturas. Comprueba si el cliente puede enviar y recibir desde el controlador de dominio en el puerto TCP 9389. Si los paquetes se envían pero no llegan, o llegan y el controlador de dominio responde pero no llegan nunca al cliente, es problema que haya un firewall entre los equipos en la red que pierda los paquetes de ese puerto. Este firewall puede ser de software o hardware, y quizás forme parte de un software de protección de extremos (antivirus) de terceros.  
  
## <a name="see-also"></a>Vea también

[AD Papelera de reciclaje, directiva de contraseña específica y el historial de PowerShell](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
