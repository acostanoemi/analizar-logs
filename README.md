# analizar-logs

Analizador de logs de seguridad en Python. Lee un log de autenticación (tipo `/var/log/auth.log`) y detecta señales de ataque: intentos de login fallidos, IPs que insisten (fuerza bruta) y a qué horas se concentran.

Complemento del [packet-sniffer](https://github.com/acostanoemi/packet-sniffer): el sniffer mira la red en vivo, este mira el registro histórico del sistema.

## Para qué sirve

Los logs tienen miles de líneas y a ojo no se ve nada. Este script los resume y marca lo sospechoso: es parte del trabajo diario de un analista de seguridad (revisar logs en busca de intrusiones). Detecta el patrón clásico de un ataque de fuerza bruta: una IP que falla el login muchas veces seguidas, probando usuarios comunes (`root`, `admin`), casi siempre de madrugada.

## Qué detecta

- Total de logins fallidos vs exitosos.
- Top de IPs con más fallos (candidatas a fuerza bruta), marcando las que superan un umbral.
- Usuarios más apuntados (si atacan `root`/`admin`, suele ser un bot).
- Fallos por hora, con un gráfico de barras para ver picos.

## Requisitos

Solo Python 3.

## Uso

```bash
python3 analizar_logs.py                      # lee /var/log/auth.log
python3 analizar_logs.py /ruta/a/otro.log     # otro archivo
python3 analizar_logs.py --umbral 10          # alerta IPs con 10+ fallos
```

Tu `/var/log/auth.log` puede necesitar sudo para leerse. Si no tenés acceso, pasale cualquier otro archivo de log.

## Uso ético

Analizá tus propios logs o de sistemas que administres.

## Roadmap

- [x] v1: detectar logins fallidos, IPs sospechosas y picos horarios
- [ ] v2: soportar más formatos de log (nginx, apache)
- [ ] v3: generar un reporte a archivo con fecha
- [ ] v4: avisar por notificación cuando detecta un ataque en curso
