# recursive-research

> Skill para [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) que **investiga recursivamente cualquier tema hasta nivel PhD** — ciencia, arte, negocio, humanidades, tecnología. Con tiering de fuentes confiables, loop auto-regulado, checkpointing a disco y WDM + Inversión Munger para decisiones autónomas.

**Versión:** 2.0.0 · **Licencia:** [MIT](LICENSE) · **Autor:** Joseph Huayhualla ([@anjos2](https://github.com/anjos2))

---

## Qué hace

Le das una **semilla de investigación** (un tema) y la skill:

1. Te pregunta modo (`web` / `local` / `mixto`), rutas locales si aplica, fuentes a priorizar/excluir, y tope de ciclos.
2. Identifica **3-5 hilos semilla** aplicando [WDM (Weighted Decision Matrix) + Inversión Munger](#wdm--inversión-munger).
3. Detecta qué MCPs tienes (Firecrawl, Context7, WebFetch, etc.) y los prioriza por velocidad y calidad.
4. **Itera en ciclos auto-regulados** — cada ciclo elige el hilo con menor cobertura, selecciona fuentes, investiga, consolida.
5. Clasifica fuentes en **Tier 1 / 2 / 3 / Rechazo** con criterios transparentes.
6. Guarda **checkpoints a disco** en cada ciclo — sobrevive al context compact.
7. Cierra cuando se cumplen los **5 criterios de nivel PhD** o cuando alcanza el tope.
8. Te pregunta si quieres seguir. **La investigación puede ser infinita.**

---

## Por qué es distinta

| Característica | Cómo lo resuelve |
|---|---|
| Funciona en **cualquier dominio** | Tiering de fuentes genérico (papers, libros académicos, archivos oficiales, datos crudos), no code-only |
| **Rechaza fuentes basura** automáticamente | Criterios explícitos: sin autor, marketing sin datos, spam SEO, contenido AI sin supervisión |
| **Sobrevive límites de contexto** | Checkpoint a disco cada ciclo + modo `--resume` para continuar en sesión nueva |
| **Auto-crítica** | Inversión Munger aplicada al conocimiento: ¿qué no sé? ¿qué sesgo tienen mis fuentes? ¿qué falta? |
| **Pregunta antes de asumir** | Fase 0 completa de interrogación al usuario |
| **Transparente** | Toda decisión autónoma no trivial aplica WDM + Munger y lo muestra |

---

## Instalación

### Como skill global de Claude Code

**Linux / macOS:**

```bash
git clone https://github.com/anjos2/recursive-research.git
mkdir -p ~/.claude/skills/recursive-research
cp recursive-research/skill.md ~/.claude/skills/recursive-research/
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/anjos2/recursive-research.git
New-Item -ItemType Directory -Force -Path "$HOME/.claude/skills/recursive-research"
Copy-Item recursive-research/skill.md "$HOME/.claude/skills/recursive-research/"
```

### Como skill local al proyecto

```bash
git clone https://github.com/anjos2/recursive-research.git
mkdir -p tu-proyecto/.claude/skills/recursive-research
cp recursive-research/skill.md tu-proyecto/.claude/skills/recursive-research/
```

Verifica con `/help` dentro de Claude Code que la skill aparece.

---

## Uso

### Invocación normal

```
/recursive-research
```

La skill te guía preguntando. Respondes con lenguaje natural.

### Reanudar investigación pausada

```
/recursive-research --resume <slug>
```

`<slug>` = nombre kebab-case del tema (ej: `memoria-episodica-humanos`).

### Listar investigaciones guardadas

```
/recursive-research --list
```

---

## Ejemplos de uso por dominio

| Dominio | Semilla sugerida |
|---------|------------------|
| Neurociencia | "Mecanismos de la memoria episódica en humanos" |
| Filosofía | "Aplicación moderna de la filosofía estoica" |
| Música | "Minimalismo en la música del siglo XX" |
| Negocio | "Modelos de monetización SaaS B2B en 2025" |
| Historia | "Caída del imperio romano de occidente: causas económicas" |
| Biología | "Inmunoterapia con células CAR-T contra cáncer" |
| Tecnología | "Arquitectura hexagonal en microservicios" |
| Derecho | "Regulación europea de IA (AI Act) y su impacto extraterritorial" |

---

## Criterio de "nivel PhD"

La skill declara PhD solo cuando se cumplen **los 5 criterios**:

1. **Cobertura ≥80%** en todos los hilos semilla
2. **≥3 fuentes Tier-1 por hilo**
3. **Saturación de hallazgos nuevos ≤5%** durante 3 ciclos consecutivos
4. **Inversión Munger aplicada** al conocimiento (qué no sé, qué contradicen las fuentes, qué sesgos hay)
5. **≥3 conexiones cruzadas** explícitas entre hilos diferentes

Si algún criterio falla, **no declara PhD** y sigue iterando — o pide confirmación al alcanzar el tope de ciclos (default 20, configurable).

---

## Tiering de fuentes

| Tier | Qué califica | Peso en WDM |
|------|--------------|-------------|
| **1** | Papers peer-reviewed · libros académicos · estándares oficiales (W3C, RFC, ISO, WHO) · archivos primarios · datos oficiales | 5 |
| **2** | Repos oficiales · blogs de autores citables · conferencias grabadas · Wikipedia con referencias · reportes con metodología | 3 |
| **3** | Blogs con citaciones a T1/T2 · foros con voto alto y fuentes · entrevistas con expertos identificables | 2 |
| **Rechazo** | Sin autor · marketing sin datos · agregadores spam · tutoriales sin fuentes · contenido AI sin supervisión | 0 |

Cada fuente consultada se registra en un archivo por tier para auditoría posterior.

---

## Fuentes semilla pre-cargadas

La skill sugiere automáticamente fuentes confiables por dominio. Algunas:

- **Ciencia**: arXiv, Semantic Scholar, Google Scholar, Connected Papers, OpenReview
- **Medicina**: PubMed, Cochrane Library, WHO, ClinicalTrials.gov
- **Humanidades**: JSTOR, SSRN, Project MUSE
- **Código**: GitHub, Context7, RFCs, W3C specs
- **Datos**: Banco Mundial, OECD Data, Our World in Data, Pew Research
- **Arte/cultura**: Europeana, Google Arts & Culture, Internet Archive, Project Gutenberg

El usuario puede añadir o rechazar cualquiera antes de arrancar.

---

## WDM + Inversión Munger

Frameworks de decisión aplicados en cada paso autónomo no trivial:

- **WDM (Weighted Decision Matrix)** — enumerar 3+ alternativas viables, criterios con peso, scoring 1-5, total comparado.
- **Inversión Munger** (Charlie Munger vía Jacobi) — preguntar invertido sobre la opción ganadora: *"¿cómo fallaría? ¿qué sesgo tiene? ¿qué estoy ignorando?"*

La skill aplica ambos para:
- Elegir hilos semilla
- Seleccionar fuentes por ciclo
- Decidir cuándo cerrar la investigación
- Validar el conocimiento consolidado al final

Referencia: [ensayo de Charlie Munger sobre inversión mental](https://fs.blog/inversion/).

---

## MCPs recomendados

La skill detecta cuáles tienes y los usa en este orden:

1. **[Firecrawl MCP](https://github.com/mendableai/firecrawl-mcp-server)** (muy recomendado) — scraping optimizado para IA
2. **[Context7 MCP](https://github.com/upstash/context7)** — docs oficiales de librerías
3. **WebSearch + WebFetch** (built-in de Claude Code) — fallback universal
4. **Chrome DevTools MCP** — solo si el contenido requiere ejecución JS real; en general **demasiado lento** para investigación a escala

---

## Archivos que genera

En `memoria/investigaciones/<slug>/` del proyecto activo:

- `estado.md` · `hilos.md` · `hallazgos.md`
- `fuentes-tier-1.md` · `fuentes-tier-2.md` · `fuentes-tier-3.md` · `fuentes-rechazadas.md`
- `ciclo-01.md`, `ciclo-02.md`, ..., `ciclo-N.md` (checkpoints)
- `sintesis.md` · `acciones.md` · `gaps.md` (al cerrar)

**Si `memoria/` no existe en el proyecto, la skill la crea** avisando al usuario — es una dependencia explícita.

---

## Contribuir

Issues y PRs bienvenidos. Si mejoras un criterio, añades un tier, encuentras un anti-pattern nuevo o quieres soporte para más dominios: abre un PR.

### Roadmap de ideas futuras

- [ ] Integración nativa con gestores de referencias (Zotero, Mendeley)
- [ ] Export de la investigación a formatos académicos (LaTeX, BibTeX)
- [ ] Modo colaborativo — múltiples agentes investigando hilos en paralelo
- [ ] Métricas de calidad de fuente automáticas (h-index, journal impact factor)
- [ ] Soporte para PDFs académicos protegidos tras paywalls legales

---

## Topics sugeridos para GitHub

Al crear el repo, añade estos topics para discoverability:

`claude-code` · `claude-code-skill` · `ai-agent` · `research-tool` · `recursive-research` · `knowledge-management` · `weighted-decision-matrix` · `mental-models`

---

## Licencia

[MIT](LICENSE) — úsala, modifícala, distribúyela. Solo mantén el aviso de copyright.

---

## Agradecimientos

- **Charlie Munger** — por "Invert, always invert" (vía Carl Jacobi)
- **Claude Code team** — por el formato de skills
- **Anthropic** — por el modelo que hace esto posible

Si esta skill te fue útil, considera darle ⭐ al repo.
