---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Unicidad de SPN y UPN
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: bca2934b5b691f69fc70cd9d5230a2865b24ac94
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966427"
---
# <a name="spn-and-upn-uniqueness"></a>Unicidad de SPN y UPN

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Autor**: Diego Turner, Ingeniero de soporte técnico de nivel superior con el grupo de Windows  
  
> [!NOTE]  
> Este contenido está escrito por un ingeniero de asistencia al cliente de Microsoft y está destinado a los arquitectos de sistemas y administradores con experiencia que están buscando explicaciones técnicas más detalladas de características y soluciones de Windows Server 2012 R2 que los temas que se suelen proporcionar en TechNet. Sin embargo, no ha experimentado los mismos pasos de edición, por lo que parte del lenguaje puede parecer menos perfeccionado de lo que se encuentra normalmente en TechNet.  
  
## <a name="overview"></a>Información general  
Los controladores de dominio que ejecutan Windows Server 2012 R2 bloquean la creación de nombres de entidad de seguridad de servicio (SPN) y nombres principales de usuario (UPN) duplicados. Esto incluye si la restauración o la reanimación de un objeto eliminado o el cambio de nombre de un objeto daría como resultado un duplicado.  
  
### <a name="background"></a>Fondo  
Los nombres principales de servicio (SPN) duplicados se producen normalmente y dan lugar a errores de autenticación y pueden provocar un uso excesivo de la CPU de LSASS. No hay ningún método en la caja para bloquear la adición de un SPN o un UPN duplicados. *  
  
Los valores de UPN duplicados interrumpen la sincronización entre AD local y Office 365.  
  
* Setspn.exe se usa normalmente para crear nuevos SPN y se integra de forma funcional en la versión publicada con Windows Server 2008 que agrega una comprobación de duplicados.  
  
**Tabla SEQ \\ \* árabe 1: singularidad de UPN y SPN**  
  
|Característica|Comentario|  
|-----------|-----------|  
|Singularidad de UPN|Los UPN duplicados interrumpen la sincronización de las cuentas de AD locales con los servicios basados en Windows Azure AD, como Office 365.|  
|Unicidad de SPN|Kerberos requiere SPN para la autenticación mutua.  Los SPN duplicados producen errores de autenticación.|  
  
Para obtener más información sobre los requisitos de unicidad de los UPN y los SPN, consulte [restricciones de unicidad](/openspecs/windows_protocols/ms-adts/3c154285-454c-4353-9a99-fb586e806944).  
  
## <a name="symptoms"></a>Síntomas  
Los códigos de error 8467 o 8468 o sus equivalentes hexadecimales, simbólicos o de cadena se registran en varios cuadros de diálogo en pantalla y en el ID. de evento 2974 en el registro de eventos de servicios de directorio. El intento de crear un UPN o un SPN duplicado solo está bloqueado en las siguientes circunstancias:  
  
-   La escritura la procesa un controlador de dominio de Windows Server 2012 R2  
  
**Tabla SEQ \\ \* letra 2: códigos de error de unicidad de UPN y SPN**  
  
|Decimal|Hex|Simbólicos|String|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|No se pudo realizar la operación porque el valor SPN proporcionado para la adición o modificación no es único en todo el bosque.|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|Se produjo un error en la operación porque el valor de UPN proporcionado para la adición o modificación no es único en todo el bosque.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Se produce un error en la creación de nuevos usuarios si el UPN no es único  
  
### <a name="dsamsc"></a>DSA. msc  
El nombre de inicio de sesión de usuario que ha elegido ya está en uso en esta empresa. Elija otro nombre de inicio de sesión e inténtelo de nuevo.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modificar una cuenta existente:  
  
El nombre de inicio de sesión de usuario especificado ya existe en la empresa. Especifique uno nuevo, bien cambiando el prefijo o seleccionando un sufijo diferente de la lista.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centro de administración de Active Directory (DSAC.exe)  
Si se intenta crear un nuevo usuario en Centro de administración de Active Directory con un UPN que ya existe, se producirá el siguiente error.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figura SEQ figura el \\ \* error árabe 1 mostrado en el centro de administración de ad cuando se produce un error en la creación de nuevos usuarios debido a un UPN duplicado**  
  
### <a name="event-2974-source-activedirectory_domainservice"></a>Origen del evento 2974: ActiveDirectory_DomainService  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figura SEQ figura \\ \* árabe 2 evento ID 2974 con el error 8648**  
  
El evento 2974 muestra el valor que se ha bloqueado y una lista de uno o más objetos (hasta 10) que ya contienen ese valor.  En la siguiente ilustración, puede ver que el valor del atributo UPN **<em>dhunt@blue.contoso.com</em>** ya existe en otros cuatro objetos.  Dado que se trata de una nueva característica de Windows Server 2012 R2, la creación accidental de UPN duplicados y SPN en un entorno mixto se seguirá produciendo cuando los DC de nivel inferior procesen el intento de escritura.  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figura SEQ figura \\ \* árabe 3 evento 2974 que muestra todos los objetos que contienen el UPN duplicado**  
  
