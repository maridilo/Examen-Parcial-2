# Examen-Parcial-2
https://github.com/maridilo/Examen-Parcial-2.git

### Misión 1: Reconexión en la Base Eco (Hoth) – Direccionamiento IP y Subredes

Para dividir eficientemente el bloque **172.16.0.0/24** en cinco subredes, aplicamos VLSM según las necesidades de cada sector:

| Subred                         | Notación        | Máscara                | Hosts útiles | Rango de hosts                   | Broadcast      |
|--------------------------------|-----------------|------------------------|--------------|----------------------------------|----------------|
| **Comando Central** (≈50 hosts)| `172.16.0.0/26` | `255.255.255.192` (/26) | 62           | `172.16.0.1` – `172.16.0.62`     | `172.16.0.63`  |
| **Defensa Perimetral** (≈30)   | `172.16.0.64/27`| `255.255.255.224` (/27) | 30           | `172.16.0.65` – `172.16.0.94`     | `172.16.0.95`  |
| **Centro Médico** (≈20)        | `172.16.0.96/27`| `255.255.255.224` (/27) | 30           | `172.16.0.97` – `172.16.0.126`    | `172.16.0.127` |
| **Hangar y Taller** (≈14)      | `172.16.0.128/28`| `255.255.255.240` (/28) | 14           | `172.16.0.129` – `172.16.0.142`   | `172.16.0.143` |
| **Enlace troncal** (2 hosts)   | `172.16.0.144/30`| `255.255.255.252` (/30) | 2            | `172.16.0.145` – `172.16.0.146`   | `172.16.0.147` |

- **/26** (62 hosts) → Comando Central (≈50 dispositivos)  
- **/27** (30 hosts) → Defensa Perimetral (≈30) y Centro Médico (≈20)  
- **/28** (14 hosts) → Hangar y Taller (≈14)  
- **/30** (2 hosts) → Enlace troncal con la antena principal  

> **Nota:** Queda espacio libre a partir de `172.16.0.148/29` para futuras ampliaciones.

### Misión 2: Sabiduría de Yoda – Algoritmos de Enrutamiento y Rutas

En la red rebelde (o en cualquier red IP), existen dos formas principales de decidir por dónde enviar los paquetes:

#### Enrutamiento Estático
- **Definición**: Las rutas se configuran manualmente en cada router.
- **Ventajas**:
  - Control total sobre el camino de los paquetes.
  - Menor consumo de CPU y memoria (no hay procesos de intercambio de información de rutas).
  - Mayor seguridad (solo existen las rutas que has programado tú).
- **Inconvenientes**:
  - No se adapta automáticamente a fallos: si un enlace o nodo cae, hay que reconfigurar manualmente.
  - Difícil de escalar: en redes grandes (muchos “planetas”), actualizar cada router es muy laborioso.
  - Propenso a errores humanos al introducir o cambiar rutas.
- **Reacción ante caída de un nodo**: la ruta configurada “muere” y no hay ruta alternativa hasta que un administrador la corrija manualmente.

#### Enrutamiento Dinámico
- **Definición**: Los routers intercambian información de alcance de redes y calculan rutas automáticamente.
- **Ventajas**:
  - Adaptabilidad: si un enlace o nodo deja de funcionar, los routers reconvergen y encuentran nuevas rutas sin intervención.
  - Escalabilidad: en redes de muchos nodos o “sistemas estelares”, basta con anunciar las redes; la lógica de enrutamiento se propaga sola.
  - Redundancia: permite múltiples caminos y balanceo de carga.
- **Inconvenientes**:
  - Mayor consumo de CPU, memoria y ancho de banda (proceso de intercambio de tablas o bases de datos de estado de enlace).
  - Posibles bucles temporales y convergencia más lenta en protocolos sencillos.
- **Protocolos de ejemplo**:
  - **RIP** (Routing Information Protocol):  
    - Tipo: vector de distancia.  
    - Métrica: salto (hop count) máximo 15.  
    - Convergencia lenta, susceptible al “count-to-infinity”.
  - **OSPF** (Open Shortest Path First):  
    - Tipo: estado de enlace.  
    - Métrica: coste (bandwidth).  
    - Convergencia rápida, buena escalabilidad, pero exige más memoria y CPU.

