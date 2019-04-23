Hay muchas maneras de configurar la resolución de nombre para el dominio de fabric. Es una forma sencilla configurar una zona de reenviador condicional de DNS para el tejido. Para configurar esta zona, ejecute los siguientes comandos en una consola de Windows PowerShell con privilegios elevados en un servidor DNS de fabric. Sustituya los nombres y direcciones en la sintaxis de Windows PowerShell a continuación según sea necesario para su entorno. Agregue los servidores maestros de los nodos adicionales de HGS.

```
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers <IP addresses of HGS server>
```

<!-- Appears in guarded-fabric-configuring-fabric-dns-ad.md and guarded-fabric-configuring-fabric-dns.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->    