---
ms.assetid: 34244b53-1206-4f5b-8c4d-3ebf574d8e24
title: Cuentas atractivas para el robo de credenciales
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2d6fdeaef6e8c90d5e723bdd3ee334369ef73c68
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367745"
---
# <a name="attractive-accounts-for-credential-theft"></a>Cuentas atractivas para el robo de credenciales

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los ataques de robo de credenciales son aquellos en los que un atacante obtiene inicialmente un privilegio más alto (raíz, administrador o sistema, según el sistema operativo en uso), el acceso a un equipo de una red y, a continuación, usa las herramientas disponibles libremente para extraer las credenciales desde las sesiones de otras cuentas que han iniciado sesión. Dependiendo de la configuración del sistema, estas credenciales se pueden extraer en forma de hashes, vales o incluso contraseñas de texto no cifrado. Si alguna de las credenciales cosechadas es para cuentas locales que probablemente existan en otros equipos de la red (por ejemplo, cuentas de administrador en Windows o cuentas raíz en OSX, UNIX o Linux), el atacante presenta las credenciales a otros equipos en la red para propagar el riesgo a equipos adicionales e intentar obtener las credenciales de dos tipos específicos de cuentas:  

1.  Cuentas de dominio con privilegios con privilegios amplios y profundos (es decir, cuentas que tienen privilegios de nivel de administrador en muchos equipos y en Active Directory). Es posible que estas cuentas no sean miembros de ninguno de los grupos de privilegios más elevados en Active Directory, pero se les puede haber concedido privilegios de nivel de administrador en muchos servidores y estaciones de trabajo del dominio o bosque, lo que hace que sean eficazmente tan eficaces como miembros de grupos con privilegios en Active Directory. En la mayoría de los casos, las cuentas a las que se les han concedido altos niveles de privilegio en una amplia swaths de la infraestructura de Windows son cuentas de servicio, por lo que las cuentas de servicio siempre deben evaluarse para el alcance y la profundidad de privilegios.  

2.  Cuentas de dominio de "persona muy importante" (VIP). En el contexto de este documento, una cuenta VIP es cualquier cuenta que tenga acceso a la información que un atacante desea (propiedad intelectual y otra información confidencial) o cualquier cuenta que se pueda usar para conceder al atacante acceso a esa información. Algunos ejemplos de estas cuentas de usuario son:  

    1.  Ejecutivos cuyas cuentas tienen acceso a información confidencial corporativa  

    2.  Cuentas para el personal del Departamento de soporte técnico responsable del mantenimiento de los equipos y las aplicaciones que usan los ejecutivos  

    3.  Cuentas para el personal legal que tiene acceso a los documentos de licitación y contrato de una organización, independientemente de si los documentos son para su propia organización o organizaciones de cliente.  

    4.  Planificadores de productos que tienen acceso a planes y especificaciones para los productos de la canalización de desarrollo de una empresa, independientemente de los tipos de productos que realiza la empresa.  

    5.  Investigadores cuyas cuentas se usan para obtener acceso a datos de estudios, formulaciones de productos o cualquier otra investigación de interés para un atacante  

Dado que las cuentas con privilegios elevados en Active Directory se pueden usar para propagar el riesgo y manipular cuentas VIP o los datos a los que pueden acceder, las cuentas más útiles para los ataques de robo de credenciales son las cuentas que son miembros de los administradores de empresas. Grupos Admins. del dominio y administradores en Active Directory.  

Dado que los controladores de dominio son los repositorios para la base de datos de AD DS y los controladores de dominio tienen acceso total a todos los datos de Active Directory, los controladores de dominio también se destinan al riesgo, ya sea en paralelo con ataques de robo de credenciales o después una o varias cuentas de Active Directory con privilegios elevados se han puesto en peligro. Aunque numerosas publicaciones (y muchos atacantes) se centran en la pertenencia al grupo Admins. del dominio al describir Pass-The-hash y otros ataques de robo de credenciales (como se describe en [reducir la superficie expuesta a ataques Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)), se puede usar una cuenta que sea miembro de alguno de los grupos enumerados aquí para poner en peligro toda la instalación de AD DS.  

