---
ms.assetid: 99a68050-8d19-4c58-ad86-e08a3dcdb4f7
title: 'Apéndice L: eventos para supervisar'
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 07/30/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e47b1fbe2df16aca9514e8a29c82d56d8dc96cba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822858"
---
# <a name="appendix-l-events-to-monitor"></a>Anexo L: eventos para supervisar

>Se aplica a: Windows Server

En la tabla siguiente se enumeran los eventos que debe supervisar en su entorno, de acuerdo con las recomendaciones proporcionadas en [supervisión Active Directory para ver los signos de riesgo](../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). En la tabla siguiente, la columna "ID. de evento de Windows actual" muestra el ID. de evento tal como se implementa en las versiones de Windows y Windows Server que se encuentran actualmente en soporte estándar.  
  
La columna "ID. de evento de Windows heredado" muestra el identificador de evento correspondiente en versiones heredadas de Windows, como equipos cliente que ejecutan Windows XP o versiones anteriores, y servidores que ejecutan Windows Server 2003 o una versión anterior. La columna "importancia crítica" identifica si el evento debe considerarse de importancia baja, media o alta en la detección de ataques, y la columna "Resumen de eventos" proporciona una breve descripción del evento.  
  
Una crítica potencial de alto significa que se debe investigar una aparición del evento. Una posible importancia media o baja significa que estos eventos solo deben investigarse si se producen de forma inesperada o en números que superan significativamente la línea de base esperada en un período de tiempo medido. Todas las organizaciones deben probar estas recomendaciones en sus entornos antes de crear las alertas que requieren respuestas investigación obligatorias. Cada entorno es diferente, y algunos de los eventos que se clasifican con una importancia crítica potencial se pueden producir debido a otros eventos inofensivos.  
  
