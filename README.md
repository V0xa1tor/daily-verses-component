# Componente DailyVerses

Este componente mostra versículos diários ou aleatórios do site [Daily Verses](https://dailyverses.net), 
assim como é instruído na página [versículo do dia em seu site](https://dailyverses.net/pt/vers%C3%ADculo-do-dia-em-seu-site).

É um component [Vue](https://vuejs.org) utilizado com [Nuxt](https://nuxt.com), [TypeScript](https://www.typescriptlang.org) e [Bootstrap](https://getbootstrap.com).

Uma implementação deste componente está visível 👉🏻 [neste site](https://ieq-residencial-america.pages.dev).

# Como ele funciona?

O script é importado do site e automaticamente executado, injetando um texto HTML dentro do `dailyVersesWrapper`.
Ele já vem formatado com um link para o site deles, que eu gostaria de tirar (desculpa).

![image](https://github.com/user-attachments/assets/1c139bba-00bb-4968-a38b-aa2fa5b9e5f9)


Para estilizar como bem entender foi necessário extrair o texto injetado.
Para isso, foi escondido o elemento `dailyVersesWrapper` e configurado um [observador](https://developer.mozilla.org/pt-BR/docs/Web/API/MutationObserver) 
para extrair o texto assim que ele fosse injetado.

# Variações

É possível pegar o versículo do dia

![image](https://github.com/user-attachments/assets/6e66ef85-125f-4ae7-9412-ca0ccb19fdad)

ou gerar um versículo aleatório

![image](https://github.com/user-attachments/assets/e26f4c73-f695-4880-8fd9-1d98830c206c)

também é possível alterar para uma tradução diferente (em português são apenas ARA, ARC e NVI).

Há pedaços do código comentados que ativam o uso do [localStorage](https://developer.mozilla.org/pt-BR/docs/Web/API/Window/localStorage) 
para salvar a escolha do usuário no próprio navegador.
