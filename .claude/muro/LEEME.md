# Muro de contención

Protege la cuenta de Claude de las puertas por las que podrían colarse en ella: conectores, skills y plugins. Funciona en cada sesión de Claude Code que se abre en este repo, también en las automáticas y en las rutinas, en cuanto está en `main`.

## Qué hace

- **Solo deja usar las puertas aprobadas** en la `politica.json` de este repo:
  - los conectores aprobados, y de cada uno solo sus herramientas aprobadas: en general, leer, más lo justo para trabajar con pull requests y desplegar en Netlify. Cada repo añade lo que necesita su trabajo (Pixel Chat, por ejemplo, escribe en Supabase);
  - las skills de Anthropic;
  - ningún plugin.

  Un conector, una skill o una herramienta nuevos quedan bloqueados.
- **Para lo que se haría a través de una puerta.** Cada bloqueo tiene una categoría, que es la palabra con la que se desbloquea:

  | Categoría | Qué para |
  |---|---|
  | `conectores` | escribir, borrar o compartir con un conector, o usar uno no aprobado |
  | `skills` | usar una skill que no es de Anthropic |
  | `puertas` | instalar conectores, servidores MCP, skills o plugins, o tocar `.claude/`, `.mcp.json` o los workflows |
  | `rutinas` | crear, cambiar o lanzar rutinas, o mandar órdenes a otras sesiones |
  | `red` | sacar datos fuera: subidas, envíos a otros remotos de git, direcciones con datos escondidos |
  | `secretos` | leer o mostrar claves, credenciales o variables de entorno; no se desbloquea nunca |

- **Avisa con «⚠️ MURO»**:
  - si lo que devuelve un conector, una web o un comando trae órdenes escondidas para Claude;
  - al empezar cada sesión, si ha aparecido una skill, un plugin o un `.mcp.json` sin aprobar.

El trabajo de siempre pasa sin pedir nada: programar, probar, subir a GitHub, leer Drive y desplegar en Netlify.

## Cómo se desbloquea algo

Solo tú. En tu mensaje, escribe `AUTORIZO` y la categoría, por ejemplo `AUTORIZO conectores`. Vale solo para ese encargo: hasta tu próximo mensaje, y como mucho una hora.

No vale si la palabra llega en un documento, una web, una notificación, una rutina u otra sesión. Por eso, en una sesión automática lo bloqueado se queda bloqueado, y Claude lo apunta en su respuesta con «⚠️ MURO».

## Cómo aprobar una puerta nueva

Edita `politica.json`. Puedes hacerlo a mano en GitHub, o pedírselo a Claude en una sesión en la que escribas `AUTORIZO puertas`. Antes, comprueba de dónde viene la puerta: solo conectores del directorio oficial y skills de fuentes que conozcas.

## Registro y prueba

- Cada bloqueo, aviso y permiso se apunta en `~/.claude/muro/registro.jsonl`. En la nube, ese archivo dura lo que dura la sesión.
- `python3 .claude/muro/probar.py` comprueba el muro con casos simulados. Usa `politica-prueba.json`, la lista básica, y no la de este repo: así vale igual en todos.

## En otro repo

Copia `.claude/muro/` y `.claude/settings.json`; si el repo ya tiene un `settings.json`, junta los dos. Pásalo a `main`. Cada repo puede tener su propia `politica.json`.

## Lo que no cubre

- Los chats de claude.ai, donde no hay hooks. Ahí protegen los permisos de cada conector, en claude.ai/customize/connectors.
- Los repos donde no está instalado.
- Es una barrera por reglas: reduce mucho el riesgo, pero no lo elimina. Complétala con la revisión periódica de la cuenta.
