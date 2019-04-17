---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: "Instalación de AD DS y descripciones de página del Asistente de eliminación"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fa023398822e79ca8c3e93d44bb1e87fc9190cee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Instalación de AD DS y descripciones de página del Asistente de eliminación

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se proporciona descripciones de los controles en las siguientes páginas del asistente que componen la instalación del rol de servidor de AD DS y eliminación en el administrador del servidor.  
  
-   [Configuración de implementación](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Opciones del controlador de dominio](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Opciones de DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Opciones de RODC](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Opciones adicionales](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Rutas de acceso](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Opciones de preparación](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Opciones de revisión](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Comprobación de requisitos previos](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [Resultados](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Credenciales de eliminación de rol](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [Advertencias y opciones de eliminación de AD DS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nueva contraseña de administrador](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Confirmar selecciones de eliminación de roles](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>Configuración de implementación  
El administrador del servidor comienza cada instalación del controlador de dominio con la **configuración de implementación** página. El resto de opciones y los campos obligatorios cambian en esta página y las páginas siguientes, según qué operación de implementación que seleccione. Por ejemplo, si creas un bosque nuevo, el **preparación opciones** página no aparece, pero si instalas el primer controlador de dominio que ejecuta Windows Server 2012 en un dominio o bosque existente.  
  
Algunas pruebas validaciones se realizan en esta página y más tarde como parte de comprobaciones de requisitos previos. Por ejemplo, si intentas instalar el primer controlador de dominio de Windows Server 2012 en un bosque que tenga el nivel funcional de Windows 2000, aparecerá un error en esta página.  
  
Las siguientes opciones aparecen cuando se crea un nuevo bosque.  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Cuando creas un bosque nuevo, debes especificar un nombre de dominio raíz del bosque. No puede ser el nombre de dominio raíz de bosque único de la etiqueta (por ejemplo, debe ser "contoso.com" en lugar de "contoso"). Debe usar permitidas convenciones de nomenclatura de dominio DNS. Puedes especificar un nombre de dominio Internacionalice (IDN). Para obtener más información sobre las convenciones de nomenclatura de dominio DNS, consulta [KB 909264](https://support.microsoft.com/kb/909264).  
  
-   No crear nuevos bosques de Active Directory con el mismo nombre que el nombre DNS externo. Por ejemplo, si la dirección URL de DNS de Internet es http://contoso.com, debes elegir un nombre diferente para el bosque interno evitar problemas de compatibilidad futura. Ese nombre debe ser único e improbables para el tráfico de la web, como corp.contoso.com.  
  
-   Debe ser miembro del grupo Administradores en el servidor donde desea crear un bosque nuevo.  
  
Para obtener más información sobre cómo crear un bosque, consulta [instalar un nuevo Windows Server 2012 bosque de Active Directory & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Las siguientes opciones aparecen cuando se crea un nuevo dominio.  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Si creas un nuevo dominio árbol, debes especificar el nombre de dominio raíz del bosque en lugar del dominio principal, pero las páginas restantes del asistente y las opciones son las mismas.  
  
-   Haz clic en **selecciona** para buscar el dominio principal o el árbol de Active Directory o escribe un nombre de dominio o el árbol de elemento primario válido. A continuación, escribe el nombre del dominio nuevo en **nuevo nombre de dominio**.  
  
-   Dominio de árbol: proporcionar un nombre de dominio raíz válido, completo; el nombre no puede ser la etiqueta única y debe usar los requisitos de nombre de dominio DNS.  
  
-   Dominio secundario: proporcionar un elemento secundario válido, etiqueta única en nombre de dominio; el nombre debe usar los requisitos de nombre de dominio DNS.  
  
-   El Asistente para la configuración de los servicios de dominio de Active Directory solicita las credenciales de dominio si sus credenciales actuales no son del dominio. Haz clic en **cambio** para proporcionar las credenciales de dominio.  
  
Para obtener más información sobre cómo crear un dominio, consulta el tema [instalar un Windows Server 2012 Active Directory un menor nueva o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Las siguientes opciones aparecen al agregar un nuevo controlador de dominio a un dominio existente.  
  
![Instalación de AD DS](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Haz clic en **selecciona** para buscar el dominio o escribe un nombre de dominio válido.  
  
-   El administrador del servidor le solicita las credenciales válidas si es necesario. Instalar un controlador de dominio requiere la pertenencia al grupo de administradores de dominio.  
  
    Además, el primer controlador de dominio que ejecuta Windows Server 2012 en un bosque de instalación son necesarias las credenciales que incluyen la pertenencia a grupos en grupos de administradores de empresa y de administradores de esquema. El Asistente para la configuración de los servicios de dominio de Active Directory solicita más adelante si sus credenciales actuales no tienen los permisos adecuados o pertenencia a grupos.  
  
Para obtener más información sobre cómo agregar un controlador de dominio a un dominio existente, consulta [instalar un controlador de dominio de réplica de Windows Server 2012 en un dominio existente & #40; Nivel 200 & #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>Opciones del controlador de dominio  
Si vas a crear un bosque nuevo, la página Opciones de controlador de dominio tiene estas opciones:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   Los niveles funcionales de bosque y dominio se establecen en Windows Server 2012 de manera predeterminada.  
  
    Hay una nueva característica en el nivel funcional de dominio de Windows Server 2012: la compatibilidad con Control de acceso dinámico y protección de la directiva de plantilla administrativa de KDC Kerberos tiene dos valores (siempre proporcionar notificaciones y producirá un error en las solicitudes de autenticación unarmored) que requieren el nivel funcional del dominio de Windows Server 2012. Para obtener más información, consulta "Soporte técnico para reclamaciones, autenticación compuesta y protección de Kerberos" en [Novedades en la autenticación Kerberos](https://technet.microsoft.com/library/hh831747.aspx).    
    El nivel funcional del bosque de Windows Server 2012 no proporciona ninguna característica nueva, pero garantiza que cualquier nuevo dominio creado en el bosque funcionará automáticamente en el nivel funcional de dominio de Windows Server 2012. El nivel funcional de dominio de Windows Server 2012 no proporciona ninguna nuevo otras características junto a la compatibilidad con el Control de acceso dinámico y protección de Kerberos, pero garantiza que cualquier controlador de dominio en el dominio ejecuta Windows Server 2012. Para obtener más información sobre otras características que están disponibles en diferentes niveles funcionales, consulta [niveles funcionales de conocimiento Active Directory Domain Services (AD DS)](../active-directory-functional-levels.md).  
  
    Más allá de los niveles funcionales, un controlador de dominio que ejecute Windows Server 2012 proporciona características adicionales que no están disponibles en un controlador de dominio que ejecute una versión anterior de Windows Server. Por ejemplo, un controlador de dominio que ejecute Windows Server 2012 puede usarse para clonación del controlador de dominio virtual, mientras que un controlador de dominio que ejecute una versión anterior de Windows Server no.  
  
-   Servidor DNS se selecciona de manera predeterminada cuando se crea un nuevo bosque. El primer controlador de dominio del bosque debe ser un servidor de catálogo global (GC) y no puede ser un controlador de dominio solo lectura (RODC).  
  
-   Se necesita la contraseña de modo de restauración de servicios de directorio (DSRM) para iniciar sesión en un controlador de dominio donde no se está ejecutando AD DS. La contraseña especificada debe cumplir con la directiva de contraseñas aplicada al servidor, que, de manera predeterminada, no requiere una contraseña segura; solo una contraseña en blanco. Siempre elegir una contraseña segura y compleja o preferiblemente, una frase de contraseña. Para obtener información sobre cómo sincronizar la contraseña DSRM con la contraseña de una cuenta de usuario de dominio, consulta [KB 961320](https://support.microsoft.com/kb/961320).  
  
Para obtener más información sobre cómo crear un bosque, consulta [instalar un nuevo Windows Server 2012 bosque de Active Directory & #40; Nivel 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Si vas a crear un dominio secundario, la página Opciones de controlador de dominio tiene estas opciones:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   El nivel funcional del dominio se establece de forma predeterminada en Windows Server 2012. Puedes especificar cualquier otro valor que tenga al menos el valor de nivel funcional del bosque o superior.  
  
-   Las opciones de controlador de dominio puede configurar incluyen **servidor DNS** y **catálogo Global**; No puedes configurar el controlador de dominio de solo lectura como el primer controlador de dominio en un dominio nuevo.  
  
    Microsoft recomienda que todos los controladores de dominio proporcionan DNS y servicios de catálogo global para una alta disponibilidad en entornos distribuidos, lo que permite que el Asistente para estas opciones de manera predeterminada al crear un nuevo dominio.  
  
-   La **opciones del controlador de dominio** página también te permite elegir la lógica adecuada de Active Directory **nombre del sitio** desde la configuración del bosque. De manera predeterminada, selecciona el sitio con la subred más correcta. Si hay un único sitio, selecciona automáticamente ese sitio.  
  
    > [!IMPORTANT]  
    > Si el servidor no pertenece a una subred de Active Directory y no hay más de un sitio, se selecciona nada y la **siguiente** botón no está disponible hasta que haya elegido un sitio desde la lista.  
  
Para obtener más información sobre cómo crear un dominio, consulta el tema [instalar un Windows Server 2012 Active Directory un menor nueva o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Si vas a agregar un controlador de dominio a un dominio, la página Opciones de controlador de dominio tiene estas opciones:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   Las opciones de controlador de dominio puede configurar incluyen **servidor DNS** y **catálogo Global**, y **controlador de dominio de solo lectura**.  
  
    Microsoft recomienda que todos los controladores de dominio proporcionan DNS y servicios de catálogo global para una alta disponibilidad en entornos distribuidos, lo que permite que el Asistente para estas opciones de manera predeterminada. Para obtener más información acerca de cómo implementar los RODC, consulta [Guía de implementación y planificación de controlador de dominio de solo lectura](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Para obtener más información sobre cómo agregar un controlador de dominio a un dominio existente, consulta [instalar un controlador de dominio de réplica de Windows Server 2012 en un dominio existente & #40; Nivel 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>Opciones de DNS  
Si instalas un servidor DNS, el siguiente **opciones DNS** aparecerá la página:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Cuando se instala el servidor DNS, deben crearse los registros de delegación que apuntan al servidor DNS como autorizado para la zona en la zona de sistema de nombres de dominio (DNS) principal. Registros de delegación transferir la autoridad de resolución de nombre y proporcionan una referencia correcta a otros servidores DNS y clientes de los servidores nuevo que se realizan autorizados para la nueva zona. Estos registros de recursos incluyen lo siguiente:  
  
-   Registro de recursos del servidor (NS) de nombre para realizar la delegación. Este registro de recursos se anuncia que el servidor denominado ns1.na.example.microsoft.com es un servidor autorizado para el subdominio delegado.  
  
-   Un registro de recursos de host (A o AAAA) también conocido como un registro de lazo de unión debe estar presente para resolver el nombre del servidor que se especifica en el registro de recursos del servidor (NS) de nombre a su dirección IP. El proceso de resolver el nombre de host en este registro de recursos en el servidor DNS delegado en el registro de recursos del servidor (NS) de nombre a veces se conoce como "pegar perseguir".  
  
Puedes hacer que el Asistente para la configuración de los servicios de dominio de Active Directory crearlos automáticamente. El asistente comprueba que existen los registros correspondientes en la zona DNS principal después de hacer clic **siguiente** en la **opciones del controlador de dominio** página. Si el asistente no puede comprobar que existen los registros en el dominio principal, el asistente proporciona automáticamente con la opción de crear una nueva delegación de DNS para un dominio nuevo (o actualizar la delegación existente) y continuar con la nueva instalación del controlador de dominio.  
  
Como alternativa, puedes crear estos registros DNS delegación antes de instalar el servidor DNS. Para crear una delegación de zona, abra **Administrador de DNS**, haz clic en el dominio principal y, a continuación, haz clic en **delegación nueva**. Sigue los pasos en el Asistente para crear la delegación.  
  
El proceso de instalación intenta crear la delegación para garantizar que los equipos de otros dominios pueden resolver consultas DNS de hosts, incluidos los controladores de dominio y equipos miembro, en el subdominio DNS. Ten en cuenta que los registros de delegación se pueden crear automáticamente solo en los servidores DNS de Microsoft. Si la zona de dominio DNS principal reside en servidores DNS de terceros, como enlace, aparece una advertencia acerca del error para crear los registros de delegación DNS en la página de comprobación de requisitos previos. Para obtener más información acerca de la advertencia, consulta [problemas para instalar AD DS conocidos](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Delegación entre el dominio principal y el subdominio promocionando puede crearse y valida antes o después de la instalación. No es necesario para retrasar la instalación de un nuevo controlador de dominio, porque no se puede crear o actualizar la delegación DNS.  
  
Para obtener más información acerca de la delegación, consulta [acerca de la delegación zona](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Si no es posible en tu situación delegación de zona, también puedes considerar otros métodos para proporcionar la resolución de nombres de otros dominios a los hosts de tu dominio. Por ejemplo, el administrador DNS de otro dominio podría configurar el reenvío condicional, zonas de código auxiliar o secundario zonas para poder resolver los nombres de tu dominio. Para obtener más información, consulta los siguientes temas:  
  
-   [Descripción de los tipos de zona](https://go.microsoft.com/fwlink/?LinkID=157399) (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Descripción de zonas de código auxiliar](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Servidores de reenvío descripción](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>Opciones de RODC  
Las siguientes opciones aparecen cuando se instala un controlador de dominio de solo lectura (RODC).  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Cuentas de administrador delegado tener permisos administrativos locales en el RODC. Estos usuarios pueden funcionar con privilegios equivalentes al grupo de administradores del equipo local. No son miembros de los administradores de dominio o los grupos Administradores de dominio integrados. Esta opción es útil para delegar la administración de office rama sin dar un vistazo a los permisos de administrador de dominio. No es necesario configurar la delegación de administración. Para obtener más información, consulta [separación de roles de administrador](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   La directiva de replicación de contraseñas actúa como una lista de control de acceso (ACL). Determina si un RODC puede almacenar en caché una contraseña. Después de que el RODC recibe una solicitud de inicio de sesión de usuario o equipo autenticada, se refiere a la directiva de replicación de contraseñas para determinar si la contraseña de la cuenta debe almacenarse en caché. La misma cuenta, a continuación, puede realizar subsiguientes inicios de sesión de forma más eficaz.  
  
    La directiva de replicación de contraseñas (PRP) enumera las cuentas cuyos contraseñas pueden almacenarse en caché y cuentas cuyas contraseñas se ha denegado explícitamente en la caché. La lista de cuentas de usuario y del equipo que se pueden almacenar en caché no implica que el RODC necesariamente ha almacenado en caché las contraseñas de las cuentas. Un administrador, por ejemplo, especificar de antemano las cuentas que se almacenarán en caché un RODC. De esta forma, el RODC puede autenticar esas cuentas, incluso si el vínculo WAN para el sitio del concentrador está sin conexión.  
  
    Los usuarios o equipos que no están permiten (incluidos implícita) o denegada no almacene en caché su contraseña. Si los usuarios o equipos no tienen acceso a un controlador de dominio grabable, pueden acceder a recursos de AD DS proporcionados o funcionalidad. Para obtener más información sobre la PRP, consulta [directiva de replicación de contraseñas](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). Para obtener más información acerca de cómo administrar la PRP, consulta [administrar la directiva de replicación de contraseña](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Para obtener más información sobre cómo instalar RODC, consulta [instalar un Windows Server 2012 Active Directory Read-Only controlador de dominio & #40; RODC & #41; & #40; Nivel 200 & #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>Opciones adicionales  
La siguiente opción aparece en la **opciones adicionales** página si vas a crear un nuevo dominio:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
Las siguientes opciones que aparecen en la **opciones adicionales** página si instalas un controlador de dominio en un dominio existente:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   Puedes especificar un controlador de dominio como el origen de replicación o permitir que el Asistente elegir cualquier controlador de dominio como el origen de replicación.  
  
-   También puedes instalar el controlador de dominio mediante una copia de seguridad de contenido multimedia con la instalación de la opción de medios (IFM). Si los medios de instalación se almacena localmente, el **instalar desde medios de ruta de acceso** opción te permite navegar a la ubicación del archivo. La opción Examinar no está disponible para una instalación remota. Puedes hacer clic en **comprobar** para garantizar la ruta de acceso proporcionado es multimedia válidos. Medios usados por la opción IFM deben crearse con copias de seguridad de Windows Server o Ntdsutil.exe desde otro equipo existente de Windows Server 2012. No puedes usar un Windows Server 2008 R2 o el sistema operativo anterior para crear medios para un controlador de dominio de Windows Server 2012. Si el medio está protegido con una SYSKEY, el administrador del servidor solicita contraseña de la imagen durante la comprobación.  
  
Para obtener más información sobre cómo crear un dominio, consulta el tema [instalar un Windows Server 2012 Active Directory un menor nueva o dominio del árbol & #40; Nivel 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Para obtener más información sobre cómo agregar un controlador de dominio a un dominio existente, consulta [instalar un controlador de dominio de réplica de Windows Server 2012 en un dominio existente & #40; Nivel 200 & #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>Rutas de acceso  
Las siguientes opciones que aparecen en la **rutas de acceso** página.  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   La **rutas de acceso** página te permite invalidar las ubicaciones predeterminadas de la carpeta de la base de datos de AD DS, los registros de transacciones de base de datos, y compartir la carpeta SYSVOL. Las ubicaciones predeterminadas de siempre están en % systemroot %.  
  
Especificar la ubicación de la base de datos de AD DS (NTDS.DIT), archivos de registro y SYSVOL. Para una instalación local, puedes buscar la ubicación donde quieres almacenar los archivos.  
  
## <a name="BKMK_AdprepCreds"></a>Opciones de preparación  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Si no actualmente sesión con credenciales suficientes para ejecutar comandos adprep.exe y adprep es necesaria para ejecutar para completar la instalación de AD DS, deberá proporcionar credenciales para ejecutar adprep.exe. Adprep se necesita para ejecutar con el fin de agregar el primer controlador de dominio que se ejecuta Windows Server 2012a un dominio o bosque existente. Más concretamente:  
  
-   Adprep /forestprep debe ejecutarse para agregar el primer controlador de dominio que se ejecuta Windows Server 2012a un bosque existente. Este comando debe ejecutarse con un miembro del grupo Administradores de empresa, el grupo de administradores de esquema y el grupo de administradores de dominio del dominio que hospeda al maestro de esquema. De este comando completar correctamente, debe haber conectividad entre el equipo donde se ejecuta el comando y el maestro de esquema del bosque.  
  
-   Adprep /domainprep debe ejecutarse para agregar el primer controlador de dominio que se ejecuta Windows Server 2012a un dominio existente. Este comando debe ejecutarse con un miembro del grupo Administradores de dominio del dominio donde vas a instalar el controlador de dominio que se ejecuta Windows Server 2012. De este comando completar correctamente, debe haber conectividad entre el equipo donde se ejecuta el comando y el maestro de infraestructura para el dominio.  
  
-   Adprep /rodcprep debe ejecutarse para agregar el primer RODC a un bosque existente. Este comando debe ejecutarse con un miembro del grupo Administradores de empresa. De este comando completar correctamente, debe haber conectividad entre el equipo donde se ejecuta el comando y el maestro de infraestructura para cada partición de directorio de la aplicación en el bosque.  
  
Para obtener más información acerca de Adprep.exe, consulta [Adprep.exe integración](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) y ver [Adprep.exe ejecutando](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>Opciones de revisión  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   La **opciones de revisión** página te permite validar la configuración y asegurarse de que cumplen los requisitos antes de iniciar la instalación. Esto no es la última oportunidad para detener la instalación con el administrador del servidor. Esta página simplemente permite revisar y confirmar la configuración antes de continuar con la configuración.  
  
-   La **opciones de revisión** también ofrece un opcional de la página en el administrador del servidor **ver Script** botón para crear un archivo de texto de Unicode que contiene la configuración actual de ADDSDeployment como una única secuencia de Windows PowerShell. Esto te permite usar la interfaz gráfica de administrador del servidor como un estudio de implementación de Windows PowerShell. Usar al Asistente para la configuración de los servicios de dominio de Active Directory para configurar las opciones, exportar la configuración y, a continuación, Cancelar al asistente. Este proceso crea una muestra sintácticamente correcta y válida para modificaciones adicionales o su uso directo.  
  
## <a name="BKMK_PrerqCheckPage"></a>Comprobación de requisitos previos  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Algunas de las advertencias que aparecen en esta página incluyen:  
  
-   Controladores de dominio que ejecutan Windows Server 2008 o posterior tienen una configuración predeterminada para "Permitir algoritmos criptográficos compatibles con Windows NT 4" que impide que los algoritmos de cifrado más débiles al establecer sesiones del canal seguro. Para obtener más información sobre el impacto potencial y una solución alternativa, consulte el artículo [942564](https://support.microsoft.com/kb/942564).  
  
-   No se pudo creada o actualizada delegación DNS. Para obtener más información, consulta [opciones DNS](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   Comprobar los requisitos previos requiere llamadas a WMI. Puede producir errores si están bloque de reglas de firewall bloqueados y devolver un servidor RPC error no está disponible.  
  
Para obtener más información sobre las comprobaciones de requisitos previos específicas que se llevan a cabo para la instalación de AD DS, consulta [pruebas de requisito previo](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="BKMK_Results"></a>Resultados  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
En esta página, puede revisar los resultados de la instalación.  
  
También puedes seleccionar para reiniciar el servidor de destino una vez completado el asistente, pero si la instalación se realiza correctamente, el servidor se reiniciará siempre independientemente de si seleccionas esta opción. En algunos casos una vez completado el Asistente en un servidor de destino que no se ha unido al dominio antes de la instalación, el estado del sistema del servidor de destino puede hacer que el servidor no accesible en la red o el estado del sistema puede impedir que puedes tener permisos para administrar el servidor remoto.  
  
Si se produce un error en el servidor de destino reiniciar en este caso, debes reiniciar manualmente. Herramientas, como shutdown.exe o Windows PowerShell no pueden reiniciarlo. Puedes usar los servicios de escritorio remoto para iniciar sesión y apagar el servidor de destino de forma remota.  
  
## <a name="BKMK_RemovalCredsPage"></a>Credenciales de eliminación de rol  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
Configurar las opciones de degradación en el **credenciales** página. Proporcionar las credenciales necesarias para realizar la degradación de la siguiente lista:  
  
-   Degradar un controlador de dominio requiere credenciales de administrador de dominio. Seleccionar **forzar la eliminación del controlador de dominio** devolverá el controlador de dominio sin quitar los metadatos del objeto de controlador de dominio de Active Directory.  
  
    > [!IMPORTANT]  
    > No actives esta opción a menos que el controlador de dominio no puede ponerse en contacto con otros controladores de dominio y no hay *ninguna forma razonable* para resolver el problema de esa red. Degradación forzada deja huérfana metadatos en Active Directory en los controladores de dominio restantes en el bosque. Además, todos los cambios sin duplicados en el controlador de dominio, como contraseñas o nuevas cuentas de usuario, se perderán para siempre. Metadatos huérfana están la causa raíz en un gran porcentaje de los casos de atención al cliente de Microsoft para AD DS, Exchange, SQL y otro software. Si forzar la degradación de un controlador de dominio, puedes *debe* manualmente limpiar los metadatos inmediatamente. Para conocer los pasos, revisar [limpia metadatos de servidor](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   Degradar el último controlador de dominio en un dominio requiere la pertenencia al grupo de administradores de empresa, como esta directiva quita del propio dominio (si se trata del último dominio del bosque, esta directiva quita del bosque). El administrador del servidor le informa de si el controlador de dominio actual es el último controlador de dominio del dominio. Selecciona **último controlador de dominio en el dominio** para confirmar el dominio controlador es el último controlador de dominio del dominio.  
  
Para obtener más información sobre cómo quitar AD DS, consulta [quitar Active Directory Domain Services (nivel 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) y [degradar controladores de dominio y dominios & #40; Nivel 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>Advertencias y opciones de eliminación de AD DS  
Si necesitas ayuda con la página de opciones de revisión, consulta opciones de revisión  
  
Si el controlador de dominio hospeda funciones adicionales, como el rol de servidor DNS o el servidor de catálogo global, aparecerá la siguiente página de advertencia:  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
Debe hacer clic **continuar con la eliminación de** para confirmar que las funciones adicionales ya no estará disponibles antes de que puedes hacer clic en **siguiente** para continuar.  
  
Si fuerza la eliminación de un controlador de dominio, se perderán los cambios de objeto de Active Directory que no se replican en otros controladores de dominio del dominio. Además, si el controlador de dominio aloja funciones de maestro de operaciones, el catálogo global o rol de servidor DNS, operaciones críticas en el dominio y bosque pueden verse afectadas como sigue. Antes de quitar un controlador de dominio que hospeda ningún rol de maestro de operaciones, pruebe a transferir la función a otro controlador de dominio. Si no es posible transferir el rol, quitar los servicios de dominio de Active Directory desde este equipo y, a continuación, usar Ntdsutil.exe para asumir la función. Usar Ntdsutil en el controlador de dominio que vas a asumir la función Si es posible, usar a un duplicador recientes en el mismo sitio como este controlador de dominio. Para obtener más información sobre cómo transferir y asumir roles de maestro de operaciones, consulta [artículo 255504](https://go.microsoft.com/fwlink/?LinkId=80395) en Microsoft Knowledge Base. Si el asistente no puede determinar si el controlador de dominio hospeda un rol de maestro de operaciones, ejecuta el comando de netdom.exe para determinar si este controlador de dominio realiza las funciones de maestro de operaciones.  
  
-   Catálogo global: los usuarios podrían tener problemas para iniciar sesión en dominios del bosque. Antes de quitar un servidor de catálogo global, asegúrate de que son suficientes servidores de catálogo global en este bosque y el sitio a los inicios de sesión de usuario de servicio. Si es necesario, designar otro servidor catálogo global y actualizar aplicaciones y los clientes con la nueva información.  
  
-   Servidor DNS: todos los datos DNS que se almacenan en las zonas integradas en Active Directory, se perderán. Después de quitar AD DS, este servidor DNS no podrá realizar la resolución de nombres de las zonas DNS que estaban integrado en Active Directory. Por lo tanto, te recomendamos que actualices la configuración DNS de todos los equipos que actualmente hacen referencia a la dirección IP de este servidor DNS para resolver el nombre con la dirección IP del servidor DNS nuevo.  
  
-   Maestro de infraestructura: los clientes en el dominio podrían tener dificultades para encontrar los objetos de otros dominios. Antes de continuar, transferir el rol de maestro de infraestructura a un controlador de dominio que no es un servidor de catálogo global.  
  
-   QUITAR maestro: es posible que tiene problemas para crear nuevas cuentas de usuario, las cuentas de equipo y grupos de seguridad. Antes de continuar, transferir el rol de maestro RID a un controlador de dominio en el mismo dominio que este controlador de dominio.  
  
-   Emulador de controlador (PDC) del dominio principal: las operaciones realizadas por el emulador PDC, como actualizaciones de la directiva de grupo y restablecimiento de contraseñas para las cuentas de distinta de AD DS, no funcionará correctamente. Antes de continuar, transferir el maestro emulador PDC a un controlador de dominio que se encuentra en el mismo dominio que este controlador de dominio.  
  
-   Maestro de esquema: ya no podrás modificar el esquema para este bosque. Antes de continuar, transferir el rol de maestro de esquema a un controlador de dominio en el dominio raíz del bosque.  
  
-   Maestro nombres de dominio: ya no podrás agregar ni quitar dominios de este bosque. Antes de continuar, transferir la función principal a un controlador de dominio en el dominio raíz del bosque de nombres de dominio.  
  
-   Se quitarán todas las particiones de directorio de aplicación en este controlador de dominio de Active Directory. Si un controlador de dominio contiene la última réplica de una o varias particiones de directorio de aplicación, una vez completada la operación de eliminación, estas particiones dejarán de existir.  
  
Ten en cuenta que el dominio dejarán de existir después de desinstalar los servicios de dominio de Active Directory desde el último controlador de dominio del dominio.  
  
Si el controlador de dominio es un servidor DNS que se delega para hospedar la zona DNS, la siguiente página ofrecen la opción para quitar el servidor DNS de la delegación de zona DNS.  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Para obtener más información sobre cómo quitar AD DS, consulta [quitar Active Directory Domain Services (nivel 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) y [degradar controladores de dominio y dominios & #40; Nivel 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>Nueva contraseña de administrador  
La **nueva contraseña de administrador** página requiere que proporciones una contraseña de cuenta de administrador del equipo local integrada, una vez que finalice la degradación y el equipo se convierte en un servidor miembro del dominio o grupo de trabajo.  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Para obtener más información sobre cómo quitar AD DS, consulta [quitar Active Directory Domain Services (nivel 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) y [degradar controladores de dominio y dominios & #40; Nivel 200 & #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>Opciones de revisión  
La **opciones de revisión** página le ofrece la posibilidad de exportar la configuración de la degradación a un script de PowerShell de Windows para que se pueden automatizar degradaciones adicionales. Haz clic en **degradar** para quitar AD DS.  
  
![Instalación de AD DS](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


