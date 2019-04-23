---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Cuentas atractivas para el robo de credenciales
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e5d2b01f73127f84f700dab3518b66687f0294f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863106"
---
# <a name="attractive-accounts-for-credential-theft"></a>Cuentas atractivas para el robo de credenciales

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ataques de robo de credenciales son aquellos en los que un atacante inicialmente obtiene mayor privilegio (raíz, administrador o sistema, según el sistema operativo en uso) acceso a un equipo en una red y, a continuación, usa herramientas disponibles gratuitamente para extraer las credenciales en las sesiones de otras cuentas de inicio de sesión. Según la configuración del sistema, se pueden extraer estas credenciales en forma de hash, vales o contraseñas de texto simple incluso. Si alguna de las credenciales obtenidas para cuentas locales que es probables que existan en otros equipos de la red (por ejemplo, las cuentas de administrador en Windows o cuentas de raíz en OSX, UNIX o Linux), el atacante presenta las credenciales a otros equipos en la red para propagar el riesgo en otros equipos y para intentar obtener las credenciales de dos tipos específicos de cuentas:  

1.  Cuentas con privilegios de dominio con privilegios amplia y profundos (es decir, las cuentas que tienen privilegios de administrador en varios equipos y en Active Directory). Estas cuentas no pueden ser miembros de cualquiera de los grupos de privilegio más alto en Active Directory, pero se les es posible que haya concedido privilegios de nivel de administrador en varios servidores y estaciones de trabajo en el dominio o bosque, que hace que sea eficaz es tan eficaz como miembros de grupos con privilegios en Active Directory. En la mayoría de los casos, las cuentas que han obtenido niveles altos de privilegios a través de las franjas amplio de la infraestructura de Windows son cuentas de servicio, por lo que siempre se deben evaluar las cuentas de servicio para amplitud y profundidad de privilegios.  

2.  "Persona muy importante" cuentas de dominio (VIP). En el contexto de este documento, una cuenta de dirección VIP es cualquier cuenta que tenga acceso a la información que desea que un atacante (la propiedad intelectual y otra información confidencial), o cualquier cuenta que puede usarse para conceder acceso a esa información. Ejemplos de estas cuentas de usuario:  

    1.  Ejecutivos cuyas cuentas tienen acceso a información corporativa confidencial  

    2.  Cuentas para el personal de soporte técnico que son responsable de mantener los equipos y aplicaciones que usan los ejecutivos  

    3.  Cuentas para el personal del departamento legal que tienen acceso a documentos de puja y contrato de la organización, si los documentos son para su propia organización o las organizaciones de cliente  

    4.  Canalización planeadores de producto que tienen acceso a planes y las especificaciones de productos en el desarrollo de la empresa, independientemente de los tipos de productos que hace que la empresa  

    5.  Investigadores cuyas cuentas se usan para tener acceso a datos de estudio, formulaciones de producto o cualquier otra investigación de interés para un atacante  

Dado que las cuentas con privilegios elevados en Active Directory pueden usarse para propagar el riesgo y manipular cuentas VIP o a los datos que puede tener acceso a, las cuentas más útiles para ataques de robo de credenciales son cuentas que son miembros de administradores de empresas, Administradores de dominio y los administradores de grupos en Active Directory.  

Dado que los controladores de dominio son los repositorios de la base de datos de AD DS y los controladores de dominio tienen acceso total a todos los datos de Active Directory, los controladores de dominio también diseñados para poner en peligro, ya sea en paralelo con ataques de robo de credenciales, o después de estar en peligro una o varias cuentas de Active Directory con privilegios elevados. Aunque numerosas publicaciones (y muchos de los atacantes) se centran en los miembros del grupo Admins. del dominio al describir los ataques de robo de credenciales de pass-the-hash y otros (como se describe en [lo que reduce la superficie de ataque de Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)) , se puede usar una cuenta que sea miembro de cualquiera de los grupos enumerados aquí para poner en peligro toda la instalación de AD DS.  