#### Vector de Distancia vs Estado de Enlace
| Característica             | Vector de Distancia         | Estado de Enlace            |
|----------------------------|-----------------------------|-----------------------------|
| **Información compartida** | Tablas de distancia a destinos (saltos) | Descripción completa de enlaces y costes |
| **Cálculo de rutas**       | Simple (Bellman-Ford)       | Complejo (Dijkstra)         |
| **Convergencia**           | Más lenta, riesgo bucles    | Más rápida, más estable     |
| **Consumo de recursos**    | Menor CPU/memoria           | Mayor CPU/memoria           |
| **Escalabilidad**          | Limitada (pequeñas redes)   | Excelente para grandes redes|

> **Ejemplo práctico**:  
> - Si el enlace de Endor (nodo) falla, un protocolo OSPF detecta el cambio, recalcula rutas en segundos y la HoloRed se autosana.  
> - Con RIP o rutas estáticas, podrías sufrir períodos de inaccesibilidad o necesitar intervención manual.

Así, como enseña el Maestro Yoda:  
> “Dinámico es el camino de la red que vive; fijo el camino de la red sin alma permanece.”  

### Misión 3: Los Nombres del Holonet – DNS y Resolución de Nombres

En la HoloRed rebelde, el **DNS (Domain Name System)** actúa como un directorio de direcciones: traduce nombres simbólicos (por ejemplo, `echo.base`) a direcciones IP numéricas que usan los routers y hosts para enrutar paquetes.

#### 1. ¿Cómo funciona el DNS?

1. **Cliente DNS (stub resolver)**  
   - Una aplicación (navegador, herramienta de ping/nslookup) solicita la resolución de un nombre al resolver local (normalmente en el sistema operativo).  
2. **Servidor Recursivo**  
   - Si no tiene la respuesta en caché, el servidor DNS recursivo del proveedor o de la Alianza hace consultas a:
     - **Servidores Raíz** (`.`) → le indican los servidores de zona de nivel superior (TLD).  
     - **Servidores TLD** (`.org`, `.rebelion.org`, etc.) → le indican los servidores autoritativos de esa zona.  
     - **Servidor Autoritativo** (p.ej. el DNS rebelde que usted administra) → devuelve el **registro A** con la IP solicitada.  
3. **Respuesta al cliente**  
   - El recursivo cachea la respuesta y la envía al stub resolver, que finalmente se la pasa a la aplicación.

#### 2. Servidores DNS y tipos de registros

- **Servidor DNS**: equipo que almacena uno o varios tipos de registros y responde a consultas.  
- **Registro A**: asocia un nombre de host a una dirección IPv4.  
- Otros registros útiles:  
  - **AAAA**: IPv6  
  - **CNAME**: nombre canónico (alias)  
  - **MX**: servidores de correo  
  - **NS**: delegación de subzonas  

#### 3. Ejemplo práctico

Supongamos que queremos resolver `holonet.rebelion.org`:

| Nombre                   | Tipo | Dirección IP     |
|--------------------------|------|------------------|
| `holonet.rebelion.org`   | A    | `203.0.113.42`   |

1. El PC envía una consulta DNS a su servidor recursivo.  
2. El recursivo, si no la tiene en caché, consulta a los servidores raíz → TLD → autoritativo.  
3. El servidor autoritativo de `rebelion.org` responde con `203.0.113.42`.  
4. El PC recibe la IP y abre la conexión (por ejemplo, SSH, HTTP) a `203.0.113.42`.

#### 4. ¿Qué pasa si el servidor DNS no está disponible?

- **Sin resolución**: los nombres de dominio dejan de traducirse.  
- **Impacto**:  
  - No puedes conectarte por nombre (p. ej. `ssh echo.base` falla).  
  - Debes usar direcciones IP “a ciegas”, lo cual es poco práctico y propenso a errores.  
- **Mitigación**:  
  - Configurar **DNS secundarios** o **caché local** (por ejemplo, un servidor DNS de respaldo en cada base).  
  - Mantener un archivo local (`/etc/hosts`) con entradas críticas de emergencia.

> **Conclusión**: el DNS es esencial para la escalabilidad y usabilidad de cualquier red TCP/IP (o HoloRed). Un fallo en DNS deja a los rebeldes “a ciegas” ante sus propios aliados, igual que si pierden contacto con un sistema estelar crucial.  

