---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Exclusividad SPN y UPN
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 81d686c81082ad29384585d541c1304d654e1924
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="spn-and-upn-uniqueness"></a>Exclusividad SPN y UPN

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Justin Turner, Senior ingeniero de soporte técnico con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de soporte técnico al cliente de Microsoft y está destinada a arquitectos de sistemas que buscan explicaciones técnicas más profundos de características y soluciones de Windows Server 2012 R2 que normalmente que proporcionan los temas en TechNet y administradores experimentados. Sin embargo, no ha sufrido las mismas fases de edición, forma parte del lenguaje que parezca que menos refinada que lo que normalmente se encuentra en TechNet.  
  
## <a name="overview"></a>Introducción  
Controladores de dominio ejecutan Windows Server 2012 R2 bloque la creación de duplican los nombres de entidad de seguridad de servicio (SPN) y el nombre principal de usuario (UPN). Esto incluye si el restablecimiento o la reactivación de un objeto eliminado o el cambio de nombre de un objeto produciría un duplicado.  
  
### <a name="background"></a>En segundo plano  
Normalmente duplicados nombres principales de servicio (SPN) se producen y producir errores de autenticación y puede ocasionar uso excesivo de CPU de LSASS. No hay ningún método en el cuadro para bloquear la adición de un duplicado SPN o el UPN. *  
  
Los valores UPN duplicados interrupción la sincronización entre locales AD y Office 365.  
  
*Setspn.exe se usa normalmente para crear un nuevo SPN y funcionalmente se creó en la versión publicada con Windows Server 2008 que agrega una comprobación para duplicados.  
  
**Tabla de tabla SEQ \\\ * árabe 1: exclusividad UPN y SPN**  
  
|Característica|Comentario|  
|-----------|-----------|  
|Exclusividad UPN|Sincronización de salto UPN duplicados de locales cuentas AD con Windows Azure AD servicios como Office 365.|  
|Exclusividad SPN|Kerberos requiere SPN para la autenticación mutua.  SPN duplicados producir errores de autenticación.|  
  
