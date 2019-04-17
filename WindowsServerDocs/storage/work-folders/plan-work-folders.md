---
ms.assetid: a7c39656-81ee-4c2b-80ef-4d017dd11b07
title: "Planificar una implementación de Carpetas de trabajo"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 4/5/2017
description: "Cómo planificar una implementación de Carpetas de trabajo, incluyendo los requisitos del sistema y la manera de preparar el entorno de red."
ms.openlocfilehash: 877b418439e77e39cbdc6821808e296f977c26dd
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="planning-a-work-folders-deployment"></a>Planificar una implementación de Carpetas de trabajo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

En este tema se explica el proceso de diseño de una implementación de Carpetas de trabajo. Se entiende que el usuario ya cuenta con lo siguiente:  
  
-   Conocimientos básicos sobre Carpetas de trabajo (según se describe en el tema [Carpetas de trabajo](work-folders-overview.md))  
  
-   Conocimientos básicos sobre los conceptos de Servicios de dominio de Active Directory (AD DS)  
  
-   Conocimientos básicos sobre el uso compartido de archivos de Windows y tecnologías relacionadas  
  
-   Conocimientos básicos sobre el uso de certificados SSL  
  
-   Conocimientos básicos sobre cómo habilitar el acceso web a recursos internos a través de un proxy inverso web  
  
 Las siguientes secciones te servirán para diseñar la implementación de Carpetas de trabajo. La implementación de Carpetas de trabajo se detalla en el siguiente tema, [Implementar Carpetas de trabajo](deploy-work-folders.md).  
  
##  <a name="BKMK_SOFT"></a> Requisitos de software  

Carpetas de trabajo presenta los siguientes requisitos de software en cuanto a servidores de archivos e infraestructura de red:  
  
-   Un servidor que ejecute Windows Server 2012 R2 o Windows Server 2016 para hospedar la sincronización de recursos compartidos con los archivos de usuario  
  
-   Un volumen con formato de sistema de archivos NTFS para almacenar archivos de usuario.  
  
-   Para aplicar directivas de contraseña en equipos con Windows 7, debes usar las directivas de contraseña de directiva de grupo. También debes excluir los equipos con Windows 7 de las directivas de contraseña de Carpetas de trabajo (si es que los usas).

-   Un certificado de servidor para cada servidor de archivos que se alojará en Carpetas de trabajo. Estos certificados deben proceder de una entidad de certificación (CA) de confianza para los usuarios; lo más indicado es que sea una entidad de certificación pública.

-   (Opcional) Un bosque de Servicios de dominio de ActiveDirectory con extensiones de esquema en WindowsServer2012R2 para poder remitir automáticamente los equipos y los dispositivos al servidor de archivos adecuado cuando se usen varios servidores de archivos. 
  
Existen más requisitos para permitir que los usuarios se sincronicen a través de Internet:  
  
-   La capacidad de lograr que un servidor sea accesible desde Internet mediante la creación de reglas de publicación en el proxy inverso o la puerta de enlace de red de la organización.  
  
-   (Opcional) Un nombre de dominio registrado públicamente y la capacidad de crear más registros DNS públicos para el dominio.  
  
-   (Opcional) Una infraestructura de Servicios de federación de Active Directory (AD FS) cuando se use la autenticación de AD FS.  
  
Carpetas de trabajo presenta los siguientes requisitos de software relativos a los equipos cliente:  
  
-   Los equipos deben ejecutar uno de los siguientes sistemas operativos:  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows7  
  
    -   Android 4.4 KitKat y versiones posteriores  
  
    -   iOS 10.2 y versiones posteriores  
  
-   Los equipos con Windows 7 deben ejecutar una de las siguientes ediciones de Windows:  
  
    -   Windows 7 Professional  
  
    -   Windows 7 Ultimate  
  
    -   Windows 7 Enterprise  
  
-   Los equipos con Windows 7 deben estar unidos al dominio de la organización (no pueden estar unidos a un grupo de trabajo).  
  