|||||  
|-|-|-|-|  
|**ID. de evento de Windows actual**|**ID. de evento de Windows heredado**|**Posible importancia**|**Resumen de eventos**|  
|4618|N/D|Alto|Se ha producido un patrón de evento de seguridad supervisado.|  
|4649|N/D|Alto|Se detectó un ataque de reproducción. Puede ser un falso positivo inofensivo debido a un error de configuración erróneo.|  
|4719|612|Alto|Se cambió la directiva de auditoría del sistema|  
|4765|N/D|Alto|Se agregó el historial de SID a una cuenta.|  
|4766|N/D|Alto|Se produjo un error al intentar agregar el historial de SID a una cuenta.|  
|4794|N/D|Alto|Se intentó establecer el Modo de restauración de servicios de directorio.|  
|4897|801|Alto|Separación de roles habilitada|  
|4964|N/D|Alto|Se han asignado Grupos especiales un nuevo inicio de sesión.|  
|5124|N/D|Alto|Se actualizó una configuración de seguridad en el servicio de respuesta de OCSP|  
|N/D|550|De medio a alto|Posible ataque de denegación de servicio (DoS)|  
|1102|517|De medio a alto|Se borró el registro de auditoría|  
|4621|N/D|Media|El administrador recuperó el sistema del error CrashOnAuditFail. Los usuarios que no sean administradores podrán iniciar sesión ahora. Es posible que no se haya registrado alguna actividad de auditoría.|  
|4675|N/D|Media|Se filtraron SID.|  
|4692|N/D|Media|Se intentó hacer una copia de seguridad de la clave maestra de protección de datos|  
|4693|N/D|Media|Se intentó recuperar la clave maestra de protección de datos|  
|4706|610|Media|Se creó una nueva confianza para un dominio|  
|4713|617|Media|Se cambió la directiva Kerberos|  
|4714|618|Media|Se cambió la directiva de recuperación de datos cifrados|  
|4715|N/D|Media|Se cambió la directiva de auditoría (SACL) en un objeto|  
|4716|620|Media|Se modificó la información de dominio de confianza|  
|4724|628|Media|Se ha realizado un intento de restablecer la contraseña de una cuenta.|  
|4727|631|Media|Se creó un grupo global con seguridad habilitada.|  
|4735|639|Media|Se cambió un grupo local con seguridad habilitada.|  
|4737|641|Media|Se cambió un grupo global con seguridad habilitada.|  
|4739|643|Media|Se cambió la directiva de dominio|  
|4754|658|Media|Se creó un grupo universal con la seguridad habilitada.|  
|4755|659|Media|Se cambió un grupo universal con la seguridad habilitada.|  
|4764|667|Media|Se eliminó un grupo con seguridad deshabilitada|  
|4764|668|Media|Se cambió el tipo de un grupo.|  
|4780|684|Media|Se estableció la ACL en cuentas que son miembros de grupos de administradores.|  
|4816|N/D|Media|RPC detectó una infracción de integridad al descifrar un mensaje entrante.|  
|4865|N/D|Media|Se agregó una entrada de información de bosque de confianza|  
|4866|N/D|Media|Se eliminó una entrada de información de bosque de confianza|  
|4867|N/D|Media|Se modificó una entrada de información de bosque de confianza|  
|4868|772|Media|El Administrador de certificados ha denegado una solicitud de certificado pendiente.|  
|4870|774|Media|Servicios de servidor de certificados revocó un certificado.|  
|4882|786|Media|Se cambiaron los permisos de seguridad para Servicios de servidor de certificados.|  
|4885|789|Media|Se cambió el filtro de auditoría para Servicios de servidor de certificados.|  
|4890|794|Media|Se cambió la configuración del administrador de certificados para Servicios de servidor de certificados.|  
|4892|796|Media|Se cambió una propiedad de Servicios de servidor de certificados.%|  
|4896|800|Media|Una o más filas se han eliminado de la base de datos de certificados.|  
|4906|N/D|Media|Se cambió el valor de CrashOnAuditFail|  
|4907|N/D|Media|Se cambió la configuración de auditoría del objeto|  
|4908|N/D|Media|Se modificó la tabla de inicio de sesión de grupos especiales|  
|4912|807|Media|Se cambió la directiva de auditoría por usuario|  
|4960|N/D|Media|IPsec descartó un paquete entrante con un error en la comprobación de integridad. Si el problema continúa, podría indicar un problema de red o que se están modificando los paquetes en tránsito a este equipo. Comprueba que los paquetes enviados desde el equipo remoto sean iguales a los recibidos en este equipo. Este error también puede indicar problemas de interoperabilidad con otras implementaciones de IPsec.|  
|4961|N/D|Media|IPsec descartó un paquete entrante con un error en la comprobación de reproducción. Si el problema continúa, podría indicar un ataque de reproducción contra este equipo.|  
|4962|N/D|Media|IPsec descartó un paquete entrante con un error en la comprobación de reproducción. El paquete entrante tenía un número de secuencia demasiado bajo para asegurarse de que no fuera una reproducción.|  
|4963|N/D|Media|IPsec descartó un paquete de texto no cifrado entrante que debería estar protegido. Esto suele deberse a que el equipo remoto cambió su directiva IPsec sin informar a este equipo. También podría ser un intento de ataque de suplantación.|  
|4965|N/D|Media|IPsec recibió un paquete de un equipo remoto con un Índice de parámetro de seguridad (SPI) incorrecto. Esto suele deberse a hardware que funciona incorrectamente y que está dañando los paquetes. Si estos errores continúan, comprueba que los paquetes enviados desde el equipo remoto sean iguales a los recibidos en este equipo. Este error también puede indicar problemas de interoperabilidad con otras implementaciones de IPsec. En ese caso, si no se impide la conectividad, se pueden omitir estos eventos.|  
|4976|N/D|Media|Durante la negociación de modo principal, IPsec recibió un paquete de negociación no válido. Si el problema continúa, podría indicar un problema de red o que se está intentando modificar o reproducir esta negociación.|  
|4977|N/D|Media|Durante la negociación de modo rápido, IPsec recibió un paquete de negociación no válido. Si el problema continúa, podría indicar un problema de red o que se está intentando modificar o reproducir esta negociación.|  
|4978|N/D|Media|Durante la negociación de modo extendido, IPsec recibió un paquete de negociación no válido. Si el problema continúa, podría indicar un problema de red o que se está intentando modificar o reproducir esta negociación.|  
|4983|N/D|Media|Error de una negociación de modo extendido de IPsec. Se eliminó la asociación de seguridad de modo principal correspondiente.|  
|4984|N/D|Media|Error de una negociación de modo extendido de IPsec. Se eliminó la asociación de seguridad de modo principal correspondiente.|  
|5027|N/D|Media|El servicio de Firewall de Windows no pudo recuperar la directiva de seguridad del almacenamiento local. El servicio continuará aplicando la directiva actual.|  
|5028|N/D|Media|El servicio Firewall de Windows no pudo analizar la nueva directiva de seguridad. El servicio continuará con la directiva aplicada actualmente.|  
|5029|N/D|Media|El servicio Firewall de Windows no pudo inicializar el controlador. El servicio continuará aplicando la directiva actual.|  
|5030|N/D|Media|No se pudo iniciar el servicio Firewall de Windows.|  
|5035|N/D|Media|No se pudo iniciar el controlador de Firewall de Windows.|  
|5037|N/D|Media|El controlador de Firewall de Windows detectó un error en tiempo de ejecución crítico. Finalizando.|  
|5038|N/D|Media|Integridad de código determinó que el hash de imagen de un archivo no es válido. El archivo pudo resultar dañado por una modificación no autorizada o el hash no válido podría indicar un posible error de dispositivo de disco.|  
|5120|N/D|Media|Servicio de respuesta de OCSP iniciado|  
|5121|N/D|Media|Servicio de respuesta de OCSP detenido|  
|5122|N/D|Media|Una entrada de configuración cambiada en el servicio de respuesta de OCSP|  
|5123|N/D|Media|Una entrada de configuración cambiada en el servicio de respuesta de OCSP|  
|5376|N/D|Media|Se hizo una copia de seguridad de las credenciales del Administrador de credenciales.|  
|5377|N/D|Media|Se restauraron las credenciales del Administrador de credenciales desde una copia de seguridad.|  
|5453|N/D|Media|Error de negociación de IPsec con un equipo remoto. No se inició el servicio de Módulos de creación de claves de IPsec para IKE y AuthIP (IKEEXT).|  
|5480|N/D|Media|Error de los servicios IPsec al obtener la lista completa de interfaces de red del equipo. Esto supone un posible riesgo de seguridad porque puede que algunas interfaces de red no obtengan la protección proporcionada por los filtros IPsec aplicados. Usa el complemento Monitor de seguridad IP para diagnosticar el problema.|  
|5483|N/D|Media|Error de los servicios IPsec al inicializar el servidor RPC. No se pudieron iniciar los servicios IPsec.|  
|5484|N/D|Media|Los servicios IPsec experimentaron un error crítico y se cerraron. El cierre de los servicios IPsec puede suponer un mayor riesgo de ataque a la red para el equipo o exponerlo a posibles riesgos de seguridad.|  
|5485|N/D|Media|Error de los servicios IPsec al procesar algunos filtros IPsec en un evento de Plug and Play para interfaces de red. Esto supone un posible riesgo de seguridad porque puede que algunas interfaces de red no obtengan la protección proporcionada por los filtros IPsec aplicados. Usa el complemento Monitor de seguridad IP para diagnosticar el problema.|  
|6145|N/D|Media|Se produjeron uno o más errores al procesar la Directiva de seguridad en los objetos de directiva de grupo.|  
|6273|N/D|Media|El Servidor de directivas de redes denegó el acceso a un usuario.|  
|6274|N/D|Media|El Servidor de directivas de redes rechazó la solicitud de un usuario.|  
|6275|N/D|Media|El Servidor de directivas de redes rechazó la solicitud de cuentas de un usuario.|  
|6276|N/D|Media|El Servidor de directivas de redes puso en cuarentena un usuario.|  
|6277|N/D|Media|El Servidor de directivas de redes concedió acceso a un usuario, pero lo sometió a un período de prueba porque el host no cumplía la directiva de mantenimiento definida.|  
|6278|N/D|Media|El Servidor de directivas de redes concedió acceso total a un usuario porque el host cumplía la directiva de mantenimiento definida.|  
|6279|N/D|Media|El Servidor de directivas de redes bloqueó la cuenta de usuario debido a varios intentos de autenticación erróneos.|  
|6280|N/D|Media|El Servidor de directivas de redes desbloqueó la cuenta de usuario.|  
|-|640|Media|Base de datos de cuentas generales modificada|  
|-|619|Media|Directiva de calidad de servicio modificada|  
|24586|N/D|Media|Se detectó un error al convertir el volumen|  
|24592|N/D|Media|Error al intentar reiniciar automáticamente la conversión en el volumen %2.|  
|24593|N/D|Media|Escritura de metadatos: el volumen %2 devuelve errores al intentar modificar los metadatos. Si continúan los errores, descifre el volumen|  
|24594|N/D|Media|Regeneración de metadatos: error al intentar escribir una copia de metadatos en el volumen %2 y puede aparecer como daños en el disco. Si los errores continúan, descifre el volumen.|  
|4608|512|Bajo|Windows se está iniciando.|  
|4609|513|Bajo|Windows se está apagando.|  
|4610|514|Bajo|La Autoridad de seguridad local ha cargado un paquete de autenticación.|  
|4611|515|Bajo|Se registró un proceso de inicio de sesión de confianza con la Autoridad de seguridad local.|  
|4612|516|Bajo|Los recursos internos asignados para la cola de mensajes de auditoría se han agotado, provocando la pérdida de algunas auditorías.|  
|4614|518|Bajo|El Administrador de cuentas de seguridad cargó un paquete de notificación.|  
|4615|519|Bajo|Uso no válido de puerto LPC.|  
|4616|520|Bajo|Se cambió la hora del sistema.|  
|4622|N/D|Bajo|La Autoridad de seguridad local cargó un paquete de seguridad.|  
|4624|528.540|Bajo|Se inició sesión correctamente en una cuenta.|  
|4625|529-537539|Bajo|No se pudo iniciar sesión en una cuenta.|  
|4634|538|Bajo|Se cerró sesión en una cuenta.|  
|4646|N/D|Bajo|Se inició el modo de prevención DoS de IKE.|  
|4647|551|Bajo|Cierre de sesión iniciada por el usuario.|  
|4648|552|Bajo|Se intentó iniciar sesión con credenciales explícitas.|  
|4650|N/D|Bajo|Se estableció una asociación de seguridad de modo principal de IPsec. No se habilitó el modo extendido. No se usó la autenticación de certificado.|  
|4651|N/D|Bajo|Se estableció una asociación de seguridad de modo principal de IPsec. No se habilitó el modo extendido. Se usó un certificado para la autenticación.|  
|4652|N/D|Bajo|Error de negociación de modo principal de IPsec.|  
|4653|N/D|Bajo|Error de negociación de modo principal de IPsec.|  
|4654|N/D|Bajo|Error de negociación de modo rápido de IPsec.|  
|4655|N/D|Bajo|Finalizó una asociación de seguridad de modo principal de IPsec.|  
|4656|560|Bajo|Se solicitó un identificador para un objeto.|  
|4657|567|Bajo|Se modificó un valor del Registro.|  
|4658|562|Bajo|Se cerró un identificador para un objeto.|  
|4659|N/D|Bajo|Se solicitó un identificador para un objeto con intención de eliminarlo.|  
|4660|564|Bajo|Se eliminó un objeto.|  
|4661|565|Bajo|Se solicitó un identificador para un objeto.|  
|4662|566|Bajo|Se realizó una operación en un objeto|  
|4663|567|Bajo|Se intentó tener acceso a un objeto.|  
|4664|N/D|Bajo|Se intentó crear un vínculo físico|  
|4665|N/D|Bajo|Se intentó crear un contexto de cliente de aplicación|  
|4666|N/D|Bajo|Una aplicación intentó una operación|  
|4667|N/D|Bajo|Se eliminó un contexto de cliente de aplicación|  
|4668|N/D|Bajo|Se inicializó una aplicación|  
|4670|N/D|Bajo|Se cambiaron los permisos de un objeto.|  
|4671|N/D|Bajo|Una aplicación intentó tener acceso a un ordinal bloqueado a través de TBS.|  
|4672|576|Bajo|Privilegios especiales asignados al nuevo inicio de sesión.|  
|4673|577|Bajo|Se llamó a un servicio con privilegios.|  
|4674|578|Bajo|Se intentó una operación en un objeto con privilegios.|  
|4688|592|Bajo|Se ha creado un nuevo proceso.|  
|4689|593|Bajo|Un proceso ha terminado.|  
|4690|594|Bajo|Se intentó duplicar un identificador en un objeto.|  
|4691|595|Bajo|Se solicitó acceso indirecto a un objeto.|  
|4694|N/D|Bajo|Se intentó proteger los datos protegidos que se pueden auditar|  
|4695|N/D|Bajo|Se intentó desproteger los datos protegidos que se pueden auditar|  
|4696|600|Bajo|Se asignó un token primario al proceso.|  
|4697|601|Bajo|Intentar instalar un servicio|  
|4698|602|Bajo|Se creó una tarea programada.|  
|4699|602|Bajo|Se eliminó una tarea programada.|  
|4700|602|Bajo|Se habilitó una tarea programada.|  
|4701|602|Bajo|Se deshabilitó una tarea programada.|  
|4702|602|Bajo|Se actualizó una tarea programada.|  
|4704|608|Bajo|Se asignó un derecho de usuario.|  
|4705|609|Bajo|Se quitó un derecho de usuario.|  
|4707|611|Bajo|Se quitó una confianza de un dominio|  
|4709|N/D|Bajo|Se iniciaron los Servicios IPsec.|  
|4710|N/D|Bajo|Se deshabilitaron los Servicios IPsec.|  
|4711|N/D|Bajo|Puede contener cualquiera de los siguientes elementos: el motor de la tienda en la que se ha aplicado la copia almacenada localmente en caché de Active Directory directiva IPsec de almacenamiento en el equipo. El motor PAStore aplicó la directiva IPsec de almacenamiento de Active Directory en el equipo. El motor PAStore aplicó una directiva IPsec de almacenamiento del Registro local en el equipo. Error del motor PAStore al aplicar una copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo. Error del motor PAStore al aplicar la directiva IPsec de almacenamiento de Active Directory en el equipo. Error del motor PAStore al aplicar una directiva IPsec de almacenamiento del Registro local en el equipo. Error del motor PAStore al aplicar algunas reglas de la directiva IPsec activa en el equipo. Error del motor PAStore al cargar la directiva IPsec de almacenamiento de directorio en el equipo. El motor PAStore cargó la directiva IPsec de almacenamiento de directorio en el equipo. Error del motor PAStore al cargar la directiva IPsec de almacenamiento local en el equipo. Motor de la restauración cargó la directiva IPsec de almacenamiento local en el equipo. El motor de la Pastore sondeó los cambios en la Directiva de IPsec activa y no detectó ningún cambio. |  
|4712|N/D|Bajo|Los Servicios IPsec detectaron un error que puede ser importante.|  
|4717|621|Bajo|Se concedió acceso de seguridad de sistema a una cuenta|  
|4718|622|Bajo|Se quitó acceso de seguridad de sistema de una cuenta|  
|4720|624|Bajo|Se creó una cuenta de usuario.|  
|4722|626|Bajo|Se habilitó una cuenta de usuario.|  
|4723|627|Bajo|Se ha realizado un intento de cambiar la contraseña de una cuenta.|  
|4725|629|Bajo|Se deshabilitó una cuenta de usuario.|  
|4726|630|Bajo|Se eliminó una cuenta de usuario.|  
|4728|632|Bajo|Se agregó un miembro a un grupo global con seguridad habilitada.|  
|4729|633|Bajo|Se quitó un miembro de un grupo global con seguridad habilitada.|  
|4730|634|Bajo|Se eliminó un grupo global con seguridad habilitada.|  
|4731|635|Bajo|Se creó un grupo local con seguridad habilitada.|  
|4732|636|Bajo|Se agregó un miembro a un grupo local con seguridad habilitada.|  
|4733|637|Bajo|Se quitó un miembro de un grupo local con seguridad habilitada.|  
|4734|638|Bajo|Se eliminó un grupo local con seguridad habilitada.|  
|4738|642|Bajo|Se cambió una cuenta de usuario.|  
|4740|644|Bajo|Se bloqueó una cuenta de usuario.|  
|4741|645|Bajo|Se cambió una cuenta de equipo.|  
|4742|646|Bajo|Se cambió una cuenta de equipo.|  
|4743|647|Bajo|Se eliminó una cuenta de equipo|  
|4744|648|Bajo|Se creó un grupo local deshabilitado para seguridad|  
|4745|649|Bajo|Se cambió un grupo local deshabilitado para seguridad|  
|4746|650|Bajo|Se agregó un miembro a un grupo local deshabilitado para seguridad|  
|4747|651|Bajo|Se quitó un miembro de un grupo local deshabilitado para seguridad|  
|4748|652|Bajo|Se eliminó un grupo local con la seguridad deshabilitada.|  
|4749|653|Bajo|Se creó un grupo global con la seguridad deshabilitada.|  
|4750|654|Bajo|Se cambió un grupo global con la seguridad deshabilitada.|  
|4751|655|Bajo|Se agregó un miembro a un grupo global con la seguridad deshabilitada.|  
|4752|656|Bajo|Se quitó un miembro de un grupo global con la seguridad deshabilitada.|  
|4753|657|Bajo|Se eliminó un grupo global con la seguridad deshabilitada.|  
|4756|660|Bajo|Se agregó un miembro a un grupo universal con la seguridad habilitada.|  
|4757|661|Bajo|Se quitó un miembro de un grupo universal con la seguridad habilitada.|  
|4758|662|Bajo|Se eliminó un grupo universal con la seguridad habilitada.|  
|4759|663|Bajo|Se creó un grupo universal con la seguridad deshabilitada.|  
|4760|664|Bajo|Se cambió un grupo universal con la seguridad deshabilitada.|  
|4761|665|Bajo|Se agregó un miembro a un grupo universal con la seguridad deshabilitada.|  
|4762|666|Bajo|Se quitó un miembro de un grupo universal con la seguridad deshabilitada.|  
|4767|671|Bajo|Se desbloqueó una cuenta de usuario.|  
|4768|672.676|Bajo|Se solicitó un vale de autenticación (TGT) de Kerberos.|  
|4769|673|Bajo|Se solicitó un vale de servicio Kerberos.|  
|4770|674|Bajo|Se renovó un vale de servicio Kerberos.|  
|4771|675|Bajo|Error de autenticación previa de Kerberos.|  
|4772|672|Bajo|Error de solicitud de vale de autenticación de Kerberos.|  
|4774|678|Bajo|Se asignó una cuenta para inicio de sesión|  
|4775|679|Bajo|No se pudo asignar una cuenta para inicio de sesión|  
|4776|680.681|Bajo|El controlador de dominio intentó validar las credenciales para una cuenta|  
|4777|N/D|Bajo|Error del controlador de dominio al validar las credenciales para una cuenta|  
|4778|682|Bajo|Se reconectó una sesión a una estación de ventana.|  
|4779|683|Bajo|Se desconectó una sesión de una estación de ventana.|  
|4781|685|Bajo|Se cambió el nombre de una cuenta:|  
|4782|N/D|Bajo|El hash de contraseña al que se obtuvo acceso a una cuenta.|  
|4783|667|Bajo|Se creó un grupo de aplicaciones básicas|  
|4784|N/D|Bajo|Se cambió un grupo de aplicaciones básicas|  
|4785|689|Bajo|Se agregó un miembro a un grupo de aplicaciones básicas|  
|4786|690|Bajo|Se quitó un miembro de un grupo de aplicaciones básicas|  
|4787|691|Bajo|Se ha agregado un miembro a un grupo de aplicación básico.|  
|4788|692|Bajo|Se quitó un miembro de un grupo de aplicación básico.|  
|4789|693|Bajo|Se eliminó un grupo de aplicaciones básicas|  
|4790|694|Bajo|Se creó un grupo de consultas LDAP|  
|4793|N/D|Bajo|Se ha llamado a la API de comprobación de directiva de contraseñas|  
|4800|N/D|Bajo|Se bloqueó la estación de trabajo.|  
|4801|N/D|Bajo|Se desbloqueó la estación de trabajo.|  
|4802|N/D|Bajo|Se invocó el protector de pantalla.|  
|4803|N/D|Bajo|Se descartó el protector de pantalla.|  
|4864|N/D|Bajo|Se detectó una colisión de espacio de nombres|  
|4869|773|Bajo|Servicios de servidor de certificados recibieron una solicitud de certificado reenviada.|  
|4871|775|Bajo|Servicios de servidor de certificados recibió una solicitud para publicar la lista de revocación de certificados (CRL).|  
|4872|776|Bajo|Servicios de servidor de certificados publicó la lista de revocación de certificados (CRL).|  
|4873|777|Bajo|Se cambió una extensión de solicitud de certificado.|  
|4874|778|Bajo|Se cambiaron uno o varios atributos de solicitud de certificado.|  
|4875|779|Bajo|Servicios de servidor de certificados recibió una solicitud para cerrar.|  
|4876|780|Bajo|La copia de seguridad de Servicios de servidor de certificados se ha iniciado.|  
|4877|781|Bajo|Se ha completado la copia de seguridad de Servicios de servidor certificados.|  
|4878|782|Bajo|Se inició la restauración de Servicios de servidor de certificados.|  
|4879|783|Bajo|Se ha completado la restauración de Servicios de servidor de certificados.|  
|4880|784|Bajo|Servicios de servidor de certificados iniciados.|  
|4881|785|Bajo|Se detuvo Servicios de servidor de certificados.|  
|4883|787|Bajo|Servicios de servidor de certificados recuperó una clave archivada.|  
|4884|788|Bajo|Servicios de servidor de certificados importó un certificado en su base de datos.|  
|4886|790|Bajo|Servicios de servidor de certificados recibió una solicitud de certificado.|  
|4887|791|Bajo|Servicios de servidor de certificados aprobó una solicitud de certificado y emitió un certificado.|  
|4888|792|Bajo|Servicios de servidor de certificados denegó una solicitud de certificado.|  
|4889|793|Bajo|Servicios de servidor de certificados estableció el estado de una solicitud de certificado en pendiente.|  
|4891|795|Bajo|Se cambió una entrada de configuración en Servicios de servidor de certificados.|  
|4893|797|Bajo|Servicios de servidor de certificados archivó una clave.|  
|4894|798|Bajo|Servicios de servidor de certificados importó y archivó una clave.|  
|4895|799|Bajo|Servicios de servidor de certificados publicó el certificado de CA en Servicios de dominio de Active Directory|  
|4898|802|Bajo|Servicios de servidor de certificados cargó una plantilla.|  
|4902|N/D|Bajo|Se creó la tabla de directiva de auditoría por usuario|  
|4904|N/D|Bajo|Se intentó registrar un origen de evento de seguridad|  
|4905|N/D|Bajo|Se intentó anular el registro de un origen de evento de seguridad|  
|4909|N/D|Bajo|Se cambió la configuración de directiva local para TBS.|  
|4910|N/D|Bajo|Se ha cambiado la configuración de directiva de grupo para TBS.|  
|4928|N/D|Bajo|Se estableció un contexto de nomenclatura de origen de réplica de Active Directory|  
|4929|N/D|Bajo|Se quitó un contexto de nomenclatura de origen de réplica de Active Directory|  
|4930|N/D|Bajo|Se modificó un contexto de nomenclatura de origen de réplica de Active Directory|  
|4931|N/D|Bajo|Se modificó un contexto de nomenclatura de destino de réplica de Active Directory|  
|4932|N/D|Bajo|Comenzó la sincronización de una réplica de un contexto de nomenclatura de Active Directory|  
|4933|N/D|Bajo|Finalizó la sincronización de una réplica de un contexto de nomenclatura de Active Directory|  
|4934|N/D|Bajo|Se replicaron los atributos de un objeto de Active Directory|  
|4935|N/D|Bajo|Empieza el error de replicación|  
|4936|N/D|Bajo|Finaliza el error de replicación|  
|4937|N/D|Bajo|Se quitó un objeto persistente de una réplica|  
|4944|N/D|Bajo|La siguiente directiva estaba activa cuando se inició el Firewall de Windows.|  
|4945|N/D|Bajo|Se mostró una regla al iniciarse el Firewall de Windows.|  
|4946|N/D|Bajo|Se ha realizado un cambio en la lista de excepciones del Firewall de Windows. Se agregó una regla.|  
|4947|N/D|Bajo|Se ha realizado un cambio en la lista de excepciones del Firewall de Windows. Se modificó una regla.|  
|4948|N/D|Bajo|Se ha realizado un cambio en la lista de excepciones del Firewall de Windows. Se eliminó una regla.|  
|4949|N/D|Bajo|Los valores de configuración del Firewall de Windows se restauraron a los valores predeterminados.|  
|4950|N/D|Bajo|Se cambió una configuración del Firewall de Windows.|  
|4951|N/D|Bajo|Se ha omitido una regla porque el Firewall de Windows no reconoció su número de versión principal.|  
|4952|N/D|Bajo|Se han omitido partes de una regla porque el Firewall de Windows no reconoció su número de versión secundaria. Se aplicarán las demás partes de la regla.|  
|4953|N/D|Bajo|Firewall de Windows omitió una regla porque no la pudo analizar.|  
|4954|N/D|Bajo|La configuración de directiva de grupo de Firewall de Windows ha cambiado. Se aplicó la nueva configuración.|  
|4956|N/D|Bajo|Firewall de Windows ha cambiado el perfil activo.|  
|4957|N/D|Bajo|Firewall de Windows no aplicó la siguiente regla:|  
|4958|N/D|Bajo|Firewall de Windows no aplicó la siguiente regla porque hacía referencia a elementos que no están configurados en este equipo:|  
|4979|N/D|Bajo|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4980|N/D|Bajo|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4981|N/D|Bajo|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4982|N/D|Bajo|Se establecieron asociaciones de seguridad de modo extendido y modo principal de IPsec.|  
|4985|N/D|Bajo|Se cambió el estado de una transacción|  
|5024|N/D|Bajo|El servicio de Firewall de Windows se ha iniciado correctamente.|  
|5025|N/D|Bajo|El servicio de Firewall de Windows se ha detenido.|  
|5031|N/D|Bajo|El servicio Firewall de Windows impidió que una aplicación aceptara conexiones entrantes en la red|  
|5032|N/D|Bajo|Firewall de Windows no pudo notificar al usuario que impidió que una aplicación aceptara conexiones entrantes en la red.|  
|5033|N/D|Bajo|El controlador de Firewall de Windows se inició correctamente.|  
|5034|N/D|Bajo|Se detuvo el controlador de Firewall de Windows.|  
|5039|N/D|Bajo|Se virtualizó una clave del Registro.|  
|5040|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se agregó un conjunto de autenticación.|  
|5041|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se modificó un conjunto de autenticación.|  
|5042|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se eliminó un conjunto de autenticación.|  
|5043|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se agregó una regla de seguridad de conexión.|  
|5044|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se modificó una regla de seguridad de conexión.|  
|5045|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se eliminó una regla de seguridad de conexión.|  
|5046|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se agregó un conjunto criptográfico.|  
|5047|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se ha modificado un conjunto criptográfico.|  
|5048|N/D|Bajo|Se realizó un cambio en la configuración de IPsec. Se ha eliminado un conjunto criptográfico.|  
|5050|N/D|Bajo|Intento de deshabilitar mediante programación el Firewall de Windows mediante una llamada a Inetfwprofile y. FirewallEnabled (false)|  
|5051|N/D|Bajo|Se virtualizó un archivo|  
|5056|N/D|Bajo|Se realizó una prueba Self criptográfica.|  
|5057|N/D|Bajo|Error de operación primitiva criptográfica.|  
|5058|N/D|Bajo|Operación de archivo de clave.|  
|5059|N/D|Bajo|Operación de migración de clave.|  
|5060|N/D|Bajo|Error en la operación de comprobación.|  
|5061|N/D|Bajo|Operación criptográfica.|  
|5062|N/D|Bajo|Se realizó una prueba automática criptográfica de modo kernel.|  
|5063|N/D|Bajo|Se intentó una operación de proveedor criptográfico.|  
|5064|N/D|Bajo|Se intentó una operación de contexto criptográfico.|  
|5065|N/D|Bajo|Se intentó modificar un contexto criptográfico.|  
|5066|N/D|Bajo|Se intentó una operación de función criptográfica.|  
|5067|N/D|Bajo|Se intentó modificar una función criptográfica.|  
|5068|N/D|Bajo|Se intentó una operación de proveedor de función criptográfica.|  
|5069|N/D|Bajo|Se intentó una operación de propiedad de función criptográfica.|  
|5070|N/D|Bajo|Se intentó modificar una propiedad de función criptográfica.|  
|5125|N/D|Bajo|Se envió una solicitud al servicio de respuesta de OCSP|  
|5126|N/D|Bajo|El servicio de respuesta de OCSP actualizó automáticamente el certificado de firma|  
|5127|N/D|Bajo|El proveedor de revocación de OCSP actualizó correctamente la información de revocación|  
|5136|566|Bajo|Se modificó un objeto de servicio de directorio|  
|5137|566|Bajo|Se creó un objeto de servicio de directorio|  
|5138|N/D|Bajo|Se recuperó un objeto de servicio de directorio|  
|5139|N/D|Bajo|Se movió un objeto de servicio de directorio|  
|5140|N/D|Bajo|Se tuvo acceso a un objeto de recurso compartido de red|  
|5141|N/D|Bajo|Se eliminó un objeto de servicio de directorio|  
|5152|N/D|Bajo|La Plataforma de filtrado de Windows bloqueó un paquete|  
|5153|N/D|Bajo|Un filtro más restrictivo de la Plataforma de filtrado de Windows bloqueó un paquete|  
|5154|N/D|Bajo|La Plataforma de filtrado de Windows permitió que una aplicación o un servicio escuche conexiones entrantes en un puerto|  
|5155|N/D|Bajo|La Plataforma de filtrado de Windows bloqueó una aplicación o un servicio para que no escuchara conexiones entrantes en un puerto|  
|5156|N/D|Bajo|La Plataforma de filtrado de Windows permitió una conexión|  
|5157|N/D|Bajo|La Plataforma de filtrado de Windows bloqueó una conexión|  
|5158|N/D|Bajo|La Plataforma de filtrado de Windows permitió un enlace con un puerto local|  
|5159|N/D|Bajo|La Plataforma de filtrado de Windows bloqueó un enlace con un puerto local|  
|5378|N/D|Bajo|Una directiva no permitió la delegación de credenciales solicitada.|  
|5440|N/D|Bajo|La siguiente llamada estaba presente cuando se inició el Motor de filtro de base de la Plataforma de filtrado de Windows.|  
|5441|N/D|Bajo|El siguiente filtro estaba presente cuando se inició el Motor de filtro de base de la Plataforma de filtrado de Windows.|  
|5442|N/D|Bajo|El siguiente proveedor estaba presente cuando se inició el Motor de filtro de base de la Plataforma de filtrado de Windows.|  
|5443|N/D|Bajo|El siguiente contexto de proveedor estaba presente cuando se inició el Motor de filtro de base de la Plataforma de filtrado de Windows.|  
|5444|N/D|Bajo|La siguiente subcapa estaba presente cuando se inició el motor de filtrado de base de la plataforma de filtrado de Windows.|  
|5446|N/D|Bajo|Se cambió una llamada de la Plataforma de filtrado de Windows.|  
|5447|N/D|Bajo|Se cambió un filtro de la Plataforma de filtrado de Windows.|  
|5448|N/D|Bajo|Se cambió un proveedor de la Plataforma de filtrado de Windows.|  
|5449|N/D|Bajo|Se cambió un contexto de proveedor de la Plataforma de filtrado de Windows.|  
|5450|N/D|Bajo|Se ha cambiado una subcapa de la plataforma de filtrado de Windows.|  
|5451|N/D|Bajo|Se estableció una asociación de seguridad de modo rápido de IPsec.|  
|5452|N/D|Bajo|Finalizó una asociación de seguridad de modo rápido de IPsec.|  
|5456|N/D|Bajo|El motor PAStore aplicó la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5457|N/D|Bajo|Error del motor PAStore al aplicar la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5458|N/D|Bajo|El motor PAStore aplicó una copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5459|N/D|Bajo|Error del motor PAStore al aplicar una copia almacenada en caché localmente de la directiva IPsec de almacenamiento de Active Directory en el equipo.|  
|5460|N/D|Bajo|El motor PAStore aplicó una directiva IPsec de almacenamiento del Registro local en el equipo.|  
|5461|N/D|Bajo|Error del motor PAStore al aplicar una directiva IPsec de almacenamiento del Registro local en el equipo.|  
|5462|N/D|Bajo|Error del motor PAStore al aplicar algunas reglas de la directiva IPsec activa en el equipo. Usa el complemento Monitor de seguridad IP para diagnosticar el problema.|  
|5463|N/D|Bajo|El motor PAStore sondeó en busca de cambios en la directiva IPsec activa y no detectó ningún cambio.|  
|5464|N/D|Bajo|El motor PAStore sondeó en busca de cambios en la directiva IPsec activa, detectó cambios y los aplicó a los servicios IPsec.|  
|5465|N/D|Bajo|El motor PAStore recibió un control para la recarga forzada de la directiva IPsec y procesó el control correctamente.|  
|5466|N/D|Bajo|El motor PAStore sondeó en busca de cambios en la directiva IPsec de Active Directory, determinó que no se podía establecer contacto con Active Directory y usará la copia almacenada en caché de la directiva IPsec de Active Directory en su lugar. No se pudieron aplicar los cambios realizados en la directiva IPsec de Active Directory desde el último sondeo.|  
|5467|N/D|Bajo|El motor PAStore sondeó en busca de cambios en la directiva IPsec de Active Directory, determinó que se podía establecer contacto con Active Directory y no encontró ningún cambio en la directiva Ya no se usa la copia almacenada en caché de la directiva IPsec de Active Directory.|  
|5468|N/D|Bajo|El motor PAStore sondeó en busca de cambios en la directiva IPsec de Active Directory, determinó que se podía establecer contacto con Active Directory, encontró cambios en la directiva y los aplicó. Ya no se usa la copia almacenada en caché de la directiva IPsec de Active Directory.|  
|5471|N/D|Bajo|El motor PAStore cargó la directiva IPsec de almacenamiento local en el equipo.|  
|5472|N/D|Bajo|Error del motor PAStore al cargar la directiva IPsec de almacenamiento local en el equipo.|  
|5473|N/D|Bajo|El motor PAStore cargó la directiva IPsec de almacenamiento de directorio en el equipo.|  
|5474|N/D|Bajo|Error del motor PAStore al cargar la directiva IPsec de almacenamiento de directorio en el equipo.|  
|5477|N/D|Bajo|Error del motor PAStore al agregar el filtro de modo rápido.|  
|5479|N/D|Bajo|Los servicios IPsec se cerraron correctamente. El cierre de los servicios IPsec puede suponer un mayor riesgo de ataque a la red para el equipo o exponerlo a posibles riesgos de seguridad.|  
|5632|N/D|Bajo|Se realizó una solicitud de autenticación en una red inalámbrica.|  
|5633|N/D|Bajo|Se realizó una solicitud de autenticación en una red con cable.|  
|5712|N/D|Bajo|Se intentó una llamada a procedimiento remoto (RPC).|  
|5888|N/D|Bajo|Se modificó un objeto del catálogo COM+.|  
|5889|N/D|Bajo|Se eliminó un objeto del catálogo COM+.|  
|5890|N/D|Bajo|Se agregó un objeto al catálogo COM +.|  
|6008|N/D|Bajo|No se esperaba el cierre del sistema anterior|  
|6144|N/D|Bajo|La Directiva de seguridad de los objetos directiva de grupo se ha aplicado correctamente.|  
|6272|N/D|Bajo|El Servidor de directivas de redes concedió acceso a un usuario.|  
|N/D|561|Bajo|Se solicitó un identificador para un objeto.|  
|N/D|563|Bajo|Objeto abierto para eliminación|  
|N/D|625|Bajo|Tipo de cuenta de usuario cambiado|  
|N/D|613|Bajo|Agente de directivas IPsec iniciado|  
|N/D|614|Bajo|Agente de directivas IPsec deshabilitado|  
|N/D|615|Bajo|Agente de directivas IPsec|  
|N/D|616|Bajo|El agente de directivas IPsec encontró un posible error grave|  
|24577|N/D|Bajo|Se inició el cifrado del volumen|  
|24578|N/D|Bajo|Se detuvo el cifrado del volumen|  
|24579|N/D|Bajo|Cifrado del volumen completado|  
|24580|N/D|Bajo|Se inició el descifrado del volumen|  
|24581|N/D|Bajo|Descifrado del volumen detenido|  
|24582|N/D|Bajo|Descifrado del volumen completado|  
|24583|N/D|Bajo|Subproceso de trabajo de conversión para el volumen iniciado|  
|24584|N/D|Bajo|Subproceso de trabajo de conversión para el volumen detenido temporalmente|  
|24588|N/D|Bajo|La operación de conversión en el volumen %2 encontró un error de sector incorrecto. Valide los datos de este volumen.|  
|24595|N/D|Bajo|El volumen %2 contiene clústeres no válidos. Estos clústeres se omitirán durante la conversión.|  
|24621|N/D|Bajo|Comprobación de estado inicial: transacción de conversión de volumen gradual en %2.|  
|5049|N/D|Bajo|Se eliminó una asociación de seguridad de IPsec.|  
|5478|N/D|Bajo|Los servicios IPsec se iniciaron correctamente.|  
  
> [!NOTE]  
> Consulte [los eventos de auditoría de seguridad de Windows](https://www.microsoft.com/download/details.aspx?id=50034) para obtener una lista de muchos identificadores de eventos de seguridad y su significado.  
>
> Ejecute **wevtutil GP Microsoft-Windows-Security-Auditing/GE/GM: true** para obtener una lista detallada de todos los identificadores de eventos de seguridad.  
  
Para obtener más información sobre los identificadores de eventos de seguridad de Windows y su significado, vea el artículo Soporte técnico de Microsoft [Descripción de los eventos de seguridad en Windows 7 y en Windows Server 2008 R2](https://support.microsoft.com/kb/977519). También puede descargar [eventos de auditoría de seguridad para los detalles de eventos de seguridad de Windows 7 y Windows server 2008 R2](https://www.microsoft.com/download/details.aspx?id=21561) y Windows [8 y Windows Server 2012](https://www.microsoft.com/download/details.aspx?id=35753), que proporcionan información detallada de eventos para los sistemas operativos a los que se hace referencia en formato de hoja de cálculo.  
