# Missão Pulso Zero

Minijogo 3D do **Trabalho Prático 1 de Computação Gráfica** (IFSULDEMINAS, Campus Machado, prof. Renan).
Um drone de manutenção recupera **5 células de energia**, desvia de **4 obstáculos** e alcança o **portal de saída**, que só abre depois da coleta.

**Integrantes:** Nome 1 e Nome 2 *(substituir antes da entrega)*

## Instalação

Requer Python 3.13, uma área de trabalho gráfica e driver OpenGL com contexto 2.1 (compatibilidade).

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate      Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
```

No Ubuntu, se faltar a biblioteca GLU: `sudo apt install libglu1-mesa`.
`PyOpenGL_accelerate` é opcional; se falhar a instalação, remova-o do `requirements.txt`.

## Execução

```bash
python missao_pulso_zero.py
```

Versão em notebook (mesmo código, passo a passo): abra `Missao_Pulso_Zero.ipynb` em um Jupyter **local** e execute as células em ordem.
Para regenerar os PNGs de `texturas/`: `python missao_pulso_zero.py --salvar-texturas`.

## Controles

| Tecla | Ação |
|---|---|
| W, A, S, D | Movimentar o drone (relativo à câmera) |
| Setas ← → ↑ ↓ | Girar / subir e descer a câmera |
| Roda do mouse | Aproximar / afastar a câmera |
| C | Alternar câmera: seguindo / aérea |
| L | Ligar / desligar a fonte de luz |
| Q, E | Mover a luz ao redor da arena |
| R | Reiniciar a missão |
| Esc | Encerrar |

## Estrutura

```
missao_pulso_zero.py        código do jogo
Missao_Pulso_Zero.ipynb     o mesmo código, organizado em etapas
requirements.txt            dependências
texturas/                   piso.png, painel.png, celula.png
```

## Origem das texturas

As três texturas foram **desenhadas por código com Pillow** (funções `criar_imagem_*`), sem imagens de terceiros.
Os arquivos em `texturas/` são a saída dessas funções. Se um deles for trocado por outra imagem, o jogo passa a usá-lo.

## Técnicas e onde aparecem

| Requisito | Aplicação |
|---|---|
| Primitivas | `GL_QUADS` (caixas, hélices), `GL_TRIANGLES` (sensor do drone), `GL_LINES` / `GL_LINE_LOOP` (guias, lasers), `GL_POINTS` (estrelas) |
| Objeto composto e hierarquia | drone: corpo, cúpula, sensor, 4 braços, motores, anéis; hélice gira no próprio centro e acompanha o drone |
| Câmera e projeção | `gluLookAt` (seguindo / aérea) e `gluPerspective`; `GL_DEPTH_TEST` para oclusão |
| Translação, rotação, escala | movimento e rumo do drone, giro de hélices e células, escala das peças e da coleta |
| Animações com `dt` | rotação contínua, flutuação (seno), coleta, abertura do portal, alerta de colisão |
| Texturas | piso (repetido 12 x 12), painel (paredes, caixas, portal), rótulo das células; UV pelo tamanho real da face |
| Iluminação | luz pontual com atenuação, ambiente, difusa e especular; normais por face; tecla L |
| Regras | limites, colisão círculo-caixa, coleta, contador, conclusão, reinício (R) |

## Limitações

- OpenGL clássico: a luz é calculada por vértice (as faces são subdivididas para compensar) e não há sombras reais; os discos escuros no chão são só um recurso visual.
- O drone voa em altura constante; a colisão é um círculo no plano horizontal contra caixas.
- O texto do HUD usa a fonte padrão do pygame.
