# CLAUDE.md — Eventos.CFT

App de gestão e inscrição de eventos feito para o CRT-ES (Conselho Regional dos Técnicos Industriais do Espírito Santo), em Streamlit (Python) com banco PostgreSQL. Tem três áreas: criar evento, inscrição pública e painel do organizador.

Projeto privado da BK Serviços, Produção & Comunicação.

## Como o dono trabalha

- **Não instala nada no computador.** Tudo roda na nuvem: GitHub Codespaces para testar, GitHub Actions para tarefas automáticas, Supabase para banco e login, Streamlit Community Cloud para manter o app no ar. Nunca orientar a instalar Python, venv, PyInstaller ou programas no PC.
- Fala português do Brasil: respostas, commits, README, textos da interface e comentários.
- Prefere passo a passo curto, com o caminho exato dos menus (por exemplo: *Settings → Secrets and variables → Codespaces*).
- Quando houver decisões a tomar, apresentar as opções como checklist clicável.

## Mapa

| Caminho | O que é |
|---|---|
| `eventoscrt.py` | App inteiro (Streamlit): tema, listas do formulário, as 3 áreas e acesso ao banco (`executar_query`) |
| `requirements.txt` | Dependências (`streamlit`, `psycopg2-binary`) |
| `assets/` | Identidade visual: `logo.svg`, `logo-escuro.svg`, `icone.svg` |
| `README.md` | Documentação no padrão BK Serviços, com estado do projeto e pendências |
| `CLAUDE.md` | Este arquivo |

Ainda **não existem** no repositório: script SQL do banco (tabelas `eventos` e `inscricoes`, RLS), `.devcontainer/`, `.streamlit/config.toml` e workflows do Actions.

## Como rodar

- Hoje não há workflow nem compilação. O app roda com `streamlit run eventoscrt.py`, mas isso deve ser feito no **Codespaces**, nunca no PC do dono.
- Destino combinado: **Supabase** (banco, Auth e Storage) + **Streamlit Community Cloud** (app), com as credenciais nos Secrets da hospedagem (`st.secrets`).
- O executável `.exe` gerado com PyInstaller foi descartado: um script Streamlit não sobe o servidor quando vira `.exe`.

## Regras do código

- Toda consulta SQL usa parâmetros (`%s`). Nunca montar SQL com f-string ou concatenação.
- Os campos específicos de cada modalidade ficam em JSON na coluna `detalhes_adicionais`. Ao criar um campo novo, gravá-lo ali, sem criar colunas soltas.
- As três modalidades (`CRT-ES na Escola`, `Sistema CFT/CRTs`, `Público Geral`) são escritas exatamente assim em todo o código. Mudar o nome de uma delas exige migrar os dados.
- Os textos de interface ficam em português, no tom institucional do CRT-ES.
- O acesso ao painel do organizador **não pode** depender só do menu lateral. A proteção tem que vir de login (Supabase Auth) e de RLS no banco.

## Segurança

- Nunca versionar tokens, chaves, senhas, `.env` ou `.streamlit/secrets.toml`. Credenciais do banco ficam em `st.secrets`, nos Secrets do Codespaces ou no Vault do Supabase.
- Se o dono colar um token ou senha no chat: não usar, não repetir, orientar a gerar outro na hora e salvá-lo direto nos Secrets.
- O app coleta dados pessoais (CPF, e-mail, telefone, idade), inclusive de estudantes que podem ser menores de idade. Toda mudança deve respeitar a LGPD: coletar só o necessário, mostrar aviso de privacidade e pedir consentimento livre (o aceite de uso de imagem não pode ser condição para se inscrever).

## Identidade visual

| Elemento | Valor |
|---|---|
| Azul-marinho (principal) | `#1B365D` |
| Dourado (destaque) | `#F2A900` |
| Texto secundário | `#5A6B85` (claro) · `#C9D3E3` (escuro) |
| Fundo escuro | `#0E1117` |
| Fonte | Montserrat (logo), Helvetica Neue / Arial (interface) |
| Logo | `assets/logo.svg` (fundo claro), `assets/logo-escuro.svg` (fundo escuro), `assets/icone.svg` (ícone e favicon) |

O ícone é um calendário com um check dourado, que representa a inscrição confirmada. No nome, o trecho ".CFT" fica sempre em dourado.

## README

Seguir o padrão dos repositórios da BK Serviços: logo centralizada no topo, frase de apresentação em negrito, selos (shields.io) nas cores da marca, Sumário numerado, seções em tabelas, estrutura do repositório em árvore, segurança, pendências com `- [ ]` e o rodapé `Projeto privado · BK Serviços, Produção & Comunicação`. Toda mudança no repositório deve ser registrada no README no mesmo trabalho.

## Repositórios novos

Sempre que o dono criar um repositório novo, perguntar se ele quer criar o `CLAUDE.md`.
