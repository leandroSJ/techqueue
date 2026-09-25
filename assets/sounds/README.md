# Sons do TechQueue

Coloque nesta pasta os arquivos de áudio usados pelo botão **Chamar atenção**.

Arquivos configurados atualmente:

- `new-message-msn.mp3`
- `dbz-pt-br-kakaroto-voce-e-uma-besta.mp3`
- `tu-sai-de-problema.mp3`
- `luladrao-vai-entrar-o-grosso.mp3`

## Adicionando outro som

1. Envie o novo arquivo MP3 para esta pasta.
2. Abra `index.html`.
3. Procure por `SOUND_CATALOG`.
4. Adicione mais um objeto:

```js
{
  id: "meu-som",
  label: "Nome que aparece no TechQueue",
  file: "./assets/sounds/meu-som.mp3",
  price: 25
}
```

O campo `price` define o custo em XP. Use `0` para um som gratuito.

Os sons com preço maior que zero entram automaticamente na Lojinha.

Se o navegador não encontrar o MP3 ou bloquear a reprodução, o TechQueue usa o bip padrão como fallback.
