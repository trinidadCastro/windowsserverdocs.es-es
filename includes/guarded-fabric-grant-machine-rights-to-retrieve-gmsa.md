1. Tiene un administrador de servicios de directorio de agregar la cuenta de equipo para el nuevo nodo al grupo de seguridad que contiene todos los servidores HGS está autorizada para permitir que esos servidores usar la cuenta de gMSA HGS.

2. Reinicie el nodo nuevo para obtener un nuevo vale de Kerberos que incluye la pertenencia del equipo en ese grupo de seguridad. Cuando se complete el reinicio, inicie sesión con una identidad de dominio que pertenezca al grupo de administradores local en el equipo.

3. Instalar la cuenta de servicio administrada de grupo HGS en el nodo.

   ```powershell
   Install-ADServiceAccount -Identity <HGSgMSAAccount>
   ```
