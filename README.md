# **Lista de Tokens Nativos do Flow**

O registro de tokens nativos do Flow mantido pela comunidade.

Seguindo a especificação da lista de tokens Uniswap encontrada aqui: **https://github.com/Uniswap/token-lists**

## **Como obter a lista de tokens nativos do Flow**

Mainnet:

- Do github: **https://raw.githubusercontent.com/FlowFans/flow-token-list/main/src/tokens/flow-mainnet.tokenlist.json**
- Do jsdelivr CDN: **https://cdn.jsdelivr.net/gh/FlowFans/flow-token-list@main/src/tokens/flow-mainnet.tokenlist.json**

Testnet:

- Do github: **https://raw.githubusercontent.com/FlowFans/flow-token-list/main/src/tokens/flow-testnet.tokenlist.json**
- Do jsdelivr CDN: **https://cdn.jsdelivr.net/gh/FlowFans/flow-token-list@main/src/tokens/flow-testnet.tokenlist.json**

## **Usando o pacote NPM**

### **Instalação**

```
bashCopy code
npm install flow-native-token-registry

```

```
bashCopy code
yarn add flow-native-token-registry

```

### **Exemplos**

### Consultar tokens disponíveis

```
typescriptCopy code
import { TokenListProvider } from 'flow-native-token-registry';

new TokenListProvider().resolve().then((tokens) => {
  const tokenList = tokens.filterByTag('nft').getList();
  console.log(tokenList);
});

```

### Renderizar ícone para token em React

```
typescriptCopy code
import React, { useEffect, useState } from 'react';
import { TokenListProvider, TokenInfo } from 'flow-native-token-registry';

export const Icon = () => {
  const [tokenMap, setTokenMap] = useState<Map<string, TokenInfo>>(new Map());

  useEffect(() => {
    new TokenListProvider().resolve().then(tokens => {
      const tokenList = tokens.getList();

      setTokenMap(tokenList.reduce((map, item) => {
        map.set(`${item.symbol}`, item);
        return map;
      }, new Map()));
    });
  }, [setTokenMap]);

  const token = tokenMap.get('FLOW');
  if (!token || !token.logoURI) return null;

  return (<img src={token.logoURI} />);
};

```

## **Adicionando novo token**

Para enviar um novo token:

- Certifique-se de que o título do seu pull request comece com **`feat(NewToken):`**
- Modificações só são permitidas dentro da pasta **`token-registry`**
- Certifique-se de que o nome do diretório do seu token está neste formato: **`A.{tokenMainnetAddress}.{tokenContractName}`**
- Certifique-se de que o nome do seu token não é o mesmo de um existente
- Você só pode adicionar um token por vez
- Você só pode adicionar os arquivos abaixo:

```
scssCopy code
logo.png (obrigatório)
token.json (obrigatório)
logo-large.png
logo.svg
testnet.token.json

```

- Arquivos PNG em seu pull request devem ser menores que 20KB
- Arquivos JSON em seu pull request devem estar de acordo com o **[esquema](https://github.com/FlowFans/flow-token-list/blob/main/src/schemas/token.schema.json)**
    - **`logoURI`**
        - Deve ser enviado para este repositório, então **`logoURI`** deve apontar para
            - jsdelivr CDN: **[https://cdn.jsdelivr.net/gh/FlowFans/flow-token-list@main/token-registry/${YOUR_DIRECTORY_NAME}/${YOUR_LOGO}](https://cdn.jsdelivr.net/gh/FlowFans/flow-token-list@main/token-registry/$%7BYOUR_DIRECTORY_NAME%7D/$%7BYOUR_LOGO%7D)** ou
            - github: **[https://raw.githubusercontent.com/FlowFans/flow-token-list/main/token-registry/${YOUR_DIRECTORY_NAME}/${YOUR_LOGO}](https://raw.githubusercontent.com/FlowFans/flow-token-list/main/token-registry/$%7BYOUR_DIRECTORY_NAME%7D/$%7BYOUR_LOGO%7D)**
    - **`tags`**
        - Tags válidas são definidas **[aqui](https://github.com/FlowFans/flow-token-list/blob/596f711e1798e358e118a0f223254b75088bd652/token-registry/template.tokenlist.json#L5)**, não use qualquer outra tag
    - **`extensions`**
        - O bloco de **`extensions`** pode conter links para seu twitter, discord, etc. Uma lista de extensões permitidas está **[aqui](https://github.com/FlowFans/flow-token-list/blob/596f711e1798e358e118a0f223254b75088bd652/src/lib/tokenlist.ts#L30)**
        - **`coingeckoId`** é a string que aparece como 'API id' na página correspondente do coingecko
- Certifique-se de que seus arquivos JSON estão formatados usando prettier(deveria ser feito automaticamente se você instalou as dependências deste projeto)
- Por favor, agrupe os commits em um único commit para limpeza

## **Modificando token existente**

Para modificar um token existente:

- Certifique-se de que o título do seu pull request comece com **`feat(UpdateToken):`**
- Modificações só são permitidas dentro da pasta **`token-registry`**
- Você só pode modificar um token por vez
- Você só pode modificar os arquivos abaixo:

```
Copy code
logo.png
token.json
logo-large.png
logo.svg
testnet.token.json

```

- Arquivos PNG em seu pull request devem ser menores que 20KB
- Arquivos JSON em seu pull request devem estar de acordo com o **[esquema](https://github.com/FlowFans/flow-token-list/blob/596f711e1798e358e118a0f223254b75088bd652/src/schemas/token.schema.json)**
- Certifique-se de que seus arquivos JSON estão formatados usando prettier(deveria ser feito automaticamente se você instalou as dependências deste projeto)
- Por favor, verifique a guia 'Arquivos alterados' em seu PR para garantir que sua mudança é como esperado
- Por favor, link o commit ou PR onde o token foi originalmente adicionado. Se o token foi adicionado por outra pessoa, ela será solicitada a confirmar que esta alteração é autorizada
- Por favor, agrupe os commits em um único commit para limpeza

## **Versionamento Semântico**

Listas incluem um campo de versão, que segue o versionamento semântico.

Versões de lista devem seguir as regras:

- Incrementar a versão principal quando tokens são removidos
- Incrementar a versão secundária quando tokens são adicionados
- Incrementar a versão de correção quando tokens já na lista têm detalhes menores alterados (nome, símbolo, URL do logo, decimais)

Alterar um endereço de token ou ID de cadeia é considerado tanto uma remoção quanto uma adição, e deve ser uma atualização da versão principal.

Observe que o versionamento da lista é usado para melhorar a experiência do usuário, mas não para segurança, ou seja, as versões da lista não se destinam a fornecer proteção contra atualizações maliciosas de uma lista de tokens; ou seja, o semver da lista é usado como uma compressão lossy do diff das atualizações da lista. As atualizações da lista ainda podem ser diferenciadas no dApp do cliente.