> [!TIP]  
> Revise el ID. de evento 2974s con regularidad para:  
>   
> -   identificar los intentos de crear UPN duplicados o SPN  
> -   identificar objetos que ya contienen duplicados  
  
8648 = "error en la operación porque el valor de UPN proporcionado para la adición o modificación no es único en todo el bosque".  
  
### <a name="setspn"></a>SetSPN  
Setspn.exe tiene una detección de SPN duplicada en ella desde la versión 2008 de Windows Server al usar la opción **"-S"** .  Sin embargo, puede omitir la detección de SPN duplicada mediante la opción **"-a"** .  La creación de un SPN duplicado se bloquea cuando el destino es un controlador de dominio de Windows Server 2012 R2 con SetSPN con la opción-A.  El mensaje de error que se muestra es el mismo que se muestra cuando se usa la opción-S: "se encontró un SPN duplicado, anulando la operación".  
  
### <a name="adsiedit"></a>ADSIEdit  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figura SEQ figura el \\ \* mensaje de error árabe 4 mostrado en ADSIEdit cuando se ha bloqueado la adición de UPN duplicado**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS que se ejecuta desde el servidor 2012 como destino de un DC de Windows Server 2012 R2:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe que se ejecutan en Windows Server 2012 como destino de un controlador de dominio de Windows Server 2012 R2:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figura SEQ figura \\ \* DSAC error de creación de usuario en un servidor que no es windows Server 2012 R2 al tener como destino windows Server 2012 R2 DC**  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figura SEQ figura \\ \* árabe 6 DSAC error de modificación de usuario en un servidor que no es windows Server 2012 R2 mientras el destino es windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Se produce un error en la restauración de un objeto que daría lugar a un UPN duplicado:  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
No se registra ningún evento cuando un objeto no se puede restaurar debido a un UPN o SPN duplicado.  
  
El UPN del objeto debe ser único para poder restaurarlo.  
  
1.  Identificar el UPN que existe en el objeto en la papelera de reciclaje  
  
2.  Identificar todos los objetos que tienen el mismo valor  
  
3.  Quitar los UPN duplicados  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identifique el UPN conflictivo en el objectUsing eliminado repadmin.exe  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Para identificar todos los objetos con el mismo UPN: mediante Repadmin.exe  
  
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
> El parámetro **/Deleted** no documentado anteriormente en repadmin.exe se usa para incluir los objetos eliminados en el conjunto de resultados.  
  
### <a name="using-global-search"></a>Uso de la búsqueda global  
  
-   Abra Centro de administración de Active Directory y navegue hasta la **búsqueda global**  
  
-   Seleccione el botón de radio **convertir en LDAP**  
  
-   Tipo **(userPrincipalName =*ConflictingUPN*)**  
  
    -   Reemplace ***ConflictingUPN*** por el UPN real que está en conflicto.  
  
-   Seleccione **aplicar**  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Uso de Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Si es necesario restaurar el objeto, deberá quitar los UPN duplicados de los demás objetos.  Para un solo objeto, es lo suficientemente sencillo como para quitar el duplicado.  Si hay varios objetos con duplicados, es posible que Windows PowerShell sea la mejor herramienta para su uso.  
  
Para anular el atributo UserPrincipalName mediante Windows PowerShell:  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> El atributo userPrincipalName es un atributo de un solo valor, por lo que este procedimiento solo quitará el UPN duplicado.  
  
### <a name="duplicate-spn"></a>SPN duplicado  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figura SEQ figura el \\ \* mensaje de error árabe 8 mostrado en ADSIEdit cuando se ha bloqueado la adición del SPN duplicado**  
  
Registrado en el registro de eventos de servicios de directorio es un **ACTIVEDIRECTORY_DOMAINSERVICE** ID. de evento **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Unicidad de SPN y UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figura SEQ ilustración \\ \* 9 error registrado cuando se bloquea la creación de un SPN duplicado**  
  
### <a name="workflow"></a>Flujo de trabajo  
  
