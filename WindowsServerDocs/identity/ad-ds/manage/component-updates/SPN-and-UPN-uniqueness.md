---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Unicidad de SPN y UPN
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 13259f7f12a37c4ceb8bdd2e35ae2fe131ec35cf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442816"
---
# <a name="spn-and-upn-uniqueness"></a>Unicidad de SPN y UPN

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, jefe ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
Controladores de dominio ejecutan Windows Server 2012 R2 bloquear la creación de duplicar los nombres principales de servicio (SPN) y nombres principales de usuario (UPN). Esto incluye si la restauración o la reanimación de un objeto eliminado o el cambio de nombre de un objeto daría como resultado un duplicado.  
  
### <a name="background"></a>Background  
Duplicados nombres principales de servicio (SPN) con frecuencia se producen y dar lugar a errores de autenticación y puede provocar que el uso excesivo de CPU de LSASS. No hay ningún método en el cuadro para bloquear la adición de un SPN duplicado o UPN. *  
  
Valores duplicados de UPN romper la sincronización entre local AD y Office 365.  
  
Se suele utilizar para crear un nuevo SPN *Setspn.exe y funcionalmente se creó en la versión publicada con Windows Server 2008, que agrega una comprobación de duplicados.  
  
**Tabla SEQ tabla \\ \* ARABIC 1: Unicidad SPN como UPN**  
  
|Característica|Comentario|  
|-----------|-----------|  
|Unicidad UPN|Sincronización de salto UPN duplicado de AD cuentas locales con servicios basados en AD de Windows Azure, como Office 365.|  
|Unicidad SPN|Kerberos requiere el SPN para la autenticación mutua.  SPN duplicados dar lugar a errores de autenticación.|  
  
