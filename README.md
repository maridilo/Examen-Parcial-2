# Examen-Parcial-2
https://github.com/maridilo/Examen-Parcial-2.git

# Examen Final de Redes II: La Restauración de la HoloRed Galáctica

## Introducción

En una galaxia muy, muy lejana, la guerra civil ha dejado la infraestructura de comunicaciones en ruinas. La HoloRed galáctica – la red de comunicaciones casi instantánea de la República, ahora bajo control imperial – se ha visto fragmentada ([starwars.fandom.com](https://starwars.fandom.com)). Los rebeldes luchan por restablecer enlaces seguros entre sistemas estelares aislados. Eres un joven aprendiz de Jedi con talento en ingeniería de redes, reclutado por la Alianza Rebelde para afrontar este desafío. Armado con tu sable de luz y tus conocimientos de TCP/IP, te embarcas en misiones épicas para reconectar la galaxia. Cada pregunta de este examen es una misión en tu bitácora rebelde; deberás aplicar conceptos de redes para cumplir los objetivos y ayudar a la Alianza a restaurar la HoloRed y devolver la esperanza a la galaxia.

## Estructura del Examen

- **Parte I – Misiones de Conocimiento Teórico (60%)**  
  Cinco misiones centradas en teoría de redes: direccionamiento IP, algoritmos de enrutamiento, capa de transporte, DNS y seguridad en comunicaciones.

- **Parte II – Misiones Prácticas con Packet Tracer (40%)**  
  Dos misiones prácticas donde deberás diseñar y configurar redes en Cisco Packet Tracer utilizando tus habilidades en enrutamiento, subredes, DNS, TCP/UDP, cifrado, etc. Se proporcionan escenarios narrativos inmersivos y se requiere entregar las topologías configuradas.

**Respuestas:** Al final del enunciado encontrarás la resolución detallada de las misiones prácticas como guía de referencia. Las misiones teóricas deberán responderse por separado (no están resueltas en este documento).

---

### Misión 1: Reconexión en la Base Eco (Hoth) – Direccionamiento IP y Subredes

**Situación:**  
La Base Eco en Hoth ha quedado incomunicada tras un bombardeo imperial. La General Leia Organa te encomienda diseñar un nuevo esquema de red para restablecer la comunicación interna y con la flota rebelde. Hoth alberga varios departamentos (mando, defensa, médico, hangar) y cada uno necesita su propia subred IP debido a requisitos de seguridad y cantidad de dispositivos. Disponéis de un bloque de direcciones limitado y es crucial usarlo eficientemente.

**Narrativa:**  
Bajo la tormenta helada de Hoth, los generadores de energía zumban mientras instalas nuevos equipos de red. Leia, con su atuendo invernal, te explica:  
> “Necesitamos reorganizar las comunicaciones de la base. Cada sector debe tener su red, y todas deben poder conectarse con la antena principal. El rango de direcciones del que disponemos es limitado, joven padawan. Debes dividir la red sabiamente, como un Jedi dividiría sus esfuerzos.”

**Objetivo de la misión:**  
Diseñar un plan de direccionamiento IP para la Base Eco utilizando subnetting. Partiendo de la red **172.16.0.0/24**, subdivide en subredes adecuadas para cada departamento:

- **Comando Central:** ~50 hosts  
- **Defensa Perimetral:** ~30 hosts  
- **Centro Médico:** ~20 hosts  
- **Hangar y Taller:** ~14 hosts  
- **Enlace troncal:** conexión con la antena principal (2 hosts)

**Pregunta:**  
> ¿Cómo dividirías la red `172.16.0.0/24` en subredes para satisfacer los requisitos anteriores? Indica para cada subred su dirección de red, máscara (/xx), número de hosts útiles, rango de hosts y broadcast. Señala cuál subred va destinada al enlace troncal interplanetario.

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

---

### Misión 2: Sabiduría de Yoda – Algoritmos de Enrutamiento y Rutas

**Situación:**  
En Dagobah, el Maestro Yoda evalúa tu comprensión de cómo circula la información por la galaxia. Las rutas pueden ser estáticas o dinámicas; Yoda quiere conocer tu filosofía de enrutamiento.

**Narrativa:**  
En la neblina de los pantanos de Dagobah, Yoda cierra los ojos y enuncia:  
> “En tus redes, joven padawan, dos caminos existen para los datos: uno fijo y otro en movimiento constante. El primero, estático es – manual, control total brinda; el segundo, dinámico – se adapta y vive la red. Grandes diferencias hay... ¿Comprenderlas tú las puedes?”

**Pregunta:**  
> Compara el enrutamiento estático con el dinámico. ¿Cuáles son las ventajas e inconvenientes de cada enfoque? Menciona al menos un protocolo dinámico (por ejemplo, RIP u OSPF) y explica cómo difieren los protocolos de vector de distancia de los de estado de enlace en rendimiento y complejidad, especialmente ante caídas de nodos o escalado de la red.

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

---

### Misión 3: Los Nombres del Holonet – DNS y Resolución de Nombres

**Situación:**  
La flota rebelde ha interceptado transmisiones imperiales con códigos de planeta y nombres en clave. Para coordinar un contraataque, necesitan entender cómo los nombres de dominio galácticos se traducen en direcciones IP.

**Narrativa:**  
A bordo de la Home One, los técnicos muestran un panel con “Unknown host”. El Almirante Ackbar frunce el ceño:  
> “¡Es una trampa... de nombres! Nuestros sistemas no reconocen los destinos.”  
Mon Mothma añade:  
> “Debemos reconstruir nuestro directorio de comunicaciones. Aprendiz, ¿cómo funciona este Sistema de Nombres de Dominio?”

**Pregunta:**  
> Explica el funcionamiento básico del DNS y su importancia. ¿Cómo resuelve la red TCP/IP un nombre de dominio a una IP? Define servidor DNS y registro A, y da un ejemplo (por ejemplo, `holonet.rebelion.org → 203.0.113.42`). Además, menciona qué ocurre si el servidor DNS no está disponible y cómo afectaría a las comunicaciones.

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

---

### Misión 4: TCP vs UDP en las transmisiones

**Situación:**  
Sobre Endor, algunas transmisiones deben ser rápidas aunque se pierda información (stream de vídeo), otras deben ser íntegros y ordenados (transferencia de planos).

**Narrativa:**  
R2-D2 proyecta diagramas dentro del X-Wing de Luke:  
> “Algunas tramas van rápidas, pero otras llegan seguras.”  
Luke pregunta por qué siente lag en unas y retrasos en otras.

**Pregunta:**  
> Compara TCP y UDP:  
> - ¿Por qué TCP es confiable y orientado a conexión, y qué impacto tiene en rendimiento?  
> - ¿Por qué UDP es no confiable y sin conexión, y cuándo su rapidez es ventajosa?  
> Da ejemplos de situaciones o aplicaciones (por ejemplo, envío de coordenadas en tiempo real con UDP vs transferencia de planos con TCP).

#### 1. Conexión y fiabilidad

- **TCP (Transmission Control Protocol)**  
  - **Orientado a conexión**: establece una sesión con un *handshake* de tres vías antes de enviar datos.  
  - **Fiabilidad**: números de secuencia y reconocimientos (ACK) garantizan que cada segmento llegue y se reensamble en orden.  
  - **Retransmisión**: los segmentos perdidos se detectan (tiempo de espera o ACK faltante) y se reenvían automáticamente.  
  - **Control de flujo**: evita saturar al receptor ajustando la ventana de recepción.  
  - **Control de congestión**: modula la tasa de envío según la congestión de la red.

- **UDP (User Datagram Protocol)**  
  - **Sin conexión**: no requiere establecimiento previo de sesión.  
  - **No fiable**: no hay confirmaciones ni retransmisiones; los datagramas pueden perderse, duplicarse o llegar fuera de orden.  
  - **Sin control de flujo ni congestión**: el emisor envía a máxima velocidad sin ajustar dinámicamente.

#### 2. Overhead y rendimiento

| Protocolo | Cabecera típica | Procesamiento adicional      | Latencia               |
|-----------|-----------------|------------------------------|------------------------|
| **TCP**   | 20 bytes mínimo | Gestión de conexiones, ACK, retransmisión, control de flujo y congestión | Mayor (más procesamiento y handshakes) |
| **UDP**   | 8 bytes         | Mínimo (solo encapsulado)    | Menor (envío directo)  |

- **TCP** añade más retraso por handshakes, confirmaciones y posibles retransmisiones, lo que reduce el rendimiento en aplicaciones sensibles a latencia.  
- **UDP** minimiza retrasos y uso de CPU, ideal cuando se prioriza la velocidad y puede tolerarse pérdida de paquetes.

#### 3. Casos de uso

- **TCP**  
  - Transferencia de archivos grandes (FTP, SFTP)  
  - Carga de páginas web (HTTP/HTTPS)  
  - Correo electrónico (SMTP, IMAP, POP3)  
- **UDP**  
  - Streaming de audio y vídeo en tiempo real (VoIP, IPTV)  
  - Juegos en línea y telemetría de sensores  
  - Protocolos de consulta simple (DNS sobre UDP)

#### 4. Resumen de diferencias clave

| Característica          | TCP                         | UDP                       |
|-------------------------|-----------------------------|---------------------------|
| Conexión                | Sí                          | No                        |
| Fiabilidad              | Entrega garantizada y orden | No garantiza entrega      |
| Retransmisiones         | Sí                          | No                        |
| Control de flujo        | Sí                          | No                        |
| Control de congestión   | Sí                          | No                        |
| Latencia                | Mayor                       | Menor                     |
| Overhead                | Alto                        | Bajo                      |
| Ejemplos                | FTP, HTTP, correo           | VoIP, streaming, DNS      |

---

### Misión 5: Comunicación Segura – Criptografía y Seguridad de la Red

**Situación:**  
El Imperio intercepta mensajes rebeldes. Mon Mothma te encarga diseñar un protocolo cifrado para que los mensajes entre bases no puedan ser leídos por el enemigo.

**Narrativa:**  
En la sala de guerra, Mon Mothma advierte:  
> “Necesitamos que nuestras transmisiones sean indescifrables para el Imperio.”  
C-3PO añade:  
> “Si caemos en manos imperiales, prefiero que apaguen mis circuitos a revelar nuestros mensajes.”

**Pregunta:**  
> Explica la diferencia entre cifrado simétrico y asimétrico:  
> - ¿Cómo funciona cada esquema y qué ventajas ofrece?  
> - Si Leia y Luke comparten una frase clave secreta, ¿qué tipo de cifrado es?  
> - ¿Cómo usaría la Alianza cifrado asimétrico para comunicarse con un nuevo aliado sin clave compartida?  
> Comenta también la importancia de autenticación y no repudio (firmas digitales) y por qué usar SSH en lugar de Telnet al administrar routers.

#### 1. Cifrado Simétrico
- **Definición**: usa una **misma clave secreta** para cifrar y descifrar mensajes.  
- **Funcionamiento**: el emisor cifra el texto plano con la clave compartida; el receptor la usa para volver a texto legible.  
- **Ventajas**:  
  - Muy rápido (algoritmos ligeros).  
  - Bajo consumo de recursos.  
- **Inconvenientes**:  
  - Gestión de claves compleja: cada par de interlocutores necesita compartir y proteger la clave.  
  - Si la clave se filtra, el adversario descifra todo el tráfico.

> **Ejemplo**: Leia y Luke comparten una frase clave secreta (“MayTheForce”) y la usan para cifrar sus holomensajes. Esto es **cifrado simétrico**.

#### 2. Cifrado Asimétrico
- **Definición**: utiliza un **par de claves** (pública y privada) distintas.  
- **Funcionamiento**:  
  1. El emisor cifra con la **clave pública** del receptor (disponible para cualquiera).  
  2. Solo la **clave privada** del receptor (guardada en secreto) puede descifrarlo.  
- **Ventajas**:  
  - No requiere compartir clave secreta de antemano.  
  - Escalable: cada usuario solo mantiene su propio par de claves.  
- **Inconvenientes**:  
  - Más lento (operaciones matemáticas complejas).  
  - Mayor uso de CPU y memoria.

> **Ejemplo**: La Alianza quiere enviar un mensaje cifrado a un nuevo aliado sin clave compartida. Cifran con la **clave pública** de ese aliado; él solo puede descifrarlo con su **clave privada**.

#### 3. Autenticación y No Repudio
- **Autenticación**: garantiza que el mensaje proviene de quien dice ser.  
- **No Repudio**: impide que el emisor niegue haber enviado el mensaje.  
- **Mecanismo**:  
  - Se usan **firmas digitales**:  
    1. El emisor calcula un hash del mensaje y lo cifra con su clave privada.  
    2. El receptor lo comprueba descifrándolo con la clave pública del emisor y comparando el hash con el mensaje recibido.  

#### 4. Protocolos Seguros de Administración Remota
- **SSH vs Telnet**  
  - **Telnet**: transmite credenciales y comandos en texto plano → vulnerable a espionaje e inyección de comandos.  
  - **SSH**: crea un **canal cifrado** (usando asimétrico para el intercambio de clave y luego simétrico para la sesión), protegiendo autenticación y toda la sesión.  

> **Conclusión**:  
> - Para cifrado rápido y de alto rendimiento entre partes que ya comparten una clave, usa **simétrico**.  
> - Para comunicaciones con nuevos interlocutores o para firmar/autenticar mensajes, usa **asimétrico** y firmas digitales.  
> - Al gestionar remotamente routers o servidores, **SSH** (cifrado asimétrico + simétrico) es imprescindible frente a Telnet.  
