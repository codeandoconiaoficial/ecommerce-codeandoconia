# ⚙️ Cursor Rules – Dev Evolution Frontend

## 📚 Fuentes de verdad y resolución de bloqueos
- Si Cursor encuentra un error o se bloquea ante una implementación:
  - **Debe acudir a la documentación oficial** de la tecnología en uso antes de proponer una solución alternativa.
  - No debe inventar APIs, hooks ni patrones que no existan en la documentación oficial.
  - Si la documentación no cubre el caso, debe:
    1. Sugerir una **implementación mínima viable**, comentando claramente la limitación detectada.
    2. **Solicitar autorización explícita del Tech Lead** (usuario Gonzalo Llantada) antes de aplicar cualquier cambio robusto, arquitectónico o fuera de las prácticas documentadas.
  - No se deben aplicar modificaciones no aprobadas o experimentales sin consentimiento estricto.
- Priorizar precisión técnica, documentación y trazabilidad por sobre rapidez o improvisación.


## 🧠 Estilo de respuesta
- Siempre escribir en TypeScript estricto.
- Prohibido usar `any` o `unknown`.
- Preferir `readonly`, `const`, y props bien tipadas.
- Mantener consistencia con el theme y tipografía MUI.
- Responder en español técnico.

## 🧩 Organización del código
- No se permite código repetido:
  - Los componentes comunes deben crearse o moverse a `/src/shared-components`.
  - Los hooks genéricos o templates deben alojarse en `/src/shared-hooks`.
  - Cursor debe sugerir la reutilización o refactorización ante duplicación.
- Cada archivo debe tener menos de **100 líneas de código**. Si supera ese límite, sugerir componentización.
- Separar lógica y render:
  - La lógica del componente debe residir en un hook `use{Componente}.ts`.
  - El archivo principal debe contener solo el render y las props.

## 🎨 UI / UX
- Aplicar Material UI + styled-components.
- Mobile-first por defecto.
- Cumplir con WCAG AA (accesibilidad).
- Evitar estilos inline; usar `theme.spacing()` y `sx` cuando sea apropiado.
- Mantener componentes atómicos, reutilizables y consistentes.
- Cumplir con **todas las normativas SEO** para un SEO eficiente y profesional:
  - Uso correcto de etiquetas semánticas (`<header>`, `<main>`, `<article>`, `<section>`, `<footer>`).
  - Jerarquía clara de encabezados (`h1`, `h2`, `h3`...).
  - Atributos `alt` obligatorios en imágenes.
  - Uso de `meta` tags dinámicos (title, description, og:image, etc.) según la vista.
  - URLs limpias y descriptivas (React Router).
  - Carga diferida (lazy loading) sin bloquear el render inicial.
  - Performance optimizada: evitar re-render innecesarios y recursos no utilizados.
  - Soporte para SSR o meta-tags pre-renderizados en caso de aplicaciones indexables.
- El diseño debe priorizar **usabilidad, performance y rastreabilidad SEO** por igual.

## 🧾 Configuraciones de UI
- Los formularios, tablas o listados deben declararse usando configuraciones externas ubicadas en `/src/config/`.
- Ejemplo:  
  - `/config/columns.ts` → definición de columnas de tabla.  
  - `/config/forms.ts` → definición de campos de formulario.
- Estas configuraciones pueden tener más de 100 líneas si contienen objetos extensos o definiciones estáticas.
- **Los componentes de UI no deben contener la definición inline de columnas o campos.**  
  Deben importar desde `/config/` y usar esas estructuras como fuente de verdad.
- Cualquier cambio de estructura o metadata de formulario se gestiona en `/config/`, no dentro del componente.

## ⚙️ Estado y lógica
- Usar Zustand para estado global.
- Evitar duplicación de estado.
- Hooks deben comenzar con `useXxx`.
- Centralizar efectos secundarios en hooks o stores.
- Los hooks compartidos se ubican en `/src/shared-hooks`.

## 🧪 Testing y DOB (Definition of Build)
- Cada tarea o feature debe cumplir estas condiciones antes de considerarse válida:
  1. **Tipado fuerte sin errores (`tsc` limpio).**
  2. **Sin errores ni warnings de lint.**
  3. **Build funcional sin fallos.**
  4. **Testing con cobertura mínima del 85%.**
- Generar test con Jest + RTL para todo componente crítico.
- Mockear Zustand stores o servicios externos.
- Usar `userEvent` y `screen` para interacciones.
- Mantener tipado fuerte en los tests.

## 📘 Documentación
- Cada módulo debe incluir descripción clara en JSDoc.
- Documentar props, estados y responsabilidades.
- Incluir ejemplos de uso y buenas prácticas.

## 🧩 Comunicación
- Explicar brevemente el porqué de cada decisión técnica.
- Usar formato markdown para reportes o documentación generada por IA.

