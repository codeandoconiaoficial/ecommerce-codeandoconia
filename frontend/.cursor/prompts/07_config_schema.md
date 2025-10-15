### Prompt – Generar configuración tipada para UI (forms, columns, filters)

Actuá como un arquitecto frontend experto en React + TypeScript + Material UI.  
Tu tarea es generar **configuraciones de UI declarativas** (sin lógica ejecutable) para manejar formularios, columnas, filtros o dashboards.  

#### 🎯 Objetivo
Centralizar la definición de estructuras de UI dentro del directorio `/src/config/`,  
garantizando consistencia, reutilización y separación de responsabilidades.

#### 🧩 Reglas generales
- Las configuraciones **no deben incluir lógica de render ni hooks**.
- Deben exportar únicamente objetos o arrays **tipados fuertemente**.
- Si una vista requiere formularios o tablas, estos deben importarse desde `/config/columns.ts` o `/config/forms.ts`.
- Los archivos de configuración pueden superar las 100 líneas, ya que contienen data declarativa, no lógica.
- Cumplir con **todas las normas SEO, accesibilidad y mobile-first.**
- Ninguna configuración puede incluir `any` ni `unknown`.  
  Si un tipo es dinámico, debe definirse explícitamente mediante interfaces o tipos discriminados.

#### 🧱 Estructura esperada
Ejemplo de tipado y contenido para columnas de tabla:

```ts
// /src/config/columns.ts
import { GridColDef } from '@mui/x-data-grid';

export interface ColumnConfig extends GridColDef {
  field: string;
  headerName: string;
  width?: number;
  flex?: number;
  sortable?: boolean;
  align?: 'left' | 'center' | 'right';
  renderCell?: never; // la lógica de render se define fuera
}

export const userTableColumns: readonly ColumnConfig[] = [
  { field: 'id', headerName: 'ID', width: 90, sortable: false },
  { field: 'name', headerName: 'Nombre', flex: 1 },
  { field: 'email', headerName: 'Email', flex: 1 },
  { field: 'role', headerName: 'Rol', flex: 1 },
];
```

Ejemplo de configuración para formularios:

```ts
// /src/config/forms.ts
export interface FormField {
  name: string;
  label: string;
  type: 'text' | 'number' | 'email' | 'select' | 'checkbox' | 'date';
  required?: boolean;
  options?: { label: string; value: string | number }[];
}

export const userFormFields: readonly FormField[] = [
  { name: 'name', label: 'Nombre', type: 'text', required: true },
  { name: 'email', label: 'Email', type: 'email', required: true },
  { name: 'role', label: 'Rol', type: 'select', options: [
      { label: 'Admin', value: 'admin' },
      { label: 'User', value: 'user' }
  ]},
];
```

#### ⚙️ Validaciones esperadas
- La configuración debe exportar constantes `readonly` para evitar mutaciones.
- Las interfaces deben estar definidas y exportadas desde `/src/types/` si se reutilizan.
- No se permite duplicar keys o estructuras entre configuraciones.
- Los nombres de export deben seguir el patrón `feature + tipo`,  
  por ejemplo: `userTableColumns`, `productFormFields`, `invoiceFilters`.

#### 🧠 Lógica de uso
Los componentes solo pueden **importar y consumir** estas configuraciones.  
Ejemplo:

```tsx
import { userFormFields } from '@/config/forms';

export function UserForm() {
  return <FormGenerator fields={userFormFields} />;
}
```

#### 🧩 Resultado esperado
Devolveme:
1. La definición tipada del schema (interface + ejemplo).
2. El archivo `/config/...` correspondiente.
3. Una breve descripción de cómo se integraría en el componente principal.
