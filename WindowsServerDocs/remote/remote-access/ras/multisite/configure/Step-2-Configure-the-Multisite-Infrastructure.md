---
title: Paso 2 configurar la infraestructura de multisitio
description: Este tema forma parte de la Guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: faec70ac-88c0-4b0a-85c7-f0fe21e28257
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2d8d5fbe427f25e9e26eac96d89dc5fae17e197b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281051"
---
# <a name="step-2-configure-the-multisite-infrastructure"></a>Paso 2 configurar la infraestructura de multisitio

>Se aplica a: Windows Server 2012 R2, Windows Server 2012

Para configurar una implementación multisitio, hay una serie de pasos necesarios para modificar la configuración de la infraestructura de red incluidas: configuración de sitios de Active Directory adicionales y controladores de dominio, configurar grupos de seguridad adicional y configurar Objetos de directiva de grupo (GPO) si no usa automáticamente configura los GPO.  
  
|Tarea|Descripción|  
|----|--------|  
|2.1. Configurar sitios de Active Directory adicionales|Configurar sitios de Active Directory adicionales para la implementación.|  
|2.2. Configurar controladores de dominio adicionales|Configure los controladores de dominio de Active Directory adicionales según sea necesario.|  
|2.3. Configurar grupos de seguridad|Configurar grupos de seguridad para todos los equipos cliente de Windows 7.|  
|2.4. Configurar GPO|Configure los objetos de directiva de grupo adicionales según sea necesario.|  
  
