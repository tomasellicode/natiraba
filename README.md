# Hanami — Nossa Jornada 🌸

Um pequeno jogo 2D de exploração, feito como presente, protagonizado por **Guilherme Tomaselli** e **Natália Maba**.
Roda inteiro no navegador (computador e celular) e pode ser publicado na Vercel como site estático.

- **Tecnologia:** React + TypeScript + Vite + Phaser 3.
- **Arte:** pixel art gerada pelo próprio código (personagens, árvores, lanternas, torii, pontes, água…). Nenhuma imagem externa é necessária.
- **Som:** trilha instrumental e efeitos gerados com Web Audio API (só começam depois de um clique).
- **Progresso:** salvo no `localStorage` do navegador.

---

## Executar localmente

```bash
npm install
npm run dev
```

Abra o endereço mostrado no terminal (normalmente `http://localhost:5173`).

Para gerar a versão final: `npm run build` (o resultado vai para `dist/`). Para conferir: `npm run preview`.

---

## Onde personalizar (sem mexer na lógica)

| O quê | Arquivo |
|---|---|
| Nome do jogo, frase de abertura, mensagem final, data misteriosa, áudio | `src/data/settings.ts` |
| Nomes, cores e sprites dos personagens | `src/data/characters.ts` |
| Fotografias, títulos, datas e legendas | `src/data/memories.ts` |
| Capítulos, títulos e objetivos | `src/data/chapters.ts` |
| Frases de carinho, mensagens das pétalas, textos de placas e segredos | `src/data/dialogues.ts` |
| Respostas dos minijogos (lanternas e cartões) | `src/data/minigames.ts` |
| Mapas das regiões e posição dos objetos | `src/game/regions/index.ts` |

### 1. Inserir fotografias
1. Copie as fotos para `public/assets/fotos/` com os nomes `foto1.jpeg` … `foto8.jpeg`.
2. Pronto. Elas aparecem nas molduras do cenário, na janela da lembrança e no diário.

Se uma foto faltar, o jogo mostra uma moldura vazia — nada quebra.

**A lembrança de novembro** (`memoria-novembro`, ao lado da placa `?? / 11`) começa como uma página "a descobrir", sem foto. Depois do grande dia, basta colocar `public/assets/fotos/foto_novembro.jpg` e reescrever a legenda: a foto passa a aparecer no lugar do mistério.
Para fotos verticais, use `focus: 'center top'` na lembrança para não cortar rostos na miniatura (a janela grande sempre mostra a foto inteira, sem distorcer).

### 2. Modificar legendas
Em `src/data/memories.ts`, troque `title`, `date` (opcional) e `caption` de cada lembrança.

### 3. Adicionar lembranças
Copie um bloco em `src/data/memories.ts`, dê um `id` novo, aponte `image` para o arquivo e escolha:
- `region`: em qual capítulo ela fica;
- `x`, `y`: posição em tiles (1 tile = 16 px; veja o mapa da região em `src/game/regions/index.ts`);
- `placement`: como a moldura aparece — `cavalete`, `arvore`, `mural`, `caixa`, `ponte`, `mobile`, `petalas` ou `santuario`;
- `optional: true` se não for necessária para avançar.

> No Capítulo 2, todas as fotos **não opcionais** daquela região precisam ser encontradas para abrir a saída.

### 4. Alterar frases
Em `src/data/dialogues.ts`:
- `affection` — frases que surgem durante a jornada;
- `petalMessages` — as mensagens das pétalas especiais (as 5 primeiras ficam no santuário; a 6ª é secreta);
- `texts` — placas, objetos, segredos. Use `{natalia}` e `{guilherme}` para inserir os nomes.

### 5. Modificar a data misteriosa
Em `src/data/settings.ts`:

```ts
mysteryDay: '??',   // troque por '15', por exemplo, quando decidir
mysteryMonth: '11',
```

Por padrão a placa final mostra **`?? / 11`**. Não existe contagem regressiva.
A mensagem final fica em `finalMessage`.

### 6. Substituir os sprites
Os personagens são desenhados automaticamente a partir das cores em `src/data/characters.ts` (pele, cabelo, roupas, mochila…). Mudar as cores já muda o visual.

Para usar um desenho próprio:
1. Crie uma sprite sheet PNG com fundo transparente, quadros de **32×40 px**, organizada assim:
   - **linhas:** 0 = olhando para baixo, 1 = esquerda, 2 = direita, 3 = cima
   - **colunas:** 0 parado A · 1 parado B · 2 piscando · 3–6 caminhada · 7 interagindo · 8–9 descobrindo · 10 sentado · 11–12 comemorando
2. Salve em `public/assets/personagens/natalia.png` (ou `guilherme.png`).
3. Em `characters.ts`, descomente a linha `spriteSheet: { url: …, frameWidth: 32, frameHeight: 40 }`.

Outros tamanhos de quadro funcionam (ajuste `frameWidth`/`frameHeight`); os pés devem ficar na parte de baixo do quadro.
Se o arquivo não existir, o jogo volta para o sprite gerado.

### 7. Música própria (opcional)
Coloque um arquivo em `public/assets/audio/` e preencha `audio.musicFile` em `settings.ts`. Use só músicas que você tem direito de usar.

---

## Publicar na Vercel

1. Envie esta pasta (`hanami/`) para um repositório no GitHub.
2. Na Vercel: **Add New → Project**, importe o repositório.
3. Se o repositório tiver outras pastas, defina **Root Directory** como `hanami`.
4. A Vercel detecta Vite automaticamente (o `vercel.json` já define `npm run build` e a pasta `dist`). Clique em **Deploy**.

Não há backend, banco de dados nem variáveis de ambiente. É um site estático.

Alternativa pela linha de comando: `npx vercel` dentro da pasta.

---

## Controles

**Computador:** WASD ou setas para caminhar · Shift apressa o passo · **E** / Espaço / Enter interagem · **Q** troca o personagem controlado · **J** diário · **M** mapa · **Esc** fecha painéis / abre opções.

**Celular:** direcional à esquerda, botão de pétala à direita (mostra o que dá para fazer), botão de troca de personagem, ou toque direto nos objetos próximos.

---

## Sobre o salvamento

O progresso (capítulos, regiões visitadas, fotos, pétalas, minijogos e preferências de áudio) fica no `localStorage` **deste navegador, neste aparelho**. Ele não é sincronizado entre dispositivos: abrir no celular começa do zero. Em janela anônima o progresso dura só aquela sessão.

Para recomeçar: **Opções (Esc) → reiniciar a jornada** (pede confirmação), ou "começar do início" na tela de abertura.

---

## A jornada

1. **11 de abril** — a clareira da grande cerejeira; o fio vermelho abre a trilha.
2. **O caminho das lembranças** — fotos à vista e escondidas (há uma passagem entre os bambus).
3. **A ponte dos pequenos detalhes** — acender as lanternas do lago na ordem do poema.
4. **O jardim dos bonsais** — ordenar os cartões de lembranças (arrastar ou tocar).
5. **O santuário das pétalas** — reunir pétalas especiais e acender, juntos, as duas lanternas de pedra.
6. **O destino desconhecido** — o portão iluminado e a placa `?? / 11`.

Segredos opcionais: a raposinha, a carpa dourada, a lanterna esquecida, a foto sob as pétalas e a caixinha escondida.
