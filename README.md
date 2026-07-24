# SGFC — Sistema de Gestão de Frotas e Condutores

**Gestão inteligente para sua frota e seus condutores.**

SGFC 2.0.0, desenvolvido em Python 3.11, CustomTkinter e MySQL.

## O que esta base já entrega

- arquitetura em camadas (`models`, `repositories`, `services`, `views` e `utils`);
- configuração independente dos ambientes Produção e Teste;
- criação e atualização versionada do banco;
- login, sessão, alteração obrigatória da senha inicial e permissões no banco;
- janela principal com menu superior clássico;
- cadastros preparados no banco: empresas, filiais, colaboradores, condutores/CNHs e frota;
- histórico de renovação, termos, vínculos de veículos, auditoria e exclusão lógica;
- estrutura pronta para dashboard, relatórios, backup e atualização automática;
- quatro modelos DOCX legados preservados em `assets/templates/legacy`.
- importação em massa de Colaboradores e CNHs via Excel, com pré-visualização,
  validação, atualização opcional e relatório de resultados;
- modelo Excel disponível em `assets/templates/modelo_importacao_sgfc.xlsx`.
- cadastro completo de empresas, com pesquisa, edição, ativação/inativação,
  logotipo, modelo DOCX e validações fiscais;
- cadastro completo de filiais como lojas de atuação independentes das empresas;
- exclusão lógica separada das permissões de gerenciamento;
- auditoria das inclusões, alterações e exclusões de empresas e filiais.
- cadastro global da Frota, sem vínculo com empresa e com filial de atuação
  opcional;
- pesquisa e filtro por situação, cadastro, edição e exclusão lógica de veículos;
- dados de placa, marca, modelo, tipo, anos, cor, combustível, RENAVAM, chassi,
  quilometragem, situação e observações;
- histórico permanente de criação, alterações e exclusão dos veículos;
- permissões independentes para visualizar, cadastrar, editar e excluir a Frota.
- cadastro completo de Colaboradores com Empresa registrada e Filial de atuação
  selecionadas independentemente;
- CPF, RG, órgão expedidor, matrícula, código Movere, cargo, departamento,
  gestor, telefone, e-mail, situação e observações;
- pesquisa, filtro por situação, histórico, auditoria e exclusão lógica de
  colaboradores.
- cadastro completo de Condutores/CNHs vinculado ao Colaborador;
- validade, dias restantes e situação calculados automaticamente;
- histórico permanente de renovações de validade e categoria;
- permissões específicas, auditoria e exclusão lógica de CNHs.
- geração de Termos Veiculares em DOCX e PDF;
- cabeçalho, logotipo e dados obtidos da Empresa registrada do colaborador;
- seleção de até oito veículos da Frota global, independentemente da Empresa;
- histórico permanente com placas, usuário, data e arquivos gerados.
- changelog atualizado e disponível pelo menu `Ajuda > Changelog`.
- Central de Relatórios de Colaboradores, CNHs, Frota e Termos Veiculares;
- filtros independentes por Empresa e Filial, além de situação, vencimento e período;
- pré-visualização e exportação profissional para Excel e PDF.
- backup manual do ambiente ativo em arquivo `.sgfcbak`, com SQL, manifesto e
  validação SHA-256;
- restauração exclusiva do Administrador, com confirmação pelo nome do banco e
  backup automático de segurança antes da operação;
- histórico e auditoria de backups e restaurações;
- Termo Veicular remodelado, numerado sequencialmente e diagramado em A4 com
  cabeçalho empresarial, tabela de veículos, cláusulas e assinaturas.
- Central de Atualizações integrada ao GitHub Releases, com verificação
  automática/manual, notas e atualizações opcionais ou obrigatórias;
- scripts de compilação `PyInstaller onedir` e instalador Inno Setup 7.

## Instalação para desenvolvimento

```bat
py -3.11 -m venv .venv
.venv\Scripts\activate
py -m pip install -r requirements.txt
```

Copie `.env.example` para `.env` e ajuste os bancos. O MySQL deve estar acessível e o usuário informado precisa ter permissão para criar/alterar o banco no primeiro uso.

```bat
copy .env.example .env
py main.py
```

No primeiro início, o SGFC cria as tabelas e os dados fundamentais automaticamente.

- usuário inicial: `admin`
- senha inicial: `admin123`

A troca dessa senha é obrigatória no primeiro acesso.

## Estrutura

```text
SGFC_1.0.0/
├── app/
│   ├── config/          configurações e ambientes
│   ├── core/            banco, migrações e sessão
│   ├── models/          entidades de domínio
│   ├── repositories/    acesso ao MySQL
│   ├── services/        regras de negócio
│   ├── utils/           segurança, caminhos e logs
│   └── views/           interface CustomTkinter
├── assets/templates/    modelos de documentos
├── database/migrations/ migrações SQL ordenadas
├── docs/                escopo e decisões
├── tests/               testes automatizados
├── .env.example
├── main.py
└── requirements.txt
```

## Etapa atual

Os módulos Empresas, Filiais, Frota, Colaboradores e Condutores/CNHs estão
disponíveis no menu `Cadastros`. A geração e o histórico de Termos Veiculares
estão disponíveis no menu `Documentos`.
Empresa e filial são cadastros independentes: a empresa identifica o vínculo
empregatício e a filial identifica somente a loja de atuação. O colaborador
poderá combinar livremente uma empresa e uma filial.
O cadastro da Frota é global e não possui vínculo com Empresa. Todos os usuários
com permissão de Frota visualizam os mesmos veículos, independentemente da
unidade. A Filial é opcional e informa somente a loja onde o veículo está
atuando, sem restringir sua visibilidade.
O perfil DFAC pode consultar, incluir e editar, mas não visualiza o botão de
exclusão.
O perfil Visualizador permanece restrito ao Dashboard.

## Backup e restauração

O módulo está disponível em `Sistema > Backup e Restauração` somente para o
perfil Administrador. O computador precisa ter `mysqldump` e `mysql` disponíveis
no `PATH`; instalações padrão do MySQL Server 8.0/8.4 e XAMPP no Windows também
são localizadas automaticamente.

Antes de restaurar, o SGFC valida a integridade e o banco indicado no manifesto,
exige a digitação exata do banco ativo e cria um ponto de retorno automático.
Após uma restauração concluída, reinicie a aplicação.

## Atualização, compilação e instalador

A Central de Atualizações fica em `Sistema > Verificar atualizações`. Por padrão,
o SGFC consulta:

```text
https://raw.githubusercontent.com/pv15suporte-gif/SGFC/main/version.js
```

Se o repositório definitivo tiver outro nome, defina `SGFC_VERSION_URL` ou
altere `VERSION_URL` em `app/config/app_config.py`. O arquivo `version.js`
incluído na raiz é o modelo que deve ser publicado no GitHub.

Para compilar em `onedir` no Windows:

```bat
build_onedir.bat
```

Depois, com Inno Setup 7 instalado:

```bat
build_installer.bat
```

O instalador será criado em `dist_installer\SGFC_Setup_v2.0.0.exe`.

## Etapa atual

A versão 2.0.0 inclui Atualizador via GitHub Releases, build onedir e instalador
Inno Setup 7, preservando Backup/Restauração e os Termos Veiculares remodelados.
