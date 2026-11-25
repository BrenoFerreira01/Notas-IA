
data: 25.11.2026
horário: 11:25
Tags: [[Estudo]], [[Programação]]

---

# Next.js - como criar o frontend de um projeto

Segunda de três notas sobre Next.js:

1. [[Next.js - como usar o básico]] (leia antes se nunca usou Next.js)
2. **Como criar o frontend** (esta nota)
3. [[Next.js - como criar o backend de um projeto]]

Exemplo usado ao longo da nota: um pequeno app de **tarefas** (lista, detalhe e formulário para criar).

## Componentes

Componentes são funções React que retornam JSX. Guarde os reutilizáveis em uma pasta `components/` na raiz (ou em `app/_components/`):

```tsx
// components/CardTarefa.tsx
type Props = {
  titulo: string;
  concluida: boolean;
};

export function CardTarefa({ titulo, concluida }: Props) {
  return (
    <div>
      <span>{concluida ? "✔" : "○"}</span> {titulo}
    </div>
  );
}
```

```tsx
// app/page.tsx
import { CardTarefa } from "@/components/CardTarefa";

export default function Home() {
  return (
    <main>
      <CardTarefa titulo="Estudar Next.js" concluida={false} />
      <CardTarefa titulo="Instalar Node" concluida={true} />
    </main>
  );
}
```

O `@/` é o atalho (alias) para a raiz do projeto, configurado no `tsconfig.json`.

## Server Components x Client Components

Este é o conceito mais importante do App Router.

**Por padrão, todo componente em `app/` é um Server Component.** Ele roda no servidor, gera HTML e não envia o JavaScript dele para o navegador.

| | Server Component (padrão) | Client Component (`"use client"`) |
|---|---|---|
| Onde roda | Servidor | Navegador (e pré-renderizado no servidor) |
| Pode ser `async` e usar `await` | Sim | Não |
| Acessar banco, arquivos, segredos | Sim | **Não** |
| `useState`, `useEffect`, hooks | Não | Sim |
| Eventos (`onClick`, `onChange`) | Não | Sim |
| APIs do navegador (`window`, `localStorage`) | Não | Sim |

Para transformar um componente em Client Component, coloque `"use client"` na **primeira linha** do arquivo:

```tsx
// components/Contador.tsx
"use client";

import { useState } from "react";

export function Contador() {
  const [n, setN] = useState(0);
  return <button onClick={() => setN(n + 1)}>Cliquei {n} vezes</button>;
}
```

Um Server Component pode usar um Client Component normalmente:

```tsx
// app/page.tsx  (Server Component)
import { Contador } from "@/components/Contador";

export default function Home() {
  return (
    <main>
      <h1>Página renderizada no servidor</h1>
      <Contador /> {/* só esta parte é interativa */}
    </main>
  );
}
```

**Regra prática:** deixe tudo como Server Component e use `"use client"` só nas "folhas" que precisam de interatividade (botões, formulários com estado, modais). Assim o navegador baixa menos JavaScript.

Cuidados:

- Props passadas de Server para Client Component precisam ser serializáveis (strings, números, objetos simples). Funções comuns não podem ser passadas; Server Actions podem (ver [[Next.js - como criar o backend de um projeto]]).
- Tudo que um arquivo `"use client"` importa também vira código de cliente. Não importe nele módulos que acessam o banco.

## Estilização

### CSS global

`app/globals.css` é importado no `layout.tsx` raiz e vale para o site inteiro. Use para resets e variáveis.

### CSS Modules

Arquivos `*.module.css` têm classes com escopo local (não vazam para outros componentes):

```css
/* components/CardTarefa.module.css */
.card {
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
}
.concluida {
  text-decoration: line-through;
  color: #888;
}
```

```tsx
// components/CardTarefa.tsx
import styles from "./CardTarefa.module.css";

export function CardTarefa({ titulo, concluida }: { titulo: string; concluida: boolean }) {
  return (
    <div className={`${styles.card} ${concluida ? styles.concluida : ""}`}>
      {titulo}
    </div>
  );
}
```

### Tailwind CSS

Se você respondeu "Yes" ao Tailwind no `create-next-app`, ele já está configurado. Você estiliza com classes utilitárias direto no JSX:

```tsx
export function CardTarefa({ titulo, concluida }: { titulo: string; concluida: boolean }) {
  return (
    <div className={`rounded-lg border p-3 ${concluida ? "text-gray-400 line-through" : ""}`}>
      {titulo}
    </div>
  );
}
```

Qual escolher? Tailwind é mais rápido para prototipar e é o padrão de muitos projetos e bibliotecas (ex.: shadcn/ui). CSS Modules é CSS puro, bom se você prefere separar estilo e marcação. Dá para misturar os dois.

## Buscando dados (data fetching)

Em Server Components você busca dados direto com `async/await`, sem `useEffect`:

```tsx
// app/tarefas/page.tsx
type Tarefa = { id: number; title: string; completed: boolean };

export default async function TarefasPage() {
  const res = await fetch("https://jsonplaceholder.typicode.com/todos?_limit=5");
  if (!res.ok) throw new Error("Falha ao carregar tarefas");
  const tarefas: Tarefa[] = await res.json();

  return (
    <ul>
      {tarefas.map((t) => (
        <li key={t.id}>{t.title}</li>
      ))}
    </ul>
  );
}
```

Também é possível consultar o banco diretamente aqui (ex.: `await db.tarefa.findMany()`), porque o código roda no servidor. Como configurar o banco está em [[Next.js - como criar o backend de um projeto]].

