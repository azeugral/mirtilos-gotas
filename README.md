# Mirtilos com gotas: protótipo de hero

Hero com mirtilos molhados caindo, renderizados em 4K no Blender (Cycles) e tocados quadro a quadro pelo scroll.

## Como funciona
- `../tools/cena.py` monta a cena: frutas com pruína e película d'água, gotas presas na casca, gotas que se soltam e ficam para trás com o arrasto do ar, e gotas soltas no ar em várias profundidades. O movimento é um loop perfeito de 120 quadros.
- `../tools/quadros.py` gera três sequências WebP: `q/4k` (3840×2160), `q/hd` (1920×1080) e `q/m` (1080×1920, recorte vertical do 4K).
- `index.html` escolhe a sequência pela tela, carrega em camadas (1 a cada 16 quadros primeiro) e desenha num canvas. O quadro é calculado assim: queda lenta parada + progresso do scroll × 120, com inércia curta. Os três textos trocam em pontos fixos do progresso.
- Com `prefers-reduced-motion`, a queda parada é desligada e o quadro só anda com o scroll.

## Refazer
```
blender -b -P tools/cena.py -- --modo final --saida render/4k --frames 120
python tools/quadros.py render/4k site/q
```
Leva cerca de 30 s por quadro em 4K na GTX 1660 SUPER, ou seja, uns 60 min para os 120 quadros.