> [!NOTE]  
> Este tema incluye cmdlets de Windows PowerShell de ejemplo que puede usar para automatizar algunos de los procedimientos descritos. Para más información, consulta [Uso de cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigAD"></a>2.1. Configurar sitios de Active Directory adicionales  
Todos los puntos de entrada pueden residir en un único sitio de Active Directory. Por lo tanto, al menos un sitio de Active Directory es necesario para la implementación de servidores de acceso remoto en una configuración multisitio. Utilice este procedimiento si necesita crear el primer sitio de Active Directory, o si desea utilizar los sitios de Active Directory adicionales para la implementación multisitio. Use el complemento Servicios y sitios de Active Directory para crear nuevos sitios de su organización "red s.  

Pertenencia a la **administradores de empresas** grupo del bosque o el **Admins. del dominio** grupo en el dominio raíz del bosque, o equivalente, como mínimo es necesario para completar este procedimiento. Revise los detalles sobre el uso de las cuentas adecuadas y pertenencia a grupos en [dominio grupos predeterminados locales y](https://go.microsoft.com/fwlink/?LinkId=83477).  

Para obtener más información, consulte [adición de un sitio al bosque](https://technet.microsoft.com/library/cc732761.aspx).  

### <a name="to-configure-additional-active-directory-sites"></a>Para configurar sitios de Active Directory adicionales  
  
1.  En el controlador de dominio principal, haga clic en **iniciar**y, a continuación, haga clic en **servicios y sitios de Active Directory**.  
  
2.  En la consola de servicios y sitios de Active Directory, en el árbol de consola, haga clic en **sitios**y, a continuación, haga clic en **nuevo sitio**.  
  
3.  En el **nuevo objeto - sitio** cuadro de diálogo el **nombre** cuadro, escriba un nombre para el nuevo sitio.  
  
4.  En **nombre de vínculo**, haga clic en un objeto de vínculo de sitio y, a continuación, haga clic en **Aceptar** dos veces.  
  
5.  En el árbol de consola, expanda **sitios**, haga clic en **subredes**y, a continuación, haga clic en **nueva subred**.  
  
6.  En el **nuevo objeto - subred** cuadro de diálogo **prefijo**, escriba el prefijo de subred IPv4 o IPv6, en la **seleccionar un objeto de sitio para este prefijo** lista, haga clic en el sitio para asociar con esta subred y, a continuación, haga clic en **Aceptar**.  
  
7.  Repita los pasos 5 y 6 hasta que haya creado todas las subredes necesarias en su implementación.  
  
8.  Cierre los servicios y sitios de Active Directory.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Para instalar la característica de Windows "Módulo de Active Directory para Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
o agregar el "Active Directory PowerShell complemento" a través de OptionalFeatures.  
  
Si ejecuta los siguientes cmdlets en Windows 7" o Windows Server 2008 R2, debe importar el módulo de PowerShell de Active Directory:  
  
```  
Import-Module ActiveDirectory  
```  
  
Para configurar un sitio de Active Directory denominado "Segundo-Site" mediante el DEFAULTIPSITELINK integrado:  
  
```  
New-ADReplicationSite -Name "Second-Site"  
Set-ADReplicationSiteLink -Identity "DEFAULTIPSITELINK" -sitesIncluded @{Add="Second-Site"}  
```  
  
Para configurar subredes IPv4 e IPv6 para el segundo sitio:  
  
```  
New-ADReplicationSubnet -Name "10.2.0.0/24" -Site "Second-Site"  
New-ADReplicationSubnet -Name "2001:db8:2::/64" -Site "Second-Site"  
```  
  
## <a name="BKMK_AddDC"></a>2.2. Configurar controladores de dominio adicionales  
Para configurar una implementación multisitio en un único dominio, se recomienda que tiene al menos un controlador de dominio grabable para cada sitio en la implementación.  
  
Para realizar este procedimiento, como mínimo que debe ser miembro del grupo Admins. del dominio en el dominio en el que se está instalando el controlador de dominio.  
  
Para obtener más información, consulte [instalar un controlador de dominio adicional](https://technet.microsoft.com/library/cc733027.aspx).
  
### <a name="to-configure-additional-domain-controllers"></a>Para configurar los controladores de dominio adicionales  
  
1.  En el servidor que actuará como un controlador de dominio en **administrador del servidor**, en el **panel**, haga clic en **agregar roles y características**.  
  
2.  Haga clic en **siguiente** tres veces para ir a la pantalla de selección de roles de servidor  
  
3.  En el **seleccionar Roles de servidor** , seleccione **Active Directory Domain Services**. Haga clic en **agregar características** cuando se le pida y, a continuación, haga clic en **siguiente** tres veces.  
  
4.  En la página **Confirmación**, haz clic en **Instalar**.  
  
5.  Cuando la instalación finalice correctamente, haga clic en **promover este servidor a controlador de dominio**.  
  
6.  En el Asistente de configuración de Active Directory Domain Services, en el **configuración de implementación** página, haga clic en **agregar un controlador de dominio a un dominio existente**.  
  
7.  En **dominio**, escriba el dominio nombre; por ejemplo, corp.contoso.com.  
  
8.  En **proporcionar las credenciales para realizar esta operación**, haga clic en **cambio**. En el **Windows Security** diálogo cuadro, proporcione el nombre de usuario y la contraseña de una cuenta que puede instalar el controlador de dominio adicional. Para instalar un controlador de dominio adicional, debe ser miembro del grupo Administradores de empresas o del grupo Administradores de dominio. Cuando termine de proporcionar las credenciales, haga clic en **Siguiente**.  
  
9. En el **opciones del controlador de dominio** página, realice lo siguiente:  
  
    1.  Realice las selecciones siguientes:  
  
        -   **Servidor de sistema de nombres (DNS) del dominio**"esta opción se selecciona de forma predeterminada para que el controlador de dominio pueda funcionar como un servidor de sistema de nombres de dominio (DNS). Si no desea que el controlador de dominio sea un servidor DNS, desactive esta opción.  
  
            Si el rol de servidor DNS no está instalado en el emulador de controlador de dominio principal (PDC) en el dominio raíz del bosque, no está disponible la opción para instalar el servidor DNS en un controlador de dominio adicional. Como solución alternativa en esta situación, puede instalar el rol de servidor DNS antes o después de la instalación de AD DS.  
  
            > [!NOTE]  
            > Si selecciona la opción para instalar el servidor DNS, es posible que reciba un mensaje que indica que no se pudo crear una delegación DNS para el servidor DNS y que debe crear manualmente una delegación DNS al servidor DNS para garantizar una resolución de nombres confiable. Si va a instalar un controlador de dominio adicional en el dominio raíz del bosque o un dominio raíz del árbol, no es necesario que crear la delegación DNS. En este caso, haga clic en **Sí** y pasar por alto el mensaje.  
  
        -   **Catálogo global (GC)** "de forma predeterminada, se selecciona esta opción. Agregue el catálogo global y las particiones del directorio de solo lectura al controlador de dominio, y activa la funcionalidad de búsqueda del catálogo global.  
  
        -   **Controlador de dominio de solo lectura (RODC)** "no se selecciona esta opción de forma predeterminada. Hace que el controlador de dominio adicional sea de sólo lectura; es decir, hace que el controlador de dominio un RODC.  
  
    2.  En **nombre del sitio**, seleccione un sitio de la lista.  
  
    3.  En **escriba la contraseña del modo de restauración de servicios de directorio (DSRM)** , en **contraseña** y **Confirmar contraseña**, escriba una contraseña segura dos veces y, a continuación, haga clic en  **Siguiente**. Esta contraseña debe usarse para iniciar los AD DS en el modo DSRM para las tareas que deben realizarse sin conexión.  
  
10. En el **opciones de DNS** página, seleccione el **delegación DNS actualizar** casilla de verificación si desea actualizar delegación DNS durante la instalación del rol y, a continuación, haga clic en **siguiente**.  
  
11. En el **opciones adicionales** página, escriba o vaya a las ubicaciones del volumen y la carpeta para el archivo de base de datos, los archivos de registro del servicio de directorio y los archivos de volumen (SYSVOL) del sistema. Especifique las opciones de replicación según sea necesario y, a continuación, haga clic en **siguiente**.  
  
12. En el **Revisar opciones** , revise las opciones de instalación y, a continuación, haga clic en **siguiente**.  
  
13. En el **comprobación de requisitos previos** página después de que se validen los requisitos previos, haga clic en **instalar**.  
  
14. Espere a que la configuración complete el asistente y, a continuación, haga clic en **cerrar**.  
  
15. Reinicie el equipo si no se reinició automáticamente.  
  
## <a name="BKMK_ConfigSG"></a>2.3. Configurar grupos de seguridad  
Una implementación multisitio requiere un grupo de seguridad adicional para los equipos cliente de Windows 7 para cada punto de entrada en la implementación que permite el acceso a los equipos cliente de Windows 7. Si hay varios dominios que contienen los equipos cliente de Windows 7, se recomienda crear un grupo de seguridad en cada dominio del mismo punto de entrada. Como alternativa, puede usarse un grupo de seguridad universal que contenga los equipos cliente desde ambos dominios. Por ejemplo, en un entorno con dos dominios, si desea permitir el acceso a los equipos cliente de Windows 7 en los puntos de entrada 1 y 3, pero no en la entrada de punto 2, a continuación, crear dos nuevos grupos de seguridad para que contenga los equipos cliente de Windows 7 para cada punto de entrada en cada uno de los  dominios.  
  
### <a name="to-configure-additional-security-groups"></a>Para configurar los grupos de seguridad adicional  
  
1.  En el controlador de dominio principal, haga clic en **iniciar**y, a continuación, haga clic en **equipos y usuarios de Active Directory**.  
  
2.  En el árbol de consola, haga clic en la carpeta en la que desea agregar un nuevo grupo, por ejemplo, corp.contoso.com/Users. Elija **Nuevo** y haga clic en **Grupo**.  
  
3.  En el **nuevo objeto - grupo** cuadro de diálogo **conmutaciónporerror**, escriba el nombre del nuevo grupo, por ejemplo, Win7_Clients_Entrypoint1.  
  
4.  En **ámbito de grupo**, haga clic en **Universal**, en **tipo de grupo**, haga clic en **seguridad**y, a continuación, haga clic en **Aceptar**.  
  
5.  Para agregar equipos al nuevo grupo de seguridad, haga doble clic en el grupo de seguridad y en el **< nombre_grupo > propiedades** cuadro de diálogo, haga clic en el **miembros** ficha.  
  
6.  En la pestaña **Miembros** , haga clic en **Agregar**.  
  
7.  Seleccione los equipos de Windows 7 para agregar a este grupo de seguridad y, a continuación, haga clic en **Aceptar**.  
  
8.  Repita este procedimiento para crear un grupo de seguridad para cada punto de entrada según sea necesario.  
  
![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
Para instalar la característica de Windows "Módulo de Active Directory para Windows PowerShell":  
  
```  
Install-WindowsFeature "Name RSAT-AD-PowerShell  
```  
  
o agregar el "Active Directory PowerShell complemento" a través de OptionalFeatures.  
  
Si ejecuta los siguientes cmdlets en Windows 7" o Windows Server 2008 R2, debe importar el módulo de PowerShell de Active Directory:  
  
```  
Import-Module ActiveDirectory  
```  
  
Para configurar un grupo de seguridad denominado Win7_Clients_Entrypoint1 y para agregar un equipo cliente denominado CLIENT2:  
  
```  
New-ADGroup -GroupScope universal -Name Win7_Clients_Entrypoint1  
Add-ADGroupMember -Identity Win7_Clients_Entrypoint1 -Members CLIENT2$  
```  
  
## <a name="ConfigGPOs"></a>2.4. Configurar GPO  
Una implementación multisitio de acceso remoto requiere los siguientes objetos de directiva de grupo:  
  
-   Un GPO para cada punto de entrada para el servidor de acceso remoto.  
  
-   Un GPO para los equipos cliente de Windows 8 para cada dominio.  
  
-   Un GPO en cada dominio que contenga los equipos cliente de Windows 7 para cada punto de entrada había configurado para admitir Windows 7, los clientes.  
  
    > [!NOTE]  
    > Si no tiene los equipos cliente de Windows 7, no es necesario crear equipos de GPO para Windows 7.  
  
Al configurar el acceso remoto, el asistente crea automáticamente los objetos de directiva de grupo necesarios si don "t ya existe. Si no tiene los permisos necesarios para crear objetos de directiva de grupo, se deben crear antes de configurar el acceso remoto. El Administrador de DirectAccess debe tener permisos completos en los GPO (editar + modificar seguridad + SUPR).  
  
> [!IMPORTANT]  
> Después de crear manualmente los GPO para el acceso remoto debe permitir suficiente tiempo para Active Directory y la replicación DFS al controlador de dominio en el sitio de Active Directory que está asociado con el servidor de acceso remoto. Si el acceso remoto crea automáticamente los objetos de directiva de grupo, a continuación, no se requiere ningún tiempo de espera.  
  
Para crear objetos de directiva de grupo, consulte [crear y editar un objeto de directiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
### <a name="DCMaintandDowntime"></a>Tiempo de inactividad y mantenimiento del controlador de dominio  
Cuando un controlador de dominio que se ejecuta como el emulador de PDC o controladores de dominio de administración de GPO de servidor experimentan tiempos de inactividad, no es posible cargar o modificar la configuración de acceso remoto. Esto no afecta a la conectividad de cliente si hay otros controladores de dominio.  
  
Para cargar o modificar la configuración de acceso remoto, puede transferir el rol de emulador PDC a otro controlador de dominio para el servidor de cliente o aplicación GPO; para los GPO de servidor, cambie los controladores de dominio que administran los GPO de servidor.  
  
> [!IMPORTANT]  
> Esta operación puede realizarse únicamente por un administrador de dominio. No se limita el impacto de cambiar el controlador de dominio principal para acceso remoto; por lo tanto, tenga cuidado al transferir el rol de emulador PDC.  
  
> [!NOTE]  
> Antes de modificar la asociación del controlador de dominio, asegúrese de que todos los GPO en la implementación de acceso remoto se han replicado a todos los controladores de dominio en el dominio. Si el GPO no está sincronizado, pueden perderse los cambios recientes de configuración después de modificar la asociación del controlador de dominio, que puede dar lugar a una configuración dañada. Para comprobar la sincronización de GPO, vea [comprobar el estado de infraestructura de directiva de grupo](https://technet.microsoft.com/library/jj134176.aspx).  
  
#### <a name="TransferPDC"></a>Para transferir el rol de emulador PDC  
  
1.  En el **iniciar** , escriba**dsa.msc**, y, a continuación, presione ENTRAR.  
  
2.  En el panel izquierdo de la consola de equipos y usuarios de Active Directory, haga clic en **equipos y usuarios de Active Directory**y, a continuación, haga clic en **cambiar el controlador de dominio**. En el cuadro de diálogo Cambiar el servidor de directorio, haga clic en **instancia de este controlador de dominio o AD LDS**, en la lista, haga clic en el controlador de dominio que el titular de la nueva función y, a continuación, haga clic en **Aceptar**.  
  
    > [!NOTE]  
    > Debe realizar este paso si no está en el controlador de dominio al que desea transferir el rol. No realice este paso si ya está conectado al controlador de dominio al que desea transferir el rol.  
  
3.  En el árbol de consola, haga clic en **equipos y usuarios de Active Directory**, apunte a **todas las tareas**y, a continuación, haga clic en **maestros de operaciones**.  
  
4.  En el cuadro de diálogo maestros de operaciones, haga clic en el **PDC** pestaña y, a continuación, haga clic en **cambio**.  
  
5.  Haga clic en **Sí** para confirmar que desea transferir la función y, a continuación, haga clic en **cerrar**.  
  
#### <a name="ChangeDC"></a>Para cambiar el controlador de dominio que administra el GPO de servidor  
  
-   Ejecute el cmdlet de Windows PowerShell `HYPERLINK "https://technet.microsoft.com/library/hh918412.aspx" Set-DAEntryPointDC` en el servidor de acceso remoto y especifique el nombre del controlador de dominio no accesible para el *ExistingDC* parámetro. Este comando modifica la asociación del controlador de dominio para el GPO de servidor de los puntos de entrada que están administrados actualmente por ese controlador de dominio.  
  
    -   Para reemplazar el controlador de dominio inalcanzables "dc1.corp.contoso.com" con el controlador de dominio "dc2.corp.contoso.com", realice lo siguiente:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "NewDC 'dc2.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
    -   Para reemplazar el controlador de dominio inalcanzables "dc1.corp.contoso.com" con un controlador de dominio en el sitio de Active Directory más cercano al servidor de acceso remoto "DA1.corp.contoso.com", realice lo siguiente:  
  
        ```  
        Set-DAEntryPointDC "ExistingDC 'dc1.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
        ```  
  
### <a name="ChangeTwoDCs"></a>Cambiar dos o más controladores de dominio que administración los GPO de servidor  
En un número mínimo de casos, dos o más controladores de dominio que administración los GPO de servidor no están disponibles. Si esto ocurre, son necesarios más pasos para cambiar la asociación del controlador de dominio para el GPO de servidor.  
  
Información de asociación del controlador de dominio se almacena en el registro de los servidores de acceso remoto y en todos los GPO de servidor. En el ejemplo siguiente, hay dos puntos de entrada con dos servidores de acceso remoto, "DA1" en "1" y "DA2" la punto de entrada de "Punto de entrada 2". El GPO de servidor "Del punto de entrada 1" se administra en el controlador de dominio "DC1", mientras que el GPO de servidor de "punto de entrada 2" se administra en el controlador de dominio "DC2". "DC1" y "DC2" están disponibles. Un tercer controlador de dominio sigue estando disponible en el dominio, "DC3", y ya se ha replicado los datos de "DC1" y "DC2" a "DC3".  
  
![Configurar la infraestructura de multisitio](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc1.png)  
  
##### <a name="to-change-two-or-more-domain-controllers-that-manage-server-gpos"></a>Para cambiar de dos o más controladores de dominio que administración los GPO de servidor  
  
1.  Para reemplazar el controlador de dominio no está disponible "DC2" con el controlador de dominio "DC3", ejecute el siguiente comando:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Este comando actualiza la asociación de controlador de dominio para el GPO en el registro de servidor de "Punto de entrada 2" de DA2 y en el servidor "Punto de entrada 2" GPO; Sin embargo, no actualiza el GPO de servidor "Punto de entrada 1" porque el controlador de dominio que administra no está disponible.  
  
    > [!TIP]  
    > Este comando usa el valor de continuar para el *ErrorAction* parámetro, que actualiza el GPO de servidor "Punto de entrada 2" a pesar del error para actualizar el GPO de servidor "1 punto de entrada".  
  
    La configuración resultante se muestra en el diagrama siguiente.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc2.png)  
  
2.  Para reemplazar el controlador de dominio no está disponible "DC1" con el controlador de dominio "DC3", ejecute el siguiente comando:  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC1' "NewDC 'DC3' "ComputerName 'DA2' "ErrorAction Continue  
    ```  
  
    Este comando actualiza la asociación del controlador de dominio para el GPO de servidor de GPO de servidor en el registro de DA1 y en el "Punto de entrada 1" y "Punto de entrada 2" de "Punto de entrada 1". La configuración resultante se muestra en el diagrama siguiente.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssoc3.png)  
  
3.  Para sincronizar la asociación del controlador de dominio para el servidor "Punto de entrada 2" del GPO en el servidor "1 punto de entrada" GPO, ejecute el comando para reemplazar "DC2" con "DC3" y especifique el servidor de acceso remoto cuyo GPO de servidor no está sincronizado, en este caso "DA1" para el  *ComputerName* parámetro.  
  
    ```  
    Set-DAEntryPointDC "ExistingDC 'DC2' "NewDC 'DC3' "ComputerName 'DA1' "ErrorAction Continue  
    ```  
  
    La configuración final se muestra en el diagrama siguiente.  
  
    ![Windows PowerShell](../../../../media/Step-2-Configure-the-Multisite-Infrastructure/DCAssocFinal.png)  
  
### <a name="ConfigDistOptimization"></a>Optimización de distribución de configuración  
Al realizar cambios de configuración, los cambios se aplican solo después de que los GPO de servidor se propagan a los servidores de acceso remoto. Para reducir el tiempo de distribución de configuración, acceso remoto selecciona automáticamente un controlador de dominio de escritura en el hipervínculo "<https://technet.microsoft.com/library/cc978016.aspx>" más cercano al servidor de acceso remoto al crear el GPO de servidor.  
  
En algunos escenarios, es posible que deba modificar manualmente el controlador de dominio que administra un GPO de servidor con el fin de optimizar el tiempo de distribución de configuración:  
  
-   No había ningún controlador de dominio de escritura en el sitio de Active Directory de un servidor de acceso remoto en el momento de agregarlo como un punto de entrada. Un controlador de dominio grabable ahora se está agregando al sitio de Active Directory del servidor de acceso remoto.  
  
-   Cambia la dirección IP o un cambio de sitios de Active Directory y las subredes que ha movido el servidor de acceso remoto a otro sitio de Active Directory.  
  
-   La asociación del controlador de dominio para un punto de entrada se modificó manualmente debido a tareas de mantenimiento en un controlador de dominio, y ahora es vuelve a conectar el controlador de dominio.  
  
En estos escenarios, ejecute el cmdlet de PowerShell `Set-DAEntryPointDC` en el servidor de acceso remoto y especifique el nombre del punto de entrada que desea optimizar el uso del parámetro *EntryPointName*. Debe hacerlo solo después de los datos de GPO desde el controlador de dominio almacena actualmente el servidor de que GPO ya están completamente replicado en el nuevo controlador de dominio deseado.  
  
> [!NOTE]  
> Antes de modificar la asociación del controlador de dominio, asegúrese de que todos los GPO en la implementación de acceso remoto se han replicado a todos los controladores de dominio en el dominio. Si el GPO no está sincronizado, pueden perderse los cambios recientes de configuración después de modificar la asociación del controlador de dominio, que puede dar lugar a una configuración dañada. Para comprobar la sincronización de GPO, vea [comprobar el estado de infraestructura de directiva de grupo](https://technet.microsoft.com/library/jj134176.aspx).  
  
Para optimizar el tiempo de distribución de configuración, realice una de las siguientes acciones:  
  
-   Para administrar el servidor de GPO de entrada de punto de "Punto de entrada 1" en un controlador de dominio en el sitio de Active Directory más cercano al servidor de acceso remoto "DA1.corp.contoso.com", ejecute el siguiente comando:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
-   Para administrar el servidor de GPO de entrada de punto de "Punto de entrada 1" en el dominio controlador "dc2.corp.contoso.com", ejecute el siguiente comando:  
  
    ```  
    Set-DAEntryPointDC "EntryPointName 'Entry point 1' "NewDC 'dc2.corp.contoso.com' "ComputerName 'DA1.corp.contoso.com' "ErrorAction Inquire  
    ```  
  
    > [!NOTE]  
    > Cuando se modifica el controlador de dominio asociado a un punto de entrada específica, debe especificar un servidor de acceso remoto que sea miembro de ese punto de entrada para el *ComputerName* parámetro.  
  
## <a name="BKMK_Links"></a>Vea también  
  
-   [Paso 3: Configurar la implementación multisitio](Step-3-Configure-the-Multisite-Deployment.md)  
-   [Paso 1: Implementar un único servidor de implementación de acceso remoto](Step-1-Implement-a-Single-Server-Remote-Access-Deployment.md)  