Para obtener más información acerca de los requisitos de unicidad de SPN y UPN, consulte [restricciones de unicidad](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Síntomas  
Códigos de error 8467 o 8468 o sus simbólico, hex o equivalentes de cadena se registran en pantalla en varias diálogos y en el evento 2974 ID en el registro de eventos de servicios de directorio. Error al intentar crear un UPN o SPN duplicados se bloquea sólo en las siguientes circunstancias:  
  
-   La operación de escritura se procesa mediante un controlador de dominio de Windows Server 2012 R2  
  
**Tabla SEQ tabla \\ \* ARABIC 2: Códigos de error de unicidad SPN como UPN**  
  
|Decimal|Hex|Simbólico|Cadena|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|Error en la operación porque el valor SPN proporcionado para la adición o modificación no es único para todo el bosque.|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|Error en la operación porque el valor UPN proporcionado para la adición o modificación no es único para todo el bosque.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Error al crear el nuevo usuario si UPN no es único  
  
### <a name="dsamsc"></a>DSA.msc  
El nombre de inicio de sesión de usuario que ha elegido ya está en uso en la empresa. Elija otro nombre de inicio de sesión y, a continuación, vuelva a intentarlo.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificar una cuenta existente:  
  
El nombre de inicio de sesión de usuario especificado ya existe en la empresa. Especifique una nueva, ya sea cambiando el prefijo o seleccionando un sufijo diferente de la lista.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro de administración de Active Directory (DSAC.exe)  
Un intento de crear un nuevo usuario en el centro de administración de Active Directory con un UPN que ya existe producirá el siguiente error.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura SEQ Figure \\ \* ARABIC 1 error en el centro de administración de AD cuando se produce un error en la creación de nuevos usuarios debido a UPN duplicado**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Origen del evento 2974: ActiveDirectory_DomainService  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura SEQ Figure \\ \* árabe 2974 Id. de evento 2 error 8648**  
  
El evento 2974 enumera el valor que se ha bloqueado y una lista de uno o varios objetos (hasta 10) que ya contienen ese valor.  En la ilustración siguiente, puede ver ese valor de atributo UPN **<em>dhunt@blue.contoso.com</em>** ya existe en cuatro otros objetos.  Puesto que esta es una característica nueva en Windows Server 2012 R2, la creación accidental de UPN y el SPN duplicados en un entorno mixto se seguirá produciendo cuando el intento de escritura de procesar de los controladores de dominio de nivel inferior.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura SEQ Figure \\ \* 2974 de evento árabe 3 que muestra todos los objetos que contiene el UPN duplicado**  
  
> [!TIP]  
> Revise con regularidad a 2974s de Id. de evento:  
>   
> -   identificar los intentos de crear SPN o UPN duplicado  
> -   identificar los objetos que ya contienen duplicados  
  
8648 = "Error en la operación porque el valor UPN proporcionado para la adición o modificación no es único para todo el bosque."  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe ha tenido integrados a él desde el lanzamiento de Windows Server 2008 de detección de SPN duplicados cuando se usa el **"-S"** opción.  Puede omitir la detección de duplicados SPN utilizando la **"-A"** opción sin embargo.  Creación de un SPN duplicado se bloquea cuando el destino es un DC Windows Server 2012 R2 mediante SetSPN con la opción - A.  El mensaje de error mostrado es la misma que la muestra al usar la opción -S: "Un SPN duplicado encontrada, anula la operación"  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura SEQ Figure \\ \* ARABIC 4 mensaje de Error en ADSIEdit cuando se bloquea la adición de UPN duplicado**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS ejecutando de Server 2012 como destino un controlador de dominio de Windows Server 2012 R2:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe que se ejecutan en Windows Server 2012 como destino un controlador de dominio de Windows Server 2012 R2:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura SEQ Figure \\ \* error de creación de usuario árabe DSAC 5 en no Windows Server 2012 R2 mientras selecciona como destino el controlador de dominio de Windows Server 2012 R2**  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura SEQ Figure \\ \* error de modificación de usuario árabe DSAC 6 en no Windows Server 2012 R2 mientras selecciona como destino el controlador de dominio de Windows Server 2012 R2**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Se produce un error en la restauración de un objeto que daría lugar a un UPN duplicado:  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Se registra ningún evento cuando se produce un error en un objeto restaurar debido a un UPN duplicado o SPN.  
  
El UPN del objeto debe ser único en orden para que pueda restaurarse.  
  
1.  Identificar el UPN que existe en el objeto en la Papelera de reciclaje  
  
2.  Identificar todos los objetos que tienen el mismo valor  
  
3.  Quite los duplicados UPN(s)  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identificar el UPN en conflicto en el repadmin.exe información eliminada  
  
```  
Repadmin /showattr DCName "DN of deleted objects container" /subtree /filter:"(msDS-LastKnownRDN=<NAME>)" /deleted /atts:userprincipalname  
```  
  
```  
repadmin /showattr DCName "CN=Deleted Objects,DC=blue,DC=contoso,DC=com" /subtree /filter:"(msDS-LastKnownRDN=Dianne Hunt2)" /deleted /atts:userprincipalname  
  
C:\>repadmin /showattr winbluedc1 "cn=deleted objects,dc=blue,dc=contoso,dc=com" /subtree /filter:"(msds-lastknownrdn=Dianne Hunt2)" /deleted /atts:userprincipalname  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Object  
s,DC=blue,DC=contoso,DC=com  
    1> userPrincipalName: dhunt@blue.contoso.com  
```  
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Para identificar todos los objetos con el mismo UPN: uso de Repadmin.exe  
  
```  
repadmin /showattr WinBlueDC1 "DC=blue,DC=contoso,DC=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
  
C:\>repadmin /showattr winbluedc1 "dc=blue,dc=contoso,dc=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
DN: CN=Administrator,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser1,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser10,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser100,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt,OU=Marketing,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Objects,DC=blue,DC=contoso,DC=com  
```  
  
> [!TIP]  
> El no documentado anteriormente **o eliminado** parámetro en repadmin.exe se usa para incluir los objetos eliminados en el conjunto de resultados  
  
### <a name="using-global-search"></a>Con búsqueda Global  
  
-   Centro de administración de Active Directory de abierto y vaya a **búsqueda Global**  
  
-   Seleccione el **convertir en LDAP** botón de radio  
  
-   Type **(userPrincipalName=*ConflictingUPN*)**  
  
    -   Reemplace ***ConflictingUPN*** con el UPN real que está en conflicto  
  
-   Seleccione **aplicar**  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Uso de Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Si el objeto debe restaurarse, debe quitar los UPN duplicados de los demás objetos.  Para un solo objeto, es lo suficientemente sencilla para usar ADSIEdit para quitar el duplicado.  Si hay varios objetos con duplicados, Windows PowerShell podría ser la mejor herramienta para utilizar.  
  
En null, el atributo UserPrincipalName con Windows PowerShell:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> El atributo userPrincipalName es el atributo de valor único, por lo que este procedimiento sólo quitará el UPN duplicado.  
  
### <a name="duplicate-spn"></a>SPN duplicado  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura SEQ Figure \\ \* árabe 8 mensaje de Error en ADSIEdit cuando se bloquea la adición de SPN duplicado**  
  
Registra en los servicios de directorio de registro de eventos es un **ActiveDirectory_DomainService** Id. de evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura SEQ Figure \\ \* árabe Error 9 registra cuando se bloquea la creación de un SPN duplicado**  
  
### <a name="workflow"></a>Flujo de trabajo  
  
-   **Si DC == GC**  
  
    -   Ninguna llamada offbox necesario, se pueden satisfacer consultas localmente  
  
    -   ***Caso UPN***  
  
        -   Índice de UPN de todo el bosque local de consulta de nombre UPN proporcionado (*userPrincipalName; un índice global*)  
  
            -   Si se devuelven las entradas == 0 -> continúa de escritura  
  
            -   Si se devuelven las entradas! = 0 -> se produce un error de escritura  
  
                -   Evento registrado  
  
                -   También devuelve el error extendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Índice de SPN de todo el bosque local de consulta para un SPN proporcionado (*servicePrincipalName; un índice global*)  
  
            -   Si se devuelven las entradas == 0 -> continúa de escritura  
  
            -   Si se devuelven las entradas! = 0 -> se produce un error de escritura  
  
                -   Evento registrado  
  
                -   También devuelve el error extendido:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Si DC! = GC**  
  
    -   Llamada Offbox **deseable** pero no es crítica, es decir, se trata de una comprobación de unicidad de mejor esfuerzo  
  
        -   Comprobación se realiza en el DIT local solo si no se encuentra el catálogo global  
  
        -   Evento iniciado para poder indicar como  
  
    -   ***Caso UPN***  
  
        -   ¿Enviar consulta LDAP en el catálogo global más cercano? índice de UPN de todo el bosque de consulta del GC para UPN proporcionado (*userPrincipalName; un índice global*)  
  
            -   Si se devuelven las entradas == 0 -> continúa de escritura  
  
            -   Si se devuelven las entradas! = 0 -> se produce un error de escritura  
  
                -   Evento registrado  
  
                -   También devuelve el error extendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   ¿Enviar consulta LDAP en el catálogo global más cercano? índice de consulta del GC para todo el bosque SPN para un SPN proporcionado (*servicePrincipalName; un índice global*)  
  
            -   Si se devuelven las entradas == 0 -> continúa de escritura  
  
            -   Si se devuelven las entradas! = 0 -> se produce un error de escritura  
  
                -   Evento registrado  
  
                -   También devuelve el error extendido:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Cuando vuelve a animado objetos eliminados, se comprueban los valores SPN o UPN presentes de unicidad. Si se encuentra un duplicado, se produce un error en la solicitud.  
  
-   Para determinados cambios de atributo como el nombre de Host de DNS, etcetera de nombre de cuenta SAM, cuando se realiza la modificación, los SPN se actualizan en consecuencia. En el proceso, se eliminan los SPN obsoletos y se construyen SPN nuevos y se agrega a la base de datos. Las modificaciones de atributo necesario en la que se desencadena esta ruta de acceso son:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Si cualquiera del nuevo valor SPN es un duplicado, se producirá un error en la modificación. De la lista anterior, los atributos importantes son ATT_DNS_HOST_NAME (nombre de equipo) y ATT_SAM_ACCOUNT_NAME (nombre de cuenta SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Pruebe lo siguiente: Exploración de unicidad de SPN y UPN  
Este es el primero de varios "**pruebe esto**" actividades en el módulo.  No hay una guía de laboratorio independiente para este módulo.  El **pruebe esto** las actividades son básicamente las actividades de forma libre que permiten explorar el material de la lección en el entorno de laboratorio.  Tiene la opción de seguir el símbolo del sistema o quede fuera de secuencia de comandos y proponer su propia actividad.  
  
> [!NOTE]  
> -   Este es el primero de varios "**pruebe esto**" actividades.  
> -   No hay una guía de laboratorio independiente para este módulo.  
> -   El **pruebe esto** las actividades son básicamente las actividades de forma libre que permiten explorar el material de la lección en el entorno de laboratorio.  
> -   Tiene la opción de seguir el símbolo del sistema o quede fuera de secuencia de comandos y proponer su propia actividad.  
> -   Mientras que todas las secciones no tienen un **pruebe esto** símbolo del sistema, se recomienda seguir para explorar el contenido de la lección en el laboratorio en su caso.  
  
Experimente con la unicidad de SPN y UPN.  Siga estas indicaciones o completar su propia.  
  
1.  Creación de nuevos usuarios con el UPN  
  
2.  Crear cuentas con SPN  
  
3.  Cree un nuevo usuario con un UPN definido anteriormente ya o cambiar el UPN de una cuenta existente.  Lo mismo para un SPN en otra cuenta  
  
    1.  Rellenar una cuenta de usuario con un UPN ya está en uso  
  
        1.  Mediante el centro administrativo de PowerShell, ADSIEDIT o Active Directory (DSAC.exe)  
  
    2.  Rellenar una cuenta existente con un SPN ya está en uso  
  
        1.  Uso de Windows PowerShell, ADSIEDIT o SetSPN  
  
4.  Observe los errores  
  
**Opcionalmente**  
  
1.  Compruebe con el instructor en aula que sirve para habilitar la *[Papelera de reciclaje de AD](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* en el centro de administración de Active Directory.  Si es así, mueva al siguiente paso.  
  
2.  Rellenar el UPN en una cuenta de usuario  
  
3.  Eliminar la cuenta  
  
4.  Rellenar una cuenta diferente con el mismo UPN como la cuenta eliminada  
  
5.  Intente utilizar la GUI de la Papelera de reciclaje para restaurar la cuenta  
  
6.  Imagine que simplemente se hayan presentado con el error que ve en el paso anterior.  (y no tiene un historial de los pasos que acaba de realizar) El objetivo es completar la restauración de la cuenta.  Consulte que el libro de ejemplo por los pasos.  
  


