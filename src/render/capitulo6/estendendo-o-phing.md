---
layout: page
title: Estendendo o Phing
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#ch.extending
---

O Phing foi projetado para ser flexível e fácilmente estendido. Seu núcleo e suas tarefas opcionais fornecem grande flexibilidade em processar arquivos, executar ações no banco de dados, e até mesmo receber retorno do usuário durante o processo de build. Em alguns casos, porém, as tarefas existentes não serão suficientes e, por causa da sua arquitetura aberta e modular, adicionar funcionalidades que você precisa é quase sempre trivial.

Neste capítulo nós vamos ver, principalmente, como criar suas próprias tasks, uma vez que esta é a maneira mais útil de se estender o Phing. Nós também daremos mais informações sobre sua arquitetura e seus processos internos.