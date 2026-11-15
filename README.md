# Movie Explorer — Studio Ghibli

Trabalho individual de Programação Web II (ISUTC, LECC31).

Aplicação Web em Vue 3, Vue Router e Vite com API pública de filmes.

## Funcionalidades
- Listagem de filmes
- Pesquisa por título
- Filtros por realizador, ano e classificação
- Página de detalhes: cartaz, título, ano, realizador, produtor, duração e sinopse
- Paginação e tratamento de erros

## API pública
https://ghibliapi.vercel.app/films

Não é necessária chave de API. A classificação da API (0–100) é apresentada na escala 0–10. O catálogo é limitado aos filmes Studio Ghibli.

## Executar
Requer Node.js 22 ou superior.

    npm ci
    npm run dev

## Publicar no GitHub Pages
1. Settings → Pages → Source: GitHub Actions
2. Actions → Publicar Movie Explorer → Run workflow
3. Site: https://telcio-calege.github.io/Movie_Explorer/

## Branches
main e desenvolvimento

## Repositório
https://github.com/TELCIO-CALEGE/Movie_Explorer
