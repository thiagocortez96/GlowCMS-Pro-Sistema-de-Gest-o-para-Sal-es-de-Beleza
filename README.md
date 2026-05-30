# GlowCMS-Pro-Sistema-de-Gest-o-para-Sal-es-de-Beleza
Sistema web completo para gerenciamento de salões de beleza, desenvolvido em HTML puro com integração ao Supabase como banco de dados em nuvem. Leve, sem dependência de servidor e fácil de hospedar.
## ✨ Funcionalidades
 
- **Agendamentos** — cronograma diário com suporte a atendimentos paralelos, valor adicional por serviço e produtos vinculados
- **Profissionais** — cadastro da equipe com agenda individual por dia
- **Serviços e Preços** — gerenciamento de valores base e percentual de comissão
- **Clientes** — cadastro completo com histórico de atendimentos e total gasto
- **Produtos** — cadastro de produtos para vincular aos atendimentos
- **Finanças & Dashboard** — gráficos de faturamento diário e mensal, comissões por profissional com exportação em PDF, extrato de caixa e controle de despesas
- **Configurações** — alteração de senha administrativa e backup em arquivo JSON
- **Confirmação via WhatsApp** — envio de mensagem de confirmação direto ao cliente
- **Sincronização em nuvem** — todos os dados salvos em tempo real no Supabase
---
 
## 🚀 Como usar
 
### 1. Crie um projeto no Supabase
 
Acesse [supabase.com](https://supabase.com), crie uma conta e um novo projeto. Após criado, vá em **Settings → API** e copie:
 
- **Project URL**
- **anon public key**
### 2. Crie as tabelas no banco
 
No **SQL Editor** do Supabase, execute o script abaixo:
 
```sql
create table config (
  chave text primary key,
  valor text not null
);
insert into config (chave, valor) values ('senha_admin', '1234');
 
create table profissionais (
  id text primary key,
  nome text not null
);
 
create table servicos (
  id text primary key,
  nome text not null,
  preco numeric not null,
  comissao integer not null
);
 
create table clientes (
  id text primary key,
  nome text not null,
  email text,
  celular text not null
);
 
create table agendamentos (
  id text primary key,
  cliente_id text references clientes(id),
  servico_id text references servicos(id),
  profissional_id text references profissionais(id),
  data date not null,
  hora text not null,
  valor_extra numeric default 0,
  produtos_usados text
);
 
create table despesas (
  id text primary key,
  nome text not null,
  valor numeric not null,
  data date not null
);
 
create table produtos (
  id text primary key,
  nome text not null,
  preco numeric not null
);
 
-- Libera acesso pela anon key
alter table config        enable row level security;
alter table profissionais enable row level security;
alter table servicos      enable row level security;
alter table clientes      enable row level security;
alter table agendamentos  enable row level security;
alter table despesas      enable row level security;
alter table produtos      enable row level security;
 
create policy "acesso_total" on config        for all using (true) with check (true);
create policy "acesso_total" on profissionais for all using (true) with check (true);
create policy "acesso_total" on servicos      for all using (true) with check (true);
create policy "acesso_total" on clientes      for all using (true) with check (true);
create policy "acesso_total" on agendamentos  for all using (true) with check (true);
create policy "acesso_total" on despesas      for all using (true) with check (true);
create policy "acesso_total" on produtos      for all using (true) with check (true);
```
 
### 3. Configure as credenciais no arquivo
 
Abra o `index.html` e localize as linhas:
 
```javascript
const SUPABASE_URL  = "SUA_URL_SUPABASE_AQUI";
const SUPABASE_ANON = "SUA_CHAVE_ANON_AQUI";
```
 
Substitua pelos valores copiados no passo 1.
 
### 4. Abra no navegador
 
Basta abrir o `index.html` diretamente no navegador ou hospedar em qualquer serviço estático como [Vercel](https://vercel.com) ou [Netlify](https://netlify.com).
 
A senha padrão de acesso é **1234**. Troque imediatamente em **Configurações → Alterar Senha**.
 
---
 
## 🛠️ Tecnologias utilizadas
 
| Tecnologia | Uso |
|---|---|
| HTML + JavaScript puro | Interface e lógica do sistema |
| [Tailwind CSS](https://tailwindcss.com) | Estilização via CDN |
| [Lucide Icons](https://lucide.dev) | Ícones |
| [Chart.js](https://chartjs.org) | Gráficos financeiros |
| [jsPDF](https://github.com/parallax/jsPDF) | Geração de PDF de comissões |
| [Supabase](https://supabase.com) | Banco de dados em nuvem |
 
---
 
## ⚠️ Atenção ao publicar
 
Nunca suba o arquivo com as credenciais reais do Supabase. Mantenha os placeholders `SUA_URL_SUPABASE_AQUI` e `SUA_CHAVE_ANON_AQUI` na versão pública do repositório.
 
---
 
## 📄 Licença
 
MIT — livre para usar, modificar e distribuir.
