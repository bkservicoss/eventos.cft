<p align="center"><img src="assets/logo.svg" alt="Eventos.CFT" width="340"></p>

<p align="center"><b>Sistema de gestão e inscrição de eventos do CRT-ES: criar eventos, receber inscrições e acompanhar os participantes num só lugar.</b></p>

<p align="center">Streamlit (Python) · PostgreSQL · destino: Supabase + Streamlit Community Cloud</p>

<p align="center">
  <img src="https://img.shields.io/badge/status-prot%C3%B3tipo-F2A900?style=flat-square" alt="Status: protótipo">
  <img src="https://img.shields.io/badge/Python-Streamlit-1B365D?style=flat-square&logo=streamlit&logoColor=white" alt="Python + Streamlit">
  <img src="https://img.shields.io/badge/banco-PostgreSQL-1B365D?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL">
  <img src="https://img.shields.io/badge/uso-privado-5A6B85?style=flat-square" alt="Uso privado">
</p>

---

## Sumário

1. [Visão geral](#1-visão-geral)
2. [Estado do projeto](#2-estado-do-projeto)
3. [Modalidades e formulário](#3-modalidades-e-formulário)
4. [Configuração](#4-configuração)
5. [Estrutura do repositório](#5-estrutura-do-repositório)
6. [Identidade visual](#6-identidade-visual)
7. [Segurança e LGPD](#7-segurança-e-lgpd)
8. [Pendências](#8-pendências)

---

## 1. Visão geral

| Área | Quem usa | O que faz |
|---|---|---|
| **1. Criar Evento (Gestão)** | Equipe do CRT-ES | Cadastra nome, descrição, período (data e horário de início e fim), modalidade, banner de capa e vídeo de divulgação |
| **2. Área de Inscrição (Público)** | Participantes | Lista os eventos, mostra capa, descrição e vídeo, e abre o formulário de inscrição da modalidade, com validação dos campos obrigatórios |
| **3. Painel do Organizador (Controle)** | Equipe do CRT-ES | Lista os inscritos com a ficha complementar, remove inscrições, edita o evento e exclui o evento com confirmação |

## 2. Estado do projeto

| Item | Situação |
|---|---|
| Interface e fluxo das 3 áreas | ✅ Pronto (versão com PostgreSQL) |
| Formulário por modalidade | ✅ Pronto, com listas de escolas, campi do IFES, cursos técnicos e os 78 municípios do ES |
| Consultas SQL parametrizadas | ✅ Sem risco de injeção de SQL |
| Banco de dados | ⚠️ O código espera as tabelas `eventos` e `inscricoes`, mas o script de criação não está no repositório |
| Credenciais do banco | ⚠️ Escritas no código como exemplo (`DB_CONFIG`). Precisam ir para `st.secrets` |
| Login do organizador | ❌ Não existe. Qualquer pessoa acessa o painel e vê os dados dos inscritos |
| RLS (segurança por linha) | ❌ O papel é definido pelo menu lateral, então na prática não protege nada |
| Hospedagem | ❌ Ainda não publicado. Destino definido: Supabase + Streamlit Community Cloud |
| Exportar inscritos (CSV/Excel) | ❌ Não existe |
| Executável `.exe` | 🗑️ Descartado: um script Streamlit não funciona empacotado com PyInstaller |

## 3. Modalidades e formulário

**Campos de todas as modalidades:** nome completo, CPF, e-mail, telefone com DDD, idade e autorização de uso de imagem.

| Modalidade | Campos próprios (gravados em `detalhes_adicionais`, JSON) |
|---|---|
| 🏫 **CRT-ES na Escola** | Tipo de escola (IFES, CEETs, SEDU, Senai ES, outra) e unidade, curso técnico, estado, cidade, ano/semestre de conclusão, se trabalha ou estagia, se pretende atuar na área e expectativas |
| 🏢 **Sistema CFT/CRTs** | Se é funcionário do Sistema e de qual unidade (CFT ou CRT), ou como soube do evento |
| 👥 **Público Geral** | Profissão ou cargo, e empresa ou instituição |

## 4. Configuração

Nada é instalado no computador. O caminho previsto é este:

| Etapa | Onde | Como |
|---|---|---|
| Banco, login e imagens | Supabase | Criar o projeto, rodar o script SQL (pendente), ativar Auth para os organizadores e Storage para os banners |
| Credenciais | Streamlit Cloud → *App settings → Secrets* | Colar os dados de conexão no formato `st.secrets`. Nunca no código |
| Testes | GitHub → *Code → Codespaces → Create codespace on main* | Rodar `streamlit run eventoscrt.py` no terminal do Codespace |
| Publicação | share.streamlit.io → *Create app* | Escolher `bkservicoss/eventos.cft`, branch `main` e arquivo `eventoscrt.py` |

Dependências (`requirements.txt`): `streamlit>=1.28.0` e `psycopg2-binary>=2.9.0`.

## 5. Estrutura do repositório

```
eventos.cft/
├── assets/
│   ├── logo.svg          # logo horizontal, fundo claro
│   ├── logo-escuro.svg   # logo horizontal, fundo escuro
│   └── icone.svg         # ícone e favicon
├── eventoscrt.py         # app completo (Streamlit + PostgreSQL)
├── requirements.txt      # dependências Python
├── CLAUDE.md             # instruções do projeto para o Claude
├── README.md             # este arquivo
└── .gitignore
```

## 6. Identidade visual

| Elemento | Valor |
|---|---|
| Azul-marinho (principal) | `#1B365D` |
| Dourado (destaque) | `#F2A900` |
| Texto secundário | `#5A6B85` (fundo claro) · `#C9D3E3` (fundo escuro) |
| Fundo escuro | `#0E1117` |
| Fonte | Montserrat na logo; Helvetica Neue / Arial na interface |
| Símbolo | Calendário com check dourado, que representa a inscrição confirmada |

| Arquivo | Uso |
|---|---|
| [`assets/logo.svg`](assets/logo.svg) | README, documentos e telas claras |
| [`assets/logo-escuro.svg`](assets/logo-escuro.svg) | Modo escuro e fundos azul-marinho |
| [`assets/icone.svg`](assets/icone.svg) | Favicon, ícone do app e redes sociais |

## 7. Segurança e LGPD

| Tema | Regra |
|---|---|
| Segredos | Senhas e chaves só em `st.secrets`, nos Secrets do Codespaces ou no Vault do Supabase. `.streamlit/secrets.toml` e `.env` ficam no `.gitignore` |
| Acesso ao painel | Tem que exigir login (Supabase Auth). Não pode depender do menu lateral |
| Dados pessoais | O app coleta CPF, e-mail, telefone e idade. É preciso coletar só o necessário e exibir aviso de privacidade |
| Menores de idade | Estudantes podem ter menos de 18 anos. A LGPD (art. 14) exige consentimento de um responsável |
| Uso de imagem | O aceite deve ser opcional. Se for condição para a inscrição, o consentimento deixa de ser livre |
| Visibilidade | O repositório está **público** no GitHub. Recomenda-se torná-lo privado em *Settings → General → Danger Zone → Change visibility* |

## 8. Pendências

**Segurança e banco**
- [ ] Criar o script SQL (`eventos`, `inscricoes`, RLS, `ON DELETE CASCADE`, CPF único por evento)
- [ ] Levar as credenciais de `DB_CONFIG` para `st.secrets`
- [ ] Login do organizador com Supabase Auth
- [ ] Proteger a conexão (`psycopg2.connect` está fora do `try`, então uma falha derruba a página)
- [ ] Aviso de privacidade, consentimento do responsável para menores e aceite de imagem opcional

**Formulário e painel**
- [ ] Mostrar os campos em vermelho logo após a validação (falta `st.rerun()`)
- [ ] Destacar em vermelho "Profissão" e "Empresa" (Público Geral)
- [ ] Limpar o formulário após a inscrição e impedir duplicidade
- [ ] Validar formato de CPF, e-mail e telefone
- [ ] Mostrar só eventos vigentes na área pública
- [ ] Editar datas com seletor de data, e permitir trocar banner e modalidade
- [ ] Liberar a busca nas listas longas (o CSS bloqueia a digitação)
- [ ] Exportar inscritos em CSV/Excel
- [ ] Banners no Supabase Storage em vez de binário no banco
- [ ] Trocar `use_container_width` (obsoleto) e o `except:` sem tipo

**Infraestrutura e marca**
- [ ] `.devcontainer/` para testar no Codespaces
- [ ] Publicar no Streamlit Community Cloud
- [ ] Aplicar a identidade no app (`.streamlit/config.toml`, logo na barra lateral, favicon)
- [ ] Tornar o repositório privado

---

<p align="center"><sub>Projeto privado · BK Serviços, Produção &amp; Comunicação</sub></p>
