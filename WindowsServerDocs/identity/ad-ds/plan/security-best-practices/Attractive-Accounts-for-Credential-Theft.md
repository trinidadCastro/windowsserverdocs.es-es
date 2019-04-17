---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Cuentas atractivas para el robo de credenciales
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ae1bfe017e8b21c3abfcdc137153b0fd379053fa
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="attractive-accounts-for-credential-theft"></a>Cuentas atractivas para el robo de credenciales

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques de robo de credenciales son aquellos en los que un atacante obtiene inicialmente privilegio más alto-(raíz, de administrador o de sistema, según el sistema operativo en uso) acceso a un equipo en una red y, a continuación, se usa libremente disponible utillaje para extraer las credenciales de las sesiones de otras cuentas de inicio de sesión. Según la configuración del sistema, se pueden extraer estas credenciales en forma de hash, vales o incluso texto no cifrado contraseñas. Si cualquiera de las credenciales cosechadas es para cuentas locales que se pueda existir en otros equipos de la red (por ejemplo, las cuentas de administrador de Windows o cuentas de raíz en OSX, UNIX o Linux), el atacante presenta las credenciales en otros equipos en la red para propagar compromiso en equipos adicionales y para intentar obtener las credenciales de dos tipos específicos de cuentas :  

1.  Cuentas de dominio con privilegios con privilegios amplios y profundos (es decir, las cuentas que tienen privilegios de administrador en varios equipos y en Active Directory). Estas cuentas no pueden ser miembros de cualquiera de los grupos de privilegio más alto en Active Directory, pero se les pueden haya concedido privilegios de nivel de administrador en varios servidores y estaciones de trabajo en el dominio o bosque, lo que hace que eficazmente tan eficaz como miembros de grupos con privilegios en Active Directory. En la mayoría de los casos, las cuentas que se han concedido privilegios elevados en franjas amplia de la infraestructura de Windows son cuentas de servicio, por lo que siempre se deben evaluar las cuentas de servicio para amplitud y profundidad de privilegios.  

2.  "Persona muy importante" cuentas de dominio (VIP). En el contexto de este documento, una cuenta de VIP es cualquier cuenta que tenga acceso a la información que un atacante quiere (la propiedad intelectual y otra información confidencial) o cualquier cuenta que puede usarse para conceder acceso a esa información. Ejemplos de estas cuentas de usuario:  

    1.  Ejecutivos cuyas cuentas tienen acceso a información corporativa confidencial  

    2.  Cuentas para el personal de servicio de asistencia que son responsable de mantenimiento de los equipos y aplicaciones utilizadas por los ejecutivos  

    3.  Cuentas de abogados que tienen acceso a documentos de oferta y el contrato de la organización, si se encuentran los documentos para su propia organización u organizaciones de cliente  

    4.  Canalización planificadores de productos que tienen acceso a los planes y especificaciones de productos en el desarrollo de la empresa, independientemente de los tipos de productos que hace que la empresa  

    5.  Investigadores cuyas cuentas se usan para acceder a datos de estudio, formulaciones de producto o cualquier otra investigación de interés a un atacante  

Porque cuentas con privilegios elevados en Active Directory pueden usarse para propagar compromiso y manipular cuentas VIP o los datos que pueden acceder a, el más útil para los ataques de robo de credenciales son cuentas que son miembros del grupo Administradores de empresa, administradores de dominio y administradores en Active Directory.  

Dado que los controladores de dominio son los repositorios de la base de datos de AD DS y controladores de dominio tienen acceso completo a todos los datos en Active Directory, controladores de dominio están destinados también compromiso, si en paralelo con ataques de robo de credenciales, o después de uno o más Active Directory con privilegios elevados cuentas han puesto en riesgo. Aunque numerosas publicaciones (y muchos de los atacantes) se centran en la pertenencia a grupos de administradores de dominio cuando se describen los ataques de robo de credenciales pass-the-hash y otras (como se describe en [reducir la superficie de ataque Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)), una cuenta que es un miembro de cualquiera de los grupos se enumeran aquí puede usarse para poner en peligro toda la instalación de AD DS.  

