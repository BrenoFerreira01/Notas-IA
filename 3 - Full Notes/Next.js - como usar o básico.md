
data: 25.11.2026
horário: 11:25
Tags: [[Estudo]], [[Programação]]

---

# Next.js - como usar o básico

Esta é a primeira de três notas sobre Next.js:

1. **Como usar o básico** (esta nota)
2. [[Next.js - como criar o frontend de um projeto]]
3. [[Next.js - como criar o backend de um projeto]]

## O que é o Next.js

Next.js é um framework construído em cima do **React**. O React sozinho só cuida da interface (componentes). O Next.js adiciona o resto que um projeto real precisa:

- **Roteamento por pastas**: cada pasta dentro de `app/` vira uma URL.
- **Renderização no servidor** (SSR), geração estática (SSG) e cache.
- **Backend no mesmo projeto**: rotas de API e Server Actions (ver [[Next.js - como criar o backend de um projeto]]).
- Otimizações prontas: imagens (`next/image`), fontes (`next/font`), divisão de código.

Nestas notas usamos o **App Router** (pasta `app/`), que é o modelo atual. Você ainda vai encontrar tutoriais antigos com a pasta `pages/` (o "Pages Router"); ele funciona, mas não é o recomendado para projetos novos.

## Pré-requisitos

- **Node.js** instalado (versão LTS atual). Confira com `node -v`.
- Noções de JavaScript/TypeScript e de React (componentes, props, JSX).

## Criando o projeto com `create-next-app`

```bash
npx create-next-app@latest meu-app
```

O assistente faz algumas perguntas. Respostas recomendadas para começar:

| Pergunta | Resposta |
|---|---|
| TypeScript? | Yes |
| ESLint? | Yes |
| Tailwind CSS? | Yes |
| `src/` directory? | Opcional (aqui: No) |
| App Router? | **Yes** |
| Customizar import alias (`@/*`)? | No (mantém o padrão `@/`) |

Depois:

```bash
cd meu-app
npm run dev
```

Abra `http://localhost:3000` no navegador. Edite `app/page.tsx`, salve, e a página atualiza sozinha (hot reload).

## Estrutura do projeto

```text
meu-app/
├── app/
│   ├── layout.tsx      # layout raiz (envolve todas as páginas)
│   ├── page.tsx        # página da rota "/"
│   ├── globals.css     # CSS global
│   └── favicon.ico
├── public/             # arquivos estáticos servidos em "/" (imagens, robots.txt...)
├── next.config.ts      # configuração do Next.js
├── package.json
└── tsconfig.json
```

Regra principal: **dentro de `app/`, as pastas definem as rotas e alguns nomes de arquivo são especiais**.

| Arquivo | Função |
|---|---|
| `page.tsx` | A página da rota. Sem `page.tsx`, a pasta não vira URL pública. |
| `layout.tsx` | Layout que envolve a página e as sub-rotas. |
| `loading.tsx` | Tela de carregamento enquanto a página busca dados. |
| `error.tsx` | Tela de erro da rota. |
| `not-found.tsx` | Tela de 404. |
| `route.ts` | Rota de API (backend), sem interface. |

`loading`, `error` e `route` aparecem nas outras duas notas.

## Páginas

Uma página é só um componente React exportado como `default` em um `page.tsx`:

```tsx
// app/page.tsx  →  rota "/"
export default function Home() {
  return (
    <main>
      <h1>Olá, Next.js!</h1>
      <p>Minha primeira página.</p>
    </main>
  );
}
```

## Layouts

O `app/layout.tsx` é obrigatório e precisa ter as tags `<html>` e `<body>`. Tudo que ficar nele (menu, rodapé) aparece em todas as páginas:

```tsx
// app/layout.tsx
import type { Metadata } from "next";
import "./globals.css";

export const metadata: Metadata = {
  title: "Meu App",
  description: "Aprendendo Next.js",
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="pt-BR">
      <body>
        <header>Meu App</header>
        {children}
        <footer>© 2026</footer>
      </body>
    </html>
  );
}
```

`children` é a página atual. Ao navegar entre páginas, o layout **não é recarregado**, só o conteúdo de `children` muda.

Você pode ter layouts aninhados: um `app/dashboard/layout.tsx` envolve só as páginas dentro de `app/dashboard/`.

## Roteamento por pastas

Criar uma rota é criar uma pasta com um `page.tsx` dentro:

```text
app/
├── page.tsx                → /
├── sobre/
│   └── page.tsx            → /sobre
└── blog/
    ├── page.tsx            → /blog
    └── [slug]/
        └── page.tsx        → /blog/qualquer-coisa  (rota dinâmica)
```

```tsx
// app/sobre/page.tsx
export default function Sobre() {
  return <h1>Sobre mim</h1>;
}
```

Outros truques de pastas que vale conhecer:

- `[slug]`: segmento dinâmico (detalhado em [[Next.js - como criar o frontend de um projeto]]).
- `(grupo)`: pasta entre parênteses **não aparece na URL**. Serve para organizar ou dar layouts diferentes, ex.: `app/(marketing)/precos/page.tsx` vira `/precos`.
- `_pasta`: pasta com `_` é privada, nunca vira rota. Bom para guardar componentes perto das páginas.

## Navegando com `Link`

Use o componente `Link` em vez de `<a>` para links internos. Ele navega sem recarregar a página inteira e faz *prefetch* das rotas visíveis:

```tsx
// app/page.tsx
import Link from "next/link";

export default function Home() {
  return (
    <nav>
      <Link href="/">Início</Link> | <Link href="/sobre">Sobre</Link> |{" "}
      <Link href="/blog/meu-primeiro-post">Primeiro post</Link>
    </nav>
  );
}
```

Para navegar por código (ex.: depois de um clique ou de salvar um formulário), use o hook `useRouter` de `next/navigation` em um Client Component:

```tsx
"use client";
import { useRouter } from "next/navigation";

export function BotaoVoltar() {
  const router = useRouter();
  return <button onClick={() => router.push("/")}>Voltar ao início</button>;
}
```

> Atenção: importe de `next/navigation`, não de `next/router` (esse é do Pages Router antigo).

## Comandos: dev, build e start

| Comando | Para quê |
|---|---|
| `npm run dev` | Desenvolvimento: servidor local com hot reload em `localhost:3000`. |
| `npm run build` | Gera a versão otimizada de produção na pasta `.next/`. Também acusa erros de tipo e de lint. |
| `npm run start` | Roda a versão gerada pelo `build` (simula produção). Precisa rodar `build` antes. |
| `npm run lint` | Verifica o código com ESLint. |

Fluxo típico antes de publicar:

```bash
npm run build
npm run start
# teste em http://localhost:3000
```

Dica: rode `npm run build` de vez em quando durante o desenvolvimento. Alguns erros só aparecem no build.

## Resumo

- `app/` define as rotas: pasta = segmento de URL, `page.tsx` = página.
- `layout.tsx` envolve páginas e persiste entre navegações.
- `Link` para links internos; `useRouter` para navegar por código.
- `dev` para desenvolver, `build` + `start` para produção.

Próximo passo: [[Next.js - como criar o frontend de um projeto]].

## Referências

- [Documentação oficial do Next.js](https://nextjs.org/docs)
- [Tutorial oficial "Learn Next.js"](https://nextjs.org/learn)