### Cache e dados atualizados

O comportamento de cache mudou entre versões do Next.js, então confira a documentação da sua versão. O jeito explícito de controlar:

```tsx
// sempre buscar dados novos a cada requisição
await fetch(url, { cache: "no-store" });

// reaproveitar por até 60 segundos
await fetch(url, { next: { revalidate: 60 } });
```

### Buscando dados no cliente

Quando os dados dependem de interação do usuário (busca enquanto digita, por exemplo), busque no Client Component, chamando uma rota de API sua. Para isso bibliotecas como **SWR** ou **TanStack Query** ajudam com cache e loading. Na maioria das telas, porém, buscar no servidor é mais simples.

## Rotas dinâmicas

Uma pasta com colchetes captura um pedaço da URL. Em `app/tarefas/[id]/page.tsx`, a URL `/tarefas/3` entrega `id = "3"`:

```tsx
// app/tarefas/[id]/page.tsx
import { notFound } from "next/navigation";

type Props = { params: Promise<{ id: string }> };

export default async function TarefaDetalhe({ params }: Props) {
  const { id } = await params; // nas versões atuais, params é uma Promise

  const res = await fetch(`https://jsonplaceholder.typicode.com/todos/${id}`);
  if (res.status === 404) notFound(); // mostra o not-found.tsx
  const tarefa = await res.json();

  return (
    <article>
      <h1>{tarefa.title}</h1>
      <p>Status: {tarefa.completed ? "concluída" : "pendente"}</p>
    </article>
  );
}
```

Variações:

- `[...slug]`: captura vários segmentos (`/docs/a/b/c` → `slug = ["a","b","c"]`).
- `searchParams`: para query string (`/tarefas?filtro=abertas`), a página recebe `searchParams` (também uma Promise) do mesmo jeito que `params`.

Linkando para a rota dinâmica:

```tsx
<Link href={`/tarefas/${tarefa.id}`}>{tarefa.title}</Link>
```

## Loading e error

### `loading.tsx`

Mostrado automaticamente enquanto a página da mesma pasta espera os dados:

```tsx
// app/tarefas/loading.tsx
export default function Loading() {
  return <p>Carregando tarefas...</p>;
}
```

Para carregar partes da página separadamente, envolva componentes lentos em `<Suspense fallback={...}>` do React.

### `error.tsx`

Captura erros lançados na rota. **Precisa ser Client Component**:

```tsx
// app/tarefas/error.tsx
"use client";

export default function Erro({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <p>Algo deu errado: {error.message}</p>
      <button onClick={() => reset()}>Tentar de novo</button>
    </div>
  );
}
```

### `not-found.tsx`

Exibido quando você chama `notFound()` ou quando a URL não existe:

```tsx
// app/not-found.tsx
import Link from "next/link";

export default function NotFound() {
  return (
    <div>
      <h1>Página não encontrada</h1>
      <Link href="/">Voltar ao início</Link>
    </div>
  );
}
```

## Formulários

O jeito moderno no Next.js é ligar o `<form>` direto a uma **Server Action** (uma função que roda no servidor). O formulário funciona até sem JavaScript no navegador.

```tsx
// app/tarefas/nova/page.tsx
import { criarTarefa } from "@/app/actions";

export default function NovaTarefa() {
  return (
    <form action={criarTarefa}>
      <input name="titulo" placeholder="Título da tarefa" required />
      <button type="submit">Salvar</button>
    </form>
  );
}
```

A função `criarTarefa` fica no backend e está explicada em [[Next.js - como criar o backend de um projeto]].

### Mostrando "salvando..." e mensagens de erro

Para feedback na tela, use um Client Component com `useActionState` (React 19):

```tsx
// components/FormTarefa.tsx
"use client";

import { useActionState } from "react";
import { criarTarefa } from "@/app/actions";

export function FormTarefa() {
  const [estado, formAction, pendente] = useActionState(criarTarefa, { erro: "" });

  return (
    <form action={formAction}>
      <input name="titulo" placeholder="Título" />
      <button disabled={pendente}>{pendente ? "Salvando..." : "Salvar"}</button>
      {estado.erro && <p style={{ color: "red" }}>{estado.erro}</p>}
    </form>
  );
}
```

Atenção: com `useActionState`, a action muda de assinatura. Ela recebe `(estadoAnterior, formData)` em vez de só `formData` e devolve o novo estado. A versão dessa action com validação está na nota de backend.

Formulários grandes e com muita validação no cliente costumam usar **React Hook Form** + **Zod**.

## Imagens e fontes (bônus)

```tsx
import Image from "next/image";

<Image src="/logo.png" alt="Logo" width={120} height={40} />
```

`next/image` redimensiona e carrega as imagens sob demanda. O arquivo fica em `public/logo.png`. Para fontes, `next/font/google` baixa a fonte no build e evita "pulos" de layout.

## Resumo

- Componentes são Server Components por padrão; use `"use client"` só onde há interatividade.
- Estilo: `globals.css`, CSS Modules ou Tailwind.
- Busque dados com `async/await` direto no Server Component.
- `[id]` cria rotas dinâmicas; `loading.tsx`, `error.tsx` e `not-found.tsx` tratam estados.
- Formulários usam `<form action={serverAction}>`, e `useActionState` dá feedback.

Próximo passo: [[Next.js - como criar o backend de um projeto]].

## Referências

- [Server e Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components)
- [Data fetching](https://nextjs.org/docs/app/getting-started/fetching-data)
- [Tailwind CSS](https://tailwindcss.com/docs)
