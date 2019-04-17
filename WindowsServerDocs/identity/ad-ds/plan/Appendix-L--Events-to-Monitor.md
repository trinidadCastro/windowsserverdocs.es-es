---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: "Apéndice L: eventos de Monitor"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d153a16b31070f2bbfac4a47a814792ef66b472
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-l-events-to-monitor"></a>Apéndice L: eventos al Monitor

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
## <a name="appendix-l-events-to-monitor"></a>Apéndice L: eventos al Monitor  
La siguiente tabla enumera los eventos que se deben supervisar en el entorno, según las recomendaciones proporcionadas en [supervisión de Active Directory de signos de comprometer](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). En la siguiente tabla, la columna "Identificador de evento actual de Windows" muestra el identificador de evento tal y como se implementa en las versiones de Windows y Windows Server que están actualmente en el soporte estándar.  
  
La columna "Identificador de evento heredados de Windows" muestra el identificador de evento correspondiente en versiones heredadas de Windows, como los equipos cliente que ejecutan Windows XP o versiones anteriores y en los servidores que ejecutan Windows Server 2003 o versiones anteriores. La "posibles importancia" columna identifica si el evento debe considerarse de bajo, medio o alto importancia en la detección de ataques y la columna "Resumen de eventos" proporciona una breve descripción del evento.  
  
Un posible importancia alta significa que una aparición del evento que debe investigarse. Importancia potencial de Media o baja significa que solo se deben investigar estos eventos si se producen de forma inesperada o en los números que superan significativamente la línea base esperada en un período de tiempo medido. Todas las organizaciones deben probar estas recomendaciones en sus entornos antes de crear las alertas que requieran respuestas de investigación obligatorio. Cada entorno es diferente, y algunos de los eventos clasificados con un posible importancia alta pueden producirse debido a otros eventos inofensivos.  
  
