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

## 2. Los 3 Pilares — micro-tarea: validador de CUIT argentino

**Herramienta:** Claude Code (claude-sonnet-4-6) con OpenSpec 1.4.1

**Contexto:**
- Implementar una función en JavaScript llamada `isValidCuit(cuit)`, que valide un CUIT argentino con formato estricto `nn-nnnnnnnn-n` y verifique el dígito verificador.
- No usar librerías externas.
- Validar formato estricto `nn-nnnnnnnn-n`.
- Si la entrada no es string, devolver `false`.
- No consultar servicios externos.
- No validar existencia real del CUIT.
- Solo validar formato y dígito verificador.

**Prompt usado:**
> Implementá una función en JavaScript llamada `isValidCuit(cuit)`, que valide un CUIT argentino con formato estricto `nn-nnnnnnnn-n` y verifique el dígito verificador.
>
> Algoritmo:
> - Quitar guiones.
> - Usar pesos [5, 4, 3, 2, 7, 6, 5, 4, 3, 2].
> - Multiplicar los primeros 10 dígitos por los pesos.
> - Sumar.
> - Calcular 11 - (suma % 11).
> - Si da 11, el dígito esperado es 0.
> - Si da 10, el dígito esperado es 9.
> - Comparar contra el último dígito del CUIT.

---

## 3. Tres observaciones de la exploración

1. **Separación de estado**: `changes/` contiene trabajo en progreso (proposal + design + tasks por change); `specs/` acumula los specs consolidados. `archive/` guarda los changes terminados. El directorio es el estado del sistema.

2. **Integración con el copiloto**: `.claude/skills/` no es decorativo — son instrucciones detalladas que el harness de Claude Code inyecta como contexto cuando se invoca `/opsx:*`. Sin estos archivos, los comandos no funcionan.

3. **`config.yaml` como contrato**: El campo `context:` permite inyectar tech stack, convenciones y dominio del negocio. El AI los usa al generar propuestas y specs. Un `config.yaml` vacío produce artefactos genéricos; uno bien completado produce artefactos alineados al proyecto real.
