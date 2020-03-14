---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: Descripciones de la página del Asistente para la instalación y eliminación de AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3563c30e86c53435c10cafc840a71c7b8c526943
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323257"
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Descripciones de la página del Asistente para la instalación y eliminación de AD DS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporcionan descripciones para los controles en las siguientes páginas del asistente que incluyen la instalación y eliminación del rol de servidor AD DS en el Administrador del servidor.  
  
-   [Configuración de implementación](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Opciones del controlador de dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Opciones de DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Opciones de RODC](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Opciones adicionales](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Rutas](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Opciones de preparación](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Opciones de revisión](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Comprobación de requisitos previos](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [Resultados](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Credenciales de eliminación de roles](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [Opciones de eliminación de AD DS y advertencias](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nueva contraseña de administrador](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Confirmar selecciones de eliminación de roles](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>Configuración de implementación  
El Administrador del servidor comienza todas las instalaciones de controlador de dominio con la página **Configuración de implementación**. Las opciones restantes y los campos requeridos cambian en esta página y en las páginas siguientes, según qué operación de implementación se seleccione. Por ejemplo, si crea un nuevo bosque, no aparece la página **Opciones de preparación** , pero sí si instala el primer controlador de dominio que ejecuta Windows Server 2012 en un bosque o dominio existente.  
  
Se realizan algunas pruebas de validación en esta página, que después se volverán a ejecutar como parte de las comprobaciones de requisitos previos. Por ejemplo, si intenta instalar el primer controlador de dominio de Windows Server 2012 en un bosque que tiene el nivel funcional de Windows 2000, aparece un error en esta página.  
  
Cuando se crea un nuevo bosque aparecen las siguientes opciones.  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Cuando se crea un nuevo bosque, se debe especificar un nombre para el dominio raíz del bosque. El nombre de dominio raíz del bosque no puede tener una etiqueta única (por ejemplo, debe ser "contoso.com" en lugar de "Contoso"). Debe usar convenciones de nomenclatura de dominio DNS permitidas. Se puede especificar un nombre de dominio internacionalizado (IDN). Para obtener más información acerca de las convenciones de nomenclatura de dominios DNS, consulte [KB 909264](https://support.microsoft.com/kb/909264).  
  
-   No cree bosques nuevos de Active Directory con el mismo nombre que el del DNS externo. Por ejemplo, si la dirección URL de DNS de Internet es http:\//contoso.com, debe elegir un nombre diferente para el bosque interno con el fin de evitar problemas de compatibilidad en el futuro. Ese nombre debe ser único y poco probable que corresponda al tráfico web, como por ejemplo corp.contoso.com.  
  
-   Debe ser miembro del grupo Administradores en el servidor donde desea crear el nuevo bosque.  
  
Para obtener más información sobre cómo crear un bosque, vea [instalar un nuevo servidor de Windows Server 2012 &#40;Active Directory nivel&#41;de bosque 200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Cuando se crea un nuevo dominio aparecen las siguientes opciones.  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Si se crea un nuevo dominio de árbol, se debe especificar el nombre del dominio raíz del bosque en lugar del dominio primario, pero las opciones y páginas restantes del asistente son las mismas.  
  
-   Haga clic en **Seleccionar** para buscar el dominio primario o el árbol de Active Directory, o escriba un dominio primario o nombre de árbol válido. A continuación, escriba el nombre del nuevo dominio en **Nuevo nombre de dominio**.  
  
-   Dominio de árbol: proporcione un nombre de dominio raíz completo y válido; el nombre no puede ser de etiqueta única y debe cumplir los requisitos de nombre de dominio DNS.  
  
-   Dominio secundario: proporcione un nombre de dominio secundario de etiqueta única y válido; el nombre debe cumplir los requisitos de nombre de dominio DNS.  
  
-   El Asistente para configuración de Servicios de dominio de Active Directory pide confirmación de las credenciales de dominio si las credenciales actuales no corresponden al dominio. Haga clic en **Cambiar** para proporcionar las credenciales de dominio.  
  
Para obtener más información sobre cómo crear un dominio, vea [instalar un nuevo Windows Server 2012 Active Directory nivel de dominio &#40;secundario o de&#41;árbol 200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Las siguientes opciones aparecen cuando se agrega un controlador de dominio nuevo a un dominio existente.  
  
![AD DS instalar](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Haga clic en **Seleccionar** para buscar el dominio, o escribe un nombre de dominio válido.  
  
-   El Administrador del servidor solicita que escribas credenciales válidas si es necesario. La instalación de un controlador de dominio adicional requiere la pertenencia al grupo Admins del dominio.  
  
    Además, la instalación del primer controlador de dominio que ejecuta Windows Server 2012 en un bosque requiere credenciales que incluyen pertenencias a grupos en los grupos administradores de empresas y administradores de esquema. El Asistente para configuración de Active Directory Domain Services te pedirá confirmación si las credenciales actuales no tienen los permisos o la pertenencia a grupos adecuados.  
  
Para obtener más información acerca de cómo agregar un controlador de dominio a un dominio existente, vea [instalar un controlador de dominio de Windows Server 2012 de &#40;réplica en&#41;un nivel de dominio existente 200](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>Opciones del controlador de dominio  
Si está creando un nuevo bosque, la página Opciones del controlador de dominio tiene estas opciones:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   Los niveles funcionales del bosque y del dominio se establecen de forma predeterminada en Windows Server 2012.  
  
    Existe una nueva característica disponible en el nivel funcional de dominio de Windows Server 2012: la compatibilidad con la Directiva de plantillas administrativas KDC de Access Control y protección de Kerberos tiene dos opciones de configuración (proporcionar siempre notificaciones y error de autenticación no blindada). solicitudes) que requieren el nivel funcional de dominio de Windows Server 2012. Para obtener más información, vea "compatibilidad con notificaciones, autenticación compuesta y protección de Kerberos" en [novedades de la autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx).    
    El nivel funcional del bosque de Windows Server 2012 no proporciona características nuevas, pero asegura que cualquier dominio nuevo creado en el bosque funcione automáticamente en el nivel funcional del dominio de Windows Server 2012. El nivel funcional de dominio de Windows Server 2012 no proporciona otras características nuevas además de la compatibilidad con el Access Control dinámico y la protección de Kerberos, pero garantiza que cualquier controlador de dominio del dominio ejecute Windows Server 2012. Para obtener más información acerca de otras características que se encuentran disponibles en distintos niveles funcionales, consulte el tema sobre la [descripción de los niveles funcionales de los Servicios de dominio de Active Directory (AD DS)](../active-directory-functional-levels.md).  
  
    Más allá de los niveles funcionales, un controlador de dominio que ejecuta Windows Server 2012 proporciona características adicionales que no están disponibles en un controlador de dominio que ejecuta una versión anterior de Windows Server. Por ejemplo, un controlador de dominio que ejecuta Windows Server 2012 puede usarse para la clonación del controlador de dominio virtual, mientras que un controlador de dominio que ejecuta una versión anterior de Windows Server no puede hacerlo.  
  
-   El servidor DNS se selecciona de forma predeterminada cuando se crea un bosque nuevo. El primer controlador de dominio en el bosque debe ser un servidor de catálogo global (GC), y no puede ser un controlador de dominio de solo lectura (RODC).  
  
-   La contraseña del Modo de restauración de servicios de directorio (DSRM) se necesita para iniciar sesión en un controlador de dominio donde no se ejecuta AD DS. La contraseña que se especifica debe cumplir con la directiva de contraseñas que se aplica al servidor, que de forma predeterminada no requiere una contraseña segura; solo una contraseña que no esté en blanco. Siempre elija una contraseña segura y compleja o, preferentemente, una frase de contraseña. Para obtener información acerca de cómo sincronizar la contraseña de DSRM con la contraseña de una cuenta de usuario de dominio, consulte [KB 961320](https://support.microsoft.com/kb/961320).  
  
Para obtener más información sobre cómo crear un bosque, vea [instalar un nuevo servidor de Windows Server 2012 &#40;Active Directory nivel&#41;de bosque 200](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Si está creando un dominio secundario, la página Opciones del controlador de dominio tiene estas opciones:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   De forma predeterminada, el nivel funcional del dominio se establece en Windows Server 2012. Se puede especificar cualquier otro valor que al menos sea el valor del nivel funcional del bosque o uno superior.  
  
-   Las opciones del controlador de dominio que pueden configurarse incluyen **Servidor DNS** y **Catálogo global**; el controlador de dominio de solo lectura no puede configurarse como el primer controlador de dominio de un dominio nuevo.  
  
    Microsoft recomienda que todos los controladores de dominio proporcionen servicios de catálogos globales y DNS para gran disponibilidad en entornos distribuidos, que es la razón por la que el asistente habilita estas opciones de forma predeterminada cuando se crea un dominio nuevo.  
  
-   La página **Opciones del controlador de dominio** también permite elegir el **nombre de sitio** lógico apropiado de Active Directory de la configuración del bosque. De forma predeterminada, selecciona el sitio con la subred más correcta. Si solo hay un sitio, lo selecciona automáticamente.  
  
    > [!IMPORTANT]  
    > Si el servidor no pertenece a una subred de Active Directory y existe más de un sitio, no se selecciona nada y el botón **Siguiente** no estará disponible hasta que elijas un sitio de la lista.  
  
Para obtener más información sobre cómo crear un dominio, vea [instalar un nuevo Windows Server 2012 Active Directory nivel de dominio &#40;secundario o de&#41;árbol 200](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Si está agregando un controlador de dominio a un dominio, la página Opciones del controlador de dominio tiene estas opciones:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   Las opciones del controlador de dominio que se pueden configurar incluyen **servidor DNS**, **Catálogo global** y **controlador de dominio de solo lectura**.  
  
    Microsoft recomienda que todos los controladores de dominio proporcionen servicios de catálogos globales y DNS para gran disponibilidad en entornos distribuidos, que es la razón por la que el asistente habilita estas opciones de forma predeterminada. Para obtener más información sobre la implementación de RODC, consulte [Guía de planeamiento e implementación del controlador de dominio de solo lectura](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Para obtener más información acerca de cómo agregar un controlador de dominio a un dominio existente, vea [instalar un controlador de dominio de Windows Server 2012 de &#40;réplica en&#41;un nivel de dominio existente 200](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>Opciones de DNS  
Si instalas el servidor DNS, aparece la siguiente página **Opciones de DNS**:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Cuando se instala el servidor DNS, los registros de delegación que apuntan al servidor DNS como autoritativos para la zona deben crearse en la zona primaria del Sistema de nombres de dominio (DNS). Los registros de delegación transfieren la autoridad de resolución de nombres y proporcionan referencias correctas a otros servidores y clientes DNS de los nuevos servidores que van a ser autoritativos para la nueva zona. Estos registros de recursos incluyen:  
  
-   Un registro de recursos de servidor de nombres (NS) para realizar la delegación. Este registro de recursos indica que el servidor con el nombre ns1.na.ejemplo.microsoft.com es un servidor autoritativo para el subdominio delegado.  
  
-   Un registro de recursos de host (A o AAAA) también conocido como registro de adherencia debe estar presente para resolver el nombre del servidor especificado en el registro de recursos de servidor de nombres (NS) en su dirección IP. En ocasiones, el proceso de resolver el nombre de host de este registro de recursos en el servidor DNS delegado del registro de recursos de servidor de nombres (NS) se denomina "búsqueda de adherencias".  
  
Puede hacer que el Asistente para configuración de Active Directory Domain Services los cree automáticamente. El asistente comprueba si existen los registros correspondientes en la zona DNS primaria después de hacer clic en **Siguiente** en la página **Opciones del controlador de dominio**. Si el asistente no puede comprobar si los registros existen en el dominio primario, el asistente proporciona la opción de crear una nueva delegación DNS para un nuevo dominio (o actualiza la delegación existente) en forma automática y continuar con la instalación del controlador de dominio nuevo.  
  
Como alternativa, se pueden crear esos registros de delegación de DNS antes de instalar el servidor DNS. Para crear una delegación de zona, abra **Administrador de DNS**, haga clic con el botón secundario en el dominio primario y, a continuación, haga clic en **Delegación nueva**. Siga los pasos del Asistente para nueva delegación para crear la delegación.  
  
El proceso de instalación intenta crear la delegación para asegurarse de que los equipos de otros dominios puedan resolver las consultas DNS para hosts (incluidos controladores de dominio y equipos miembro) en el subdominio DNS. Tenga en cuenta que los registros de delegación pueden crearse automáticamente solo en los servidores DNS de Microsoft. Si la zona primaria de dominio DNS reside en servidores DNS de terceros, como por ejemplo BIND, aparece una advertencia sobre el error al crear registros de delegación DNS en la página de comprobación Requisitos previos. Para obtener más información acerca de la advertencia, consulte [problemas conocidos de la instalación de AD DS](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Las delegaciones entre el dominio primario y el subdominio que se va a promocionar se pueden crear y validar antes o después de la instalación. No existen motivos para demorar la instalación de un controlador de dominio nuevo, porque no se puede crear ni actualizar la delegación DNS.  
  
Para obtener más información acerca de la delegación, consulte Descripción de la [delegación de zona](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Si la delegación de zonas no es posible en su situación, puede considerar otros métodos para proporcionar una resolución de nombres de otros dominios a los hosts de su dominio. Por ejemplo, el administrador de DNS de otro dominio puede configurar el reenvío condicional, zonas de rutas internas o zonas secundarias para resolver los nombres en su dominio. Para obtener más información, vea los temas siguientes:  
  
-   [Descripción](https://go.microsoft.com/fwlink/?LinkID=157399) de los tipos de zona (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Descripción de las zonas de rutas internas](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Descripción de los reenviadores](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>Opciones de RODC  
Las siguientes opciones aparecen cuando se instala un controlador de dominio de solo lectura (RODC).  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Las cuentas de administrador delegadas obtienen permisos administrativos locales para RODC. Estos usuarios pueden trabajar con privilegios equivalentes al grupo de administradores del equipo local. No son miembros de los grupos Admins del dominio o de las cuentas predefinidas de administrador del dominio. Esta opción es útil para delegar la administración de la sucursal sin dar permisos administrativos de dominio. No es necesario configurar la delegación de la administración. Para obtener más información, consulte [separación de roles de administrador](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   La Directiva de replicación de contraseñas actúa como una lista de control de acceso (ACL). Determina si un RODC puede almacenar en memoria caché una contraseña. Después de que el RODC reciba una solicitud de inicio de sesión de un equipo o usuario autenticado, consulta la Directiva de replicación de contraseñas para determinar si se debería almacenar en caché la contraseña de la cuenta. La misma cuenta podrá iniciar sesiones posteriores en forma más eficiente.  
  
    La Directiva de replicación de contraseñas (PRP) muestra una lista de cuentas cuyas contraseñas pueden almacenarse en memoria caché, y de cuentas en las cuales se deniega en forma explícita que sus contraseñas se almacenen en memoria caché. La lista de cuentas de usuario y equipo en que se permite el almacenamiento en memoria caché no implica que el RODC haya almacenado necesariamente en memoria caché las contraseñas de esas cuentas. Un administrador puede, por ejemplo, especificar de antemano las cuentas que el RODC almacenará en memoria caché. De esta manera, el RODC puede autenticar aquellas cuentas, aunque el vínculo WAN al sitio del concentrador se encuentre sin conexión.  
  
    Los usuarios o equipos a los que no se les permite (incluidos de forma implícita) o a los que se les deniega el almacenamiento de la contraseña en memoria caché. Si esos usuarios o equipos no tiene acceso a un controlador de dominio de escritura, no pueden tener acceso a los recursos o funciones proporcionados por AD DS. Para obtener más información acerca de la PRP, consulte [Directiva de replicación de contraseñas](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). Para obtener más información sobre la administración de la PRP, consulte [administrar el Directiva de replicación de contraseñas](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Para obtener más información sobre la instalación de RODC, consulte [instalar un servidor de Windows 2012 Active Directory RODC &#40;&#41; &#40;de controlador de&#41;dominio de solo lectura nivel 200](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>Opciones adicionales  
La siguiente opción aparece en la página **Opciones adicionales** si se está creando un nuevo dominio:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
Las siguientes opciones aparecen en la página **Opciones adicionales** si se instala un controlador de dominio adicional en un dominio existente:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   Puede especificar un controlador de dominio como origen de replicación, o bien permitir que el asistente elija cualquier controlador de dominio como origen de replicación.  
  
-   También puede eligir instalar el controlador de dominio con medios con copia de seguridad mediante la opción Instalar desde medios (IFM). Si los medios de instalación se encuentran almacenados localmente, la opción **Instalar desde el medio/Ruta de acceso** le permite buscar la ubicación del archivo. La opción para examinar no está disponible si la instalación es remota. Puede hacer clic en **Comprobar** para asegurarse de que la ruta proporcionada corresponda a medios válidos. Los medios usados por la opción IFM solo deben crearse con Copias de seguridad de Windows Server o Ntdsutil. exe desde otro equipo con Windows Server 2012 existente; no se puede usar un sistema operativo Windows Server 2008 R2 o anterior para crear medios para un controlador de dominio de Windows Server 2012. Si los medios están protegidos con SYSKEY, el Administrador del servidor pregunta la contraseña de la imagen durante la comprobación.  
  
Para obtener más información sobre cómo crear un dominio, vea [instalar un nuevo Windows Server 2012 Active Directory nivel de dominio &#40;secundario o de&#41;árbol 200](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Para obtener más información acerca de cómo agregar un controlador de dominio a un dominio existente, vea [instalar un controlador de dominio de Windows Server 2012 de &#40;réplica en&#41;un nivel de dominio existente 200](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>Rutas  
Las siguientes opciones aparecen en la página **Rutas de acceso**.  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   La página **Rutas de acceso** permite reemplazar las ubicaciones de carpeta predeterminadas de la base de datos AD DS, los registros de transacciones de la base de datos y el recurso compartido SYSVOL. Las ubicaciones predeterminadas siempre están en %systemroot%.  
  
Especifique la ubicación para la base de datos de AD DS (NTDS.DIT), archivos de registro y SYSVOL. Para realizar una instalación local, se puede examinar la ubicación donde se desean almacenar los archivos.  
  
## <a name="BKMK_AdprepCreds"></a>Opciones de preparación  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Si actualmente no ha iniciado sesión con suficientes credenciales para ejecutar los comandos de adprep.exe y es necesario que adprep se ejecute para completar la instalación de AD DS, se le pide que proporcione las credenciales para ejecutar adprep.exe. Se requiere que Adprep se ejecute para agregar el primer controlador de dominio que ejecuta Windows Server 2012 a un dominio o bosque existente. Más específicamente:  
  
-   Se debe ejecutar Adprep/ForestPrep para agregar el primer controlador de dominio que ejecuta Windows Server 2012 a un bosque existente. Este comando debe ejecutarse por un miembro de los grupos Administradores de empresas, Administradores de esquema y Admins. del dominio del dominio que hospeda el maestro de esquema. Para que este comando se complete correctamente, debe haber conectividad entre el equipo en que se ejecuta el comando y el maestro de esquema para el bosque.  
  
-   Se debe ejecutar Adprep/DomainPrep para agregar el primer controlador de dominio que ejecuta Windows Server 2012 a un dominio existente. Este comando debe ejecutarse por un miembro del grupo Admins. del dominio del dominio en el que se va a instalar el controlador de dominio que ejecuta Windows Server 2012. Para que este comando se complete correctamente, debe haber conectividad entre el equipo en que se ejecuta el comando y el maestro de infraestructura para el dominio.  
  
-   Debe ejecutarse Adprep /rodcprep para agregar el primer RODC a un bosque existente. Este comando debe ejecutarse por un miembro del grupo Administradores de empresas. Para que este comando se complete correctamente, debe haber conectividad entre el equipo en que se ejecuta el comando y el maestro de infraestructura para cada partición de directorio de aplicación en el bosque.  
  
Para obtener más información acerca de adprep. exe, consulte [integración de adprep. exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) y vea [Ejecutar adprep. exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>Opciones de revisión  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   La página **Revisar opciones** permite validar la configuración y asegura que esta cumpla con los requisitos antes de comenzar con la instalación. Esta no es la última oportunidad para detener la instalación con el Administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con esta.  
  
-   La página **Revisar opciones** del Administrador del servidor también ofrece un botón **Ver script** opcional para crear un archivo de texto Unicode que contiene la configuración ADDSDeployment actual como un script Windows PowerShell único. Esto le permite usar la interfaz gráfica del Administrador del servidor como un estudio de implementación de Windows PowerShell. Use el Asistente para configuración de Servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, cancele el asistente. Este proceso crea un ejemplo válido y sintácticamente correcto para realizar más modificaciones o la utilización directa.  
  
## <a name="BKMK_PrerqCheckPage"></a>Comprobación de requisitos previos  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Algunas de las advertencias que aparecen en esta página incluyen:  
  
-   Los controladores de dominio que ejecutan Windows Server 2008 o posterior tienen una configuración predeterminada para "permitir algoritmos de criptografía compatibles con Windows NT 4" que impide los algoritmos de criptografía más débiles al establecer sesiones de canal seguro. Para obtener más información sobre el impacto potencial y una solución alternativa, consulte el artículo [942564](https://support.microsoft.com/kb/942564)de Knowledge base.  
  
-   No se pudo crear ni actualizar la delegación DNS. Para obtener más información, vea [Opciones de DNS](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   La comprobación de requisitos previos requiere llamadas WMI. Puede producirse un error si son reglas de firewall bloqueadas, y devuelven un error de servidor RPC no disponible.  
  
Para obtener más información acerca de comprobaciones de requisitos previos específicas que se realizan para la instalación de AD DS, consulte el tema sobre las [pruebas de requisitos previos](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="BKMK_Results"></a>Resultados  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
En esta página, se pueden revisar los resultados de la instalación.  
  
También puede seleccionar reiniciar el servidor de destino después de que finalice el asistente; pero si la instalación se realiza correctamente, el servidor siempre se reiniciará, aunque no se seleccione esta opción. En algunos casos, después de que finaliza el asistente en un servidor de destino que no se unió al dominio antes la instalación, el estado del sistema del servidor de destino puede hacer que el servidor esté inaccesible en la red, o el estado del sistema puede impedir que se tengan permisos para administrar el servidor remoto.  
  
Si el servidor de destino no se puede reiniciar en este caso, deberá reiniciarlo manualmente. Las herramientas tales como shutdown.exe o Windows PowerShell no pueden reiniciarlo. Puede usar los Servicios de Escritorio remoto para iniciar sesión y apagar en forma remota el servidor de destino.  
  
## <a name="BKMK_RemovalCredsPage"></a>Credenciales de eliminación de roles  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
Las opciones de degradación se configuran en la página **Credenciales** . Proporcione las credenciales necesarias para realizar la degradación de la siguiente lista:  
  
-   La degradación de un controlador de dominio adicional requiere credenciales de Administrador de dominio. Al seleccionar **forzar eliminación del controlador de dominio** , se degrada el controlador de dominio sin quitar los metadatos del objeto de controlador de dominio de Active Directory.  
  
    > [!IMPORTANT]  
    > No seleccione esta opción a menos que el controlador de dominio no pueda establecer contacto con otros controladores de dominio y no haya una *forma razonable* para resolver el problema de red. La degradación forzada deja metadatos huérfanos en Active Directory en los controladores de dominio restantes del bosque. Además, todos los cambios no replicados en ese controlador de dominio, como por ejemplo contraseñas o cuentas de usuario nuevas, se pierden para siempre. Los metadatos huérfanos son la causa raíz en un porcentaje significativo de casos del Soporte al cliente de Microsoft para AD DS, Exchange, SQL y otro software. Si degrada a la fuerza un controlador de dominio, *debe* realizar la limpieza manual de los metadatos en forma inmediata. Para conocer los pasos necesarios, consulta el tema [Limpiar metadatos de servidor](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   La degradación del último controlador de dominio en un dominio requiere la pertenencia al grupo Administradores de empresas, ya que este quita el dominio en sí (si este es el último dominio en el bosque, esto quita el bosque). El Administrador del servidor le informa si el controlador de dominio actual es el último controlador de dominio en el dominio. Seleccione **Último controlador de dominio en el dominio** para confirmar que el controlador de dominio es el último en el dominio.  
  
Para obtener más información acerca de cómo quitar AD DS, consulte [quitar Active Directory Domain Services (nivel 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) y [degradar los &#40;controladores de&#41;dominio y los dominios de nivel 200](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>Opciones de eliminación de AD DS y advertencias  
Si necesita ayuda con la página Revisar opciones, consulte Opciones de revisión  
  
Si el controlador de dominio hospeda roles adicionales, tales como el rol de servidor DNS o el servidor de catálogo global, aparece la siguiente página de advertencia:  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
Debe hacer clic en **Continuar con la eliminación** para reconocer que los roles adicionales ya no estarán disponibles, para poder hacer clic en **Siguiente** y continuar.  
  
Si se fuerza la eliminación de un controlador de dominio, los cambios en los objetos de Active Directory que no se hayan replicado con otros controladores de dominio en el dominio se perderán. Además, si el controlador de dominio hospeda roles de maestro de operaciones, el catálogo global o el rol de servidor DNS, podrá impactar de la siguiente manera en las operaciones críticas en el dominio y bosque. Antes de quitar un controlador de dominio que hospeda roles maestros de operaciones, intente transferir el rol a otro controlador de dominio. Si no es posible transferir el rol, primero quite Active Directory Domain Services de este equipo y, a continuación, use Ntdsutil.exe para asumir el rol. Use Ntdsutil en el controlador de dominio al que planea asumir el rol; si es posible, use un asociado de replicación reciente en el mismo sitio que este controlador de dominio. Para obtener más información acerca de cómo transferir y asumir roles de maestro de operaciones, consulte el [artículo 255504](https://go.microsoft.com/fwlink/?LinkId=80395) en Microsoft Knowledge base. Si el asistente no puede determinar si el controlador de dominio hospeda un rol maestro de operaciones, ejecute el comando netdom.exe para determinar si este controlador de dominio realiza roles maestros de operaciones.  
  
-   Catálogo global: los usuarios podrían tener problemas para iniciar sesión en los dominios del bosque. Antes de quitar un servidor de catálogo global, asegúrese de que haya suficientes servidores de catálogo global en este bosque y sitio para prestar servicio a los inicios de sesión de los usuarios. Si es necesario, designe otro servidor de catálogo global y actualice los clientes y aplicaciones con la nueva información.  
  
-   Servidor DNS: todos los datos DNS almacenados en zonas integradas de Active Directory se perderán. Después de quitar AD DS, este servidor DNS no podrá realizar la resolución de nombres para las zonas DNS que estaban integradas con Active Directory. Por lo tanto, se recomienda que actualice la configuración de DNS de todos los equipos que actualmente hacen referencia a la dirección IP de este servidor DNS para la resolución de nombres con la dirección IP de un nuevo servidor DNS.  
  
-   Maestro de infraestructura: es posible que a los clientes del dominio les sea difícil encontrar objetos en otros dominios. Antes de continuar, transfiera el rol maestro de infraestructura a un controlador de dominio que no sea un servidor de catálogo global.  
  
-   Maestro de RID: es posible que tenga problemas al crear cuentas de usuario, cuentas de equipo y grupos de seguridad nuevos. Antes de continuar, transfiera el rol maestro RID a un controlador de dominio en el mismo dominio que este controlador de dominio.  
  
-   Emulador de controlador de dominio principal (PDC): las operaciones que realice el emulador de PDC, como las actualizaciones de Directivas de grupo y el restablecimiento de contraseñas para cuentas que no son de AD DS, no funcionarán apropiadamente. Antes de continuar, transfiera el rol maestro de emulador de PDC a un controlador de dominio que esté en el mismo dominio que este controlador de dominio.  
  
-   Maestro de esquema: ya no podrá modificar el esquema para este bosque. Antes de continuar, transfiera el rol maestro de esquema a un controlador de dominio en el dominio raíz del bosque.  
  
-   Maestro de nomenclatura de dominios: ya no podrá agregar dominios a este bosque, o quitarlos de él. Antes de continuar, transfiera el rol maestro de nomenclatura de dominios en el dominio raíz del bosque.  
  
-   Se quitarán todas las particiones de directorio de aplicaciones en este controlador de dominio de Active Directory. Si un controlador de dominio mantiene la última replica de una o más particiones de directorio de aplicaciones, cuando se complete la operación de eliminación, esas particiones ya no existirán.  
  
Tenga cuidado que el dominio ya no existirá después de desinstalar los Servicios de dominio de Active Directory del último controlador de dominio en el dominio.  
  
Si el controlador de dominio es un servidor DNS que se delega para hospedar la zona DNS, la siguiente página proporcionará la opción para quitar el servidor DNS de la delegación de zona DNS.  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Para obtener más información acerca de cómo quitar AD DS, consulte [quitar Active Directory Domain Services (nivel 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) y [degradar los &#40;controladores de&#41;dominio y los dominios de nivel 200](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>Nueva contraseña de administrador  
La página **nueva contraseña de administrador** requiere que proporcione una contraseña para la cuenta de administrador del equipo local integrada, una vez que se complete la degradación y el equipo se convierta en un servidor miembro de dominio o equipo de grupo de trabajo.  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Para obtener más información acerca de cómo quitar AD DS, consulte [quitar Active Directory Domain Services (nivel 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) y [degradar los &#40;controladores de&#41;dominio y los dominios de nivel 200](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>Opciones de revisión  
La página **Revisar opciones** proporciona la posibilidad de exportar la configuración para la degradación a un script de Windows PowerShell para que pueda automatizar degradaciones adicionales. Haga clic en **Disminuir nivel** para quitar AD DS.  
  
![AD DS instalar](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


