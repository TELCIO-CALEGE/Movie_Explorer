# SHADOW'S_MOVIES

Aplicação de descoberta de filmes em Vue 3, Vue Router e Vite, para Programação Web II.

## Funcionalidades
- Página inicial cinematográfica e interface responsiva
- Listagem, pesquisa por título, filtros por realizador/ano/classificação
- Ordenação, paginação e página individual de detalhes
- Tratamento de erros e navegação

## API
Studio Ghibli API: https://ghibliapi.vercel.app/films — pública e sem chave. Catálogo limitado aos filmes do Studio Ghibli; sinopses em inglês.

## Executar
Node.js 22.18+ ou 24.12+.

    npm ci
    npm run dev

## GitHub Pages
Settings → Pages → Source: GitHub Actions. A branch main publica automaticamente.
Vite utiliza base relativa, funcionando antes e depois da renomeação do repositório.

- Antes: https://telcio-calege.github.io/Movie_Explorer/
- Após renomear para SHADOWS_MOVIES: https://telcio-calege.github.io/SHADOWS_MOVIES/

O nome visual SHADOW'S_MOVIES inclui apóstrofo; o nome do repositório não.

## Branches
main e desenvolvimento.
