
data: 25.11.2026
horário: 11:25
Tags: [[Estudo]], [[Programação]]

---

# Next.js - como criar o backend de um projeto

Terceira de três notas sobre Next.js:

1. [[Next.js - como usar o básico]]
2. [[Next.js - como criar o frontend de um projeto]]
3. **Como criar o backend** (esta nota)

O Next.js é *full stack*: o mesmo projeto pode ter interface e backend. Há duas formas principais de escrever código de servidor:

| | Route Handlers (`route.ts`) | Server Actions (`"use server"`) |
|---|---|---|
| O que é | Endpoint HTTP (API REST) | Função do servidor chamada direto pelo React |
| Quem chama | Qualquer cliente: seu front, app mobile, webhooks, outros sistemas | Seus próprios componentes e formulários |
| Quando usar | API pública, webhooks, integrações | Criar/editar/excluir dados a partir do seu próprio site |

Para **ler** dados na sua própria interface, muitas vezes nenhum dos dois é necessário: o Server Component consulta o banco direto (ver [[Next.js - como criar o frontend de um projeto]]).

## Route Handlers (rotas de API)

Um arquivo `route.ts` dentro de `app/` vira um endpoint. Cada método HTTP é uma função exportada. A convenção é colocá-los em `app/api/`:

```ts
// app/api/tarefas/route.ts  →  GET e POST em /api/tarefas
import { NextResponse } from "next/server";

type Tarefa = { id: number; titulo: string };
const tarefas: Tarefa[] = [{ id: 1, titulo: "Estudar Next.js" }]; // "banco" em memória, só para teste

export async function GET() {
  return NextResponse.json(tarefas);
}

export async function POST(request: Request) {
  const body = await request.json();
  const nova = { id: tarefas.length + 1, titulo: String(body.titulo) };
  tarefas.push(nova);
  return NextResponse.json(nova, { status: 201 });
}
```

Testando no terminal:

```bash
curl http://localhost:3000/api/tarefas
curl -X POST http://localhost:3000/api/tarefas -H "Content-Type: application/json" -d '{"titulo":"Nova"}'
```

### Rota com parâmetro e query string

```ts
// app/api/tarefas/[id]/route.ts  →  /api/tarefas/3
import { NextResponse } from "next/server";

export async function GET(
  request: Request,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = await params;
  const url = new URL(request.url);
  const detalhado = url.searchParams.get("detalhado"); // ?detalhado=true

  return NextResponse.json({ id, detalhado });
}

export async function DELETE(
  _request: Request,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = await params;
  // ...apagar no banco
  return new Response(null, { status: 204 });
}
```

Regras: não pode existir `page.tsx` e `route.ts` na mesma pasta. Métodos suportados: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `HEAD`, `OPTIONS`.

## Server Actions

Uma Server Action é uma função `async` marcada com `"use server"`. Você a chama do frontend como uma função comum, e o Next.js cuida da requisição por baixo.

```ts
// app/actions.ts
"use server";

import { revalidatePath } from "next/cache";
import { redirect } from "next/navigation";

export async function criarTarefa(formData: FormData) {
  const titulo = formData.get("titulo");
  if (typeof titulo !== "string" || titulo.trim() === "") return;

  // await db.tarefa.create({ data: { titulo } });  (ver seção de banco)

  revalidatePath("/tarefas"); // descarta o cache da lista para mostrar a nova tarefa
  redirect("/tarefas");       // manda o usuário de volta para a lista
}
```

Uso no formulário (detalhes na nota de frontend):

```tsx
<form action={criarTarefa}>
  <input name="titulo" />
  <button>Salvar</button>
</form>
```

Também dá para chamar de um Client Component em um evento: `onClick={() => excluirTarefa(id)}`.

> **Segurança:** uma Server Action é um endpoint público. Qualquer pessoa pode chamá-la com dados quaisquer. Sempre **valide os dados** e **verifique se o usuário está logado e tem permissão** dentro da própria action.

## Banco de dados: Prisma ou Drizzle

Para conversar com um banco (PostgreSQL, MySQL, SQLite) em TypeScript, os dois ORMs mais usados são:

| | Prisma | Drizzle |
|---|---|---|
| Esquema | Arquivo próprio `schema.prisma` | Código TypeScript |
| Estilo | API de alto nível (`findMany`, `create`) | Parecido com SQL (`select().from()`) |
| Pontos fortes | Muito fácil para iniciantes, ótima documentação, Prisma Studio | Leve, rápido, bom para serverless/edge, você "vê" o SQL |