Para obtener más información sobre los requisitos de exclusividad de UPN y SPN, consulta [restricciones de exclusividad](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Síntomas  
Códigos de error 8467 o 8468 o sus hex, simbólico o equivalentes de cadena se registran en pantalla en diversos cuadros de diálogo y en 2974 de identificador de evento en el registro de eventos de servicios de directorio. Se bloquea al intentar crear un duplicado UPN o SPN solo en las siguientes circunstancias:  
  
-   La escritura se procesa mediante un controlador de dominio de Windows Server 2012 R2  
  
**Tabla de tabla SEQ \\\ * 2 árabe: códigos de error de exclusividad UPN y SPN**  
  
|Decimal|Hex|Simbólicos|Cadena|  
|-----------|-------|------------|----------|  
|8467|21C 7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|Error en la operación porque el valor SPN proporcionado para la adición o modificación no es todo el bosque único.|  
|8648|21C 8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|Error en la operación porque el valor UPN proporcionado para la adición o la modificación no es todo el bosque único.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Creación de un usuario nuevo se produce un error si no es único UPN  
  
### <a name="dsamsc"></a>DSA.msc  
El nombre de inicio de sesión de usuario que ha elegido ya está en uso en la empresa. Elige otro nombre de inicio de sesión y vuelve a intentarlo.  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificar una cuenta existente:  
  
El nombre de inicio de sesión de usuario especificado ya existe en la empresa. Especificar una nueva, o bien cambiando el prefijo o seleccionando un sufijo diferente de la lista.  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro de administración de Active Directory (DSAC.exe)  
Intentar crear un nuevo usuario en el centro de administración de Active Directory con un UPN que ya existe dará como resultado el siguiente mensaje de error.  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura SEQ ilustración \\\ * error árabe 1 muestra en el centro de administración de anuncios cuando se produce un error en la creación de un usuario nuevo debido a UPN duplicado**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Origen del evento 2974: ActiveDirectory_DomainService  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura SEQ ilustración \\\ * árabe 2974 de identificador de evento 2 con error 8648**  
  
El evento 2974 muestra el valor que estaba bloqueado y una lista de uno o más objetos (hasta 10) que contenga ese valor.  En la siguiente figura, puedes ver ese valor de atributo UPN *** dhunt@blue.contoso.com *** ya existe en cuatro otros objetos.  Dado que esto es una nueva característica de Windows Server 2012 R2, creación accidental de duplicados UPN y SPN en un entorno aún ocurrirá cuando el intento de escritura de procesar de controladores de dominio de nivel inferior.  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura SEQ ilustración \\\ * árabe 2974 de evento 3 que muestra todos los objetos que contiene el UPN duplicado**  
  
> [!TIP]  
> Revisa periódicamente a 2974s de identificador de evento:  
>   
> -   identificar los intentos para crear duplicado UPN o el SPN  
> -   identificar objetos que contienen ya duplicados  
  
8648 = "Error en la operación porque el valor UPN proporcionado para la adición o la modificación no es todo el bosque único."  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe ha tenido integrado a ella desde el lanzamiento de Windows Server 2008 de detección de SPN duplicados cuando uses la **"-S"** opción.  Puedes omitir la detección de SPN duplicados mediante el uso de la **"-A"** opción pero.  Creación de un SPN duplicado se bloquea cuando tienen como destino un DC Windows Server 2012 R2 con SetSPN con la opción - A.  El mensaje de error que se muestra es el mismo que el que aparece cuando se usa la opción -S: "Duplicar SPN encuentra, anulando operación!"  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura SEQ ilustración \\\ * árabe 4 mensaje de Error en ADSIEdit cuando se bloquea la adición de UPN duplicado**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS ejecutando desde Server 2012 destinadas a un controlador de dominio de Windows Server 2012 R2:  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe ejecutándose en Windows Server 2012 destinadas a un controlador de dominio de Windows Server 2012 R2:  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura SEQ ilustración \\\ * error al crear el usuario árabe DSAC 5 en no - Windows Server 2012 R2 mientras destinados a Windows Server 2012 R2 DC**  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura SEQ ilustración \\\ * error modificación de usuario árabe DSAC 6 en no - Windows Server 2012 R2 mientras destinados a Windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Se produce un error en la restauración de un objeto que produciría un UPN duplicado:  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
No se registra ningún evento cuando se produce un error en un objeto restaurar debido a un UPN duplicado / SPN.  
  
El UPN del objeto debe ser único en orden para que pueda restaurarse.  
  
1.  Identificar el UPN que existe en el objeto de la Papelera de reciclaje  
  
2.  Identificar todos los objetos que tienen el mismo valor  
  
3.  Quitar el UPN(s) duplicados  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identificar el UPN en conflicto en la repadmin.exe información eliminados  
  
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
> Previamente no **/ eliminan** parámetro repadmin.exe se usa para incluir objetos eliminados en el conjunto de resultados  
  
### <a name="using-global-search"></a>Usar la búsqueda Global  
  
-   Centro de administración de abrir Active Directory y navegar a **búsqueda Global**  
  
-   Selecciona el **convertir a LDAP** botón de radio  
  
-   Tipo * *(userPrincipalName =*ConflictingUPN*) **  
  
    -   Reemplazar ***ConflictingUPN*** con el UPN real que está en conflicto  
  
-   Selecciona **aplicar**  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Uso de Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Si el objeto necesita restaurarse, necesitas quitará el UPN duplicados de los otros objetos.  Para un solo objeto, es fácil usar ADSIEdit para quitar el duplicado.  Si hay varios objetos con duplicados, Windows PowerShell puede ser la mejor herramienta para usar.  
  
En "null", el atributo UserPrincipalName mediante Windows PowerShell:  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> El atributo userPrincipalName es un solo valor de atributo, por lo que este procedimiento solo quitará el UPN duplicado.  
  
### <a name="duplicate-spn"></a>Duplicar SPN  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura SEQ ilustración \\\ * árabe 8 mensaje de Error en ADSIEdit cuando se bloquea la adición de SPN duplicado**  
  
Registra en los servicios de directorio, registro de eventos es una **ActiveDirectory_DomainService** identificador de evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Exclusividad SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura SEQ ilustración \\\ * árabe Error de 9 ha iniciado cuando se bloquea la creación de SPN duplicado**  
  
### <a name="workflow"></a>Flujo de trabajo  
  
-   **Si DC == GC**  
  
    -   Ninguna llamada remoto es necesario, se puede satisfacer consulta localmente  
  
    -   ***Caso UPN***  
  
        -   Índice UPN de todo el bosque local de consulta for UPN proporcionado (*userPrincipalName; un índice global*)  
  
            -   Si devuelven entradas == 0 -> ganancias de escritura  
  
            -   Si devuelven entradas! = 0 -> se produce un error de escritura  
  
                -   Registra el evento  
  
                -   También se devuelve un error extendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   Índice SPN de todo el bosque local de consulta para el SPN proporcionado (*principal de servicio; un índice global*)  
  
            -   Si devuelven entradas == 0 -> ganancias de escritura  
  
            -   Si devuelven entradas! = 0 -> se produce un error de escritura  
  
                -   Registra el evento  
  
                -   También se devuelve un error extendido:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Si DC! = GC**  
  
    -   Llamada remoto **deseable** pero no crítica, es decir, se trata de una comprobación de exclusividad mejor esfuerzo  
  
        -   Ganancias por la verificación contra DIT local solo si no se encuentra el catálogo global  
  
        -   Eventos registrado para indicar por ejemplo  
  
    -   ***Caso UPN***  
  
        -   ¿Enviar la consulta LDAP GC más cercana? índice UPN de todo el bosque de consulta de GC for UPN proporcionado (*userPrincipalName; un índice global*)  
  
            -   Si devuelven entradas == 0 -> ganancias de escritura  
  
            -   Si devuelven entradas! = 0 -> se produce un error de escritura  
  
                -   Registra el evento  
  
                -   También se devuelve un error extendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso SPN***  
  
        -   ¿Enviar la consulta LDAP GC más cercana? índice SPN de todo el bosque de consulta de GC de SPN proporcionado (*principal de servicio; un índice global*)  
  
            -   Si devuelven entradas == 0 -> ganancias de escritura  
  
            -   Si devuelven entradas! = 0 -> se produce un error de escritura  
  
                -   Registra el evento  
  
                -   También se devuelve un error extendido:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Cuando vuelva a animado objetos eliminados, se comprueban los valores SPN o UPN presentes para comprobar su exclusividad. Si se encuentra un duplicado, se produce un error en la solicitud.  
  
-   Para determinados cambios de atributo como nombre de Host DNS, etcetera de nombre de cuenta SAM, cuando se realiza la modificación, SPN se actualizan según corresponda. En el proceso, se eliminan los SPN obsoletos y se construye SPN nuevos y se agregan a la base de datos. Las modificaciones necesarias atributo con respecto al cual se desencadena esta ruta de acceso son:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Si cualquiera del nuevo valor SPN un duplicado, se produce un error la modificación. De la lista anterior, los atributos importantes son ATT_DNS_HOST_NAME (nombre de equipo) y ATT_SAM_ACCOUNT_NAME (nombre de cuentas SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Lo prueba siguiente: Explorar exclusividad SPN y UPN  
Este es el primero de varios "**probar**" actividades en el módulo.  No es una guía de laboratorio independiente para este módulo.  La **probar** las actividades son básicamente actividades de forma libre que permiten explorar el material lección en el entorno de laboratorio.  Tienes la opción de siguiendo el símbolo del sistema o salga de script y logras introducir tu propia actividad.  
  
> [!NOTE]  
> -   Este es el primero de varios "**probar**" actividades.  
> -   No es una guía de laboratorio independiente para este módulo.  
> -   La **probar** las actividades son básicamente actividades de forma libre que permiten explorar el material lección en el entorno de laboratorio.  
> -   Tienes la opción de siguiendo el símbolo del sistema o salga de script y logras introducir tu propia actividad.  
> -   Si bien no todas las secciones tienen una **probar** símbolo del sistema, se recomienda seguir para explorar el contenido de lección en el laboratorio donde corresponda.  
  
Experimenta con SPN y UPN exclusividad.  Sigue estas instrucciones o completar tus propios.  
  
1.  Crear nuevos usuarios con UPN  
  
2.  Crear cuentas con SPN  
  
3.  Crear un nuevo usuario con un UPN definido ya previamente o cambiar UPN de la cuenta existente.  Haz lo mismo para un SPN en otra cuenta  
  
    1.  Rellenar una cuenta de usuario con un UPN ya está en uso  
  
        1.  Usar el centro de administración PowerShell, ADSIEDIT o Active Directory (DSAC.exe)  
  
    2.  Rellenar una cuenta existente con un SPN ya está en uso  
  
        1.  Con Windows PowerShell, ADSIEDIT o SetSPN  
  
4.  Observar los errores  
  
**Opcionalmente**  
  
1.  Comprueba con el instructor las clases que ocurre nada para habilitar la * [AD Papelera de reciclaje](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin) * en el centro de administración de Active Directory.  Si es así, se mueve el paso siguiente.  
  
2.  Rellenar el UPN en una cuenta de usuario  
  
3.  Elimina la cuenta  
  
4.  Rellenar una cuenta distinta con la misma UPN como la cuenta eliminada  
  
5.  Tratar de usar la GUI de Papelera de reciclaje para recuperar la cuenta  
  
6.  Imagina que solo se presenten con el error que ves en el paso anterior.  (y no tiene un historial de los pasos que ha realizado) Tu objetivo es completar la restauración de la cuenta.  Consulta que el libro por ejemplo los pasos.  
  