-   Debe haber espacio libre suficiente en una unidad local con formato NTFS para almacenar todos los archivos de usuario en Carpetas de trabajo, además de otros 6 GB extra de espacio libre en caso de que Carpetas de trabajo se encuentre en la unidad del sistema (como sucede de forma predeterminada). Carpetas de trabajo usa la siguiente ubicación de forma predeterminada **%USERPROFILE%\Work Folders**  
  
     No obstante, esta ubicación se puede modificar durante la instalación (se pueden usar ubicaciones como tarjetas microSD y unidades USB con el formato del sistema de archivos NTFS, pero cabe recordar que la sincronización se detendrá si se extraen las unidades).  
  
     El tamaño máximo de los archivos individuales es de 10 GB de forma predeterminada. No existe límite de almacenamiento por usuario, si bien los administradores pueden hacer uso de la función de cuotas del Administrador de recursos del servidor de archivos para implementar cuotas.  
  
-   Carpetas de trabajo no admite revertir el estado de máquina virtual de las máquinas virtuales cliente. En su lugar, debes realizar operaciones de copia de seguridad y restauración desde la máquina virtual cliente, ya sea por medio de Copia de seguridad de imagen del sistema o de cualquier otra aplicación de copia de seguridad.  
  
> [!NOTE]
>  Asegúrate de instalar el paquete acumulativo de actualizaciones de disponibilidad general de Windows 8.1 y Windows Server 2012 R2 en todos los servidores de Carpetas de trabajo y en cualquier equipo cliente que ejecute Windows 8.1 o Windows Server 2012 R2. Para más información, consulta el artículo [2883200](http://support.microsoft.com/kb/2883200) en Microsoft Knowledge Base.  
  
## <a name="deployment-scenarios"></a>Escenarios de implementación  
 Carpetas de trabajo se puede implementar en un número indeterminado de servidores de archivos de un entorno de cliente. Gracias a ello, las implementaciones de Carpetas de trabajo pueden escalar de acuerdo a las necesidades del cliente y así ser tremendamente individualizadas. No obstante, casi todas las implementaciones coinciden con uno de los tres siguientes escenarios básicos.  
  
### <a name="single-site-deployment"></a>Implementación de sitio único  
 En una implementación de sitio único, los servidores de archivos se hospedan en un sitio central dentro de la infraestructura del cliente. Este tipo de implementación es más frecuente en clientes con una infraestructura muy centralizada o con pequeñas sucursales que no mantienen servidores de archivos locales. Se trata de un modelo de implementación más fácil de administrar para el personal de TI, ya que todos los activos de servidor son locales y la entrada/salida de Internet probablemente esté centralizada también en esa ubicación. Sin embargo, este modelo depende de una buena conectividad de WAN entre el sitio central y las sucursales, con lo cual los usuarios de las sucursales están expuestos a posibles interrupciones del servicio a causa del estado de la red.  
  
### <a name="multiple-site-deployment"></a>Implementación con varios sitios  
 En una implementación con varios sitios, los servidores de archivos se hospedan en diversas ubicaciones dentro de la infraestructura del cliente, que podrían ser varios centros de datos o varias sucursales donde se mantienen los servidores de archivos. Este tipo de implementación es el habitual en entornos de cliente de mayor tamaño o en clientes con varias sucursales grandes donde se mantienen activos de servidores locales. Este modelo de implementación es más difícil de administrar para el personal de TI y depende de una minuciosa coordinación entre el almacenamiento de datos y el mantenimiento de Servicios de dominio de Active Directory (AD DS); así se garantiza que los usuarios usan el servidor de sincronización adecuado para Carpetas de trabajo.  
  
### <a name="hosted-deployment"></a>Implementación hospedada  
 En una implementación hospedada, los servidores de sincronización están implementados en una solución IaaS (infraestructura como servicio) como, por ejemplo, VM de Windows Azure. Este método de implementación tiene la ventaja de que la disponibilidad de los servidores de archivos depende en menor medida de la conectividad de WAN dentro de la empresa de un cliente. Si un dispositivo puede conectarse a Internet, podrá tener acceso a su servidor de sincronización. Sin embargo, los servidores implementados en el entorno hospedado deben seguir teniendo acceso al dominio de Active Directory de la organización para autenticar usuarios, por lo que el cliente sacrifica requisitos locales debido a la dificultad extra que supone mantener esa conexión.  
  
## <a name="deployment-technologies"></a>Tecnologías de implementación  
 Las implementaciones de Carpetas de trabajo constan de una serie de tecnologías que funcionan de manera conjunta para prestar servicio a dispositivos en redes tanto internas como externas. Antes de diseñar una implementación de Carpetas de trabajo, los clientes deben estar familiarizados con los requisitos de cada una de las siguientes tecnologías.  
  
### <a name="active-directory-domain-services"></a>Servicios de dominio de Active Directory  
 AD DS proporciona dos servicios importantes en una implementación de Carpetas de trabajo. En primer lugar, igual que sucede con el back-end de la autenticación de Windows, AD DS presta los servicios de autenticación y seguridad que se usan para dar acceso a los datos de usuario. Si no se puede obtener acceso a un controlador de dominio, un servidor de archivos no podrá autenticar una solicitud entrante y el dispositivo no podrá tener acceso a ninguno de los datos almacenados en el recurso compartido de sincronización de dicho servidor.  
  
 En segundo lugar, AD DS (con la actualización de esquema de Windows Server 2012 R2) mantiene en cada usuario el atributo msDS-SyncServerURL, que sirve para dirigir automáticamente a los usuarios al servidor de sincronización correspondiente.  
  
### <a name="file-servers"></a>Servidores de archivos  
 Los servidores de archivos que ejecutan Windows Server 2012 R2 o Windows Server 2016 hospedan tanto el servicio de rol de Carpetas de trabajo, como los recursos compartidos de sincronización donde se almacenan los datos de Carpetas de trabajo de los usuarios. Los servidores de archivos también pueden hospedar los datos almacenados por otras tecnologías activas en la red interna (como los recursos compartidos de archivos) y, asimismo, se pueden agrupar para disponer de tolerancia a errores en los datos de usuario.  
  
###  <a name="GroupPolicy"></a> Directiva de grupo  
 Si tienes equipos con Windows 7 en el entorno, se recomienda lo siguiente:  
  
-   Usa la directiva de grupo para controlar las directivas de contraseña de todos los equipos unidos al dominio que usen Carpetas de trabajo.  
  
-   Usa la directiva **Bloquear pantalla automáticamente y requerir contraseña** de Carpetas de trabajo en los equipos que no estén unidos al dominio.  
  
 La directiva de grupo también se puede usar para especificar un servidor de Carpetas de trabajo en equipos unidos a dominios. Esto simplifica en cierto modo la configuración de Carpetas de trabajo; de otro modo, los usuarios tendrían que indicar su dirección de correo electrónico del trabajo para buscar la configuración (siempre y cuando Carpetas de trabajo se haya configurado correctamente), o bien indicar la dirección URL de Carpetas de trabajo que hayan recibido explícitamente por correo electrónico o por cualquier otro medio de comunicación.  
  
 La directiva de grupo también puede servir para configurar Carpetas de trabajo forzosamente de manera individual (por cada usuario o equipo), pese a que esto hace que Carpetas de trabajo se sincronice en todos los equipos en los que un usuario inicie sesión (si se usa la configuración de directiva por usuario) e impide que los usuarios establezcan otra ubicación en su PC para Carpetas de trabajo (como una tarjeta microSD, a fin de ahorrar espacio en la unidad principal). Aconsejamos evaluar detenidamente las necesidades de los usuarios antes de implantar la configuración automática.  
  
### <a name="windows-intune"></a>Windows Intune  
 Windows Intune aporta un nivel de seguridad y facilidad de administración para aquellos dispositivos no unidos a dominios que de otra forma no estarían presentes. Windows Intune se puede usar para configurar y administrar dispositivos personales de los usuarios como, por ejemplo, tabletas que se conectan a Carpetas de trabajo desde Internet. Asimismo, Windows Intune proporciona a los dispositivos la dirección URL del servidor de sincronización que deben usar; de no ser así, los usuarios tendrían que indicar su dirección de correo electrónico del trabajo para buscar la configuración (si la dirección URL de Carpetas de trabajo se publica con el formato https://workfolders.*contoso.com*), o bien escribir directamente la dirección URL del servidor de sincronización.  
  
 Sin una implementación de Windows Intune, los usuarios deben configurar los dispositivos externos manualmente, lo que puede traducirse en una mayor demanda del personal del servicio de asistencia.  
  
 Windows Intune también puede servir para borrar de forma selectiva los datos de Carpetas de trabajo del dispositivo de un usuario sin que ello afecte al resto de datos, algo que resulta especialmente útil si alguien deja la empresa o si se sustrae un dispositivo.  
  
### <a name="web-application-proxyazure-ad-application-proxy"></a>Proxy de aplicación web/Proxy de aplicación de Azure AD  
 Carpetas de trabajo surge de la idea de permitir que los dispositivos conectados a Internet obtengan datos empresariales de la red interna sin ningún tipo de riesgo. Gracias a esto, los usuarios pueden "llevarse los datos allá donde vayan" en sus tabletas y dispositivos, en los que normalmente no podrían tener acceso a los archivos de trabajo. Para lograrlo, se debe usar un proxy inverso para publicar direcciones URL de servidor de sincronización y ponerlas a disposición de los clientes de Internet. 
 
La opción Carpetas de trabajo admite el uso del Proxy de aplicación web, el Proxy de aplicación de Azure AD o soluciones de proxy inverso de otros fabricantes: 

-  El Proxy de aplicación web es una solución de proxy inverso local. Para obtener más información, consulta [Proxy de aplicación web en WindowsServer2016](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server).  
  
-  El Proxy de aplicación de Azure AD es una solución de proxy inverso en la nube. Para obtener más información, consulta [Cómo ofrecer acceso remoto seguro a aplicaciones locales](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-application-proxy-get-started)

## <a name="additional-design-considerations"></a>Otras consideraciones de diseño  
 Aparte de conocer cada uno de los componentes anteriores, los clientes deben dedicar tiempo a pensar en el número de servidores y recursos de sincronización que debe haber y, asimismo, si conviene usar clústeres de conmutación por error para que exista tolerancia a errores en estos servidores de sincronización.  
  
### <a name="number-of-sync-servers"></a>Número de servidores de sincronización  
 Un cliente puede poner en marcha varios servidores de sincronización en un solo entorno. Esta configuración puede ser realmente adecuada por diversos motivos:  
  
-   Dispersión geográfica de los usuarios: por ejemplo, servidores de archivos de sucursales o centros de datos regionales.  
  
-   Requisitos de almacenamiento de datos: puede que algunos departamentos empresariales presenten requisitos de tratamiento o almacenamiento de los datos que sean más fáciles de cubrir con un servidor dedicado.  
  
-   Equilibrio de carga: en entornos de gran tamaño, el rendimiento y el tiempo de actividad de los servidores pueden aumentar si los datos se almacenan en varios servidores.  
  
 Para obtener información sobre el rendimiento y la escala de los servidores de Carpetas de trabajo, consulta [Performance Considerations for Work Folders Deployments (Consideraciones de rendimiento de las implementaciones de Carpetas de trabajo)](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx).  
  
> [!NOTE]
>  Cuando se usen varios servidores de sincronización, recomendamos configurar la detección automática de servidores de los usuarios. Este proceso se basa en la configuración de un atributo de cada cuenta de usuario en AD DS. Este atributo es **msDS-SyncServerURL** y está disponible en las cuentas de usuario después de que un controlador de dominio de Windows Server 2012 R2 se haya agregado al dominio o se hayan aplicado actualizaciones de esquema de Active Directory. Este atributo debe definirse en cada usuario si se quiere garantizar que los usuarios se conecten al servidor de sincronización adecuado. Si usan la detección automática de servidores, las organizaciones pueden publicar Carpetas de trabajo en una dirección URL descriptiva como, por ejemplo, *https://workfolders.contoso.com*, independientemente del número de servidores de sincronización existente.  
  
### <a name="number-of-sync-shares"></a>Número de recursos compartidos de sincronización  
 Los servidores de sincronización individuales pueden mantener varios recursos compartidos de sincronización, lo que puede ser práctico por los motivos siguientes:  
  
-   Requisitos de auditoría y seguridad: si los datos que un determinado departamento utiliza se deben auditar o retener de manera más controlada y prolongada, tener recursos compartidos de sincronización independientes ayuda a los administradores a mantener por separado las carpetas de usuario con distintos niveles de auditoría.  
  
-   Filtros de archivos o cuotas diferentes: disponer de recursos compartidos por separado puede ser de ayuda si quieres establecer distintos límites o cuotas de almacenamiento para permitir ciertos tipos de archivo en Carpetas de trabajo (filtros de archivos) de diversos grupos o usuarios.  
  
-   Control por departamentos: si las obligaciones administrativas están repartidas por departamentos, el uso de recursos compartidos separados en cada departamento ayuda a los administradores a imponer cuotas y otras directivas.  
  
-   Directivas de dispositivo divergentes: el uso de múltiples recursos compartidos hace posible que una organización pueda mantener varias directivas de dispositivo (como el cifrado de Carpetas de trabajo) para distintos grupos o usuarios.  
  
-   Capacidad de almacenamiento: si un servidor de archivos tiene varios volúmenes, se pueden usar recursos compartidos extra con los que sacar partido a esos volúmenes adicionales. Un recurso compartido concreto tiene acceso únicamente al volumen en el que está hospedado y no podrá usar ningún otro volumen de un servidor de archivos.  
  
#### <a name="access-to-sync-shares"></a>Acceso a recursos compartidos de sincronización  

Del mismo modo que el servidor de sincronización al que un usuario tiene acceso viene determinado por la dirección URL especificada en el cliente (o la dirección URL publicada para dicho usuario en AD DS si se usa la detección automática de servidores), el acceso a recursos compartidos de sincronización individuales viene determinado por los permisos existentes en el recurso compartido en cuestión.  
  
En consecuencia, si un cliente hospeda varios recursos compartidos de sincronización en el mismo servidor, deberá tener cuidado y asegurarse de que cada usuario tenga permiso de acceso a únicamente uno de esos recursos compartidos. En caso contrario, cuando los usuarios se conecten al servidor, puede existir la posibilidad de que sus clientes se conecten al recurso compartido equivocado. Para evitarlo, puedes crear un grupo de seguridad independiente para cada recurso compartido de sincronización.  
  
Además, el acceso a la carpeta de un usuario concreto dentro de un recurso compartido de sincronización viene determinado por los derechos de propiedad respecto a la carpeta. Al crear un recurso compartido de sincronización, Carpetas de trabajo concede a los usuarios acceso exclusivo a sus archivos de forma predeterminada (la herencia se deshabilita y tales usuarios pasan a ser los propietarios de sus carpetas individuales).  
  
## <a name="design-checklist"></a>Lista de comprobación de diseño  

La siguiente relación de preguntas está pensada para ayudar a los clientes a diseñar la mejor implementación de Carpetas de trabajo en sus entornos. Conviene que los clientes se detengan a analizar esta lista de comprobación antes de intentar implementar servidores.  
  
-   Usuarios objetivo  
  
    -   ¿Qué usuarios van a usar Carpetas de trabajo?  
  
    -   ¿Cómo están organizados los usuarios? (Geográficamente, por oficina, por departamento, etc.)  
  
    -   ¿Hay usuarios que tengan requisitos especiales de almacenamiento, seguridad o retención de datos?  
  
    -   ¿Hay usuarios que tengan requisitos específicos de directiva de dispositivo, como el cifrado?  
  
    -   ¿Para qué equipos y dispositivos cliente necesitas compatibilidad? (Windows 8.1, Windows RT 8.1, Windows 7)  
  
         Si tienes efectúas el soporte técnico de equipos con Windows 7 y quieres usar directivas de contraseña, excluye el dominio que almacena las cuentas de equipo de la directiva de contraseña de Carpetas de trabajo y, en su lugar, usa las directivas de contraseña de la directiva de grupo para los equipos unidos a ese dominio.  
  
    -   ¿Necesitas migrar datos desde otras soluciones de administración de datos de usuario (como Redirección de carpetas) o interoperar con ellas?  
  
    -   ¿Es necesario que los usuarios de varios dominios puedan sincronizarse por Internet con un único servidor?  
  
    -   ¿Es necesario admitir a aquellos usuarios que no sean miembros del grupo Administradores local en los equipos unidos al dominio? (Si es así, deberás excluir los dominios en cuestión de las directivas de dispositivo de Carpetas de trabajo, como las directivas de contraseña y cifrado).  
  
-   Planificación de la infraestructura y la capacidad  
  
    -   ¿En qué sitios de la red deben estar los servidores de sincronización?  
  
    -   ¿Alguno de los servidores de sincronización va a estar hospedado por un proveedor de IaaS (infraestructura como servicio), como una VM de Azure?  
  
    -   ¿Vas a necesitar servidores dedicados a grupos de usuarios específicos? De ser así, ¿cuántos usuarios por cada servidor dedicado?  
  
    -   ¿Dónde están los puntos de entrada/salida de Internet en la red?  
  
    -   ¿Se van a agrupar los servidores de sincronización para la tolerancia a errores?  
  
    -   ¿Deben los servidores de sincronización mantener varios volúmenes de datos para hospedar datos de usuario?  
  
-   Seguridad de los datos  
  
    -   ¿Necesitas crear varios recursos compartidos de sincronización en cualquiera de los servidores de sincronización?  
  
    -   ¿Qué grupos se van a usar para dar acceso a los recursos compartidos de sincronización?  
  
    -   Si usas varios servidores de sincronización, ¿qué grupo de seguridad vas a crear para la capacidad delegada de modificar la propiedad msDS-SyncServerURL de los objetos de usuario?  
  
    -   ¿Existen requisitos especiales de auditoría o seguridad en algún recurso compartido de sincronización concreto?  
  
    -   ¿Vas a necesitar la autenticación multifactor (MFA)?  
  
    -   ¿Necesitas poder borrar de forma remota los datos de Carpetas de trabajo de equipos y dispositivos?  
  
-   Acceso del dispositivo  
  
    -   ¿Qué dirección URL se va a usar para dar acceso a dispositivos basados en Internet (*la dirección URL predeterminada que se necesita para la detección automática de servidores y que está basada en el correo electrónico es workfolders.domainname*)?  
  
    -   ¿De qué manera se va a publicar la dirección URL en Internet?  
  
    -   ¿Vas a usar la detección automática de servidores?  
  
    -   ¿Vas a usar la directiva de grupo para configurar equipos unidos al dominio?  
  
    -   ¿Vas a usar Windows Intune para configurar dispositivos externos?  
  
    -   ¿Será necesario el registro de dispositivos para que los dispositivos puedan conectarse?  
  
## <a name="next-steps"></a>Pasos siguientes  
 Tras diseñar la implementación de Carpetas de trabajo, será el momento de ponerla en marcha. Para obtener más información, consulta [Implementar Carpetas de trabajo](deploy-work-folders.md).  
  
## <a name="see-also"></a>Consulta también  
 Para obtener más información relacionada, consulta los siguientes recursos.  
  
|Tipo de contenido|Referencias|  
|------------------|----------------|  
|**Evaluación del producto**|-   [Carpetas de trabajo](work-folders-overview.md)<br />-   [Carpetas de trabajo para Windows7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (entrada de blog)|  
|**Implementación**|-   [Diseñar una implementación de carpetas de trabajo](plan-work-folders.md)<br />-   [Implementar Carpetas de trabajo](deploy-work-folders.md)<br />-   [Implementación de Carpetas de trabajo con ADFS y Proxy de aplicación web (WAP)](deploy-work-folders-adfs-overview.md)<br />- [Implementación de Carpetas de trabajo con Proxy de aplicación de Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />-   [Consideraciones de rendimiento en las implementaciones de Carpetas de trabajo](http://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [Carpetas de trabajo para Windows 7 (descarga de 64 bits)](http://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Carpetas de trabajo para Windows 7 (descarga de 32 bits)](http://www.microsoft.com/download/details.aspx?id=42559)<br />-   [Implementación de prueba de Carpetas de trabajo](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (entrada de blog)|