Ambos são boas escolhas. Abaixo, exemplo com Prisma.

### Exemplo com Prisma (SQLite para testar localmente)

```bash
npm install prisma --save-dev
npm install @prisma/client
npx prisma init --datasource-provider sqlite
```

```prisma
// prisma/schema.prisma
model Tarefa {
  id        Int      @id @default(autoincrement())
  titulo    String
  concluida Boolean  @default(false)
  criadaEm  DateTime @default(now())
}
```

```bash
npx prisma migrate dev --name init   # cria a tabela e gera o cliente
```

Crie **uma única instância** do cliente (evita abrir conexões demais durante o hot reload do `npm run dev`):

```ts
// lib/db.ts
import { PrismaClient } from "@prisma/client";

const globalForPrisma = globalThis as unknown as { prisma?: PrismaClient };

export const db = globalForPrisma.prisma ?? new PrismaClient();

if (process.env.NODE_ENV !== "production") globalForPrisma.prisma = db;
```

Usando em uma página e em uma action:

```tsx
// app/tarefas/page.tsx
import { db } from "@/lib/db";

export default async function TarefasPage() {
  const tarefas = await db.tarefa.findMany({ orderBy: { criadaEm: "desc" } });
  return <ul>{tarefas.map((t) => <li key={t.id}>{t.titulo}</li>)}</ul>;
}
```

```ts
// app/actions.ts (trecho)
await db.tarefa.create({ data: { titulo } });
```

O mesmo com **Drizzle**, só para comparar o estilo:

```ts
import { db } from "@/lib/db";       // instância do drizzle
import { tarefas } from "@/db/schema"; // tabela definida em TypeScript

const lista = await db.select().from(tarefas);
await db.insert(tarefas).values({ titulo: "Nova" });
```

Observações da versão: o Prisma mudou detalhes de configuração nas versões mais recentes (por exemplo, o local onde o cliente é gerado). Se algum comando acima divergir, siga o guia oficial da versão instalada.

## Variáveis de ambiente

Segredos (senha do banco, chaves de API) ficam em arquivos `.env*` na raiz, **nunca** no código:

```bash
# .env.local  (não vai para o git; o .gitignore do create-next-app já ignora)
DATABASE_URL="postgresql://usuario:senha@localhost:5432/meubanco"
API_SECRET="valor-secreto"
NEXT_PUBLIC_SITE_NAME="Meu App"
```

```ts
const segredo = process.env.API_SECRET; // só funciona no servidor
```

Regras importantes:

- Variáveis **sem** prefixo só existem no servidor (Server Components, actions, route handlers).
- Variáveis com prefixo **`NEXT_PUBLIC_`** são embutidas no JavaScript do navegador. **Nunca** coloque segredos nelas.
- Ordem comum: `.env.local` (máquina local, secreto), `.env` (padrões). Em produção, configure as variáveis no painel da hospedagem.
- Mude o `.env`? Reinicie o `npm run dev`.

## Validação com Zod

Nunca confie no que chega do cliente. **Zod** valida e tipa os dados ao mesmo tempo:

```bash
npm install zod
```

```ts
// app/actions.ts
"use server";

import { z } from "zod";
import { revalidatePath } from "next/cache";
import { db } from "@/lib/db";

const TarefaSchema = z.object({
  titulo: z.string().trim().min(3, "O título precisa de pelo menos 3 letras").max(100),
});

type Estado = { erro: string };

// assinatura compatível com useActionState (ver nota de frontend)
export async function criarTarefa(_anterior: Estado, formData: FormData): Promise<Estado> {
  const resultado = TarefaSchema.safeParse({ titulo: formData.get("titulo") });

  if (!resultado.success) {
    return { erro: resultado.error.issues[0].message };
  }

  await db.tarefa.create({ data: resultado.data });
  revalidatePath("/tarefas");
  return { erro: "" };
}
```

Nos Route Handlers é igual: `TarefaSchema.safeParse(await request.json())` e, se falhar, responda com `status: 400`.

## Autenticação (visão geral)

Implementar login do zero (hash de senha, sessões, cookies, recuperação de senha) é fácil de errar. O comum é usar uma biblioteca ou serviço:

