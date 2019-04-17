---
ms.assetid: bd64a766-5362-4f29-b963-5465c2bb79e7
title: "Planeación de asignación de rol de maestro de operaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d271ed3a54e78106aed0d4dbfdb610cc8b9a460
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="planning-operations-master-role-placement"></a>Planeación de asignación de rol de maestro de operaciones

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los servicios de dominio de Active Directory (AD DS) admite la replicación de datos de directorio, lo que significa que cualquier controlador de dominio puede aceptar los cambios de directorio y replicar los cambios en todos los demás controladores de dominio. Sin embargo, algunos cambios, como las modificaciones de esquema, resultan poco prácticos para llevar a cabo de forma multifuncional. Por este motivo determinados controladores de dominio, conocidos como maestros de operaciones, mantén funciones responsable de aceptar solicitudes de determinados cambios específicos.  
  
> [!NOTE]  
> Los titulares de rol de maestro de operaciones deben ser capaces de escribir información en la base de datos de Active Directory. Dada la naturaleza de solo lectura de la base de datos de Active Directory en un controlador de dominio de solo lectura (RODC), RODC no pueden actuar como titulares de rol de maestro de operaciones.  
  
Roles de maestro de operaciones de tres (operaciones de maestro único también conocido como flexibles o FSMO) existen en cada dominio:  
  
-   El maestro de operaciones de emulador de dominio principal (PDC) del controlador procesa todas las actualizaciones de contraseña.  
  
-   El maestro de operaciones de identificador (RID) relativo mantiene el conjunto de RID global para el dominio y asigna los grupos de RID locales en todos los controladores de dominio para garantizar que todas las entidades de seguridad que se crean en el dominio tienen un identificador único.  
  
-   El maestro de operaciones de infraestructura de un dominio dado mantiene una lista de las entidades de seguridad de otros dominios que son miembros del grupo dentro de su dominio.  
  
Además de las funciones de maestro de operaciones de nivel de dominio tres roles de maestro de operaciones de dos existen en cada bosque:  
  
-   El maestro de operaciones de esquema rige cambios en el esquema.  
  
-   El maestro de operaciones de nombres de dominio agrega y quita otras particiones de directorio (por ejemplo, particiones de sistema de nombres de dominio (DNS) aplicaciones) y dominios del bosque.  
  
Colocar los controladores de dominio que hospedar estos roles de maestro de operaciones en áreas donde es alta confiabilidad de la red y asegúrate de que el emulador PDC y el maestro RID están disponibles de forma coherente.  
  
Los titulares de rol de maestro de operaciones se asignan automáticamente cuando se crea el primer controlador de dominio de un dominio. Las dos funciones de nivel de bosque (maestro de esquema y maestro nombres de dominio) se asignan al primer controlador de dominio creado en un bosque. Además, se asignan las tres funciones de nivel de dominio (maestro RID, maestro de infraestructura y emulador PDC) para el primer controlador de dominio creado en un dominio.  
  
> [!NOTE]  
> Asignaciones de titular del rol de maestro de operaciones automática se realizan solo cuando se crea un nuevo dominio y cuando se degrada un titular de la función actual. Todos los demás cambios a los propietarios de función deben ser iniciadas por un administrador.  
  
Estas asignaciones de función de maestro de operaciones automáticas para hacer uso de CPU muy alto en el primer controlador de dominio creado en el dominio o bosque. Para evitar esto, asigna las operaciones de (transferencia) roles de maestro a varios controladores de dominio en el dominio o bosque. Coloca los controladores de dominio que funciones en las áreas donde la red confiable del maestro de operaciones de host y donde todos los demás controladores de dominio del bosque pueden tener acceso a los maestros de operaciones.  
  
También debe designar las operaciones del modo de espera (alternativo) roles de maestro de patrones para todas las operaciones. Los maestros de operaciones en espera son los controladores de dominio al que se transfiere los roles de maestro de operaciones en caso de los titulares de la función original producirá un error. Asegúrate de que los maestros de operaciones en espera sean duplicadores directa de los maestros de operaciones real.  
  
## <a name="planning-the-pdc-emulator-placement"></a>Planeamiento de la ubicación de emulador PDC  
El emulador PDC procesa los cambios de contraseña de cliente. Solo un controlador de dominio actúa como el emulador PDC en cada dominio del bosque.  
  
Aunque todos los controladores de dominio se actualizan a Windows 2000, Windows Server 2003 y Windows Server 2008 y el dominio está funcionando en el nivel funcional nativo de Windows 2000, el emulador PDC recibe replicación preferente de cambios de contraseña realizados por otros controladores de dominio del dominio. Si recientemente se cambió una contraseña, ese cambio tarda para replicar en todos los controladores de dominio del dominio. Si se produce un error en la autenticación de inicio de sesión en otro controlador de dominio debido a una contraseña incorrecta, ese controlador de dominio reenvía la solicitud de autenticación en el emulador PDC antes de decidir si aceptar o rechazar el intento de inicio de sesión.  
  
