# isla-pbd-django52

Deck Slidev — **Django 5.2: A camada aplicacional sobre a base de dados**.

Sessão de 4 horas (teoria + prática) que introduz o Django 5.2 à turma de
**PBD (Programação de Bases de Dados)** — LEI, ISLA Gaia, regime nocturno,
em português. Pensada para estudantes de engenharia informática que já
dominam PL/pgSQL, triggers, transações e segurança, e que querem a
componente compensatória de *integração com aplicações* (até 20 pontos
percentuais) no Trabalho Prático. Encaixa na Aula 12 — "Integração com
aplicações".

## Correr localmente

```sh
npm install
npm run dev        # abre http://localhost:3030
```

## Exportar

```sh
npm run build          # site estático -> dist/
npm run export:pdf     # PDF para distribuir
```

## Tema

Tema com a identidade do ISLA (`style.css` + `global-bottom.vue`), derivado
do esquema de cores "Facet Isla" do `Isla.potx` — vinho `#5B1F2E`, carmesim
`#A40624`, malva `#BA969A`, creme — sobre a base `seriph` do Slidev. O
logótipo (`isla-logo-white.png`) está junto ao `slides.md`.

## Código de apoio

A prática usa a **polls app** do tutorial do Django sobre PostgreSQL — o
repositório `isla-pbd-polls` (`github.com/davidmgvaz/isla-pbd-polls`):

- ramo `main` — o esqueleto para acompanhar a aula
- ramo `solution` — a referência completa (inclui DRF, SQL bruto, signals)

## Adicionar ao repositório `davidmgvaz/slides`

Copiar esta pasta para a raiz do repositório, acrescentar um cartão no
`index.html` e fazer push — o workflow de deploy descobre automaticamente
qualquer pasta que contenha um `slides.md`.