| Opção | Tipo |
|---|---|
| **Auth.js** (NextAuth) | Biblioteca open source, login com Google/GitHub/e-mail, integra com Prisma/Drizzle |
| **Better Auth** | Biblioteca open source em TypeScript, bastante usada em projetos novos |
| **Clerk**, **Auth0** | Serviços prontos (pagos acima de um limite), com telas de login incluídas |
| **Supabase Auth** | Se você já usa o Supabase como banco |

O padrão de uso é sempre parecido:

1. A biblioteca guarda a sessão em um **cookie** seguro.
2. No servidor, você lê a sessão (ex.: `const session = await auth()`).
3. Em **toda** página protegida, action e route handler, verifique a sessão antes de agir:

```ts
"use server";
import { auth } from "@/auth"; // exemplo com Auth.js

export async function excluirTarefa(id: number) {
  const session = await auth();
  if (!session?.user) throw new Error("Não autorizado");
  // ...e confira se a tarefa pertence a esse usuário
}
```

## Middleware

O middleware roda **antes** de cada requisição chegar à rota. Serve para redirecionar, reescrever URLs e checagens rápidas (ex.: "tem cookie de sessão?"). Fica na raiz do projeto:

```ts
// middleware.ts
import { NextResponse, type NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const temSessao = request.cookies.has("session");

  if (!temSessao) {
    return NextResponse.redirect(new URL("/login", request.url));
  }
  return NextResponse.next();
}

// em quais rotas o middleware roda
export const config = {
  matcher: ["/dashboard/:path*", "/tarefas/:path*"],
};
```

Observações:

- Nas versões mais novas do Next.js esse arquivo foi renomeado para `proxy.ts` (função `proxy`). A ideia é a mesma; confira qual nome a sua versão usa.
- Middleware é uma **primeira barreira**, não a única. Continue verificando a sessão dentro das actions e route handlers, porque elas podem ser chamadas diretamente.
- Evite consultas pesadas ao banco no middleware: ele roda em toda requisição.

## Deploy

**Vercel** (empresa que mantém o Next.js) é o caminho mais simples:

1. Suba o projeto para o GitHub.
2. Em vercel.com, importe o repositório. Ele detecta o Next.js sozinho.
3. Cadastre as variáveis de ambiente (`DATABASE_URL` etc.) no painel.
4. Cada `git push` gera um novo deploy; cada pull request ganha uma URL de preview.

Outras opções:

- **Servidor próprio / VPS**: `npm run build` e depois `npm run start` (use PM2 ou similar para manter rodando).
- **Docker**: ative `output: "standalone"` no `next.config.ts` para gerar uma imagem menor.
- **Netlify, Railway, Render, Cloudflare**: também suportam Next.js, com diferenças em recursos avançados.

Checklist de deploy:

- [ ] `npm run build` passa sem erros localmente.
- [ ] Variáveis de ambiente configuradas na hospedagem (e nenhum segredo com `NEXT_PUBLIC_`).
- [ ] Banco de produção é acessível pela hospedagem. SQLite **não** serve em hospedagem serverless como a Vercel (o disco não é persistente); use PostgreSQL gerenciado (Neon, Supabase, Vercel Postgres etc.).
- [ ] Migrations aplicadas no banco de produção (ex.: `npx prisma migrate deploy`).

## Resumo

- **Route Handlers** (`app/api/.../route.ts`): endpoints HTTP para clientes externos.
- **Server Actions** (`"use server"`): mutações chamadas pelo seu próprio front; sempre valide e cheque autenticação.
- **Banco**: Prisma (mais simples) ou Drizzle (mais próximo do SQL), com uma instância única do cliente.
- **Segredos** em `.env.local`; só `NEXT_PUBLIC_` vai para o navegador.
- **Zod** para validar, uma biblioteca pronta para **autenticação**, **middleware** para redirecionar cedo.
- **Deploy** na Vercel ou em qualquer servidor Node com `build` + `start`.

Voltar ao início: [[Next.js - como usar o básico]].

## Referências

- [Route Handlers](https://nextjs.org/docs/app/getting-started/route-handlers-and-middleware)
- [Server Actions e mutações](https://nextjs.org/docs/app/getting-started/updating-data)
- [Variáveis de ambiente](https://nextjs.org/docs/app/guides/environment-variables)
- [Prisma com Next.js](https://www.prisma.io/docs)
- [Drizzle ORM](https://orm.drizzle.team/docs/overview)
- [Zod](https://zod.dev)
- [Auth.js](https://authjs.dev)