Coloca el emulador PDC en una ubicación que contiene un gran número de usuarios de ese dominio contraseña reenvío operaciones si es necesario. Además, asegúrate de que la ubicación está bien conectada a otras ubicaciones para reducir la latencia de replicación.  
  
Para que una hoja de cálculo que le ayudarán a documentar la información sobre dónde va a colocar los emuladores PDC y el número de usuarios para cada dominio que se representa en cada ubicación, consulta el trabajo Aid para Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558) ), descargar Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip y abrir la ubicación del controlador de dominio (DSSTOPO_4.doc).  
  
Debes hacer referencia a la información sobre ubicaciones en el que deberás colocar los emuladores de PDC al implementar dominios regionales. Para obtener más información acerca de la implementación de dominios regionales, consulta [Regional dominios de implementación de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
  
## <a name="requirements-for-infrastructure-master-placement"></a>Requisitos para la colocación de maestro de infraestructura  

El maestro de infraestructura actualiza los nombres de entidades de seguridad de otros dominios que se agregan a los grupos en su propio dominio. Por ejemplo, si un usuario de un dominio es un miembro de un grupo en un segundo dominio y se cambia el nombre del usuario en el primer dominio, el segundo dominio no se notifica que se debe actualizar el nombre del usuario en la lista de miembros del grupo. Dado que los controladores de dominio en un dominio no replican a entidades de seguridad en controladores de dominio en otro dominio, el dominio de segundo nunca reconoce el cambio en la ausencia de maestro de infraestructura.  
  
El maestro de infraestructura constantemente monitores pertenencia a grupos, buscando entidades de seguridad de otros dominios. Si lo encuentra, comprueba con el dominio de la entidad de seguridad para comprobar que se actualiza la información. Si la información está actualizada, el maestro de infraestructura realiza la actualización y, a continuación, replica el cambio a otros controladores de dominio en su dominio.  
  
Se aplican dos excepciones a esta regla. En primer lugar, si todos los controladores de dominio son servidores de catálogo global, el controlador de dominio que hospeda el rol de maestro de infraestructura es insignificante, porque catálogos globales replican la información actualizada, independientemente del dominio al que pertenecen. En segundo lugar, si el bosque tiene un solo dominio, el controlador de dominio que hospeda el rol de maestro de infraestructura es insignificante porque no existen entidades de seguridad de otros dominios.  
  
No se coloca al maestro de infraestructura en un controlador de dominio que también es un servidor de catálogo global. Si el maestro de infraestructura y el catálogo global están en el mismo controlador de dominio, el maestro de infraestructura no funcionará. El maestro de infraestructura no encontrará nunca datos que están actualizadas; por lo tanto, que nunca replicará los cambios en los demás controladores de dominio en el dominio.  
  
## <a name="operations-master-placement-for-networks-with-limited-connectivity"></a>Colocación de redes de maestro de operaciones con conectividad limitada  
Ten en cuenta que si el entorno tiene una ubicación central o un sitio de concentrador en el que puedes colocar los titulares de rol de maestro de operaciones, determinadas operaciones de controlador de dominio que dependen de la disponibilidad de las operaciones de función de maestro titulares pueden verse afectados.  
  
Por ejemplo, supongamos que una organización crea sitios A, B, C, y vínculos a sitios D. existen entre A y B entre B y C y conectividad C y D. red exactamente refleja la conectividad de red de los vínculos de sitios. En este ejemplo, el sitio de todas las operaciones de roles de maestro se colocan en una opción para la **enlazar todos los vínculos a sitios** no está seleccionado.  
  
Aunque esta configuración provoca replicación correcta entre todos los sitios, las funciones de rol de maestro de operaciones tienen las siguientes limitaciones:  
  
-   Controladores de dominio en sitios C y D no tiene acceso el emulador PDC en el sitio para actualizar una contraseña o para buscar una contraseña que se ha actualizado recientemente.  
  
-   Controladores de dominio en sitios C y D no tiene acceso al maestro RID en el sitio para obtener un conjunto de RID inicial después de la instalación de Active Directory y actualizar grupos RID tal como agotarse.  
  
-   Controladores de dominio en sitios C y D no se pueden agregar o quitar particiones de la aplicación personalizada, DNS o directorio.  
  
-   Controladores de dominio en sitios C y D no pueden hacer cambios en el esquema.  
  
Para que una hoja de cálculo que le ayudarán a planear la asignación de rol de maestro de operaciones, consulta ayudas de trabajo para [Kit de implementación de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip de descargar y abrir ubicación del controlador de dominio (DSSTOPO_4.doc).  
  
Tendrás que hacer referencia a esta información cuando se crea el dominio raíz del bosque y dominios regionales. Para obtener más información acerca de la implementación de dominio raíz del bosque, vea implementar un [implementar Windows Server 2008 dominio raíz del bosque](https://technet.microsoft.com/library/cc731174.aspx). Para obtener más información acerca de la implementación de dominios regionales, consulta [Regional dominios de implementación de Windows Server 2008](https://technet.microsoft.com/library/cc755118.aspx).  
  


