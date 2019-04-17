---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: "Administración de avanzada de AD DS con el centro de administración de Active Directory (nivel 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19e83ce0123bd484c61d5a4522ebdd90cd40241e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Administración de avanzada de AD DS con el centro de administración de Active Directory (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema cubre el centro de administración de Active Directory con su nueva directiva de contraseña muy específicas, Visor de historial de Windows PowerShell y la Papelera de reciclaje de Active Directory actualizada con más detalle, incluida la arquitectura, ejemplos para tareas comunes y la información para solucionar problemas. Para obtener una introducción, consulta [Introduction to Active Directory mejoras del centro de administrativas & #40; Nivel 100 & #41; ](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
-   [Arquitectura del centro de administración de Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
  
-   [Habilitar y administrar Active Directory mediante el centro de administración de Active Directory de la Papelera de reciclaje](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
  
-   [Configurar y administrar directivas de contraseña específicas mediante el centro de administración de Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
  
-   [Mediante el Visor de historial de PowerShell de Windows de centro de administración de Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
  
-   [Solucionar problemas de administración de AD DS](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Arquitectura del centro de administración de Active Directory  
  
### <a name="adprep-executables-dlls"></a>ADPrep archivos ejecutables, archivos DLL  
El módulo y la arquitectura subyacente de centro de administración de Active Directory no ha cambiado con la nueva Papelera de reciclaje, FGPP y capacidades de Visor de historial.  
  
-   Microsoft.ActiveDirectory.Management.UI.dll  
  
-   Microsoft.ActiveDirectory.Management.UI.resources.dll  
  
-   Microsoft.ActiveDirectory.Management.dll  
  
-   Microsoft.ActiveDirectory.Management.resources.dll  
  
-   ActiveDirectoryPowerShellResources.dll  
  
La capa de operaciones para la nueva funcionalidad de la Papelera de reciclaje y Windows PowerShell subyacente se muestran a continuación:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>Habilitar y administrar Active Directory mediante el centro de administración de Active Directory de la Papelera de reciclaje  
  
### <a name="capabilities"></a>Funcionalidades  
  
-   El Centro Windows Server 2012 Active Directory administrativas te permite configurar y administrar Active Directory Papelera para las particiones de dominio en un bosque. Ya no es un requisito para usar Windows PowerShell o Ldp.exe para habilitar la Papelera de reciclaje de Active Directory o restaurar objetos de particiones de dominio.  
  
-   El centro de administración de Active Directory ha avanzado criterios de filtrado, facilitando la restauración de destino en entornos grandes con muchos objetos intencionadamente eliminados.  
  
### <a name="limitations"></a>Limitaciones  
  
-   Dado que el centro de administración de Active Directory solo puede administrar las particiones de dominio, no pueden restaurar objetos eliminados de las particiones de configuración, DNS del dominio o bosque DNS (no se puede eliminar objetos de la partición de esquema). Para restaurar los objetos de particiones no del dominio, usa [restauración ID](https://technet.microsoft.com/library/ee617262.aspx).  
  
-   El centro de administración de Active Directory no se puede restaurar subárboles de objetos en una única acción. Por ejemplo, si eliminas una unidad organizativa con anidados unidades organizativas, los usuarios, grupos y equipos, restaurar la unidad organizativa base no restaurar los objetos secundarios.  
  
    > [!NOTE]  
    > La operación de restauración del lote de centro de administración de Active Directory hace "mejor esfuerzo" tipo de los objetos eliminados *dentro de la selección solo* para que los padres se ordenan antes de los elementos secundarios de la lista de restauración. En casos sencillos de prueba, es posible restaurar subárboles de objetos en una única acción. Pero en casos de esquina, como una selección que contiene los árboles parciales - árboles con algunos de los nodos primario eliminado falta - o en casos de error, como la omisión de los objetos secundarios cuando restauración principal se produce un error, no funcionen según lo esperado. Por este motivo, siempre debe restaurar subárboles de objetos como una acción independiente después de restaurar los objetos primarios.  
  
La Papelera de reciclaje de Active Directory requiere Windows Server 2008 R2 nivel funcional del bosque y debe ser miembro del grupo Administradores de empresa. Una vez habilitado, no se puede deshabilitar la Papelera de reciclaje de Active Directory. La Papelera de reciclaje de Active Directory aumenta el tamaño de la base de datos de Active Directory (NTDS. DIT) en cada controlador de dominio del bosque. Espacio en disco usado por la Papelera de reciclaje continúa aumentado con el tiempo a medida que conserva los objetos y todos sus datos de atributo.  
  
Para obtener más información, consulta [Active Directory Papelera de reciclaje guía paso a paso](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx) y [evaluar los requisitos de Hardware (para la Papelera de reciclaje Active Directory)](https://technet.microsoft.com/library/cc753439(WS.10).aspx).  
  
Puedes *inferior* el bosque y dominio niveles funcionales de Windows Server 2012 para Windows Server 2008 R2, incluso después de habilitar la Papelera de reciclaje de Active Directory. Esto requiere el uso de la **conjunto ADForestMode** y **conjunto ADDomainMode** cmdlets de Active Directory.  
  
Por ejemplo:  
  
```  
Set-AdForestMode -identity corp.contoso.com -server dc1.corp.contoso.com -forestmode Windows2008R2Forest  
Set-AdDomainMode -identity research.corp.contoso.com -server dc3.research.corp.contoso.com -domainmode Windows2008R2Domain  
  
```  
  
No puedes usar el centro de administración de Active Directory para realizar este cambio: solo genera niveles funcionales.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Habilitar Active Directory Papelera de reciclaje mediante el centro de administración de Active Directory  
Para habilitar la Papelera de reciclaje de Active Directory, abra el **centro de administración de Active Directory** y haz clic en el nombre del bosque en el panel de navegación. Desde el **tareas** panel, haz clic en **habilitar la Papelera de reciclaje**.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
El centro de administración de Active Directory, se muestra la **confirmación de la Papelera de reciclaje habilitar** cuadro de diálogo. Este cuadro de diálogo le advertirá que la habilitación de la Papelera de reciclaje es irreversible. Haz clic en **Aceptar** para habilitar la Papelera de reciclaje de Active Directory. Otro cuadro de diálogo que te Recuerde que la Papelera de reciclaje de Active Directory no es totalmente funcional hasta que todos los controladores de dominio replican el cambio de configuración se muestra en el centro de administración de Active Directory.  
  
> [!IMPORTANT]  
> La opción para habilitar la Papelera de reciclaje de Active Directory no está disponible si:  
>   
> -   El nivel funcional del bosque es menor que Windows Server 2008 R2  
> -   Ya está habilitado  
  
El cmdlet de Windows PowerShell de Active Directory equivalente sigue siendo:  
  
```  
Enable-ADOptionalFeature  
```  
  
Para obtener más información sobre cómo usar Windows PowerShell para habilitar la Papelera de reciclaje de Active Directory, consulte la [Active Directory Papelera de reciclaje guía paso a paso](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Administración de Active Directory Papelera de reciclaje mediante el centro de administración de Active Directory  
Esta sección usa en el ejemplo de un dominio existente denominado **corp.contoso.com**. En este dominio organiza los usuarios en una unidad organizativa denominado principal **UserAccounts**. La **UserAccounts** UO contiene tres unidades organizativas secundarias denominadas por departamento, que contienen más unidades organizativas, los usuarios y grupos.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Almacenamiento y filtrado  
La Papelera de reciclaje de Active Directory conserva todos los objetos que se eliminará en el bosque. Guarda estos objetos según el **msDS deletedObjectLifetime** atributo, que, de manera predeterminada, se establece para que coincida con el **tombstoneLifetime** atributo del bosque. En cualquier bosque creado con Windows Server 2003 SP1 o posterior, el valor de **tombstoneLifetime** se establece en 180 días de forma predeterminada. En cualquier bosque instalado con Windows Server 2003 (sin service pack) o actualizar desde Windows 2000, no se establece el atributo de tombstoneLifetime predeterminado y por lo tanto, Windows usa el valor predeterminado interno de 60 días. Todo esto es configurable. Puedes usar el centro de administración de Active Directory para restaurar todos los objetos eliminados de las particiones de dominio del bosque. Debe seguir usar el cmdlet **restauración ID** restaurar objetos eliminados desde otras particiones, como Configuration.Enabling hace que la Papelera de reciclaje de Active Directory la **objetos eliminados** contenedor visible en cada partición de dominio en el centro de administración de Active Directory.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
La **objetos eliminados** contenedor muestra todos los objetos que se pueda restaurar en esa partición de dominio. Elimina los objetos anteriores a **msDS deletedObjectLifetime** se conocen como objetos reciclados. El centro de administración de Active Directory no muestra objetos reciclados y no se puede restaurar estos objetos con el centro de administración de Active Directory.  
  
Para obtener una explicación más profunda de la arquitectura de la Papelera de reciclaje y reglas de procesamiento, consulta [la Papelera de reciclaje de anuncios: descripción, implementación, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx).  
  
El centro de administración de Active Directory artificialmente limita el número predeterminado de los objetos devueltos desde un contenedor a 20.000 objetos. Puedes generar este límite tan alto como 100.000 objetos haciendo clic en el **administrar** menú, a continuación, **lista de opciones de administración**.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Restauración  
  
##### <a name="filtering"></a>Filtrado  
Centro de administración de Active Directory ofrece criterios potente y opciones de filtrado que debes familiarizarte con antes de usarlos en una restauración de la vida real. Dominios eliminarán intencionadamente muchos objetos a través de su duración. Con una duración del objeto probablemente eliminado de 180 días, simplemente no puede restaurar todos los objetos cuando se produce un accidente.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
En lugar de escribir los filtros LDAP complejos y convertir los valores UTC en fechas y horas, usa básicas y avanzadas **filtro** menú para mostrar solo los objetos pertinentes. Si conoces el día de eliminación, los nombres de objetos o cualquier otro dato clave, usa una ventaja cuando se filtran. Activar o desactivar las opciones de filtro avanzado haciendo clic en las comillas angulares a la derecha del cuadro de búsqueda.  
  
La operación de restauración es compatible con todas las opciones de criterios de filtro estándar, igual que cualquier otra búsqueda. De los filtros integrados, más importante para restaurar objetos normalmente son:  
  
-   *ANR (resolución de nombres ambiguos - no aparece en el menú, pero lo que se usa cuando se escribe en el *** filtro *** cuadro)*  
  
-   Última modificación entre dada fechas  
  
-   Objeto es la unidad de usuario/inetorgperson o equipo o grupo/organización  
  
-   Nombre  
  
-   Cuando elimina  
  
-   Último principal conocido  
  
-   Tipo  
  
-   Descripción  
  
-   Ciudad  
  
-   País/región  
  
-   Departamento  
  
-   Id. de empleado  
  
-   Nombre  
  
-   Puesto de trabajo  
  
-   Apellidos  
  
-   SAMaccountname  
  
-   Estado o provincia  
  
-   Número de teléfono  
  
-   UPN  
  
-   Código Postal  
  
Puedes agregar varios criterios. Por ejemplo, puedes encontrar todos los objetos de usuario eliminados 24 de septiembre<sup>th</sup> 2012 de Chicago, Illinois con un título de trabajo de administrador.  
  
También puedes agregar, modificar o cambiar el orden de los encabezados de columna para proporcionar más detalles al evaluar los objetos que desea recuperar.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Para obtener más información acerca de la resolución de nombres ambiguos, consulta [ANR atributos](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Objeto único  
Restauración de objetos eliminados siempre ha sido una sola operación.  El centro de administración de Active Directory facilita la operación. Para restaurar un objeto eliminado, como un único usuario:  
  
1.  Haz clic en el nombre de dominio en el panel de navegación del centro de administración de Active Directory.  
  
2.  Haz doble clic en **objetos eliminados** en la lista de administración.  
  
3.  Haz clic en el objeto y, a continuación, haz clic en **restaurar**, o haz clic en **restaurar** desde el **tareas** panel.  
  
El objeto se restablece a su ubicación original.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Haz clic en **restaurar a... ** para cambiar la ubicación de restauración. Esto es útil si el contenedor primario del objeto eliminado: se eliminó también pero no quieres restaurar al elemento primario.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Varios objetos de sistema del mismo nivel  
Puedes restaurar varios objetos de nivel de sistema del mismo nivel, como todos los usuarios en una unidad organizativa. Mantén presionada la tecla CTRL y haz clic en uno o más objetos eliminados que deseas restaurar. Haz clic en **restaurar** desde el panel de tareas. También puedes seleccionar objetos mostrados manteniendo presionada la tecla CTRL y una claves, o un intervalo de objetos mediante la tecla MAYÚS y haciendo clic.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Varias principal y los objetos secundarios  
Es fundamental para comprender el proceso de restauración de una restauración de múltiples parent y secundarios porque el centro de administración de Active Directory no se puede restaurar un árbol anidado de objetos eliminados con una única acción.  
  
1.  El objeto eliminado de nivel superior en un árbol de restauración.  
  
2.  Restaurar a los elementos secundarios inmediatos de dicho objeto primario.  
  
3.  Restaurar a los elementos secundarios inmediatos de esos objetos principales.  
  
4.  Repite según sea necesario hasta restauración todos los objetos.  
  
No puede restaurar un objeto secundario antes de restaurar a su elemento primario. Intentar esta restauración devuelve el siguiente error:  
  
**No se pudo realizar la operación porque el elemento primario del objeto se instanciado o se elimina.**  
  
La **último principal conocido** atributo muestra la relación de elemento primario de cada objeto. La **último principal conocido** cambios de atributos de la ubicación eliminada a la ubicación restaurada cuando actualices el centro de administración de Active Directory después de restaurar un elemento primario. Por lo tanto, puedes restaurar dicho objeto secundario cuando la ubicación de un objeto primario ya no muestra el nombre completo del contenedor de objetos eliminados.  
  
Ten en cuenta el escenario donde un administrador accidentalmente elimina la unidad organizativa de ventas, que contiene las unidades organizativas secundarias y los usuarios.  
  
En primer lugar, observa el valor de la **último principal conocido** atributo para todos los usuarios eliminados y cómo se lee * *UO = Sales\0ADEL:*< guid + contenedor de objetos eliminados distinguirse nombre >***:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtrar las ventas de nombre ambiguo para devolver la unidad organizativa eliminada, que, a continuación, restaurar:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Actualizar el centro de administración de Active Directory para ver el usuario eliminado atributo del objeto principal conocido último cambio en el nombre distintivo de unidad organizativa Ventas restaurado:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtrar por todos los usuarios de ventas. Mantén presionada la tecla CTRL y una teclas para seleccionar todos los usuarios de ventas eliminados. Haz clic en **restaurar** para mover los objetos de la **objetos eliminados** contenedor a la unidad organizativa de ventas con su pertenencia a grupos y los atributos intactas.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Si la **ventas** UO contenidos UO secundarias de su propio, a continuación, ¿restaurar las unidades organizativas de secundarios antes de restaurar sus elementos secundarios y así sucesivamente.  
  
Para restaurar anidados todos los objetos eliminados especificando un contenedor primario eliminado, consulte [Apéndice B: restaurar varios, eliminar objetos de Active Directory (Script de muestra)](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
El cmdlet de Windows PowerShell de Active Directory para restaurar objetos eliminados es:  
  
```  
Restore-adobject  
  
```  
  
La **restauración ID** funcionalidad cmdlet no ha cambiado entre Windows Server 2008 R2 y Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtrado del lado servidor  
Es posible que con el tiempo, el contenedor de objetos eliminados acumularse objetos más de 20.000 (o incluso 100.000) en las empresas medianas y grandes y tienen dificultades para mostrar todos los objetos. Dado que el mecanismo de filtro en el centro de administración de Active Directory se basa en el filtrado de cliente, no pueden mostrar estos objetos adicionales. Para evitar esta limitación, usa los siguientes pasos para realizar una búsqueda de servidor:  
  
1.  Haz clic en el **objetos eliminados** contenedor y haz clic en **buscar en este nodo**.  
  
2.  Haz clic en las comillas angulares para exponer el **+ agregar criterios** menú, seleccionar y agregar **modificó por última vez entre dada fechas**. La hora de la última modificación (la **whenChanged** atributo) es parecido de la hora de eliminación; en la mayoría de los entornos, son idénticos. Esta consulta realiza una búsqueda en el servidor.  
  
3.  Busca los objetos eliminados restaurar mediante otra pantalla filtrar y ordenar, y así sucesivamente en los resultados y restaurarlos normalmente.  
  
## <a name="BKMK_FGPP"></a>Configurar y administrar directivas de contraseña específicas mediante el centro de administración de Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configuración de directivas de contraseña muy específicas  
El centro de administración de Active Directory permite crear y administrar los objetos de directiva de contraseña muy específicas (FGPP). Windows Server 2008 introdujo la característica FGPP pero Windows Server 2012 tiene la primera interfaz gráfica de administración para ella. Aplicar directivas de contraseña específicas en un nivel de dominio y permite invalidar la contraseña del dominio solo requerida Windows Server 2003. Al crear diferentes FGPP con distintas opciones de configuración, los usuarios individuales o grupos Obtén diferentes directivas de contraseña en un dominio.  
  
Para obtener información acerca de la directiva de contraseña muy específicas, consulte [contraseñas específicas de AD DS y directiva de bloqueo de cuenta Step-by-Step guía (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
En el panel de navegación, haz clic en la vista de árbol, haga clic en el dominio, **sistema**, haz clic en **contenedor de configuraciones de contraseña**y, a continuación, en el panel de tareas, haz clic en **nueva** y **contraseña configuración**.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Administración de directivas de contraseña específicas  
Crear un nuevo FGPP o editar una existente, se mostrará el **contraseña configuración** editor. Desde aquí, configurar todas las directivas de contraseña deseado, igual que en Windows Server 2008 o Windows Server 2008 R2, ahora con un editor específico.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Rellena todos los campos (asterisco rojo) y los campos opcionales y, a continuación, haz clic en **agregar** para establecer los usuarios o grupos que recibe esta directiva. FGPP reemplaza la configuración de directiva de dominio predeterminada para esas entidades de seguridad especificado. En la figura anterior, una directiva extremadamente restrictiva se aplica solo a la cuenta predefinida de administrador, para evitar ataques. La directiva es demasiado compleja para cumplir con los usuarios estándar, pero es perfecta para una cuenta de alto riesgo utilizada solo de los profesionales de TI.  
  
También establecer prioridad y a los usuarios y grupos se aplica la directiva de un dominio.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Los cmdlets de Active Directory de Windows PowerShell para la directiva de contraseña muy específicas son:  
  
```  
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
  
```  
  
Funcionalidad de cmdlet de directiva de contraseña muy específica no ha cambiado entre la Windows Server 2008 R2 y Windows Server 2012. Para mayor comodidad, el siguiente diagrama ilustra los argumentos asociados con los cmdlets de:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
El centro de administración de Active Directory también te permite encontrar el conjunto resultante de FGPP aplicada para un usuario específico. Haga clic con el botón secundario del mouse en cualquier usuario y haz clic en **ver configuración contraseña resultante... ** para abrir el *contraseña configuración* página que se aplica a ese usuario a través de la asignación implícito o explícito:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
Examinar la **propiedades** de cualquier usuario o grupo se muestra la **asociadas directamente la configuración de contraseña**, que son el FGPPs asignados explícitamente:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
Asignación de FGPP implícito no se muestra aquí; Para ello, debes usar el **ver configuración contraseña resultante... ** opción.  
  
## <a name="BKMK_HistoryViewer"></a>Mediante el Visor de historial de PowerShell de Windows de centro de administración de Active Directory  
El futuro de administración de Windows es Windows PowerShell. Herramientas gráficas de disposición en capas encima de un marco de automatización de la tarea, la administración de los sistemas distribuidos más complejos se convierte en coherente y eficaz. Debes comprender el funcionamiento de Windows PowerShell para alcanzar su potencial máximo y maximizar las inversiones de informáticas.  
  
El centro de administración de Active Directory ahora ofrece un historial completo de todos los cmdlets de Windows PowerShell que se ejecuta y los argumentos y los valores. Puedes copiar el historial de cmdlet en otro lugar de estudio o modificación y volver a usar. Puedes crear notas de la tarea para ayudar a aislar qué el centro de administración de Active Directory comandos aparecía en Windows PowerShell. También puedes filtrar el historial para buscar puntos de interés.  
  
Propósito del Visor Active Directory administrativas Centro Windows PowerShell historial es puedes aprender a través de la experiencia práctica.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Haz clic en la comilla angular (flecha) para mostrar el Visor de historial de Windows PowerShell.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
A continuación, crea un usuario o modificar la pertenencia a un grupo. El Visor de historial continuamente actualiza con una vista de cada cmdlet que se ejecutó el centro de administración de Active Directory con los argumentos especificados contraída.  
  
Expande cualquier elemento de línea de interés para ver todos los valores proporcionados en los argumentos del cmdlet:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Haz clic en el **Iniciar tarea** menú para crear una notación manual antes de usar el centro de administración de Active Directory para crear, modificar o eliminar un objeto. Escribe lo que estaba haciendo.  Cuando termine con el cambio, selecciona **Finalizar tarea**. Los grupos de nota tareas todas las acciones realizadas en una nota contraíble que puedes usar para comprender mejor.  
  
Por ejemplo, para ver los comandos de Windows PowerShell que se usa para cambiar la contraseña de un usuario y quitarlo de un grupo:  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
También activa la casilla Mostrar todo muestra Get-* verbo cmdlets de Windows PowerShell que solo se recuperan datos.  
  
![Administración de avanzada de AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
El Visor del historial muestra el literal comandos que se ejecutan en el centro de administración de Active Directory y es posible que tenga en cuenta que algunos cmdlets parezca ejecutarse innecesariamente. Por ejemplo, puedes crear un nuevo usuario con:  
  
```  
new-aduser   
```  
  
y no es necesario que uses:  
  
```  
set-adaccountpassword  
enable-adaccount  
set-aduser  
  
```  
  
Diseño de la administración centro de Active Directory necesario modularidad y uso de un código mínimo. Por lo tanto, en lugar de un conjunto de funciones que crear nuevos usuarios y otro conjunto que modificar los usuarios existentes, como mínimo realiza cada función y, a continuación, se encadena junto con los cmdlets. Ten esto en cuenta cuando se estén aprendiendo a Active Directory de Windows PowerShell. También puedes usar como una técnica de aprendizaje, donde puedes ver cómo simplemente puedes usar Windows PowerShell para completar una tarea única.  
  
## <a name="BKMK_Tshoot"></a>Solucionar problemas de administración de AD DS  
  
### <a name="introduction-to-troubleshooting"></a>Introducción a la solución de problemas  
Debido a su novedad relativa y la falta de capacidad de uso en entornos de clientes existentes, el centro de administración de Active Directory tiene limitados opciones de solución de problemas.  
  
### <a name="troubleshooting-options"></a>Opciones de solución de problemas  
  
#### <a name="logging-options"></a>Opciones de registro  
El centro de administración de Active Directory contiene ahora un registro integrados en Windows Server 2012, como parte de un archivo de configuración de seguimiento. Crear o modificar el archivo en la misma carpeta que dsac.exe siguiente:  
  
**dsac.exe.config**  
  
Crea el siguiente contenido:  
  
```  
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
  
El nivel de detalle niveles para **DsacLogLevel** son **ninguno**, **Error**, **advertencia**, **información**, y **Verbose**. El nombre de archivo de salida es configurable y escribe en la misma carpeta que dsac.exe. Más información sobre cómo funciona ADAC, qué controladores de dominio estableció contacto con, ¿qué comandos de Windows PowerShell que se ejecuta, ¿cuáles son las respuestas y más detalles, puede indicar el resultado.  
  
Por ejemplo, al usar el nivel de información, que devuelve todos los resultados excepto el nivel de detalle de nivel de seguimiento:  
  
-   Inicia DSAC.exe  
  
-   Iniciado el registro  
  
-   Controlador de dominio solicitado para devolver información del dominio inicial  
  
    ```  
    [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
    [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
    ```  
  
-   El controlador de dominio DC1 devuelto desde el dominio Corp  
  
-   PS Cargado unidad virtual de anuncios  
  
    ```  
    [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
    [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
  
    ```  
  
-   Obtener información de DSE raíz del dominio  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
  
    ```  
  
-   Obtener información de dominio de AD Papelera de reciclaje  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50  
  
    ```  
  
-   Obtener el bosque de AD  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADForest  
    -Identity:"corp.contoso.com"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Obtener información del esquema para tipos de cifrado compatibles, FGPP, determinada información del usuario  
  
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
  
-   Obtén toda la información sobre el objeto de dominio para mostrar al administrador que se hizo clic en el encabezado del dominio.  
  
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
  
Establecer el nivel del Verbose también muestra las pilas de .NET para cada función, pero estos no incluyen datos suficientes para ser especialmente útil salvo cuando la Dsac.exe sufrir una infracción de acceso o el bloqueo de solución de problemas.  
  
> [!IMPORTANT]  
> También hay una versión de fuera de banda del servicio llama la [Active Directory Management Gateway](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), que se ejecuta en Windows Server 2008 SP2 y Windows Server 2003 SP2.  
  
Son las dos causas posibles de este problema:  
  
-   No se está ejecutando el servicio ADWS cualquier accesible en controladores de dominio.  
  
-   Se bloquean las comunicaciones de red con el servicio ADWS desde el equipo que ejecuta el centro de administración de Active Directory  
  
Los errores que se muestra cuando no hay instancias de los servicios de Web de Active Directory disponibles son:  
  
|||  
|-|-|  
|Error|Operación|  
|"No se puede conectar a un dominio. Actualizar o inténtalo de nuevo cuando la conexión está disponible"|Se muestra en el inicio de la aplicación Centro de administración de Active Directory|  
|"No se puede encontrar un servidor disponible en la * <NetBIOS domain name> * dominio que se ejecuta el servicio de Web de Active Directory (ADWS)"|Se muestra cuando se intenta seleccionar un nodo de dominio en la aplicación Centro de administración de Active Directory|  
  
Para solucionar este problema, siga estos pasos:  
  
1.  Validar los servicios de Active Directory Web servicio se inicia en al menos un controlador de dominio en el dominio (y preferiblemente todos los controladores de dominio del bosque). Asegúrate de que se ha establecido para iniciarse automáticamente en todos los controladores de dominio.  
  
2.  Desde el equipo con el centro de administración de Active Directory, comprueba que puedes encontrar un servidor que ejecuta ADWS ejecutando estos comandos NLTest.exe:  
  
    ```  
    nltest /dsgetdc:<domain NetBIOS name> /ws /force   
    nltest /dsgetdc:<domain fully qualified DNS name> /ws /force  
  
    ```  
  
    Si se produce un error en esas pruebas aunque está ejecutando el servicio ADWS, el problema está relacionado con la resolución de nombres o LDAP y no ADWS o centro de administración de Active Directory. Esta prueba se produce el error "1355 0x54B ERROR_NO_SUCH_DOMAIN" si ADWS no se ejecuta en los controladores de dominio no obstante, por lo tanto, ¡revisa dos veces antes de llegar a cualquier conclusiones.  
  
3.  En el controlador de dominio devuelto por NLTest, la lista de puerto escucha con el comando de volcado:  
  
    ```  
    Netstat -anob > ports.txt  
  
    ```  
  
    Examina el archivo ports.txt y validar que el servicio ADWS está escuchando en el puerto 9389. Ejemplo:  
  
    ```  
    TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    TCP    [::]:9389       [::]:0       LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    ```  
  
    Si escuchas, validar las reglas de Firewall de Windows y asegúrate de que le permiten 9389 TCP entrantes. De manera predeterminada, los controladores de dominio habilitan regla de firewall "Active Directory Web Services (TCP de entrada)". Si no se escucha, volver a validar que el servicio se ejecuta en este servidor y reiniciarlo. Valida que ningún otro proceso ya está escuchando en el puerto 9389.  
  
4.  Instalar NetMon u otra utilidad de captura de red en el equipo que ejecuta el centro de administración de Active Directory y en el controlador de dominio devuelto por NLTEST. Recopilar red simultáneas captura desde ambos equipos, donde puedes iniciar el centro de administración de Active Directory y muestran el error antes de detener la captura. Validar que el cliente es capaz de enviar y recibir desde el controlador de dominio en el puerto TCP 9389. Si los paquetes se envían pero nunca llegan o llegan y las respuestas de controlador de dominio, pero nunca llegue al cliente, es probable que haya un firewall entre los equipos de la red pierde paquetes en ese puerto. Este servidor puede ser software o hardware y puede formar parte de terceros endpoint protection software antivirus.  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problemas conocidos o probable y escenarios de soporte técnico  
El problema sólo probable con el centro de administración de Active Directory es la incapacidad para conectarse para el servicio de Web de directorio activo (ADWS) ejecuta en un controlador de dominio de Windows Server 2012 o Windows Server 2008 R2.  
  
## <a name="see-also"></a>Consulta también  
[Introducción a las mejoras del centro de administración de Active Directory & #40; Nivel 100 & #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
  