-   **Si DC = = GC**  
  
    -   No se requiere ninguna llamada a offbox, la consulta se puede satisfacer localmente  
  
    -   ***Caso de UPN***  
  
        -   Consulta el índice UPN de todo el bosque local para el UPN proporcionado (*userPrincipalName; índice global*)  
  
            -   Si las entradas devueltas = = 0-> escritura continúa  
  
            -   Si se devuelven entradas! = 0-> escritura errónea  
  
                -   Evento registrado  
  
                -   También devuelve un error extendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso de SPN***  
  
        -   Consulta el índice de SPN para todo el bosque local para el SPN proporcionado (*ServicePrincipalName; un índice global*)  
  
            -   Si las entradas devueltas = = 0-> escritura continúa  
  
            -   Si se devuelven entradas! = 0-> escritura errónea  
  
                -   Evento registrado  
  
                -   También devuelve un error extendido:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **If DC! = GC**  
  
    -   Offbox llamada **deseada** pero no crítica, es decir, se trata de una comprobación de unicidad del mejor esfuerzo  
  
        -   La comprobación continúa en el DIT local solo si no se puede encontrar GC  
  
        -   Evento registrado para indicar  
  
    -   ***Caso de UPN***  
  
        -   ¿Enviar consulta LDAP al catálogo global más cercano? consultar el índice UPN de todo el bosque de GC para el UPN proporcionado (*userPrincipalName; índice global*)  
  
            -   Si las entradas devueltas = = 0-> escritura continúa  
  
            -   Si se devuelven entradas! = 0-> escritura errónea  
  
                -   Evento registrado  
  
                -   También devuelve un error extendido:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Caso de SPN***  
  
        -   ¿Enviar consulta LDAP al catálogo global más cercano? consultar el índice de SPN para todo el bosque de GC para el SPN proporcionado (*ServicePrincipalName; un índice global*)  
  
            -   Si las entradas devueltas = = 0-> escritura continúa  
  
            -   Si se devuelven entradas! = 0-> escritura errónea  
  
                -   Evento registrado  
  
                -   También devuelve un error extendido:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Cuando se vuelven a animar los objetos eliminados, se comprueba la unicidad de los valores SPN o UPN presentes. Si se encuentra un duplicado, se produce un error en la solicitud.  
  
-   En el caso de ciertos cambios de atributo como el nombre de host DNS, el nombre de cuenta SAM, etc., cuando se realiza la modificación, los SPN se actualizan en consecuencia. En el proceso, se eliminan los SPN obsoletos y se crean nuevos SPN y se agregan a la base de datos. Las modificaciones de atributo que se deben realizar con respecto a esta ruta de acceso son:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Si alguno de los nuevos valores de SPN es un duplicado, se producirá un error en la modificación. En la lista anterior, los atributos importantes son ATT_DNS_HOST_NAME (nombre del equipo) y ATT_SAM_ACCOUNT_NAME (nombre de cuenta SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Pruebe lo siguiente: explorar la unicidad de SPN y UPN  
Esta es la primera de varias actividades "**pruebe esto**" en el módulo.  No hay ninguna guía de laboratorio independiente para este módulo.  La **prueba de estas** actividades es esencialmente actividades de forma libre que le permiten explorar el material de la lección en el entorno de laboratorio.  Tiene la opción de seguir el símbolo del sistema o salir del script y obtener su propia actividad.  
  
> [!NOTE]  
> -   Esta es la primera de varias actividades "**pruebe esto**".  
> -   No hay ninguna guía de laboratorio independiente para este módulo.  
> -   La **prueba de estas** actividades es esencialmente actividades de forma libre que le permiten explorar el material de la lección en el entorno de laboratorio.  
> -   Tiene la opción de seguir el símbolo del sistema o salir del script y obtener su propia actividad.  
> -   Aunque no todas las secciones tienen una **instrucción try this** , se recomienda explorar el contenido de la lección en el laboratorio cuando sea necesario.  
  
Experimente con la singularidad de los UPN y el SPN.  Siga estas indicaciones o complete las suyas propias.  
  
1.  Crear nuevos usuarios con UPN  
  
2.  Crear cuentas con SPN  
  
3.  Cree un nuevo usuario con un UPN ya definido previamente o cambie el UPN de una cuenta existente.  Hacer lo mismo para un SPN en otra cuenta  
  
    1.  Rellenar una cuenta de usuario existente con un UPN ya en uso  
  
        1.  Usar PowerShell, ADSIEDIT o Centro de administración de Active Directory (DSAC.exe)  
  
    2.  Rellenar una cuenta existente con un SPN ya en uso  
  
        1.  Usar Windows PowerShell, ADSIEDIT o SetSPN  
  
4.  Observe los errores  
  
**Opcionalmente**  
  
1.  Compruebe con el instructor del aula que es correcto habilitar la *[papelera de reciclaje de ad](../../get-started/adac/advanced-ad-ds-management-using-active-directory-administrative-center--level-200-.md#BKMK_EnableRecycleBin)* en centro de administración de Active Directory.  Si es así, vaya al paso siguiente.  
  
2.  Rellenar el UPN en una cuenta de usuario  
  
3.  Eliminación de la cuenta  
  
4.  Rellenar una cuenta diferente con el mismo UPN que la cuenta eliminada  
  
5.  Intentar usar la GUI de la papelera de reciclaje para restaurar la cuenta  
  
6.  Imagine que acaba de presentar el error que se muestra en el paso anterior.  (y no tiene un historial de los pasos que acaba de realizar) El objetivo es completar la restauración de la cuenta.  Vea el libro para ver los pasos de ejemplo.  
  
