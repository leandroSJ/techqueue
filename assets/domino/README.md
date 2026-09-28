# Imagens das pedras de dominó

Coloque nesta pasta as 28 imagens PNG do jogo.

O TechQueue procura automaticamente por:

- `a-b.png`
- se não encontrar, tenta `b-a.png`

Então os arquivos podem seguir exatamente o padrão que você já possui, por exemplo:

```
0-0.png
0-1.png
1-1.png
2-0.png
2-1.png
2-2.png
3-0.png
3-1.png
3-2.png
3-3.png
4-0.png
4-1.png
4-2.png
4-3.png
4-4.png
5-0.png
5-1.png
5-2.png
5-3.png
5-4.png
5-5.png
6-0.png
6-1.png
6-2.png
6-3.png
6-4.png
6-5.png
6-6.png
```

Caminho esperado no repositório:

```
assets/domino/
```

Se uma imagem estiver ausente, o jogo continua funcionando e mostra a pedra numérica antiga como fallback.

As pedras comuns são exibidas na horizontal. As buchas (0-0, 1-1, 2-2 etc.) ficam na vertical.