> [!NOTE]  
> Para obtener información completa sobre pass-the-hash y otros ataques de robo de credenciales, consulta el [ataques Mitigating Pass-the-Hash (PTH) y otras técnicas de robo de credenciales](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) notas del producto enumerados en [Apéndice M: los vínculos del documento y recomienda lectura](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Para obtener más información acerca de los ataques de determinados adversarios, que se denomina "las amenazas permanentes avanzadas" (apt), consulta [adversarios determinado y ataques dirigida](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Actividades que aumentan la probabilidad de compromiso  
Dado que el destino de robo de credenciales suele ser cuentas de dominio con privilegios elevados y VIP, es importante para los administradores ser consciente de actividades que aumentan la probabilidad de éxito de un ataque de robo de credenciales. Aunque los atacantes también destino cuentas VIP, si VIPs no tienen altos niveles de privilegios en los sistemas o en el dominio, el robo de credenciales de sus requiere otros tipos de ataques, como la dirección VIP para proporcionar información confidencial de ingeniería social. O bien, el atacante debe obtener acceso con privilegios en un sistema en qué VIP se almacenan en caché las credenciales. Por este motivo, las actividades que aumentan la posibilidad de robo de credenciales que se describen aquí se centran principalmente en evitar la adquisición de las credenciales administrativas con privilegios elevados. Estas actividades son los mecanismos comunes por las que los atacantes son capaces de poner en peligro sistemas para obtener credenciales con privilegios.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>Inicio de sesión en equipos no protegidos con cuentas con privilegios  
La vulnerabilidad de core que permite a los ataques de robo de credenciales correctamente consiste en iniciar sesión en equipos que no son seguras con cuentas que son ampliamente y profundamente con privilegios en todo el entorno. Estos inicios de sesión pueden ser el resultado de varios errores de configuración que se describen aquí.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>No mantener separadas las credenciales administrativas  
Si bien es relativamente común, en la evaluación varias instalaciones de AD DS, hemos encontrado a los empleados con una sola cuenta para todo su trabajo. La cuenta es miembro de al menos uno de los grupos con privilegios más elevados en Active Directory y es la misma cuenta que el uso de los empleados para iniciar sesión en sus estaciones de trabajo de la mañana, visita su correo electrónico, explorar sitios de Internet y descargar el contenido en sus equipos. Cuando los usuarios ejecutan con cuentas que se conceden permisos y derechos de administrador local, que exponen el equipo local para completar el compromiso. Cuando esas cuentas son también miembros de los grupos más con privilegios en Active Directory, exponen todo el bosque para poner en peligro, lo que facilita la trivial para que un atacante a obtener el control completo del entorno de Active Directory y Windows.  

De forma similar, en algunos entornos, hemos encontrado que el mismo nombres de usuario y contraseña se usa para las cuentas de raíz en equipos que no sean de Windows como se usan en el entorno de Windows, lo que permite a los atacantes ampliar el compromiso de sistemas de UNIX o Linux en sistemas Windows y viceversa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Inicios de sesión en peligro las estaciones de trabajo o servidores miembro con cuentas con privilegios  
Cuando se usa una cuenta de dominio con privilegios elevados para iniciar una sesión interactiva en un servidor miembro o estación de trabajo en peligro, dicho equipo peligro puede recopilar las credenciales de cualquier cuenta que inicia sesión en el sistema.  

#### <a name="unsecured-administrative-workstations"></a>Estaciones de trabajo administrativas no seguras  
En muchas organizaciones, el personal de TI usa varias cuentas. Se usa una cuenta para iniciar sesión en la estación de trabajo del empleado, y porque son personal de TI, a menudo tienen derechos de administrador local en sus estaciones de trabajo. En algunos casos, es UAC queda activada para que el usuario reciba al menos un acceso dividida token al iniciar sesión y debe elevar cuando se requieren privilegios. Cuando los usuarios realizan las actividades de mantenimiento, suelen usar herramientas de administración instalado de forma local y proporcionar las credenciales para sus cuentas de dominio con privilegios, seleccionando el **ejecutar como administrador** opción o proporcionando las credenciales cuando se te solicite. Aunque esta configuración puede parecer adecuada, que expone el entorno para poner en riesgo porque:  

-   La cuenta de usuario "normal" que los empleados que se usa para iniciar sesión en su estación de trabajo tiene derechos de administrador local, el equipo es vulnerable a [descargas](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) ataques en el que el usuario se convencer a instalar software malintencionado.  

-   El malware está instalado en el contexto de una cuenta de administrador, el equipo se puede usar ahora para capturar las pulsaciones de teclas, contenido del Portapapeles, capturas de pantalla y las credenciales de residentes en memoria, que puede provocar la exposición de las credenciales de una cuenta de dominio eficaces.  

Los problemas en este escenario son dos. En primer lugar, aunque se usan cuentas separadas para la administración local y de dominio, el equipo no es seguro y no protege contra el robo de las cuentas de. En segundo lugar, la cuenta de usuario normal y la cuenta de administrador se han concedido permisos y derechos excesivos.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Navegar por Internet con una cuenta con privilegios elevados  
Los usuarios que inicien sesión en equipos con cuentas que son miembros del grupo de administradores local en el equipo o los miembros de grupos con privilegios en Active Directory y que, a continuación, explorar Internet (o en una intranet en peligro) exponen el equipo local y el directorio en peligro.  

Acceso a un sitio Web con contenido perjudicial con un explorador que se ejecuta con privilegios administrativos puede permitir que un atacante Ingrese un código malintencionado en el equipo local en el contexto del usuario con privilegios. Si el usuario tiene derechos de administrador local en el equipo, los atacantes pueden engañar que el usuario descargar el código malintencionado o abrir datos adjuntos de correo electrónico que aprovechan las vulnerabilidades de la aplicación y aprovechan los privilegios del usuario para extraer localmente en caché las credenciales para todos los usuarios activos en el equipo. Si el usuario tiene derechos administrativos en el directorio por la pertenencia a los administradores de empresa, administradores de dominio o grupo de administradores en Active Directory, el atacante puede extraer las credenciales de dominio y usarlos para poner en peligro el todo el dominio de AD DS o bosque, sin necesidad de poner en peligro cualquier otro equipo en el bosque.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configurar las cuentas locales con privilegios con las mismas credenciales en sistemas  
Configurar el mismo nombre de cuenta de administrador local y la contraseña en varios o todos los equipos permite las credenciales de robo de la base de datos de SAM en un equipo que se utilizará para todos los demás equipos que usan las mismas credenciales poner en peligro. Como mínimo, debes usar contraseñas diferentes para las cuentas de administrador locales en todos los sistemas unidos a un dominio. También pueden ser un nombre único cuentas de administrador local, pero el uso de contraseñas diferentes para las cuentas locales con privilegios de cada sistema es suficiente para garantizar que las credenciales no pueden usarse en otros sistemas.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation y el uso excesivo de grupos con privilegios de dominio  
Concesión de pertenencia de los grupos EA, DA o BA en un dominio, crea un destino para los atacantes. Cuanto mayor sea el número de miembros de estos grupos, mayor será la probabilidad de que un usuario con privilegios puede hacer un uso indebido de las credenciales y exponerlos para robo de credenciales accidentalmente los ataques. Todos a la que un usuario con privilegios de dominio, inicia sesión en el servidor o estación de trabajo presenta un mecanismo posibles por las que las credenciales de usuario con privilegios pueden cosechadas y se usa para poner en peligro el dominio de AD DS y el bosque.  

### <a name="poorly-secured-domain-controllers"></a>Controladores de dominio mal protegido  
Controladores de dominio albergar una réplica de base de datos de un dominio AD DS. En el caso de los controladores de dominio de solo lectura, la réplica local de la base de datos contiene las credenciales para solo un subconjunto de las cuentas en el directorio, ninguna de las cuales son cuentas de dominio con privilegios de manera predeterminada. En los controladores de dominio de lectura y escritura, cada controlador de dominio mantiene una réplica completa de la base de datos de AD DS, incluidas las credenciales no solo para los usuarios con privilegios como administradores de dominio, pero cuentas con privilegios como cuentas de controlador de dominio o la cuenta del dominio Krbtgt, que es la cuenta que está asociada con el KDC de servicio en controladores de dominio. Si se instalan las aplicaciones adicionales que no son necesarias para la funcionalidad del controlador de dominio en controladores de dominio, o si los controladores de dominio no son estrictas revisados y protegidos, los atacantes pueden poner en peligro ellos a través de las vulnerabilidades sin revisión o aprovechan otros vectores de ataque para instalar el software malintencionado directamente en ellos.  

## <a name="privilege-elevation-and-propagation"></a>La propagación y la elevación de privilegios  
Independientemente de los métodos de ataque usados, Active Directory siempre como destino cuando se ataca un entorno de Windows, porque se controla en última instancia el acceso a todo lo que quieren los atacantes. Esto no significa que esté destinado todo el directorio, sin embargo. Cuentas específicas, servidores y componentes de infraestructura normalmente son los principales objetivos de ataques contra Active Directory. A continuación se describen estas cuentas.  

### <a name="permanent-privileged-accounts"></a>Cuentas con privilegios permanente  
Dado que la introducción de Active Directory, ha sido posible usar altamente privilegiada cuentas para crear el bosque de Active Directory y, a continuación, derechos de delegado y los permisos necesarios para realizar la administración diaria a cuentas con menos privilegios. Pertenencia a los grupos Administradores, administradores de dominio o administradores de empresa en Active Directory se requiere solo temporalmente y rara vez en un entorno que implementa los enfoques de privilegios mínimos para la administración diaria.  

Cuentas con privilegios permanente son que se han colocado en grupos con privilegios y allí izquierda de un día a día. Si tu organización pone cinco cuentas en el grupo de administradores de dominio para un dominio, esas cinco cuentas pueden ser específico de 24 horas al día, siete días a la semana. Sin embargo, suele ser necesario real usar cuentas con privilegios de administrador de dominio solo para la configuración específica de todo el dominio y por períodos cortos de tiempo.  

### <a name="vip-accounts"></a>VIP cuentas  
Un destino a menudo se pasa por alto las infracciones de Active Directory es las cuentas de "personas muy importantes" (o VIPs) de una organización. Dado que esas cuentas pueden conceder acceso a los atacantes, que les permite poner en peligro o incluso destruir sistemas de destino, como se describió anteriormente en esta sección, se dirigen cuentas con privilegios.  

### <a name="privilege-attached-active-directory-accounts"></a>Cuentas de "Adjuntos privilegios" Active Directory  
"Adjuntos privilegios" cuentas de Active Directory son cuentas de dominio que no se han realizado miembros de cualquiera de los grupos que tienen los niveles más altos de privilegios en Active Directory, pero en su lugar, se le haya concedido altos niveles de privilegios en muchos servidores y estaciones de trabajo en el entorno. Estas cuentas son mayoría a menudo basado en dominio que están configurados para ejecutar servicios en sistemas unidos a un dominio, por lo general para aplicaciones que se ejecutan en grandes secciones de la infraestructura. Aunque estas cuentas no tienen privilegios en Active Directory, si se les concede privilegios elevados en un gran número de sistemas, que pueden usarse para poner en peligro o incluso destruir grandes segmentos de la infraestructura de lograr el mismo efecto que ponga en peligro una cuenta con privilegios de Active Directory.  
