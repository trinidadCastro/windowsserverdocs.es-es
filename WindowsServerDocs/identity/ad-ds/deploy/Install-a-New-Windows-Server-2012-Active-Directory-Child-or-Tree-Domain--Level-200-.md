---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: "Instalar un nuevo elemento secundario Active Directory de Windows Server 2012 o dominio del árbol (nivel 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc0eecc44bbc5f7459f22aceb5ebe41cd61948b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>Instalar un nuevo elemento secundario Active Directory de Windows Server 2012 o dominio del árbol (nivel 200)

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema explica cómo agregar dominios secundarios y árbol a un bosque existente de Windows Server 2012, con el administrador del servidor o Windows PowerShell.  
  
-   [Flujo de trabajo de dominio de árbol y secundarios](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [Elemento secundario y árbol dominio Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [Implementación](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>Flujo de trabajo de dominio de árbol y secundarios  
El siguiente diagrama ilustra el proceso de configuración de los servicios de dominio de Active Directory cuando ya habías instalado el rol de AD DS y ha iniciado al dominio servicios de configuración de Asistente para Active Directory mediante el administrador del servidor para crear un nuevo dominio en un bosque existente.  
  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>Elemento secundario y árbol dominio Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|Argumentos (**negrita** argumentos son necesarios. *El formato de cursiva* argumentos pueden especificarse mediante Windows PowerShell o el Asistente para la configuración de AD DS.)|  
|**Instalación AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-Confirmar<br /><br />*-CreateDNSDelegation*<br /><br />***-Credenciales***<br /><br />*Ruta:*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*-DomainMode*<br /><br />***-DomainType***<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-El nombre de sitio*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> La **-credenciales** argumento sólo es necesario cuando no actualmente sesión como miembro del grupo Administradores de empresa. La **- NewDomainNetBIOSName** argumento es obligatorio si quieres cambiar el nombre de 15 caracteres generado automáticamente según el prefijo de nombre de dominio DNS o si el nombre es superior a 15 caracteres.  
  
## <a name="BKMK_Deployment"></a>Implementación  
  
### <a name="deployment-configuration"></a>Configuración de implementación  
Captura de pantalla siguiente muestra las opciones para agregar un dominio secundario:  
  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
Captura de pantalla siguiente muestra las opciones para agregar un dominio de árbol:  
  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
El administrador del servidor comienza cada promoción del controlador de dominio con la **configuración de implementación** página. El resto de opciones y los campos obligatorios cambian en esta página y las páginas siguientes, según qué operación de implementación que seleccione.  
  
En este tema combina dos operaciones discretas: promoción de dominio del hijo y promoción de dominio de árbol. La única diferencia entre las dos operaciones es el tipo de dominio que elijas para crear. Todos los demás pasos son idénticos entre las dos operaciones.  
  
-   Para crear un nuevo dominio secundario, haz clic en **agregar un dominio a un bosque existente** y elige **dominio secundario**. Para **nombre del dominio principal**, escribe o selecciona el nombre del dominio principal. A continuación, escribe el nombre del dominio nuevo en el **nuevo nombre de dominio** cuadro. Proporcionar a un elemento secundario válido, etiqueta única en nombre de dominio; el nombre debe usar los requisitos de nombre de dominio DNS.  
  
-   Para crear un dominio de árbol dentro de un bosque existente, haz clic en **agregar un dominio a un bosque existente** y elige **dominio árbol**. Escribe el nombre de dominio raíz del bosque y, a continuación, escribe el nombre del nuevo dominio. Proporcionar un nombre de dominio raíz válido, completo; el nombre no puede ser la etiqueta única y debe usar los requisitos de nombre de dominio DNS.  
  
Para obtener más información sobre los nombres DNS, consulta [convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264).  
  
El Asistente de configuración de servicios de Active Directory dominio de Server Manager pide las credenciales de dominio si sus credenciales actuales no son del dominio. Haz clic en **cambio** para proporcionar las credenciales de dominio para la operación de promoción.  
  
El cmdlet ADDSDeployment de configuración de implementación y los argumentos son:  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>Opciones del controlador de dominio  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
La **opciones del controlador de dominio** página especifica las opciones de controlador de dominio para el nuevo controlador de dominio. Las opciones de controlador de dominio puede configurar incluyen **servidor DNS** y **catálogo Global**; No puedes configurar el controlador de dominio de solo lectura como el primer controlador de dominio en un dominio nuevo.  
  
Microsoft recomienda que todos los controladores de dominio proporcionan servicios DNS y GC alta disponibilidad en entornos distribuidos. GC siempre está activada de manera predeterminada y DNS está activada de manera predeterminada, si el dominio actual hospeda DNS ya se encuentran en sus controladores de dominio, en función de una consulta de inicio de la entidad. También debes especificar un **nivel funcional del dominio**. El nivel funcional predeterminado es Windows Server 2012, y puedes elegir cualquier otro valor que sea igual o mayor que el nivel funcional del bosque actual.  
  
La **opciones del controlador de dominio** página también te permite elegir la lógica adecuada de Active Directory **nombre del sitio** desde la configuración del bosque. De manera predeterminada, se selecciona el sitio con la subred más correcta. Si hay un único sitio, se activa automáticamente.  
  
> [!IMPORTANT]  
> Si el servidor no pertenece a una subred de Active Directory y no hay más de un sitio de Active Directory, se selecciona nada y la **siguiente** botón no está disponible hasta que haya elegido un sitio desde la lista.  
  
Especificado **contraseña de modo de restauración de servicios de directorio** deben cumplir con la directiva de contraseñas aplicada al servidor. Siempre elegir una contraseña segura y compleja o preferiblemente, una frase de contraseña.  
  
La **opciones del controlador de dominio** ADDSDeployment cmdlet argumentos son:  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> El nombre del sitio debe existir cuando proporcionado como un valor para la **sitio** argumento. La **instalación AddsDomainController** cmdlet no crea los nombres de los sitios. Puedes usar la **nueva adreplicationsite** cmdlet para crear sitios nuevos.  
  
La **instalación ADDSDomainController** cmdlet argumentos siguen los valores predeterminados mismo como el administrador del servidor, si no se especifica.  
  
La **SafeModeAdministratorPassword** operación del argumento es especial:  
  
-   Si *no especifica* como un argumento, el cmdlet te pedirá que escriba y confirme una contraseña enmascarada. Este es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
    Por ejemplo, para crear a un nuevo elemento secundario dominio denominado Norteamérica del bosque Contoso.com y se te pida que escriba y confirme una contraseña enmascarada:  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
    ```  
  
-   Si se especifica *con un valor*, el valor debe ser una cadena segura. No es el uso preferido cuando se ejecuta el cmdlet de manera interactiva.  
  
Por ejemplo, puedes manualmente pedir contraseña utilizando la **Read-Host** cmdlet para pedir al usuario una cadena segura:  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Como la opción anterior confirma la contraseña, Ten mucho cuidado: la contraseña no está visible.  
  
También puedes proporcionar una cadena segura como una variable de texto no cifrado convertida, aunque no se recomienda.  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
Por último, podrías almacenar la contraseña ofuscada en un archivo y, a continuación, volver a utilizarlo más tarde, sin la contraseña de texto no cifrado deja de aparecer. Por ejemplo:  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> No se recomienda proporcionar ni almacenar una contraseña de texto activa o desactiva ofuscados. Cualquier persona que ejecutar este comando en un script o buscando por encima del hombro conozca la contraseña DSRM de ese controlador de dominio.  Cualquier persona que tenga acceso al archivo puede invertir esa contraseña ofuscada. Con estos datos, pueden iniciar sesión en un controlador de dominio iniciado en DSRM y finalmente suplantar el propio controlador de dominio, eleve sus privilegios para el nivel más alto en un bosque de AD. Un conjunto adicional de pasos mediante **System.Security.Cryptography** para cifrar el archivo de texto datos están recomendable pero fuera del ámbito. El procedimiento recomendado es evitar por completo el almacenamiento de contraseña.  
  
El módulo de ADDSDeployment ofrece una opción adicional para omitir la configuración automática de configuración del cliente DNS, servidores de reenvío y sugerencias de raíz. Esto no es configurable al usar el administrador del servidor. Este argumento es importante solo si has instalado el servicio de servidor DNS antes de configurar el controlador de dominio:  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>Opciones de DNS y las credenciales de la delegación de DNS  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
La **opciones DNS** página te permite proporcionar credenciales de administrador de DNS alternativas para la delegación.  
  
Cuando se instala un nuevo dominio en un bosque existente - donde seleccionó la instalación de DNS en la **opciones del controlador de dominio** página - no puede configurar las opciones; la delegación sucede de forma irrevocable y automáticamente. Tienes la opción de proporcionar credenciales administrativas de DNS alternativas con derechos para actualizar esa estructura.  
  
La **opciones DNS** ADDSDeployment Windows PowerShell argumentos son:  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
Para obtener más información acerca de la delegación de DNS, consulta [acerca de la delegación zona](https://technet.microsoft.com/library/cc771640.aspx).  
  
### <a name="additional-options"></a>Opciones adicionales  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
La **opciones adicionales** página muestra el nombre NetBIOS del dominio y te permite invalidarlo. De manera predeterminada, el nombre de dominio NetBIOS coincide con la etiqueta de extremo izquierdo del nombre de dominio completo que se proporciona en el **configuración de implementación** página. Por ejemplo, si se proporciona el nombre de dominio completo Corp.contoso.com, el nombre de dominio NetBIOS predeterminado es Corporation.  
  
Si el nombre es de 15 caracteres o menos y no entre en conflicto con otro nombre NetBIOS, no se ha alterado. Si entre en conflicto con otro nombre NetBIOS, un número se anexa al nombre. Si el nombre es más de 15 caracteres, el asistente proporciona una sugerencia única, truncada. En cualquier caso, el Asistente primero valida el nombre no está en uso a través de una búsqueda WINS y NetBIOS difusión.  
  
Para obtener más información sobre los nombres DNS, consulta [convenciones de nomenclatura en Active Directory para equipos, dominios, sitios y unidades organizativas](https://support.microsoft.com/kb/909264).  
  
La **instalación AddsDomain** argumentos que siguen los valores predeterminados mismo como el administrador del servidor, si no se especifica. La **valor de DomainNetBIOSName** operación es especial:  
  
1.  Si la **NewDomainNetBIOSName** argumento no se especifica con un nombre de dominio de NetBIOS y el nombre de dominio de prefijo de etiqueta única en la **NombreDominio** argumento es de 15 caracteres o menos, promoción continúa con un nombre generado automáticamente.  
  
2.  Si la **NewDomainNetBIOSName** argumento no se especifica con un nombre de dominio de NetBIOS y el nombre de dominio de prefijo de etiqueta única en la **NombreDominio** argumento es de 16 caracteres o más, se produce un error en la promoción.  
  
3.  Si la **NewDomainNetBIOSName** argumento se especifica con un nombre de dominio NetBIOS de 15 caracteres o menos, a continuación, promoción continúa con ese nombre especificado.  
  
4.  Si la **NewDomainNetBIOSName** argumento se especifica con un nombre de dominio NetBIOS de 16 caracteres o más y, luego, se produce un error en la promoción.  
  
La **opciones adicionales** ADDSDeployment cmdlet argumento es:  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>Rutas de acceso  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
La **rutas de acceso** página te permite invalidar las ubicaciones predeterminadas de la carpeta de la base de datos de AD DS, los registros de transacciones de base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas de siempre están en los subdirectorios de % systemroot %.  
  
La **rutas de acceso** ADDSDeployment cmdlet argumentos son:  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>Opciones de revisión y el Script de vista  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
La **opciones de revisión** página te permite validar la configuración y asegúrate de que cumplen los requisitos antes de iniciar la instalación. Esto no es la última oportunidad para detener la instalación al usar el administrador del servidor. Esto es simplemente una opción para confirmar su configuración antes de continuar con la configuración  
  
La **opciones de revisión** también ofrece un opcional de la página en el administrador del servidor **ver Script** botón para crear un archivo de texto de Unicode que contiene la configuración actual de ADDSDeployment como una única secuencia de Windows PowerShell. Esto te permite usar la interfaz gráfica de administrador del servidor como un estudio de implementación de Windows PowerShell. Usar al Asistente para la configuración de los servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, Cancelar al asistente.  Este proceso crea una muestra sintácticamente correcta y válida para modificaciones adicionales o su uso directo. Por ejemplo:  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Por lo general, rellena el administrador del servidor en todos los argumentos con los valores de promoción y no se basa en valores predeterminados (como pueden cambiar entre las versiones futuras de Windows o service Pack). La única excepción a esto es la **- safemodeadministratorpassword** argumento (que se omite deliberadamente desde el script). Para forzar a un mensaje de confirmación, omite el valor cuando se ejecuta el cmdlet de manera interactiva.  
  
Usar opcional **Whatif** argumento con la **instalación ADDSForest** cmdlet para revisar la información de configuración. Esto te permite ver los valores implícitos y explícitos de los argumentos de un cmdlet.  
  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>Comprobación de requisitos previos  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
La **comprobación de requisitos previos** es una nueva característica en la configuración de dominio de AD DS. Esta nueva fase valida que la configuración del servidor es capaz de admitir un nuevo dominio de AD DS.  
  
Al instalar un nuevo dominio raíz, el Asistente de configuración de servicios de Active Directory dominio de Server Manager invoca una serie de pruebas modulares serializadas. Estas pruebas avisan con opciones de reparación sugeridas. Puedes ejecutar las pruebas tantas veces como sea necesario. El proceso de controlador de dominio no puede continuar hasta que todos los requisitos previos pruebas pasar.  
  
La **comprobación de requisitos previos** superficies también información relevante, como los cambios de seguridad que afectan a los sistemas operativos anteriores.  
  
Para obtener más información sobre los requisitos previos controles específicos, consulta [comprobación de requisito previo](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
No se puede omitir la **comprobar requisitos previos** cuando usa el administrador del servidor, pero puedes omitir el proceso cuando se usa el cmdlet de implementación de AD DS con el argumento siguiente:  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft recomienda no omitiendo comprobar los requisitos previos como que puede provocar una promoción del controlador de dominio parcial o daña el bosque de AD DS.  
  
Haz clic en **instalar** para iniciar el proceso de promoción de controlador de dominio. Esta es la última oportunidad para cancelar la instalación. No se puede cancelar el proceso de promoción una vez que comienza. El equipo se reiniciará automáticamente al final de promoción, independientemente de los resultados de la promoción.  
  
### <a name="installation"></a>Instalación  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
Cuando la **instalación** muestra la página, la configuración del controlador de dominio comienza y no se han detenido o cancela. Operaciones detalladas en esta página muestran y se escriben en registros:  
  
-   %systemroot%\Debug\Dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
Para instalar un nuevo uso del módulo de ADDSDeployment de dominio de Active Directory, usa el cmdlet siguiente:  
  
```  
Install-addsdomain  
```  
  
Consulta [menor y dominio de Windows PowerShell de árbol](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS) para argumentos necesarios y opcionales. La **instalación addsdomain** cmdlet solo tiene dos fases (comprobación de requisitos previos e instalación). Las dos figuras siguientes muestran la fase de instalación con los argumentos requeridos mínimos de **- domaintype**, **- newdomainname**, **- parentdomainname**, y **-credenciales**. Ten en cuenta cómo hacerlo, al igual que el administrador del servidor, **instalación ADDSDomain** te recuerda que promoción reiniciará automáticamente el servidor.  
  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
Para aceptar automáticamente la consulta de reinicio, usa el **-forzar** o **-confirmar: $false** argumentos con cualquier cmdlet ADDSDeployment Windows PowerShell. Para evitar que el servidor reiniciar automáticamente al final de promoción, usa el **- norebootoncompletion** argumento.  
  
> [!WARNING]  
> No se recomienda reemplazar el reinicio. Debes reiniciar el controlador de dominio para que funcione correctamente  
  
### <a name="results"></a>Resultados  
![Instalar a un nuevo elemento secundario de anuncios](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La **resultados** página muestra el éxito o error de la promoción y cualquier información administrativa importante. El controlador de dominio se reiniciará automáticamente después de 10 segundos.  
  