|||||  
|-|-|-|-|  
|**Identificador de evento actual de Windows**|**Identificador de evento heredados de Windows**|**Importancia posible**|**Resumen de eventos**|  
|4618|N/D|Alto|Se ha producido un patrón de eventos de seguridad supervisado.|  
|4649|N/D|Alto|Se detectó un ataque de reproducción. Puede ser un falso positivo inofensivo debido al error de configuración incorrecta.|  
|4719|612|Alto|Se cambió la directiva de auditoría del sistema.|  
|4765|N/D|Alto|SID History se ha agregado una cuenta.|  
|4766|N/D|Alto|Error al intentar agregar SID History a una cuenta.|  
|4794|N/D|Alto|Se intentó establecer el modo de restauración de servicios de directorio.|  
|4897|801|Alto|Separación de roles habilitada|  
|4964|N/D|Alto|Se han asignado grupos especiales a un nuevo inicio de sesión.|  
|5124|N/D|Alto|Una configuración de seguridad se ha actualizado en el servicio del Respondedor OCSP|  
|N/D|550|Medio a alto|Ataques de posibles denegación de servicio (DoS)|  
|1102|517|Medio a alto|Se borró el registro de auditoría|  
|4621|N/D|Mediano|Administrador recuperó el sistema del error CrashOnAuditFail. Los usuarios que no sean administradores ahora se podrá iniciar sesión. Es posible que no se haya registrado alguna actividad de auditoría.|  
|4675|N/D|Mediano|Se filtraron SID.|  
|4692|N/D|Mediano|Se intentó realizar la copia de seguridad de la clave maestra de protección de datos.|  
|4693|N/D|Mediano|Se intentó realizar la recuperación de la clave maestra de protección de datos.|  
|4706|610|Mediano|Se creó una nueva confianza a un dominio.|  
|4713|617|Mediano|Se cambió la directiva Kerberos.|  
|4714|618|Mediano|Se cambió la directiva de recuperación de datos cifrados.|  
|4715|N/D|Mediano|Se cambió la directiva de auditoría (SACL) en un objeto.|  
|4716|620|Mediano|Se modificó la información de dominio de confianza.|  
|4724|628|Mediano|Se ha intentado restablecer la contraseña de la cuenta.|  
|4727|631|Mediano|Se creó un grupo global con seguridad habilitada.|  
|4735|639|Mediano|Se cambió un grupo local con seguridad habilitada.|  
|4737|641|Mediano|Se cambió un grupo global con seguridad habilitada.|  
|4739|643|Mediano|Se cambió la directiva de dominio.|  
|4754|658|Mediano|Se creó un grupo universal con la seguridad habilitada.|  
|4755|659|Mediano|Se cambió un grupo universal con la seguridad habilitada.|  
|4764|667|Mediano|Se eliminó un grupo de seguridad deshabilitada|  
|4764|668|Mediano|Se cambió el tipo de un grupo.|  
|4780|684|Mediano|Se estableció la ACL en cuentas que son miembros del grupo de administradores.|  
|4816|N/D|Mediano|RPC detectó una infracción de integridad al descifrar un mensaje entrante.|  
|4865|N/D|Mediano|Se agregó una entrada de información de bosque de confianza.|  
|4866|N/D|Mediano|Se quitó una entrada de información de bosque de confianza.|  
|4867|N/D|Mediano|Se modificó una entrada de información de bosque de confianza.|  
|4868|772|Mediano|El Administrador de certificados denegó una solicitud de certificado pendiente.|  
|4870|774|Mediano|Servicios de certificados revocaron un certificado.|  
|4882|786|Mediano|Cambiaron los permisos de seguridad para los servicios de certificados.|  
|4885|789|Mediano|Cambia el filtro de auditoría de servicios de certificados.|  
|4890|794|Mediano|Cambia la configuración del Administrador de certificados para los servicios de certificados.|  
|4892|796|Mediano|Cambia de una propiedad de los servicios de certificados.|  
|4896|800|Mediano|Se han eliminado una o varias filas de la base de datos de certificado.|  
|4906|N/D|Mediano|El valor de CrashOnAuditFail ha cambiado.|  
|4907|N/D|Mediano|Se cambió la configuración de auditoría del objeto.|  
|4908|N/D|Mediano|Se modificó la tabla de inicio de sesión de grupos especial.|  
|4912|807|Mediano|Se cambió por la directiva de auditoría de usuario.|  
|4960|N/D|Mediano|IPsec descartó un paquete de entrada que no se pudo realizar una comprobación de integridad. Si el problema continúa, podría indicar que un problema de red o que los paquetes se están modificando en tránsito a este equipo. Comprueba que los paquetes enviados desde el equipo remoto sean iguales a los recibidos en este equipo. Este error también puede indicar problemas de interoperabilidad con otras implementaciones de IPsec.|  
|4961|N/D|Mediano|IPsec descartó un paquete de entrada que no se pudo realizar la comprobación de reproducción. Si el problema continúa, podría indicar un ataque de reproducción contra este equipo.|  
|4962|N/D|Mediano|IPsec descartó un paquete de entrada que no se pudo realizar la comprobación de reproducción. El paquete entrante tenía demasiado bajo un número de secuencia para garantizar que no fuera una reproducción.|  
|4963|N/D|Mediano|IPsec descartó un paquete de texto no cifrado entrante que debería estar protegido. Esto suele deberse al equipo remoto cambió su directiva IPsec sin informar a este equipo. También podría ser un intento de ataque de suplantación de identidad.|  
|4965|N/D|Mediano|IPsec recibió un paquete desde un equipo remoto con un índice de parámetro de seguridad (SPI) incorrecto. Esto está provocado habitualmente por hardware no funciona correctamente y que está dañando los paquetes. Si estos errores continúan, comprueba que los paquetes enviados desde el equipo remoto sean iguales a los recibidos en este equipo. Este error también puede indicar problemas de interoperabilidad con otras implementaciones de IPsec. En ese caso, si no se impide la conectividad, pueden omitirse estos eventos.|  
|4976|N/D|Mediano|Durante la negociación de modo principal, IPsec recibió un paquete de negociación no válido. Si el problema continúa, podría indicar un problema de red o un intento de modificar o reproducir esta negociación.|  
|4977|N/D|Mediano|Durante la negociación de modo rápido, IPsec recibió un paquete de negociación no válido. Si el problema continúa, podría indicar un problema de red o un intento de modificar o reproducir esta negociación.|  
|4978|N/D|Mediano|Durante la negociación de modo extendido, IPsec recibió un paquete de negociación no válido. Si el problema continúa, podría indicar un problema de red o un intento de modificar o reproducir esta negociación.|  
|4983|N/D|Mediano|Error de una negociación de modo extendido de IPsec. Se eliminó la asociación de seguridad de modo principal correspondiente.|  
|4984|N/D|Mediano|Error de una negociación de modo extendido de IPsec. Se eliminó la asociación de seguridad de modo principal correspondiente.|  
|5027|N/D|Mediano|El servicio de Firewall de Windows no pudo recuperar la directiva de seguridad desde el almacenamiento local. El servicio continuará aplicando la directiva actual.|  
|5028|N/D|Mediano|El servicio de Firewall de Windows no pudo analizar la nueva directiva de seguridad. El servicio continuará con la directiva aplicada actualmente.|  
|5029|N/D|Mediano|El servicio de Firewall de Windows no pudo inicializar el controlador. El servicio continuará aplicando la directiva actual.|  
|5030|N/D|Mediano|No se pudo iniciar el servicio de Firewall de Windows.|  
|5035|N/D|Mediano|No se pudo iniciar el controlador de Firewall de Windows.|  
|5037|N/D|Mediano|El controlador de Firewall de Windows detecta errores críticos en tiempo de ejecución. Terminación.|  
|5038|N/D|Mediano|Integridad de código determinó que el hash de imagen de un archivo no es válido. El archivo pudo resultar dañado por una modificación no autorizada o el hash no válido podría indicar un posible error de dispositivo de disco.|  
|5120|N/D|Mediano|Servicio del Respondedor OCSP iniciado|  
|5121|N/D|Mediano|Servicio del Respondedor OCSP detenido|  
|5122|N/D|Mediano|Una entrada de configuración que se cambió en el servicio del Respondedor OCSP|  
|5123|N/D|Mediano|Una entrada de configuración que se cambió en el servicio del Respondedor OCSP|  
|5376|N/D|Mediano|Las credenciales del Administrador de credenciales se hizo una copia de.|  
|5377|N/D|Mediano|Las credenciales del Administrador de credenciales se han restaurado desde una copia de seguridad.|  
|5453|N/D|Mediano|Error en la negociación de IPsec con un equipo remoto, no se inicia el servicio IKE y AuthIP módulos de claves de IPsec (IKEEXT).|  
|5480|N/D|Mediano|No se pudo obtener la lista completa de interfaces de red en el equipo de los servicios IPsec. Esto supone un riesgo de seguridad, ya que algunas de las interfaces de red no pueden obtener la protección proporcionada por los filtros IPsec aplicados. Usar el complemento Monitor de seguridad IP para diagnosticar el problema.|  
|5483|N/D|Mediano|Los servicios IPsec no pudo inicializar el servidor RPC. No se pudo iniciar los servicios IPsec.|  
|5484|N/D|Mediano|Los servicios IPsec experimentaron un error crítico y se ha cerrado. El cierre de los servicios IPsec puede suponer un mayor riesgo de ataque de red o exponer el equipo a posibles riesgos de seguridad.|  
|5485|N/D|Mediano|Los servicios IPsec error al procesar algunos filtros IPsec en un evento de plug and play para interfaces de red. Esto supone un riesgo de seguridad, ya que algunas de las interfaces de red no pueden obtener la protección proporcionada por los filtros IPsec aplicados. Usar el complemento Monitor de seguridad IP para diagnosticar el problema.|  
|6145|N/D|Mediano|Se ha producido uno o más errores durante el procesamiento de directiva de seguridad de los objetos de directiva de grupo.|  
|6273|N/D|Mediano|Servidor de directivas de redes denegó el acceso a un usuario.|  
|6274|N/D|Mediano|Servidor de directivas de redes rechazó la solicitud de un usuario.|  
|6275|N/D|Mediano|Servidor de directivas de redes rechazó la solicitud de cuentas de un usuario.|  
|6276|N/D|Mediano|Servidor de directivas de redes puso en cuarentena a un usuario.|  
|6277|N/D|Mediano|Servidor de directivas de redes concedió acceso a un usuario, pero lo sometió porque el host no cumplía la directiva de mantenimiento definida.|  
|6278|N/D|Mediano|Servidor de directivas de redes concedió acceso total a un usuario porque el host cumplía la directiva de mantenimiento definida.|  
|6279|N/D|Mediano|El servidor de directivas de red había bloqueado la cuenta de usuario debido a los intentos de autenticación erróneos repetidos.|  
|6280|N/D|Mediano|El servidor de directivas de red desbloquear la cuenta de usuario.|  
|-|640|Mediano|Base de datos de cuenta general cambiado|  
|-|619|Mediano|Calidad de servicio directiva cambiado|  
|24586|N/D|Mediano|Se detectó un error de conversión de volumen|  
|24592|N/D|Mediano|Error al intentar reiniciar automáticamente la conversión de volumen %2.|  
|24593|N/D|Mediano|Escribir metadatos: devolvió errores al intentar modificar metadatos de volumen %2. Si continúan errores, descifrar el volumen|  
|24594|N/D|Mediano|Reconstruir metadatos: un intento de escribir una copia de metadatos en el volumen %2 no pudo y puede que aparezca como disco está dañado. Si continúan errores, descifrar el volumen.|  
|4608|512|Baja|Windows se está iniciando.|  
|4609|513|Baja|Windows se cierra.|  
|4610|514|Baja|La autoridad de seguridad Local cargó un paquete de autenticación.|  
|4611|515|Baja|Se ha registrado un proceso de inicio de sesión de confianza con la autoridad de seguridad Local.|  
|4612|516|Baja|Los recursos internos asignados para la cola de mensajes de auditoría han se han agotado, provocando la pérdida de algunas auditorías.|  
|4614|518|Baja|Por el Administrador de cuentas de seguridad cargó un paquete de notificación.|  
|4615|519|Baja|Uso no válido de puerto LPC.|  
|4616|520|Baja|Se cambió la hora del sistema.|  
|4622|N/D|Baja|La autoridad de seguridad Local cargó un paquete de seguridad.|  
|4624|528,540|Baja|Una cuenta se inició sesión correctamente.|  
|4625|529-537,539|Baja|No se pudo iniciar sesión una cuenta.|  
|4634|538|Baja|Una cuenta se cerró sesión.|  
|4646|N/D|Baja|Iniciar el modo de prevención DoS IKE.|  
|4647|551|Baja|Cierre de sesión iniciada por el usuario.|  
|4648|552|Baja|Se intentó un inicio de sesión con credenciales explícitas.|  
|4650|N/D|Baja|Se estableció una asociación de seguridad de modo principal de IPsec. No se ha habilitado el modo extendido. No se usó la autenticación de certificado.|  
|4651|N/D|Baja|Se estableció una asociación de seguridad de modo principal de IPsec. No se ha habilitado el modo extendido. Se usó un certificado para la autenticación.|  
|4652|N/D|Baja|Error de una negociación de modo principal de IPsec.|  
|4653|N/D|Baja|Error de una negociación de modo principal de IPsec.|  
|4654|N/D|Baja|Error de una negociación de modo rápido de IPsec.|  
|4655|N/D|Baja|Finalizó una asociación de seguridad de modo principal de IPsec.|  
|4656|560|Baja|Se solicitó un identificador para un objeto.|  
|4657|567|Baja|Se modificó un valor del registro.|  
|4658|562|Baja|Se cerró el identificador de objeto.|  
|4659|N/D|Baja|Se solicitó un identificador a un objeto con intención de eliminarlo.|  
|4660|564|Baja|Se eliminó un objeto.|  
|4661|565|Baja|Se solicitó un identificador para un objeto.|  
|4662|566|Baja|Se realizó una operación en un objeto.|  
|4663|567|Baja|Se intentó tener acceso a un objeto.|  
|4664|N/D|Baja|Se intentó crear un vínculo físico.|  
|4665|N/D|Baja|Se intentó crear un contexto de cliente de la aplicación.|  
|4666|N/D|Baja|Una aplicación intentó una operación:|  
|4667|N/D|Baja|Se eliminó un contexto de cliente de la aplicación.|  
|4668|N/D|Baja|Se inicializó una aplicación.|  
|4670|N/D|Baja|Se cambiaron los permisos de un objeto.|  
|4671|N/D|Baja|Una aplicación intentó tener acceso a un ordinal bloqueado a través de TBs.|  
|4672|576|Baja|Privilegios especiales asignados al nuevo inicio de sesión.|  
|4673|577|Baja|Se llama a un servicio con privilegios.|  
|4674|578|Baja|Se intentó una operación en un objeto con privilegios.|  
|4688|592|Baja|Se ha creado un nuevo proceso.|  
|4689|593|Baja|Un proceso ha terminado.|  
|4690|594|Baja|Se intentó duplicar un identificador para un objeto.|  
|4691|595|Baja|Se solicitó acceso indirecto a un objeto.|  
|4694|N/D|Baja|Se intentó realizar la protección de los datos auditables protegidos.|  
|4695|N/D|Baja|Se intentó desproteger los datos auditables protegidos.|  
|4696|600|Baja|Se asignó un token principal a procesar.|  
|4697|601|Baja|Intenta instalar un servicio|  
|4698|602|Baja|Se creó una tarea programada.|  
|4699|602|Baja|Se eliminó una tarea programada.|  
|4700|602|Baja|Se habilitó una tarea programada.|  
|4701|602|Baja|Se deshabilitó una tarea programada.|  
|4702|602|Baja|Se actualizó una tarea programada.|  
|4704|608|Baja|Se asignó un derecho de usuario.|  
|4705|609|Baja|Se quitó un derecho de usuario.|  
|4707|611|Baja|Se quitó una confianza a un dominio.|  
|4709|N/D|Baja|Se iniciaron los servicios IPsec.|  
|4710|N/D|Baja|Se ha deshabilitado los servicios IPsec.|  
|4711|N/D|Baja|Puede contener cualquiera de las siguientes acciones: el motor PAStore aplicó una copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo. El motor PAStore aplicó la directiva IPsec de almacenamiento de Active Directory en el equipo. El motor PAStore aplicó la directiva IPsec de almacenamiento del registro local en el equipo. El motor PAStore no se pudo aplicar la copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo. Error del motor PAStore al aplicar la directiva IPsec de almacenamiento de Active Directory en el equipo. El motor PAStore no se pudo aplicar la directiva IPsec de almacenamiento del registro local en el equipo. Error del motor PAStore al aplicar algunas reglas de la directiva IPsec activa en el equipo. El motor PAStore podido cargar en el equipo de directiva IPsec de almacenamiento de directorio. El motor PAStore cargó en el equipo de directiva IPsec de almacenamiento de directorio. El motor PAStore podido cargar en el equipo de directiva IPsec de almacenamiento local. El motor PAStore cargó en el equipo de directiva IPsec de almacenamiento local. El motor PAStore sondeó en busca de cambios en la directiva IPsec activa y no detectó ningún cambio.|  
|4712|N/D|Baja|Los servicios IPsec detectaron un error que puede ser importante.|  
|4717|621|Baja|Se concedió acceso al sistema de seguridad a una cuenta.|  
|4718|622|Baja|Se quitó acceso de seguridad del sistema desde una cuenta.|  
|4720|624|Baja|Se creó una cuenta de usuario.|  
|4722|626|Baja|Se habilitó una cuenta de usuario.|  
|4723|627|Baja|Se intentó para cambiar la contraseña de la cuenta.|  
|4725|629|Baja|Se deshabilitó una cuenta de usuario.|  
|4726|630|Baja|Se eliminó una cuenta de usuario.|  
|4728|632|Baja|Se agregó un miembro a un grupo global con seguridad habilitada.|  
|4729|633|Baja|Se quitó un miembro de un grupo global con seguridad habilitada.|  
|4730|634|Baja|Se eliminó un grupo global con seguridad habilitada.|  
|4731|635|Baja|Se creó un grupo local con seguridad habilitada.|  
|4732|636|Baja|Se agregó un miembro a un grupo local con seguridad habilitada.|  
|4733|637|Baja|Se quitó un miembro de un grupo local con seguridad habilitada.|  
|4734|638|Baja|Se eliminó un grupo local con seguridad habilitada.|  
|4738|642|Baja|Se cambió una cuenta de usuario.|  
|4740|644|Baja|Se bloqueó una cuenta de usuario.|  
|4741|645|Baja|Se cambió una cuenta de equipo.|  
|4742|646|Baja|Se cambió una cuenta de equipo.|  
|4743|647|Baja|Se eliminó una cuenta de equipo.|  
|4744|648|Baja|Se creó un grupo local de la seguridad deshabilitada.|  
|4745|649|Baja|Se cambió un grupo local de la seguridad deshabilitada.|  
|4746|650|Baja|Se agregó un miembro a un grupo local de la seguridad deshabilitada.|  
|4747|651|Baja|Se quitó un miembro de un grupo local de la seguridad deshabilitada.|  
|4748|652|Baja|Se eliminó un grupo local de la seguridad deshabilitada.|  
|4749|653|Baja|Se creó un grupo global con la seguridad deshabilitada.|  
|4750|654|Baja|Se cambió un grupo global con la seguridad deshabilitada.|  
|4751|655|Baja|Se agregó un miembro a un grupo global con la seguridad deshabilitada.|  
|4752|656|Baja|Se quitó un miembro de un grupo global con la seguridad deshabilitada.|  
|4753|657|Baja|Se eliminó un grupo global con la seguridad deshabilitada.|  
|4756|660|Baja|Se agregó un miembro a un grupo universal con la seguridad habilitada.|  
|4757|661|Baja|Se quitó un miembro de un grupo universal con la seguridad habilitada.|  
|4758|662|Baja|Se eliminó un grupo universal con la seguridad habilitada.|  
|4759|663|Baja|Se creó un grupo universal con la seguridad deshabilitada.|  
|4760|664|Baja|Se cambió un grupo universal con la seguridad deshabilitada.|  
|4761|665|Baja|Se agregó un miembro a un grupo universal con la seguridad deshabilitada.|  
|4762|666|Baja|Se quitó un miembro de un grupo universal con la seguridad deshabilitada.|  
|4767|671|Baja|Se desbloqueó una cuenta de usuario.|  
|4768|672,676|Baja|Se solicitó un vale de autenticación (TGT) de Kerberos.|  
|4769|673|Baja|Se solicitó un vale de servicio Kerberos.|  
|4770|674|Baja|Se renovó un vale de servicio Kerberos.|  
|4771|675|Baja|No se pudo realizar la autenticación previa de Kerberos.|  
|4772|672|Baja|No se pudo realizar una solicitud de vale de autenticación de Kerberos.|  
|4774|678|Baja|Se asignó una cuenta para inicio de sesión.|  
|4775|679|Baja|No se pudo asignar una cuenta para inicio de sesión.|  
|4776|680,681|Baja|El controlador de dominio intentó validar las credenciales para una cuenta.|  
|4777|N/D|Baja|El controlador de dominio no se pudo validar las credenciales para una cuenta.|  
|4778|682|Baja|Se reconectó una sesión a una estación de ventana.|  
|4779|683|Baja|Se desconectó una sesión de una estación de ventana.|  
|4781|685|Baja|Se cambió el nombre de una cuenta:|  
|4782|N/D|Baja|Se accedió el hash de contraseña de una cuenta.|  
|4783|667|Baja|Se creó un grupo de aplicaciones básicas.|  
|4784|N/D|Baja|Se cambió un grupo de aplicaciones básicas.|  
|4785|689|Baja|Se agregó un miembro a un grupo de aplicaciones básicas.|  
|4786|690|Baja|Se quitó un miembro de un grupo de aplicaciones básicas.|  
|4787|691|Baja|Se agregó un no miembro a un grupo de aplicaciones básicas.|  
|4788|692|Baja|Se quitó un no miembro de un grupo de aplicaciones básicas.|  
|4789|693|Baja|Se eliminó un grupo de aplicaciones básicas.|  
|4790|694|Baja|Se creó un grupo de consulta LDAP.|  
|4793|N/D|Baja|Se ha llamado a la API de comprobación de directiva de contraseña.|  
|4800|N/D|Baja|Se bloqueó la estación de trabajo.|  
|4801|N/D|Baja|Se desbloqueó la estación de trabajo.|  
|4802|N/D|Baja|Se invocó el protector de pantalla.|  
|4803|N/D|Baja|Se descartó el protector de pantalla.|  
|4864|N/D|Baja|Se detectó una colisión de espacio de nombres.|  
|4869|773|Baja|Servicios de certificados recibieron una solicitud de certificado reenviada.|  
|4871|775|Baja|Servicios de certificados recibieron una solicitud para publicar la lista de revocación de certificados (CRL).|  
|4872|776|Baja|Los servicios de certificados publicaron la lista de revocación de certificados (CRL).|  
|4873|777|Baja|Se cambió una extensión de solicitud de certificado.|  
|4874|778|Baja|Cambiar de uno o varios atributos de solicitud de certificado.|  
|4875|779|Baja|Servicios de certificados recibieron una solicitud para apagar tu PC.|  
|4876|780|Baja|Inicia la copia de seguridad de los servicios de certificados.|  
|4877|781|Baja|Completado de copia de seguridad de los servicios de certificados.|  
|4878|782|Baja|Se inició la restauración de servicios de certificados.|  
|4879|783|Baja|Ha completado la restauración de servicios de certificados.|  
|4880|784|Baja|Iniciar los servicios de certificados.|  
|4881|785|Baja|Se detuvieron los servicios de certificados.|  
|4883|787|Baja|Servicios de certificados recuperaron una clave archivada.|  
|4884|788|Baja|Servicios de certificados importaron un certificado a su base de datos.|  
|4886|790|Baja|Servicios de certificados recibieron una solicitud de certificado.|  
|4887|791|Baja|Servicios de certificados aprobados una solicitud de certificado y emitieron un certificado.|  
|4888|792|Baja|Servicios de certificados denegó una solicitud de certificado.|  
|4889|793|Baja|Servicios de certificados de establecen el estado de una solicitud de certificado en pendiente.|  
|4891|795|Baja|Entrada de configuración que se cambió en servicios de certificados.|  
|4893|797|Baja|Servicios de certificados archivaron una clave.|  
|4894|798|Baja|Servicios de certificados importados y archivaron una clave.|  
|4895|799|Baja|Servicios de certificados publican el certificado de CA en servicios de dominio de Active Directory.|  
|4898|802|Baja|Servicios de certificados cargan una plantilla.|  
|4902|N/D|Baja|Se creó la tabla de directiva de auditoría por usuario.|  
|4904|N/D|Baja|Se intentó registrar un origen de eventos de seguridad.|  
|4905|N/D|Baja|Se intentó anular el registro de un origen de eventos de seguridad.|  
|4909|N/D|Baja|Se cambió la configuración de directiva local para TBS.|  
|4910|N/D|Baja|Se cambió la configuración de directiva de grupo para TBS.|  
|4928|N/D|Baja|Se estableció un contexto de nomenclatura de origen de réplica de Active Directory.|  
|4929|N/D|Baja|Se quitó un contexto de nomenclatura de origen de réplica de Active Directory.|  
|4930|N/D|Baja|Se modificó un contexto de nomenclatura de origen de réplica de Active Directory.|  
|4931|N/D|Baja|Se modificó un contexto de nomenclatura de destino de réplica de Active Directory.|  
|4932|N/D|Baja|Empezó la sincronización de una réplica de un contexto de nomenclatura de Active Directory.|  
|4933|N/D|Baja|Ha finalizado la sincronización de una réplica de un contexto de nomenclatura de Active Directory.|  
|4934|N/D|Baja|Se replicaron los atributos de un objeto de Active Directory.|  
|4935|N/D|Baja|Comienza el error de replicación.|  
|4936|N/D|Baja|Finaliza el error de replicación.|  
|4937|N/D|Baja|Se quitó un objeto persistente de una réplica.|  
|4944|N/D|Baja|La siguiente directiva estaba activa cuando se inició el Firewall de Windows.|  
|4945|N/D|Baja|Se mostró una regla al iniciarse el Firewall de Windows.|  
|4946|N/D|Baja|Se ha realizado un cambio en la lista de excepciones del Firewall de Windows. Se agregó una regla.|  
|4947|N/D|Baja|Se ha realizado un cambio en la lista de excepciones del Firewall de Windows. Se modificó una regla.|  
|4948|N/D|Baja|Se ha realizado un cambio en la lista de excepciones del Firewall de Windows. Se eliminó una regla.|  
|4949|N/D|Baja|Configuración de Firewall de Windows se restauraron a los valores predeterminados.|  
|4950|N/D|Baja|Se cambió una configuración de Firewall de Windows.|  
|4951|N/D|Baja|Se ha omitido una regla porque Firewall de Windows no reconoció su número de versión principal.|  
|4952|N/D|Baja|Se han omitido partes de una regla porque Firewall de Windows no reconoció su número de versión secundaria. Se aplicarán las demás partes de la regla.|  
|4953|N/D|Baja|Se ha omitido una regla de Firewall de Windows porque no se pudo analizar la regla.|  
|4954|N/D|Baja|Configuración de directiva de grupo de Firewall de Windows ha cambiado. Se ha aplicado la nueva configuración.|  
|4956|N/D|Baja|Firewall de Windows ha cambiado el perfil activo.|  
|4957|N/D|Baja|Firewall de Windows no aplicó la siguiente regla:|  
|4958|N/D|Baja|Firewall de Windows no aplicó la siguiente regla porque hacía referencia a los elementos que no están configurados en este equipo:|  
|4979|N/D|Baja|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4980|N/D|Baja|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4981|N/D|Baja|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4982|N/D|Baja|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4985|N/D|Baja|Ha cambiado el estado de una transacción.|  
|5024|N/D|Baja|El servicio de Firewall de Windows se ha iniciado correctamente.|  
|5025|N/D|Baja|El servicio de Firewall de Windows se ha detenido.|  
|5031|N/D|Baja|El servicio de Firewall de Windows impidió que una aplicación aceptara conexiones entrantes en la red.|  
|5032|N/D|Baja|Firewall de Windows no pudo notificar al usuario que impidió que una aplicación aceptara conexiones entrantes en la red.|  
|5033|N/D|Baja|El controlador de Firewall de Windows se ha iniciado correctamente.|  
|5034|N/D|Baja|El controlador de Firewall de Windows se ha detenido.|  
|5039|N/D|Baja|Se virtualizó una clave del registro.|  
|5040|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se agregó un conjunto de autenticación.|  
|5041|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se modificó un conjunto de autenticación.|  
|5042|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se eliminó un conjunto de autenticación.|  
|5043|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se agregó una regla de seguridad de conexión.|  
|5044|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se modificó una regla de seguridad de conexión.|  
|5045|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se eliminó una regla de seguridad de conexión.|  
|5046|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se agregó un conjunto criptográfico.|  
|5047|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se modificó un conjunto criptográfico.|  
|5048|N/D|Baja|Se ha realizado un cambio a la configuración de IPsec. Se eliminó un conjunto criptográfico.|  
|5050|N/D|Baja|Intentar mediante programación deshabilitar el Firewall de Windows mediante una llamada a InetFwProfile.FirewallEnabled(False)|  
|5051|N/D|Baja|Se virtualizó un archivo.|  
|5056|N/D|Baja|Se realizó una prueba automática criptográfica.|  
|5057|N/D|Baja|No se pudo realizar la operación primitiva criptográfica.|  
|5058|N/D|Baja|Operación de archivo de clave.|  
|5059|N/D|Baja|Operación de migración de clave.|  
|5060|N/D|Baja|No se pudo realizar la operación de comprobación.|  
|5061|N/D|Baja|Operación criptográfica.|  
|5062|N/D|Baja|Se realizó una criptográfica de modo kernel de prueba.|  
|5063|N/D|Baja|Se intentó una operación de proveedor criptográfico.|  
|5064|N/D|Baja|Se intentó una operación de contexto criptográfico.|  
|5065|N/D|Baja|Se intentó modificar un contexto criptográfico.|  
|5066|N/D|Baja|Se intentó una operación de función criptográfica.|  
|5067|N/D|Baja|Se intentó modificar una función criptográfica.|  
|5068|N/D|Baja|Se intentó una operación de proveedor de función criptográfica.|  
|5069|N/D|Baja|Se intentó una operación de propiedad de función criptográfica.|  
|5070|N/D|Baja|Se intentó modificar una propiedad de función criptográfica.|  
|5125|N/D|Baja|Se envió una solicitud al servicio del Respondedor OCSP|  
|5126|N/D|Baja|Certificado de firma se actualizó automáticamente por el servicio del Respondedor OCSP|  
|5127|N/D|Baja|El proveedor de revocación de OCSP actualizado correctamente la información de revocación|  
|5136|566|Baja|Se modificó un objeto de servicio de directorio.|  
|5137|566|Baja|Se creó un objeto de servicio de directorio.|  
|5138|N/D|Baja|Se recuperó un objeto de servicio de directorio.|  
|5139|N/D|Baja|Se movió un objeto de servicio de directorio.|  
|5140|N/D|Baja|Un objeto de recurso compartido de red se tuvo acceso.|  
|5141|N/D|Baja|Se eliminó un objeto de servicio de directorio.|  
|5152|N/D|Baja|La plataforma de filtrado de Windows bloqueó un paquete.|  
|5153|N/D|Baja|Un filtro más restrictivo de la plataforma de filtrado de Windows bloqueó un paquete.|  
|5154|N/D|Baja|La plataforma de filtrado de Windows permitió una aplicación o servicio para escuchar las conexiones entrantes en un puerto.|  
|5155|N/D|Baja|La plataforma de filtrado de Windows bloqueó una aplicación o un servicio escuche conexiones entrantes en un puerto.|  
|5156|N/D|Baja|La plataforma de filtrado de Windows permitió una conexión.|  
|5157|N/D|Baja|La plataforma de filtrado de Windows bloqueó una conexión.|  
|5158|N/D|Baja|La plataforma de filtrado de Windows permitió un enlace a un puerto local.|  
|5159|N/D|Baja|La plataforma de filtrado de Windows bloqueó un enlace a un puerto local.|  
|5378|N/D|Baja|Directiva no permitió la delegación de credenciales solicitada.|  
|5440|N/D|Baja|La siguiente llamada estaba presente cuando se inició la plataforma de filtrado de Windows motor de filtro Base.|  
|5441|N/D|Baja|El siguiente filtro estaba presente cuando se inició la plataforma de filtrado de Windows motor de filtro Base.|  
|5442|N/D|Baja|El siguiente proveedor estaba presente cuando se inició la plataforma de filtrado de Windows motor de filtro Base.|  
|5443|N/D|Baja|El siguiente contexto de proveedor estaba presente cuando se inicia la plataforma de filtrado de Windows motor de filtro Base.|  
|5444|N/D|Baja|El siguiente subnivel estaba presente cuando se inicia la plataforma de filtrado de Windows motor de filtro Base.|  
|5446|N/D|Baja|Se cambió una llamada de la plataforma de filtrado de Windows.|  
|5447|N/D|Baja|Se cambió un filtro de la plataforma de filtrado de Windows.|  
|5448|N/D|Baja|Se cambió un proveedor de la plataforma de filtrado de Windows.|  
|5449|N/D|Baja|Se cambió un contexto de proveedor de la plataforma de filtrado de Windows.|  
|5450|N/D|Baja|Se cambió un subnivel de la plataforma de filtrado de Windows.|  
|5451|N/D|Baja|Se estableció una asociación de seguridad de modo rápido de IPsec.|  
|5452|N/D|Baja|Finalizó una asociación de seguridad de modo rápido de IPsec.|  
|5456|N/D|Baja|El motor PAStore aplicó la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5457|N/D|Baja|Error del motor PAStore al aplicar la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5458|N/D|Baja|El motor PAStore aplicó una copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5459|N/D|Baja|El motor PAStore no se pudo aplicar la copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5460|N/D|Baja|El motor PAStore aplicó la directiva IPsec de almacenamiento del registro local en el equipo.|  
|5461|N/D|Baja|El motor PAStore no se pudo aplicar la directiva IPsec de almacenamiento del registro local en el equipo.|  
|5462|N/D|Baja|Error del motor PAStore al aplicar algunas reglas de la directiva IPsec activa en el equipo. Usar el complemento Monitor de seguridad IP para diagnosticar el problema.|  
|5463|N/D|Baja|El motor PAStore sondeó en busca de cambios en la directiva IPsec activa y no detectó ningún cambio.|  
|5464|N/D|Baja|El motor PAStore sondeó en busca de cambios en la directiva IPsec activa, detectó cambios y aplicó a los servicios IPsec.|  
|5465|N/D|Baja|El motor PAStore recibió un control para la recarga forzada de la directiva IPsec y procesó el control correctamente.|  
|5466|N/D|Baja|Motor PAStore sondeó en busca de cambios en la directiva IPsec de Active Directory, determinada que no se puede alcanzar Active Directory y usará la copia almacenada en caché de la directiva IPsec de Active Directory en su lugar. Los cambios realizados a la directiva IPsec de Active Directory, ya que no se pudo aplicar el último sondeo.|  
|5467|N/D|Baja|Motor PAStore sondeó en busca de cambios en la directiva IPsec de Active Directory, determinada que Active Directory puede alcanza y no encontró ningún cambio en la directiva. Ya no se usa la copia almacenada en caché de la directiva IPsec de Active Directory.|  
|5468|N/D|Baja|El motor PAStore busca de cambios en la directiva IPsec de Active Directory, determinada que Active Directory pueden alcanzarse, encontró cambios en la directiva y aplicar los cambios. Ya no se usa la copia almacenada en caché de la directiva IPsec de Active Directory.|  
|5471|N/D|Baja|El motor PAStore cargó en el equipo de directiva IPsec de almacenamiento local.|  
|5472|N/D|Baja|El motor PAStore podido cargar en el equipo de directiva IPsec de almacenamiento local.|  
|5473|N/D|Baja|El motor PAStore cargó en el equipo de directiva IPsec de almacenamiento de directorio.|  
|5474|N/D|Baja|El motor PAStore podido cargar en el equipo de directiva IPsec de almacenamiento de directorio.|  
|5477|N/D|Baja|El motor PAStore no pudo agregar el filtro de modo rápido.|  
|5479|N/D|Baja|Los servicios IPsec se cerraron correctamente. El cierre de los servicios IPsec puede suponer un mayor riesgo de ataque de red o exponer el equipo a posibles riesgos de seguridad.|  
|5632|N/D|Baja|Se realizó una solicitud para autenticar a una red inalámbrica.|  
|5633|N/D|Baja|Se realizó una solicitud para autenticar a una red cableada.|  
|5712|N/D|Baja|Se intentó una llamada a procedimiento remoto (RPC).|  
|5888|N/D|Baja|Se modificó un objeto en el catálogo COM +.|  
|5889|N/D|Baja|Se eliminó un objeto del catálogo COM +.|  
|5890|N/D|Baja|Se agregó un objeto al catálogo COM +.|  
|6008|N/D|Baja|Se esperaba el cierre anterior del sistema|  
|6144|N/D|Baja|Se ha aplicado correctamente la directiva de seguridad de los objetos de directiva de grupo.|  
|6272|N/D|Baja|Servidor de directivas de redes concedió acceso a un usuario.|  
|N/D|561|Baja|Se solicitó un identificador para un objeto.|  
|N/D|563|Baja|Abrir objeto para eliminar|  
|N/D|625|Baja|Cambiar el tipo de cuenta de usuario|  
|N/D|613|Baja|Se ha iniciado el agente de directiva IPsec|  
|N/D|614|Baja|Agente de directiva IPsec deshabilitado|  
|N/D|615|Baja|Agente de directiva IPsec|  
|N/D|616|Baja|Agente de directiva IPsec detectó un error grave posible|  
|24577|N/D|Baja|Cifrado de volumen iniciado|  
|24578|N/D|Baja|Cifrado de volumen detenido|  
|24579|N/D|Baja|Cifrado de volumen completado|  
|24580|N/D|Baja|Descifrado de volumen iniciado|  
|24581|N/D|Baja|Descifrado de volumen detenido|  
|24582|N/D|Baja|Descifrado de volúmenes completado|  
|24583|N/D|Baja|Subproceso de trabajo de conversión para iniciar el volumen|  
|24584|N/D|Baja|Temporalmente detiene el subproceso de trabajo de conversión para el volumen|  
|24588|N/D|Baja|La operación de conversión en el volumen %2 encontró un error de sectores defectuosos. Valide los datos en este volumen|  
|24595|N/D|Baja|Volumen %2 contiene no válidos. Estos se omitirán durante la conversión.|  
|24621|N/D|Baja|Comprobar estado inicial: transacción de conversión de volumen de giro en %2.|  
|5049|N/D|Baja|Se eliminó una asociación de seguridad de IPsec.|  
|5478|N/D|Baja|Los servicios IPsec se ha iniciado correctamente.|  
  
> [!NOTE]  
> Consulte [artículo de Microsoft Support 947226](https://support.microsoft.com/kb/947226) para una lista de identificadores muchos de los eventos de seguridad y su significado.  
>   
> Ejecutar **wevtutil gp Microsoft-Windows--las auditorías de seguridad /ge /gm:true** para obtener una lista muy detallada de seguridad de todos los identificadores de evento  
  
Para obtener más información sobre los identificadores de eventos de seguridad de Windows y su significado, consulta los artículos de Microsoft Support [descripción de eventos de seguridad en Windows Vista y Windows Server 2008](https://support.microsoft.com/kb/947226) y [descripción de eventos de seguridad en Windows 7 y Windows Server 2008 R2](https://support.microsoft.com/kb/977519). También puedes descargar [eventos de auditoría de seguridad para Windows 7 y Windows Server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) y [Windows 8 y los detalles del evento de seguridad de Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), que proporcionan información detallada de eventos para los sistemas operativos que se hace referencia en formato de hoja de cálculo.  
  