> [!NOTE]  
> Para obtener información completa acerca de pass-the-hash y otros ataques de robo de credenciales, vea el [ataques Mitigating Pass-the-Hash (PTH) y otras técnicas de robo de credenciales](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) notas del producto que aparece en [Apéndice M : Vínculos a documentos y lecturas recomendadas](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Para obtener más información sobre los ataques por determinados adversarios, que se conocen a veces como "amenazas persistentes avanzadas" (apt), consulte [determina los adversarios y ataques de destino](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Actividades que aumentan la probabilidad de riesgo  
Dado que el destino de robo de credenciales es normalmente cuentas de dominio con privilegios elevados y VIP, es importante que los administradores a ser consciente de las actividades que aumentan la probabilidad de éxito de un ataque de robo de credenciales. Aunque los atacantes también tener como destino cuentas VIP, si las direcciones VIP no se proporcionan altos niveles de privilegios en los sistemas o en el dominio, el robo de sus credenciales requiere otros tipos de ataques, como la dirección VIP para proporcionar información secreta de ingeniería social. O bien, el atacante debe obtener acceso con privilegios a un sistema en qué dirección VIP se almacenan en caché las credenciales. Por este motivo, las actividades que aumentan la probabilidad de robo de credenciales que se describen aquí se centran principalmente en la prevención de la adquisición de credenciales administrativas con privilegios elevados. Estas actividades son mecanismos habituales por el que los atacantes son capaces de poner en peligro los sistemas para obtener credenciales con privilegios.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>Iniciar sesión en equipos no protegidos con las cuentas con privilegios  
La vulnerabilidad de núcleo que permite a los ataques de robo de credenciales se realice correctamente es el acto de iniciar sesión en equipos que no son seguras con las cuentas que son ampliamente y profundamente con privilegios en todo el entorno. Estos inicios de sesión pueden ser el resultado de varios errores de configuración que se describe aquí.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>No mantener credenciales administrativas separadas  
Aunque esto es relativamente raro, en la evaluación de varias instalaciones de AD DS, hemos encontrado a los empleados de TI con una única cuenta para todo su trabajo. La cuenta es miembro de al menos uno de los grupos con privilegios más elevados en Active Directory y es la misma cuenta que los empleados usan para iniciar sesión en sus estaciones de trabajo de la mañana, compruebe su correo electrónico, explorar sitios de Internet y descargar contenida para sus equipos. Cuando los usuarios se ejecuten con cuentas que se conceden permisos y derechos de administrador local, que exponen el equipo local para completar el riesgo. Cuando esas cuentas también son miembros de los grupos con más privilegios en Active Directory, que exponen todo el bosque para poner en peligro, lo que muy fácil para un atacante asumiera el control total del entorno de Active Directory y Windows.  

De forma similar, en algunos entornos, hemos encontrado que los mismos nombres de usuario y contraseñas se usan para las cuentas de raíz en los equipos que no sean Windows ya se usan en el entorno de Windows, que permite a los atacantes ampliar poner en peligro de sistemas UNIX o Linux a sistemas de Windows y viceversa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Inicios de sesión en estaciones de trabajo en peligro o los servidores miembro con las cuentas con privilegios  
Cuando se usa una cuenta de dominio con privilegios elevados para iniciar sesión interactivamente en un servidor miembro o estación de trabajo en peligro, dicho equipo en peligro puede recoger las credenciales desde cualquier cuenta que inicie sesión en el sistema.  

#### <a name="unsecured-administrative-workstations"></a>Estaciones de trabajo administrativas no seguras  
En muchas organizaciones, el personal de TI usa varias cuentas. Se utiliza una cuenta para iniciar sesión en la estación de trabajo del empleado y, dado que son el personal de TI, a menudo tienen derechos de administrador local en sus estaciones de trabajo. En algunos casos, UAC está habilitado de modo que el usuario recibe al menos un acceso de la división de izquierda token de inicio de sesión y debe elevar cuando se requieren privilegios. Cuando estos usuarios son realizar las actividades de mantenimiento, suelen usar las herramientas de administración instalado localmente y proporcione las credenciales de sus cuentas de dominio con privilegios, seleccionando la **ejecutar como administrador** opción o mediante proporcionar las credenciales cuando se le solicite. Aunque esta configuración puede resultar adecuada, expone el entorno para poner en peligro porque:  

-   La cuenta de usuario "normal" que el empleado se utiliza para iniciar sesión en su estación de trabajo tiene derechos de administrador local, el equipo es vulnerable a [descargas](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) ataques en el que el usuario está convencido para instalar el software malintencionado.  

-   El malware se instala en el contexto de una cuenta administrativa, el equipo puede usarse ahora para capturar pulsaciones de teclas, el contenido del Portapapeles, capturas de pantalla y credenciales residentes en memoria, que puede dar lugar a la exposición de las credenciales de un dominio eficaces cuenta.  

Los problemas en este escenario son doble. En primer lugar, aunque se usan cuentas independientes para la administración local y de dominio, el equipo no es seguro y no protege las cuentas contra el robo. En segundo lugar, la cuenta de usuario normal y la cuenta administrativa se han concedido los permisos y derechos excesivos.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Explorar Internet con una cuenta con privilegios elevados  
Los usuarios que inician sesión en equipos con cuentas que son miembros del grupo Administradores local en el equipo, o los miembros de grupos con privilegios en Active Directory y que, a continuación, explorar Internet (o una intranet en peligro) exponen el equipo local y el directorio que se va a poner en peligro.  

Obtener acceso a un sitio Web diseñado de manera malintencionada con un explorador que se ejecuta con privilegios administrativos puede permitir que un atacante depositar código malintencionado en el equipo local en el contexto del usuario con privilegios. Si el usuario tiene derechos de administrador local en el equipo, los atacantes pueden engañar al usuario descargar código malintencionado o abrir datos adjuntos de correo electrónico que aprovechan vulnerabilidades de aplicaciones y aprovechan los privilegios del usuario para extraer localmente en caché credenciales para todos los usuarios activos en el equipo. Si el usuario tiene derechos administrativos en el directorio por la pertenencia a los administradores de organización, Admins. del dominio o grupos de administradores en Active Directory, el atacante puede extraer las credenciales de dominio y usarlos para poner en peligro el todo el dominio AD DS o un bosque, sin necesidad de poner en peligro de cualquier otro equipo en el bosque.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configuración Local las cuentas con las mismas credenciales con privilegios en todos los sistemas  
Configurar el mismo nombre de cuenta de administrador local y la contraseña en varios o todos los equipos permite robadas credenciales de la base de datos SAM en un equipo que se usará para poner en peligro a todos los demás equipos que usan las mismas credenciales. Como mínimo, debe usar diferentes contraseñas para cuentas de administrador locales en cada sistema unido al dominio. Cuentas de administrador local también se pueden denominar forma única, pero con diferentes contraseñas para cuentas con privilegios locales de cada sistema es suficiente para asegurarse de que las credenciales no se puede usar en otros sistemas.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Overpopulation y el uso excesivo de grupos de dominio con privilegios  
Conceder la pertenencia en los grupos de EA, DA o BA en un dominio, crea un destino para los atacantes. El mayor número de miembros de estos grupos, mayor será la probabilidad de que un usuario con privilegios puede hacer un mal uso de las credenciales y los expone para el robo de credenciales accidentalmente los ataques. Cada estación de trabajo o a la que se registra un usuario de dominio con privilegios en el servidor presenta un posible mecanismo por el que las credenciales del usuario con privilegios pueden recopilándose y usa para poner en peligro el dominio de AD DS y el bosque.  

### <a name="poorly-secured-domain-controllers"></a>Controladores de dominio mal protegido  
Los controladores de dominio almacenan una réplica de base de datos de un dominio AD DS. En el caso de los controladores de dominio de solo lectura, la réplica local de la base de datos contiene las credenciales para solo un subconjunto de las cuentas en el directorio, ninguno de los cuales son cuentas de dominio con privilegios de forma predeterminada. En los controladores de dominio de lectura y escritura, cada controlador de dominio mantiene una réplica completa de la base de datos de AD DS, incluidas las credenciales no solo para los usuarios con privilegios como administradores de dominio, pero las cuentas con privilegios, como las cuentas de controlador de dominio o Krbtgt del dominio cuenta, que es la cuenta que está asociada con el servicio KDC en controladores de dominio. Si otras aplicaciones que no son necesarias para la funcionalidad del controlador de dominio están instaladas en los controladores de dominio, o si los controladores de dominio no rigurosamente se revisen y protegidos, los atacantes pueden poner en peligro ellos a través de las vulnerabilidades sin revisiones, o bien puede sacar provecho de otros tipos de ataque para instalar el software malintencionado directamente en ellos.  

## <a name="privilege-elevation-and-propagation"></a>Propagación y elevación de privilegios  
Independientemente de los métodos de ataque usados, siempre se destina Active Directory cuando un entorno de Windows es un ataque, dado que en última instancia controla el acceso a todo lo que desean que los atacantes. Esto no significa que se destina todo el directorio, sin embargo. Cuentas específicas, servidores y componentes de infraestructura suelen ser los objetivos principales de ataques contra Active Directory. A continuación se describen estas cuentas.  

### <a name="permanent-privileged-accounts"></a>Cuentas con privilegios permanentes  
Dado que la introducción de Active Directory, ha sido posible a las cuentas de uso con altos privilegiada para crear el bosque de Active Directory y, a continuación, al delegado los derechos y permisos necesarios para realizar la administración diaria a las cuentas con menos privilegios. Pertenencia a los grupos Administradores de organización, Admins. del dominio o administradores en Active Directory se necesita sólo temporalmente y con poca frecuencia en un entorno que se implementa con privilegios mínimos enfoques para la administración diaria.  

Cuentas con privilegios permanentes son cuentas que se han colocado en grupos con privilegios y haya dejado de un día para otro. Si su organización pone cinco cuentas en el grupo de administradores de dominio para un dominio, las cinco cuentas pueden ser destino de 24 horas un día, siete días a la semana. No obstante, suele ser la necesidad real de utilizar cuentas con privilegios de administrador de dominio para la configuración específica de todo el dominio y durante breves períodos de tiempo.  

### <a name="vip-accounts"></a>Cuentas de VIP  
Un destino a menudo se pasa por alto infracciones de Active Directory es las cuentas de "las personas muy importantes" (o VIP) de una organización. Las cuentas con privilegios se destinan porque esas cuentas pueden conceder acceso a los atacantes, lo que permite poner en peligro o destruir incluso sistemas de destino, como se describió anteriormente en esta sección.  

### <a name="privilege-attached-active-directory-accounts"></a>Cuentas de Active Directory "Conexión con privilegios"  
"Conexión con privilegios" cuentas de Active Directory son cuentas de dominio que no se han realizado los miembros de cualquiera de los grupos que tienen los niveles más altos de privilegios en Active Directory, pero en su lugar, se le han concedido los altos niveles de privilegio en muchos servidores y estaciones de trabajo en el entorno. Estas cuentas son mayoría a menudo basado en dominio que están configurados para ejecutar servicios en los sistemas unidos a un dominio, normalmente para aplicaciones que se ejecutan en grandes secciones de la infraestructura. Aunque estas cuentas no tienen privilegios en Active Directory, si se les otorgan con privilegios elevados en un gran número de sistemas, puede usarse para poner en peligro o destruir incluso grandes segmentos de la infraestructura, para lograr el mismo efecto que poner en peligro un cuenta de Active Directory con privilegios.  
