# Entrega — Ejercicio OpenSpec Sandbox

## 1. Evidencia de instalación operativa

**Versión instalada:**
```
openspec --version
→ 1.4.1
```

**Estructura generada por `openspec init`:**
```
openspec/
├── config.yaml
├── changes/
│   └── archive/
└── specs/

.claude/
├── commands/opsx/
│   ├── apply.md
│   ├── archive.md
│   ├── explore.md
│   ├── propose.md
│   └── sync.md
└── skills/
    ├── openspec-apply-change/SKILL.md
    ├── openspec-archive-change/SKILL.md
    ├── openspec-explore/SKILL.md
    ├── openspec-propose/SKILL.md
    └── openspec-sync-specs/SKILL.md
```

---

## 2. Parte A — Micro-tarea y 3 pilares

### Micro-tarea

Validar un CUIT argentino, verificando formato y dígito verificador.

### Pilar 1 — Herramienta

Usé Claude Code como copiloto dentro de VS Code.

Usé Claude Code porque me permite trabajar dentro del proyecto en VS Code y pedir ayuda paso a paso para definir la tarea, construir el contexto y revisar una primera propuesta de solución.

### Pilar 2 — Contexto

- Lenguaje: JavaScript sobre Node.js.
- La función será standalone, sin integrarse todavía a una aplicación existente.
- Nombre de la función: `validarCUIT(valor)`.
- Entrada: un valor que debería ser un string con CUIT argentino.
- Formatos aceptados: CUIT con guiones o sin guiones.
- La función normaliza internamente quitando guiones.
- Salida: booleano, `true` si el CUIT es válido y `false` en cualquier otro caso.
- No usa librerías externas.
- No consulta servicios externos.
- No lanza excepciones.
- Las entradas inválidas, como `null`, `undefined`, string vacío, números u otros tipos, devuelven `false`.
- La validación incluye formato y dígito verificador.

### Pilar 3 — Prompt

El prompt final aprobado le pide a Claude Code implementar una función `validarCUIT(valor)` en JavaScript puro, sin librerías externas, que acepte CUIT con o sin guiones, normalice la entrada, valide formato y dígito verificador, y devuelva siempre un booleano.

También especifica:
- no usar servicios externos;
- no lanzar excepciones;
- usar el algoritmo de dígito verificador con pesos `[5, 4, 3, 2, 7, 6, 5, 4, 3, 2]`;
- incluir comentarios explicativos;
- incluir ejemplos ejecutables con Node.js.

### Resultado

Claude Code generó una primera versión de la función `validarCUIT(valor)`.

La función:
- rechaza entradas no string o vacías;
- valida si el formato tiene 11 dígitos o formato con guiones;
- normaliza quitando guiones;
- aplica el algoritmo de dígito verificador;
- devuelve `true` o `false`.

No creé todavía un archivo definitivo porque el objetivo de esta etapa fue recorrer el proceso de definición de micro-tarea, contexto y prompt antes de avanzar con una implementación final.

### Evaluación

El proceso ayudó a entender que antes de pedir código conviene definir con claridad la herramienta, el contexto y el prompt.

El prompt fue suficientemente claro para que Claude generara una primera versión razonable de la función. Sin embargo, apareció una mejora posible: los ejemplos de CUIT válidos deberían estar definidos con datos de prueba confiables para evitar comentarios ambiguos.

La principal lección es que el contexto reduce ambigüedad y evita que el copiloto tome decisiones no pedidas.

---

## 3. Tres observaciones de la exploración

1. **Separación de estado**: `changes/` contiene trabajo en progreso (proposal + design + tasks por change); `specs/` acumula los specs consolidados. `archive/` guarda los changes terminados. El directorio es el estado del sistema.

2. **Integración con el copiloto**: `.claude/skills/` no es decorativo — son instrucciones detalladas que el harness de Claude Code inyecta como contexto cuando se invoca `/opsx:*`. Sin estos archivos, los comandos no funcionan.

3. **`config.yaml` como contrato**: El campo `context:` permite inyectar tech stack, convenciones y dominio del negocio. El AI los usa al generar propuestas y specs. Un `config.yaml` vacío produce artefactos genéricos; uno bien completado produce artefactos alineados al proyecto real.