> [!NOTE]  
> Para obtener información completa sobre Pass-The-hash y otros ataques de robo de credenciales, consulte las notas del producto [mitigar los ataques Pass-The-hash (PTH) y otras técnicas de robo de credenciales](https://download.microsoft.com/download/7/7/A/77ABC5BD-8320-41AF-863C-6ECFB10CB4B9/Mitigating%20Pass-the-Hash%20(PtH)%20Attacks%20and%20Other%20Credential%20Theft%20Techniques_English.pdf) que aparecen en el [Apéndice M: vínculos de documentos y lecturas recomendadas](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md). Para obtener más información acerca de los ataques mediante la determinación de los adversarios, a la que a veces se hace referencia como "amenazas persistentes avanzadas" (APT), consulte el [los adversarios determinado y los ataques dirigidos](https://www.microsoft.com/download/details.aspx?id=34793).  

## <a name="activities-that-increase-the-likelihood-of-compromise"></a>Actividades que aumentan la probabilidad de peligro  
Dado que el objetivo del robo de credenciales suele ser cuentas de dominio con privilegios elevados y cuentas VIP, es importante que los administradores sean conscientes de las actividades que aumentan la probabilidad de éxito de un ataque de robo de credenciales. Aunque los atacantes también tienen como destino cuentas VIP, si no se les proporcionan altos niveles de privilegios en los sistemas o en el dominio, el robo de sus credenciales requiere otros tipos de ataques, como la ingeniería social de la VIP para proporcionar información secreta. O el atacante primero debe obtener acceso con privilegios a un sistema en el que las credenciales de VIP estén almacenadas en caché. Por este motivo, las actividades que aumentan la probabilidad de robo de credenciales que se describen aquí se centran principalmente en evitar la adquisición de credenciales administrativas con privilegios elevados. Estas actividades son mecanismos comunes por los que los atacantes pueden poner en peligro los sistemas para obtener credenciales con privilegios.  

### <a name="logging-on-to-unsecured-computers-with-privileged-accounts"></a>Inicio de sesión en equipos no seguros con cuentas con privilegios  
La vulnerabilidad principal que permite que los ataques de robo de credenciales se realicen correctamente es el acto de iniciar sesión en equipos que no son seguros con cuentas que tienen privilegios generales y amplios en todo el entorno. Estos inicios de sesión pueden ser el resultado de varias configuraciones indistintas que se describen aquí.  

#### <a name="not-maintaining-separate-administrative-credentials"></a>No mantener credenciales administrativas independientes  
Aunque esto es relativamente poco habitual, en la evaluación de varias instalaciones AD DS, hemos encontrado empleados de ti que usan una sola cuenta para todo su trabajo. La cuenta es miembro de al menos uno de los grupos con más privilegios en Active Directory y es la misma cuenta que usan los empleados para iniciar sesión en sus estaciones de trabajo de la mañana, consultar su correo electrónico, explorar sitios de Internet y descargar contenido en su los. Cuando los usuarios se ejecutan con cuentas a las que se les conceden derechos y permisos de administrador local, exponen el equipo local para poner en peligro. Cuando esas cuentas también son miembros de los grupos con más privilegios en Active Directory, exponen todo el bosque a peligro, lo que facilita a un atacante tener un control total sobre el Active Directory y el entorno de Windows.  

Del mismo modo, en algunos entornos, hemos descubierto que se usan los mismos nombres de usuario y contraseñas para las cuentas raíz en equipos que no son de Windows y que se usan en el entorno de Windows, lo que permite a los atacantes ampliar el riesgo de sistemas UNIX o Linux a sistemas Windows. y viceversa.

#### <a name="logons-to-compromised-workstations-or-member-servers-with-privileged-accounts"></a>Inicios de sesión en estaciones de trabajo en peligro o servidores miembro con cuentas con privilegios  
Cuando se usa una cuenta de dominio con privilegios elevados para iniciar sesión de forma interactiva en un servidor miembro o estación de trabajo en peligro, ese equipo en peligro puede recopilar credenciales de cualquier cuenta que inicie sesión en el sistema.  

#### <a name="unsecured-administrative-workstations"></a>Estaciones de trabajo administrativas no seguras  
En muchas organizaciones, el personal de ti usa varias cuentas. Una cuenta se usa para iniciar sesión en la estación de trabajo del empleado y, como es el personal de ti, a menudo tienen derechos de administrador local en sus estaciones de trabajo. En algunos casos, UAC se deja habilitado para que el usuario reciba al menos un token de acceso dividido en el inicio de sesión y debe elevarse cuando se requieran privilegios. Cuando estos usuarios están realizando actividades de mantenimiento, normalmente usan las herramientas de administración instaladas localmente y proporcionan las credenciales para sus cuentas con privilegios de dominio, seleccionando la opción **Ejecutar como administrador** o proporcionando las credenciales cuando se le solicite. Aunque es posible que esta configuración parezca adecuada, expone el entorno en peligro porque:  

-   La cuenta de usuario "normal" que usa el empleado para iniciar sesión en su estación de trabajo tiene derechos de administrador local, el equipo es vulnerable a los ataques [por descarga](https://www.microsoft.com/security/sir/glossary/drive-by-download-sites.aspx) en los que el usuario se convenció de instalar malware.  

-   El malware se instala en el contexto de una cuenta administrativa, el equipo se puede usar ahora para capturar pulsaciones de teclas, contenido del portapapeles, capturas de pantallas y credenciales residentes en memoria, cualquiera de las cuales puede provocar la exposición de las credenciales de un dominio eficaz. Código.  

Los problemas de este escenario son dobles. En primer lugar, aunque se usan cuentas independientes para la administración local y de dominio, el equipo no está protegido y no protege las cuentas frente al robo. En segundo lugar, la cuenta de usuario normal y la cuenta administrativa tienen derechos y permisos excesivos.  

### <a name="browsing-the-internet-with-a-highly-privileged-account"></a>Explorar Internet con una cuenta con privilegios elevados  
Los usuarios que inician sesión en equipos con cuentas que son miembros del grupo local de administradores en el equipo o miembros de grupos con privilegios en Active Directory y que, a continuación, examinan Internet (o una intranet en peligro) exponen el equipo local y el directorio en peligro.  

El acceso a un sitio web diseñado de forma malintencionada con un explorador que se ejecuta con privilegios administrativos puede permitir que un atacante pueda depositar código malintencionado en el equipo local en el contexto del usuario con privilegios. Si el usuario tiene derechos de administrador local en el equipo, los atacantes pueden engañar al usuario para que descargue código malintencionado o abra datos adjuntos de correo electrónico que aprovechen las vulnerabilidades de la aplicación y aprovechen los privilegios del usuario para extraer almacenamiento en caché local. credenciales para todos los usuarios activos del equipo. Si el usuario tiene derechos administrativos en el directorio por la pertenencia a los grupos administradores de empresas, administradores de dominio o administradores de Active Directory, el atacante puede extraer las credenciales de dominio y usarlas para poner en peligro todo el dominio de AD DS o bosque, sin necesidad de poner en peligro ningún otro equipo del bosque.  

### <a name="configuring-local-privileged-accounts-with-the-same-credentials-across-systems"></a>Configuración de cuentas con privilegios locales con las mismas credenciales en varios sistemas  
La configuración del mismo nombre y contraseña de la cuenta de administrador local en muchos o en todos los equipos permite usar las credenciales robadas de la base de datos SAM en un equipo para poner en peligro todos los demás equipos que usan las mismas credenciales. Como mínimo, debe usar contraseñas diferentes para las cuentas de administrador local en cada sistema unido a un dominio. Las cuentas de administrador local también pueden tener un nombre único, pero el uso de contraseñas diferentes para las cuentas locales con privilegios de cada sistema es suficiente para garantizar que las credenciales no se puedan usar en otros sistemas.  

### <a name="overpopulation-and-overuse-of-privileged-domain-groups"></a>Sobrellenado y uso excesivo de grupos de dominio con privilegios  
La concesión de la pertenencia a los grupos EA, DA o BA en un dominio crea un destino para los atacantes. Cuanto mayor sea el número de miembros de estos grupos, mayor será la probabilidad de que un usuario con privilegios pueda usar involuntariamente las credenciales y exponerlas en ataques de robo de credenciales. Cada estación de trabajo o servidor en el que un usuario de dominio con privilegios inicia sesión presenta un posible mecanismo por el que las credenciales del usuario con privilegios se pueden recopilar y usar para poner en peligro el dominio y el bosque de AD DS.  

### <a name="poorly-secured-domain-controllers"></a>Controladores de dominio mal protegidos  
Los controladores de dominio alojan una réplica de la base de datos de AD DS de un dominio. En el caso de los controladores de dominio de solo lectura, la réplica local de la base de datos contiene las credenciales solo para un subconjunto de las cuentas del directorio, ninguna de las cuales son cuentas de dominio con privilegios de forma predeterminada. En los controladores de dominio de lectura y escritura, cada controlador de dominio mantiene una réplica completa de la AD DS base de datos, incluidas las credenciales no solo para los usuarios con privilegios, como los administradores de dominio, pero las cuentas con privilegios, como las cuentas de controlador de dominio o la krbtgt de dominio. cuenta, que es la cuenta que está asociada con el servicio KDC en los controladores de dominio. Si las aplicaciones adicionales que no son necesarias para la funcionalidad del controlador de dominio se instalan en controladores de dominio, o si los controladores de dominio no se aplican y protegen de forma estricta, los atacantes pueden ponerlos en peligro a través de vulnerabilidades sin revisión o puede aprovechar otros vectores de ataque para instalar software malintencionado directamente en ellos.  

## <a name="privilege-elevation-and-propagation"></a>Elevación y propagación de privilegios  
Independientemente de los métodos de ataque utilizados, Active Directory siempre se destina cuando un entorno de Windows está atacado, ya que, en última instancia, controla el acceso a cualquier atacante que desee. Sin embargo, esto no significa que todo el directorio sea de destino. Las cuentas, los servidores y los componentes de infraestructura específicos suelen ser los principales objetivos de ataques contra Active Directory. Estas cuentas se describen como se indica a continuación.  

### <a name="permanent-privileged-accounts"></a>Cuentas con privilegios permanentes  
Dado que la introducción de Active Directory, se han podido usar cuentas con privilegios elevados para compilar el Active Directory bosque y, a continuación, delegar los derechos y permisos necesarios para realizar la administración diaria a cuentas con menos privilegios. La pertenencia a los grupos administradores de organización, Admins. del dominio o administradores de Active Directory solo se requiere temporalmente y con poca frecuencia en un entorno que implementa enfoques con privilegios mínimos en la administración diaria.  

Las cuentas con privilegios permanentes son cuentas que se han colocado en grupos con privilegios y quedan desde el día hasta el día. Si su organización coloca cinco cuentas en el grupo Admins. del dominio para un dominio, esas cinco cuentas se pueden destinar 24 horas al día, siete días a la semana. Sin embargo, la necesidad real de usar cuentas con privilegios de administrador de dominio suele ser solo para una configuración específica del dominio y durante breves períodos de tiempo.  

### <a name="vip-accounts"></a>Cuentas de VIP  
Un objetivo que a menudo se pasa por alto en infracciones de Active Directory son las cuentas de "personas muy importantes" (o VIP) de una organización. Las cuentas con privilegios están destinadas porque esas cuentas pueden conceder acceso a los atacantes, lo que les permite poner en peligro o incluso destruir sistemas de destino, como se describió anteriormente en esta sección.  

### <a name="privilege-attached-active-directory-accounts"></a>Cuentas de Active Directory con privilegios adjuntos  
Las cuentas de Active Directory adjuntas con privilegios son cuentas de dominio que no se han convertido en miembros de ninguno de los grupos que tienen los niveles más altos de privilegios en Active Directory, pero que se les han concedido altos niveles de privilegios en muchos servidores. estaciones de trabajo en el entorno. Estas cuentas suelen ser cuentas basadas en dominio que están configuradas para ejecutar servicios en sistemas Unidos a un dominio, normalmente para las aplicaciones que se ejecutan en grandes secciones de la infraestructura. Aunque estas cuentas no tienen privilegios en Active Directory, si se les conceden privilegios elevados en un gran número de sistemas, se pueden usar para poner en peligro o incluso destruir segmentos grandes de la infraestructura, con lo que se consigue el mismo efecto que el riesgo de un cuenta de Active Directory privilegiada.  
