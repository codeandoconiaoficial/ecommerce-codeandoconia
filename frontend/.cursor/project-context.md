# 🧠 Project Context – Dev Evolution Frontend

## 🧩 Stack principal
- React 18 + TypeScript (strict mode)
- Material UI (MUI v5)
- Zustand (state manager)
- React Router DOM (routing)
- Styled Components (estilos con theme)
- Jest + React Testing Library (testing)
- ESLint + Prettier (lint y formato)
- Vite (build tool)
- Arquitectura mobile-first + responsive

## 📂 Estructura recomendada
src/
 ├─ components/         → componentes específicos de features
 ├─ shared-components/  → componentes reutilizables entre vistas
 ├─ pages/              → vistas principales (rutas)
 ├─ hooks/              → hooks específicos de feature
 ├─ shared-hooks/       → hooks templates y utilitarios reutilizables
 ├─ stores/             → Zustand stores
 ├─ services/           → mocks / integraciones frontend
 ├─ config/             → definiciones de columnas, formularios y metadatos de UI
 ├─ theme/              → paleta, tipografía, spacing
 ├─ types/              → tipos y contratos compartidos
 ├─ tests/              → test unitarios
 └─ utils/              → helpers puros

 💡 Nota: los archivos dentro de /config pueden superar las 100 líneas, ya que representan declaraciones estructuradas, no lógica ejecutable.

## 🎯 Propósito
Crear aplicaciones frontend escalables, accesibles y mantenibles, con modularidad estricta y reutilización inteligente.

## 💡 Filosofía Dev Evolution
1. Tipado fuerte y consistente.
2. Componentes y hooks reutilizables (sin duplicación).
3. Estado global predecible y centralizado.
4. UI accesible y moderna.
5. Testing integrado desde el día 1.
6. Documentación viva.
